---
title: "IntelliJ aaaManage storage-fiókok használatával hello Azure-kezelővel |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure storage accounts IntelliJ hello Azure Explorer használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b094bdcd2fa2782cb12b67c96ac406fbe4c1aa3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-storage-accounts-by-using-hello-azure-explorer-for-intellij"></a>Az IntelliJ hello Azure Explorer használatával storage-fiókok kezelése

hello Azure-kezelővel hello Azure eszköztára IntelliJ része, a Java fejlesztők egy könnyen kezelhető megoldás biztosít a fiókjuk Azure storage-fiók kezeléséhez hello IntelliJ integrált fejlesztési környezeti (IDE) belül.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>A storage-fiók létrehozása az intellij-t

a storage-fiókok hello Azure Explorer használatával toocreate hello a következő:

1. Jelentkezzen be Azure-fiók tooyour hello [hello Azure eszköztára Eclipse bejelentkezési utasítások]. 

2. A hello **Azure Explorer** megtekintéséhez bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **Tárfiókok**, és kattintson a **Storage-fióklétrehozása**.

   ![A Tárfiók parancs létrehozása][CS01]

3. A hello **Storage-fiók létrehozása** párbeszédpanelen adja meg az alábbi beállítások hello:

   ![Hozzon létre új Tárfiók párbeszédpanel][CS02]

   * **Név**: hello új tárfiók hello nevét határozza meg.

   * **Fiók kind**: hello adja meg a tárolási fiók toocreate (például "Blob tároló"). További információkért lásd: [tudnivalók az Azure storage-fiókok]. 

   * **Teljesítmény**: Megadja, hogy mely tárolási toouse ajánlat hello kijelölt közzétételi (például "Prémium"). További információkért lásd: [az Azure storage méretezhetőségi és Teljesítménycélok]. 

   * **Replikációs**: hello replikációs hello tárfiókhoz (például "Zónaredundáns") határozza meg. További információkért lásd: [az Azure storage replikációs]. 

   * **Előfizetés**: hello Azure-előfizetést, amelyet az toouse hello új tárfiók határozza meg.

   * **Hely**: hello helyet a tárfiók létrehozásának helyét (például "Nyugati US") határoz meg.

   * **Erőforráscsoport**: hello erőforrás csoportot a virtuális gép határozza meg. Válasszon egyet az alábbi beállítások hello:
      * **Új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.
      * **Meglévő**: meghatározza, hogy az erőforráscsoportok az Azure-fiókjával társított listából kiválaszthatja.

4. Ha megadott hello megelőző beállítások mindegyikével, kattintson **OK**.

## <a name="create-a-storage-container-in-intellij"></a>A tároló létrehozása az intellij-t

egy tároló hello Azure Explorer használatával toocreate hello a következő:

1. A hello Azure terület Intézőbeli nézetében, kattintson a jobb gombbal hello tárfiókot, ha szeretné, hogy a tároló toocreate, és kattintson a **létrehozás blob tároló**.

   ![A blob-tároló parancs létrehozása][CC01]

2. A hello **létrehozás blob tároló** párbeszédpanelen adja meg a tároló hello nevét, és kattintson **OK**. A tároló elnevezésével kapcsolatos további információkért lásd: [elnevezési és a tárolók, blobok és metaadatok hivatkozó].

   ![Hozzon létre tárolási tároló párbeszédpanel][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>Az intellij-t a tároló törlése

egy tároló hello Azure Explorer használatával toodelete hello a következő:

1. Hello Azure terület Intézőbeli nézetében, kattintson a jobb gombbal a hello tárolót, és kattintson **törlése**.

   ![Tárolási tároló parancs törlése][DC01]

2. Hello ablak, kattintson **Igen**.

   ![Tárolási tároló ablak törlése][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>Az IntelliJ a tárfiók törlése

a storage-fiókok hello Azure Explorer használatával toodelete hello a következő:

1. A hello **Azure Explorer** nézetben kattintson a jobb gombbal a hello tárfiók, és válassza ki **törlése**.

   ![Felhasználóifiók-menüjéből tároló törlése][DS01]

2. Hello ablak, kattintson **Igen**.

   ![Törölje a tárolási fiók ablak][DS02]

## <a name="next-steps"></a>Következő lépések
Az Azure storage-fiókok, méretek és árképzési kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Azure Storage bemutatása tooMicrosoft]
* [tudnivalók az Azure storage-fiókok]
* Azure storage-fiókok méretének
  * [Windows Azure storage-fiók mérete]
  * [Az Azure storage-fiókok Linux mérete]
* Az Azure storage-fiók díjszabása
  * [A Windows storage-fiók díjszabása]
  * [Linux-tárfiók díjszabása]

A Java IDEs Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Eclipse Azure eszköztára]
  * [What's new in hello Eclipse Azure eszköztára]
  * [Hello Azure eszköztára Eclipse telepítése]
  * [hello Azure eszköztára Eclipse bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in hello IntelliJ Azure eszköztára]
  * [Az IntelliJ hello Azure eszközkészlet telepítése]
  * [Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[hello Azure eszköztára Eclipse bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

[Azure Storage bemutatása tooMicrosoft]: /azure/storage/storage-introduction
[tudnivalók az Azure storage-fiókok]: /azure/storage/storage-create-storage-account
[az Azure storage replikációs]: /azure/storage/storage-redundancy
[Az Azure storage méretezhetőségi és teljesítménycéloknak]: /azure/storage/storage-scalability-targets
[elnevezési és a tárolók, blobok és metaadatok hivatkozó]: http://go.microsoft.com/fwlink/?LinkId=255555

[Windows Azure storage-fiók mérete]: /azure/virtual-machines/virtual-machines-windows-sizes
[Az Azure storage-fiókok Linux mérete]: /azure/virtual-machines/virtual-machines-linux-sizes
[A Windows storage-fiók díjszabása]: /pricing/details/virtual-machines/windows/
[Linux-tárfiók díjszabása]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
