---
title: "Nagy telepítések aaaDeploying"
description: "Ismerje meg, hogyan toodeploy nagy üzembe helyezése hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 6b1d2a7a5e49c78154fc856a221e64ca8dcfbe9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-large-deployments"></a>Nagy központi telepítése
Ha a telepítés túl nagy toobe hello alapértelmezett approot mappában található, használhatja a helyi tároló egyik erőforrásához hello telepítési legfelső szintű mappa a JDK és az alkalmazáskiszolgálóhoz.

## <a name="toouse-a-local-storage-resource-as-hello-deployment-root-folder-for-large-deployments"></a>a helyi tároló egyik erőforrásához hello telepítési legfelső szintű mappa a nagyméretű telepítések toouse
1. Hozzon létre egy új helyi tároló-erőforrás. hello hello erőforrás neve nem lényeges. Tároló-erőforrások hello szerepkör szintjén van meghatározva. hello leggyorsabb módon tooaccess hello helyi tároló konfigurációs párbeszédpanel, ahol létrehozhat egy új helyi tároló-erőforrás van hello lépések segítségével: kattintson a jobb gombbal hello szerepköre hello **Project Explorer** nézet (bontsa ki a Azure-projekt csomópont Ha hello szerepkör nem látható), kattintson a **Azure**, és kattintson a **helyi tároló**. Hello belül **helyi tároló** párbeszédpanel, kattintson a **Hozzáadás** toocreate egy új helyi tároló-erőforrás.

2. Set hello szükséges méret tooat legalább 2048 MB-ot (semmit kevesebb problémái lehetnek hello ugyanazon fájl méretét, akkor tapasztal hello approot).

3. Győződjön meg arról, hogy **hello tartalmának törlése hello szerepkörpéldányt újrahasznosítása esetén** be van jelölve; ez segít futtathatják a hello telepítési ügyfélindítási logikája ütközik a már meglévő fájlok hello erőforrás amikor hello szerepkör a példány újrahasznosított.

4. Győződjön meg arról, hogy hello **környezeti változó tárolásához hello erőforrás könyvtár elérési útja a telepítés utáni** toohello karakterlánc értéke **DEPLOYROOT**. A helyi tároló-erőforrás párbeszédpanel hasonló toohello következő fog kinézni.

   ![][ic667943]

Azt is megteheti Ha **DEPLOYROOT** hello, *neve* a helyi erőforrás, és hogy ne változtassa meg hello automatikusan generált környezeti változó neve (amely lesz beállítva, túl **DEPLOYROOT_PATH** ebben az esetben), működő lenne, az alkalmazáshoz.

A helyi tároló egyik erőforrásához létrehozásával kapcsolatos további információk találhatók [helyi tároló tulajdonságainak][Local storage properties].

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
