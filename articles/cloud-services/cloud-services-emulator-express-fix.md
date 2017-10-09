---
title: "aaaSetup emulator express toodebug Felhőszolgáltatások alkalmazásokat a Visual Studióban |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooinstall hello C++ redistributable tooenable emulátor a Visual Studio Express"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 22b20f7a-23f4-4f7f-b536-3bf1e01adcd1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: 6fb506f0b1384f2e52310799eb5ae2a102d777bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-emulator-express-toodebug-cloud-services-application-in-vs-2017"></a><span data-ttu-id="ffdb7-103">Alkalmazásával Emulator Express toodebug Felhőszolgáltatások Visual STUDIO 2017</span><span class="sxs-lookup"><span data-stu-id="ffdb7-103">Use Emulator Express toodebug Cloud Services application in VS 2017</span></span>
<span data-ttu-id="ffdb7-104">Ez a cikk azt ismerteti, hogyan toouse Emulator Express toodebug Felhőszolgáltatások alkalmazások VS 2017.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-104">This article explains how toouse Emulator Express toodebug Cloud Services applications in VS 2017.</span></span>

## <a name="background-context"></a><span data-ttu-id="ffdb7-105">Háttér-környezet</span><span class="sxs-lookup"><span data-stu-id="ffdb7-105">Background context</span></span>
<span data-ttu-id="ffdb7-106">Cloud Services webes és feldolgozói szerepkörök a Visual Studio hibakeresési Emulator Express használja alapértelmezetten.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-106">Emulator Express is used by default for debugging Cloud Services Web and Worker roles in Visual Studio.</span></span> <span data-ttu-id="ffdb7-107">Ez a beállítás van megadva a hello Felhőszolgáltatások projekt Tulajdonságok lapján.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-107">This setting is specified in hello Cloud Services project properties page.</span></span>

![Nyissa meg a projekt tulajdonságai][0]

![Alapértelmezett emulátor expressz legyen kijelölve][1]

<span data-ttu-id="ffdb7-110">Hello [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] az emulátor által igényelt Visual Studio express.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-110">hello [Visual C++ Redistributable][Visual C++ Redistributable] for Visual Studio is required by Emulator express.</span></span> <span data-ttu-id="ffdb7-111">Jelenleg nincs telepítve az Azure számítási hello.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-111">Currently it is not installed with hello Azure workload.</span></span> <span data-ttu-id="ffdb7-112">F5 esetén akár többérintéses kézmozdulatokkal toodebug egy Felhőszolgáltatás-alkalmazások, a Visual Studio ehhez kérni tooinstall ezt az összetevőt, és folytassa a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-112">Upon F5 gesture toodebug a Cloud Services applications, Visual Studio would prompt tooinstall this component and proceed with debugging.</span></span>

![Rákérdezés a C++ újraterjeszthető csomag telepítése][2]

<span data-ttu-id="ffdb7-114">Kattintson az Igen tooinstall C++ újraterjeszthető csomag.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-114">Click Yes tooinstall C++ Redistributable.</span></span>

![C++ terjeszthető változatának telepítése][3]

<span data-ttu-id="ffdb7-116">Nyomja le az F5 újra toolaunch hibakeresési munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-116">Press F5 again toolaunch debugging sessions.</span></span>

![A hibakeresés elindításához][4]

![Hibakeresés sikeres][5]

> <span data-ttu-id="ffdb7-119">Megjegyzés: A Visual C++ terjeszthető változatának telepítése egy alkalommal beavatkozást.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-119">Note: Installing Visual C++ Redistributable is a onetime effort.</span></span> <span data-ttu-id="ffdb7-120">Ha az Azure SDK-t egy korábbi verziójából volt frissítése és Emulator Express telepítése, majd nem észlel a probléma.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-120">If you were upgrading from an older version of Azure SDK and have installed Emulator Express, then you won’t encounter this problem.</span></span>
> 
> 

## <a name="manual-workaround"></a><span data-ttu-id="ffdb7-121">Manuális megoldás</span><span class="sxs-lookup"><span data-stu-id="ffdb7-121">Manual workaround</span></span>
<span data-ttu-id="ffdb7-122">Hello is telepíthet [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] manuálisan, és, hogyan Visual Studio telepítve a rendszeren, ugyanaz az eredmény lesz alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="ffdb7-122">You can also install hello [Visual C++ Redistributable][Visual C++ Redistributable] manually and same effect will be applied as how Visual Studio installed it on your system.</span></span>

<span data-ttu-id="ffdb7-123">[vcredist_x86.exe][vcredist_x86.exe]</span><span class="sxs-lookup"><span data-stu-id="ffdb7-123">[vcredist_x86.exe][vcredist_x86.exe]</span></span>

<span data-ttu-id="ffdb7-124">[vcredist_x64.exe][vcredist_x64.exe]</span><span class="sxs-lookup"><span data-stu-id="ffdb7-124">[vcredist_x64.exe][vcredist_x64.exe]</span></span>

## <a name="next-steps"></a><span data-ttu-id="ffdb7-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ffdb7-125">Next Steps</span></span>
<span data-ttu-id="ffdb7-126">További információ Azure számítógép emulátor toodebug a Felhőszolgáltatások alkalmazásokat a Visual Studióban: [Emulator Express használatával toorun és hibakeresési egy felhőalapú szolgáltatás a helyi számítógépen][Using Emulator Express toorun and debug a cloud service on a local machine]</span><span class="sxs-lookup"><span data-stu-id="ffdb7-126">Learn more about using Azure Computer Emulator toodebug your Cloud Services applications in Visual Studio: [Using Emulator Express toorun and debug a cloud service on a local machine][Using Emulator Express toorun and debug a cloud service on a local machine]</span></span>

[Visual C++ Redistributable]:https://www.microsoft.com/en-us/download/details.aspx?id=30679
[vcredist_x86.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe
[vcredist_x64.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
[Using Emulator Express toorun and debug a cloud service on a local machine]:https://azure.microsoft.com/en-us/documentation/articles/vs-azure-tools-emulator-express-debug-run/

[0]: ./media/cloud-services-emulator-express-fix/vs-05.png
[1]: ./media/cloud-services-emulator-express-fix/vs-06.png
[2]: ./media/cloud-services-emulator-express-fix/vs-01.png
[3]: ./media/cloud-services-emulator-express-fix/vs-02.png
[4]: ./media/cloud-services-emulator-express-fix/vs-03.png
[5]: ./media/cloud-services-emulator-express-fix/vs-04.png
