---
title: "Azure Szolgáltatásvégpontokra"
description: "Az Eclipse Azure eszköztára Azure szolgáltatás végpontjának beállításait ismerteti."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9c6125ec-7278-461e-b69c-ed56e844f742
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 6059c292c2687f1bf3d9be04c03aaaaf6adde945
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-endpoints"></a>Azure Szolgáltatásvégpontokra
Azure szolgáltatásvégpontokra határozza meg, hogy az alkalmazás központi telepítése és a globális Azure platform által kezelt, Azure által üzemeltetett Kína, vagy egy olyan magánhálózat 21Vianet Azure platformon. A **Szolgáltatásvégpontok** párbeszédpanel lehetővé teszi, hogy mely használni kívánt Szolgáltatásvégpontok megadását. Megnyitásához a **Szolgáltatásvégpontok** párbeszédpaneljén eclipse-ben kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure**, és kattintson a **Szolgáltatásvégpontok**. Beállítás a **aktív beállítása** mező határozza meg, hogy melyik Azure szolgáltatásvégpontokra fog történni az Azure projektek az aktuális munkaterületen.

Az alábbi látható a **Szolgáltatásvégpontok** párbeszédpanel.

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>A szolgáltatás végpontokat
Az a **Szolgáltatásvégpontok** párbeszédpanelen tegye a következők közül:

* Ha szeretné-e a globális Azure platformon, használja a **aktív beállítása** legördülő listában válassza ki **windowsazure.com** kattintson **OK**.

* Ha a Kínában a 21Vianet által működtetett Azure használni kívánt a **aktív beállítása** legördülő listában válassza ki **windowsazure.cn (Kína)** kattintson **OK**.

* Ha szeretné használni a saját Azure platformon:

  1. Kattintson a **Szerkesztés** gombra.

  2. Megnyílik egy párbeszédpanel, hogy értesítést küld, amely a **Szolgáltatásvégpontok** párbeszédpanel bezárul, és a készletek beállításfájlt fogja megnyitni. Kattintson az **OK** gombra.

  3. A preferencesets.xml fájlban, hozzon létre egy új `preferenceset` elemet. Az új elem létrehozása `name`, `blob`, `management`, `portalURL` és `publishsettings` attribútumot, majd adja hozzá az értékek azokat, amelyek megfelelnek a saját Azure platformon. Használhatja a meglévő megadott értékek `preferenceset` elemek sablonként. **Megjegyzés:**: használt érték a `blob` attribútum az URL-cím "blob" szöveget kell tartalmaznia.

  4. Mentse és zárja be a preferencesets.xml.

  5. Nyissa meg újra a **Szolgáltatásvégpontok** párbeszédpanel.

  6. Az a **aktív beállítása** legördülő listában válassza ki az aktív beállítása létrehozott, és kattintson a **OK**.

  7. Ha létrehozta a saját Azure platformon `preferenceset` elem, kattintva rendelt értékeket módosíthatja a **szerkesztése** gombra a **Services végpontjánál** párbeszédpanel. Több privát Azure platformon is létrehozhat `preferenceset` elemek, ha felügyelni.

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse] 

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
