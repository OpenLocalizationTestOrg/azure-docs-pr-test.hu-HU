---
title: "Azure aaaCommand külön buildet |} Microsoft Docs"
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
ms.openlocfilehash: 295b61ba162dd4373ee3f56cc1462decb3e16762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="building-azure-projects-from-hello-command-line"></a>Az Azure projektek hello parancssorból felépítése
Hello Microsoft Build Engine (MSBuild) használ, termékek, amelyen nincs telepítve a Visual Studio build-laborkörnyezetekben hozhat létre. MSBuild bővíthető és teljes mértékben támogatja a Microsoft project fájlok XML formátumot használ. Hello MSBuild fájlformátum használatával, leírhatja elemeket kell egy vagy több platformon, és konfigurációk készült.

MSBuild hello parancssorból is futtathatja, és ez a témakör ismerteti azt a módszert. Hello parancssorban hozhat létre a projekt specifikus konfigurációk állítható be. Ehhez hasonlóan is definiálhat MSBuild épülő hello célokat. Parancssori paraméterek és MSBuild kapcsolatos további információkért lásd: [MSBuild parancssori útmutatóban](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>MSBuild-paraméterek
hello legegyszerűbb módja toocreate csomag a hello MSBuild toorun `/t:Publish` lehetőséget. Alapértelmezés szerint ez a parancs létrehoz egy könyvtárat kapcsolat toohello gyökérmappában hello projekthez, például a `<ProjectDirectory>\bin\Configuration\app.publish\`. Azure-projekt összeállításakor két fájlok jönnek létre: hello csomagfájl és hello hozzá tartozó konfigurációs fájlt:

* Alkalmazáscsomag fájl (`project.cspkg`)
* Konfigurációs fájl (`ServiceConfiguration.TargetProfile.cscfg`)

Alapértelmezés szerint minden Azure-projekt helyi (hibakereséshez) buildek ki egy szolgáltatás-konfigurációs fájlt, majd egy másikat a felhő (átmeneti vagy üzemi) buildek foglalja magában. Azonban hozzáadása vagy eltávolítása a szolgáltatás-konfigurációs fájlok, igény szerint. Visual Studio csomag összeállításakor mellett hello csomag mely szolgáltatás-konfigurációs fájl tooinclude kell adnia. Ha egy csomag MSBuild hozhat létre, hello helyi szolgáltatás-konfigurációs fájl veszi fel alapértelmezés szerint. tooinclude egy másik szolgáltatás-konfigurációs fájl, set hello `TargetProfile` hello MSBuild parancs tulajdonságát (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Ha azt szeretné, hogy toouse hello a másik könyvtár tárolt csomag- és konfigurációs fájljait, set hello elérési hello segítségével `/p:PublishDir=Directory\` beállítást, többek között a záró perjelet elválasztó hello.

## <a name="next-steps"></a>Következő lépések
Miután összeállította hello csomag, ezután telepítheti azt tooAzure. Az oktatóanyag azt mutatja be, hogyan tooautomate, amely feldolgozni, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](./cloud-services/cloud-services-dotnet-continuous-delivery.md).

