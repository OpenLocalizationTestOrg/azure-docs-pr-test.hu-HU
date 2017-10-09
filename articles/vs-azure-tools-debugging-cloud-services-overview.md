---
title: "Azure hibakereséshez aaaOptions a felhőalapú szolgáltatások |} Microsoft Docs"
description: "Hibakeresési Azure cloud services csomag"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: kraigb
ms.openlocfilehash: 8b7779ca33cfd82fd5fbccf229ea822c311bdea0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-hello-various-ways-toodebug-an-azure-cloud-service"></a>Ismerje meg a különböző módokon hello toodebug Azure-felhőszolgáltatás
Ez a cikk ismerteti hivatkozások toohello különböző módokon toodebug egy Azure felhőszolgáltatást. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Egy Azure-felhőszolgáltatásban a Visual Studio hibakereső
Mentheti időt és pénzt hello Azure compute emulator toodebug használatával a felhőalapú szolgáltatás a helyi számítógépen. Által a szolgáltatás telepítése előtt helyileg hibakeresés, akkor is megbízhatóságának és teljesítményének javítása számítási alkalommal fizető nélkül. Azonban néhány hiba is felléphet, csak akkor, ha egy felhőalapú szolgáltatás futtatja az Azure-ban. Csak akkor, ha egy felhőalapú szolgáltatás futtatja az Azure-ban jelentkező hibákról is indítja el a távoli hibakeresés, a szolgáltatás közzétételekor, és majd csatolása hello hibakereső tooa szerepkörpéldányt engedélyezése. További információkért lásd: [a felhőalapú szolgáltatás a helyi számítógépen Debug](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-azure-diagnostics"></a>Az Azure Diagnostics használatával 
Használhatja az Azure Diagnostics toolog részletes kód futását szerepköröket, az adatokat, hogy hello szerepkörök hello fejlesztői környezetben vagy az Azure-ban futtatja. További információkért lásd: [Azure Diagnostics engedélyezése az Azure Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=400450).

## <a name="using-intellitrace"></a>Az IntelliTrace 
Ha használja a Visual Studio Enterprise toowrite szerepkörök megcélzott .NET-keretrendszer 4.5, IntelliTrace engedélyezheti, hogy telepít egy Azure-felhőszolgáltatásban a Visual Studio eszközből hello időpontban. IntelliTrace biztosít a napló használható a Visual Studio toodebug az alkalmazás, mintha csak az Azure-ban futna. További információkért lásd: [egy közzétett felhőszolgáltatás IntelliTrace és a Visual Studio hibakeresési](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Távoli hibakeresés 
Engedélyezheti a távoli hibakeresés a felhőszolgáltatásokban hello felhőalapú szolgáltatás, a Visual Studio eszközből telepítésekor hello időpontban. Ha úgy dönt, hogy a távoli hibakeresés a központi telepítés tooenable, távoli hibakeresési szolgáltatásai telepítve vannak minden szerepkörpéldányt futó virtuális gépek hello. Ezek szolgáltatások – például `msvsmon.exe` – nincs hatása a teljesítményre vagy további költségeket eredményez. További információkért lásd: [hibakeresése az Azure-felhőszolgáltatás](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Következő lépések
- [Egy Azure-felhőszolgáltatásban vagy a virtuális gép a Visual Studio hibakeresési](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -megtudhatja, hogyan toodebug Azure cloud services hello részleteit.
