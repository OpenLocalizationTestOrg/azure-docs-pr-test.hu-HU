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
# <a name="use-emulator-express-toodebug-cloud-services-application-in-vs-2017"></a>Alkalmazásával Emulator Express toodebug Felhőszolgáltatások Visual STUDIO 2017
Ez a cikk azt ismerteti, hogyan toouse Emulator Express toodebug Felhőszolgáltatások alkalmazások VS 2017.

## <a name="background-context"></a>Háttér-környezet
Cloud Services webes és feldolgozói szerepkörök a Visual Studio hibakeresési Emulator Express használja alapértelmezetten. Ez a beállítás van megadva a hello Felhőszolgáltatások projekt Tulajdonságok lapján.

![Nyissa meg a projekt tulajdonságai][0]

![Alapértelmezett emulátor expressz legyen kijelölve][1]

Hello [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] az emulátor által igényelt Visual Studio express. Jelenleg nincs telepítve az Azure számítási hello. F5 esetén akár többérintéses kézmozdulatokkal toodebug egy Felhőszolgáltatás-alkalmazások, a Visual Studio ehhez kérni tooinstall ezt az összetevőt, és folytassa a hibakeresést.

![Rákérdezés a C++ újraterjeszthető csomag telepítése][2]

Kattintson az Igen tooinstall C++ újraterjeszthető csomag.

![C++ terjeszthető változatának telepítése][3]

Nyomja le az F5 újra toolaunch hibakeresési munkamenetek.

![A hibakeresés elindításához][4]

![Hibakeresés sikeres][5]

> Megjegyzés: A Visual C++ terjeszthető változatának telepítése egy alkalommal beavatkozást. Ha az Azure SDK-t egy korábbi verziójából volt frissítése és Emulator Express telepítése, majd nem észlel a probléma.
> 
> 

## <a name="manual-workaround"></a>Manuális megoldás
Hello is telepíthet [Visual C++ újraterjeszthető csomag] [ Visual C++ Redistributable] manuálisan, és, hogyan Visual Studio telepítve a rendszeren, ugyanaz az eredmény lesz alkalmazva.

[vcredist_x86.exe][vcredist_x86.exe]

[vcredist_x64.exe][vcredist_x64.exe]

## <a name="next-steps"></a>Következő lépések
További információ Azure számítógép emulátor toodebug a Felhőszolgáltatások alkalmazásokat a Visual Studióban: [Emulator Express használatával toorun és hibakeresési egy felhőalapú szolgáltatás a helyi számítógépen][Using Emulator Express toorun and debug a cloud service on a local machine]

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
