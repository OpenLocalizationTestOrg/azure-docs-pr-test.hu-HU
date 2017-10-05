---
title: "Az a Visual Studio Felhőszolgáltatás-alkalmazások hibakeresését emulator express telepítő |} Microsoft Docs"
description: "Ismerteti, hogyan telepítse a C++ terjeszthető változatának emulátor a Visual Studio Express engedélyezése"
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
ms.openlocfilehash: 05d672dcb1335c617bb8d8cae43947bcd5e9ab3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-emulator-express-to-debug-cloud-services-application-in-vs-2017"></a><span data-ttu-id="fe03e-103">A Visual STUDIO 2017 Felhőszolgáltatások alkalmazás hibakeresése a Emulator Express használatával</span><span class="sxs-lookup"><span data-stu-id="fe03e-103">Use Emulator Express to debug Cloud Services application in VS 2017</span></span>
<span data-ttu-id="fe03e-104">Ez a cikk ismerteti, hogyan Emulator Express az VS 2017 a Felhőszolgáltatás-alkalmazások hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="fe03e-104">This article explains how to use Emulator Express to debug Cloud Services applications in VS 2017.</span></span>

## <a name="background-context"></a><span data-ttu-id="fe03e-105">Háttér-környezet</span><span class="sxs-lookup"><span data-stu-id="fe03e-105">Background context</span></span>
<span data-ttu-id="fe03e-106">Cloud Services webes és feldolgozói szerepkörök a Visual Studio hibakeresési Emulator Express használja alapértelmezetten.</span><span class="sxs-lookup"><span data-stu-id="fe03e-106">Emulator Express is used by default for debugging Cloud Services Web and Worker roles in Visual Studio.</span></span> <span data-ttu-id="fe03e-107">Ez a beállítás a Felhőszolgáltatások tulajdonságai lap van megadva.</span><span class="sxs-lookup"><span data-stu-id="fe03e-107">This setting is specified in the Cloud Services project properties page.</span></span>

![Nyissa meg a projekt tulajdonságai][0]

![Alapértelmezett emulátor expressz legyen kijelölve][1]

<span data-ttu-id="fe03e-110">A [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] az emulátor által igényelt Visual Studio express.</span><span class="sxs-lookup"><span data-stu-id="fe03e-110">The [Visual C++ Redistributable][Visual C++ Redistributable] for Visual Studio is required by Emulator express.</span></span> <span data-ttu-id="fe03e-111">Jelenleg nincs telepítve az Azure áttelepítendő feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fe03e-111">Currently it is not installed with the Azure workload.</span></span> <span data-ttu-id="fe03e-112">F5 hitelesítési módok egy Felhőszolgáltatás-alkalmazások hibakeresését, akkor a Visual Studio telepíteni ezt az összetevőt, és folytassa a hibakeresés volna kérni.</span><span class="sxs-lookup"><span data-stu-id="fe03e-112">Upon F5 gesture to debug a Cloud Services applications, Visual Studio would prompt to install this component and proceed with debugging.</span></span>

![Rákérdezés a C++ újraterjeszthető csomag telepítése][2]

<span data-ttu-id="fe03e-114">Kattintson az Igen gombra C++ újraterjeszthető csomag telepítését.</span><span class="sxs-lookup"><span data-stu-id="fe03e-114">Click Yes to install C++ Redistributable.</span></span>

![C++ terjeszthető változatának telepítése][3]

<span data-ttu-id="fe03e-116">Nyomja le az F5 újra indítsa el a hibakeresési munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="fe03e-116">Press F5 again to launch debugging sessions.</span></span>

![A hibakeresés elindításához][4]

![Hibakeresés sikeres][5]

> <span data-ttu-id="fe03e-119">Megjegyzés: A Visual C++ terjeszthető változatának telepítése egy alkalommal beavatkozást.</span><span class="sxs-lookup"><span data-stu-id="fe03e-119">Note: Installing Visual C++ Redistributable is a onetime effort.</span></span> <span data-ttu-id="fe03e-120">Ha az Azure SDK-t egy korábbi verziójából volt frissítése és Emulator Express telepítése, majd nem észlel a probléma.</span><span class="sxs-lookup"><span data-stu-id="fe03e-120">If you were upgrading from an older version of Azure SDK and have installed Emulator Express, then you won’t encounter this problem.</span></span>
> 
> 

## <a name="manual-workaround"></a><span data-ttu-id="fe03e-121">Manuális megoldás</span><span class="sxs-lookup"><span data-stu-id="fe03e-121">Manual workaround</span></span>
<span data-ttu-id="fe03e-122">Is telepíthet a [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] manuálisan, és, hogyan Visual Studio telepítve a rendszeren, ugyanaz az eredmény lesz alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="fe03e-122">You can also install the [Visual C++ Redistributable][Visual C++ Redistributable] manually and same effect will be applied as how Visual Studio installed it on your system.</span></span>

<span data-ttu-id="fe03e-123">[vcredist_x86.exe][vcredist_x86.exe]</span><span class="sxs-lookup"><span data-stu-id="fe03e-123">[vcredist_x86.exe][vcredist_x86.exe]</span></span>

<span data-ttu-id="fe03e-124">[vcredist_x64.exe][vcredist_x64.exe]</span><span class="sxs-lookup"><span data-stu-id="fe03e-124">[vcredist_x64.exe][vcredist_x64.exe]</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe03e-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe03e-125">Next Steps</span></span>
<span data-ttu-id="fe03e-126">További tudnivalók a Visual Studio Felhőszolgáltatás-alkalmazások hibakeresését Azure számítógép Emulator használatával: [Emulator Express használatával futtatni, és egy felhőalapú szolgáltatás a helyi számítógépen hibakeresési][Using Emulator Express to run and debug a cloud service on a local machine]</span><span class="sxs-lookup"><span data-stu-id="fe03e-126">Learn more about using Azure Computer Emulator to debug your Cloud Services applications in Visual Studio: [Using Emulator Express to run and debug a cloud service on a local machine][Using Emulator Express to run and debug a cloud service on a local machine]</span></span>

[Visual C++ Redistributable]:https://www.microsoft.com/en-us/download/details.aspx?id=30679
[vcredist_x86.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe
[vcredist_x64.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
[Using Emulator Express to run and debug a cloud service on a local machine]:https://azure.microsoft.com/en-us/documentation/articles/vs-azure-tools-emulator-express-debug-run/

[0]: ./media/cloud-services-emulator-express-fix/vs-05.png
[1]: ./media/cloud-services-emulator-express-fix/vs-06.png
[2]: ./media/cloud-services-emulator-express-fix/vs-01.png
[3]: ./media/cloud-services-emulator-express-fix/vs-02.png
[4]: ./media/cloud-services-emulator-express-fix/vs-03.png
[5]: ./media/cloud-services-emulator-express-fix/vs-04.png
