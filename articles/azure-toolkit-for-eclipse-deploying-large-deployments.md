---
title: "Nagy központi telepítése"
description: "Megtudhatja, hogyan telepítheti az Azure-eszközkészlet használata az Eclipse nagy méretű telepítéséhez."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: e12e379e2b6727653e2377b1760c3745596a1e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploying-large-deployments"></a>Nagy központi telepítése
Ha a telepítés túl nagy ahhoz, hogy az alapértelmezett approot mappában található, használhatja a helyi tároló egyik erőforrásához, a központi telepítés gyökérmappa a JDK és az alkalmazáskiszolgálóhoz.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>A helyi tároló egyik erőforrásához használandó telepítési gyökérmappájába nagy méretű telepítéséhez
1. Hozzon létre egy új helyi tároló-erőforrás. Az erőforrás neve nem lényeges. Tároló-erőforrások a szerepkör szintjén van meghatározva. A leggyorsabban úgy tudja elérni a helyi tároló konfigurációs párbeszédpanel, ahol létrehozhat egy új helyi tároló-erőforrás van a következő lépések segítségével: kattintson a jobb gombbal a szerepkör a **Project Explorer** nézet (bontsa ki az Azure-projekt csomópont Ha a szerepkör nem látható), kattintson **Azure**, és kattintson a **helyi tároló**. Belül a **helyi tároló** párbeszédpanel, kattintson a **Hozzáadás** válasszal létrehozhat egy új helyi tárterület.

2. A kívánt méretet állítsa legalább 2048 MB-ra (semmit kevesebb problémái lehetnek ugyanazt a fájlt méretét, akkor tapasztal a approot).

3. Győződjön meg arról, hogy **tiszta tartalmát, amikor a szerepkör példánya újraindul** be van jelölve; ez segít a központi telepítés ügyfélindítási logikája a szerepkörpéldány esetén az erőforrás már meglévő fájlok való ütközések futásának megakadályozása felhasználását.

4. Győződjön meg arról, hogy a **környezeti változó tárolja az erőforrás elérési útja a telepítés utáni** a karakterlánc értéke **DEPLOYROOT**. A helyi tároló-erőforrás párbeszédpanel az alábbihoz hasonlóan fog kinézni.

   ![][ic667943]

Azt is megteheti Ha **DEPLOYROOT** , a *neve* a helyi erőforrások és, ne módosítsa az automatikusan generált környezeti változó neve (amely úgy lesz beállítva, **DEPLOYROOT_ Elérési út** ebben az esetben), működő lenne, az alkalmazáshoz.

A helyi tároló egyik erőforrásához létrehozásával kapcsolatos további információk találhatók [helyi tároló tulajdonságainak][Local storage properties].

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
