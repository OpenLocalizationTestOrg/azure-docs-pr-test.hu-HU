---
title: Az Azure parancssori build |} Microsoft Docs
description: Az Azure parancssori build
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 5fe910e2757dd5ec783538e23e7f52e2f5725b39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="building-azure-projects-from-the-command-line"></a>A parancssorból Azure projektek felépítése
A Microsoft Build motor (MSBuild) használata, amelyen nincs telepítve a Visual Studio build-laborkörnyezetekben termékek hozhat létre. MSBuild bővíthető és teljes mértékben támogatja a Microsoft project fájlok XML formátumot használ. Az MSBuild formátumát használva leírhatja elemeket kell egy vagy több platformon, és konfigurációk készült.

MSBuild a parancssorból is futtathatja, és ez a témakör ismerteti azt a módszert. A parancssorban hozhat létre a projekt specifikus konfigurációk állítható be. Ehhez hasonlóan is meghatározhat, amelyeknek MSBuild alkot. Parancssori paraméterek és MSBuild kapcsolatos további információkért lásd: [MSBuild parancssori útmutatóban](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>MSBuild-paraméterek
A legegyszerűbben-csomag létrehozása után az MSBuild futtatásához a `/t:Publish` lehetőséget. Alapértelmezés szerint ez a parancs létrehoz egy könyvtárat a projekthez, amely a gyökérmappában található viszonyítva például `<ProjectDirectory>\bin\Configuration\app.publish\`. Azure-projekt összeállításakor két fájlok jönnek létre: a csomag fájl maga és a hozzá tartozó konfigurációs fájlt:

* Alkalmazáscsomag fájl (`project.cspkg`)
* Konfigurációs fájl (`ServiceConfiguration.TargetProfile.cscfg`)

Alapértelmezés szerint minden Azure-projekt helyi (hibakereséshez) buildek ki egy szolgáltatás-konfigurációs fájlt, majd egy másikat a felhő (átmeneti vagy üzemi) buildek foglalja magában. Azonban hozzáadása vagy eltávolítása a szolgáltatás-konfigurációs fájlok, igény szerint. Visual Studio csomag összeállításakor mely szolgáltatás-konfigurációs fájlt a mellett a csomag kell adnia. Ha egy csomag MSBuild hozhat létre, a helyi szolgáltatás-konfigurációs fájl veszi fel alapértelmezés szerint. Egy másik szolgáltatás-konfigurációs fájlt, adja meg a `TargetProfile` az MSBuild parancs tulajdonságának (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Szeretne másik könyvtár a tárolt csomag- és konfigurációs fájljait használja, ha az elérési útjának beállítása használatával a `/p:PublishDir=Directory\` beállítást, többek között a záró perjelet elválasztó.

## <a name="next-steps"></a>Következő lépések
Miután összeállította a csomagot, telepítheti az Azure-bA. Ez az oktatóanyag bemutatja, hogyan folyamat automatizálásához, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](./cloud-services/cloud-services-dotnet-continuous-delivery.md).

