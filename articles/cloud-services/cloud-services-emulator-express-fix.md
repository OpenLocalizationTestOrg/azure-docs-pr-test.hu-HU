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
# <a name="use-emulator-express-to-debug-cloud-services-application-in-vs-2017"></a>A Visual STUDIO 2017 Felhőszolgáltatások alkalmazás hibakeresése a Emulator Express használatával
Ez a cikk ismerteti, hogyan Emulator Express az VS 2017 a Felhőszolgáltatás-alkalmazások hibakeresését.

## <a name="background-context"></a>Háttér-környezet
Cloud Services webes és feldolgozói szerepkörök a Visual Studio hibakeresési Emulator Express használja alapértelmezetten. Ez a beállítás a Felhőszolgáltatások tulajdonságai lap van megadva.

![Nyissa meg a projekt tulajdonságai][0]

![Alapértelmezett emulátor expressz legyen kijelölve][1]

A [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] az emulátor által igényelt Visual Studio express. Jelenleg nincs telepítve az Azure áttelepítendő feladatokhoz. F5 hitelesítési módok egy Felhőszolgáltatás-alkalmazások hibakeresését, akkor a Visual Studio telepíteni ezt az összetevőt, és folytassa a hibakeresés volna kérni.

![Rákérdezés a C++ újraterjeszthető csomag telepítése][2]

Kattintson az Igen gombra C++ újraterjeszthető csomag telepítését.

![C++ terjeszthető változatának telepítése][3]

Nyomja le az F5 újra indítsa el a hibakeresési munkamenetek.

![A hibakeresés elindításához][4]

![Hibakeresés sikeres][5]

> Megjegyzés: A Visual C++ terjeszthető változatának telepítése egy alkalommal beavatkozást. Ha az Azure SDK-t egy korábbi verziójából volt frissítése és Emulator Express telepítése, majd nem észlel a probléma.
> 
> 

## <a name="manual-workaround"></a>Manuális megoldás
Is telepíthet a [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] manuálisan, és, hogyan Visual Studio telepítve a rendszeren, ugyanaz az eredmény lesz alkalmazva.

[vcredist_x86.exe][vcredist_x86.exe]

[vcredist_x64.exe][vcredist_x64.exe]

## <a name="next-steps"></a>Következő lépések
További tudnivalók a Visual Studio Felhőszolgáltatás-alkalmazások hibakeresését Azure számítógép Emulator használatával: [Emulator Express használatával futtatni, és egy felhőalapú szolgáltatás a helyi számítógépen hibakeresési][Using Emulator Express to run and debug a cloud service on a local machine]

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
