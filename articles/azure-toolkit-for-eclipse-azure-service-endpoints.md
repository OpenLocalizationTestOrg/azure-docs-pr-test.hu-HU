---
title: "aaaAzure Szolgáltatásvégpontok"
description: "Hello Azure eszköztára Eclipse hello Azure szolgáltatás végpontjának beállításait ismerteti."
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
ms.openlocfilehash: 357aa56409a894719077f2c8f302575c8ebb6883
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-endpoints"></a>Azure Szolgáltatásvégpontokra
Azure szolgáltatásvégpontokra határozza meg,-e az alkalmazás telepített tooand hello globális Azure platform által kezelt, Azure által üzemeltetett Kína, vagy egy olyan magánhálózat 21Vianet Azure platformon. Hello **Szolgáltatásvégpontok** párbeszédpanelen kiválaszthatja, amely szolgáltatás végpontjait toouse kívánt toospecify. tooopen hello **Szolgáltatásvégpontok** párbeszédpaneljén eclipse-ben kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure**, és kattintson a **Szolgáltatás végpontjait**. A beállítás hello **aktív beállítása** mező határozza meg, hogy melyik Azure szolgáltatás végpontok használandó hello Azure projektek az aktuális munkaterületen.

hello következő mutatja hello **Szolgáltatásvégpontok** párbeszédpanel.

![][ic719493]

## <a name="tooset-hello-service-endpoints"></a>tooset hello szolgáltatás végpontok
A hello **Szolgáltatásvégpontok** párbeszédpanelen tegye a következő műveletek hello:

* Ha azt szeretné, toouse hello globális Azure platformon, a hello **aktív beállítása** legördülő listában válassza ki **windowsazure.com** kattintson **OK**.

* Ha azt szeretné, Azure Kínában hello a 21Vianet által működtetett toouse **aktív beállítása** legördülő listában válassza ki **windowsazure.cn (Kína)** kattintson **OK**.

* Ha azt szeretné, hogy a saját Azure platformon toouse:

  1. Kattintson a **Szerkesztés** gombra.

  2. Megnyílik egy párbeszédpanel, amely arról tájékoztatja, hogy hello **Szolgáltatásvégpontok** párbeszédpanel bezárul, és hello beállítása beállításfájlt fognak megnyílni. Kattintson az **OK** gombra.

  3. Hello preferencesets.xml fájlban, hozzon létre egy új `preferenceset` elemet. Az új elem létrehozása `name`, `blob`, `management`, `portalURL` és `publishsettings` attribútumok, és azokat, amelyek megfelelnek a saját Azure platformon tooyour értékek hozzáadásához. Hello meglévő előírt hello értékeket is használhat `preferenceset` elemek sablonként. **Megjegyzés:**: hello használt érték hello `blob` attribútum hello szöveget "blob" hello URL-címet kell tartalmaznia.

  4. Mentse és zárja be a preferencesets.xml.

  5. Nyissa meg újra a hello **Szolgáltatásvégpontok** párbeszédpanel.

  6. A hello **aktív beállítása** legördülő listából válassza hello aktív beállítása létrehozott, és kattintson a **OK**.

  7. Ha létrehozta a saját Azure platformon `preferenceset` elem, módosíthatja hello hozzárendelt értékek tooit hello kattintva **szerkesztése** hello gombjára **Services végpontjánál** párbeszédpanel. Több privát Azure platformon is létrehozhat `preferenceset` elemek, ha felügyelni.

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse] 

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
