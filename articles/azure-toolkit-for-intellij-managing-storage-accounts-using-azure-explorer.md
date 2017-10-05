---
title: "Az intellij-t az Azure-kezelővel a storage-fiókok kezelése |} Microsoft Docs"
description: "Útmutató az Azure storage-fiókok kezelése az intellij-t az Azure-kezelővel használatával."
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
ms.openlocfilehash: a1b56cc2751fc43a1ad6917eca77eec460f26694
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Az intellij-t az Azure-kezelővel a storage-fiókok kezelése

Az Azure-kezelővel, amely az IntelliJ Azure eszköztára része, Java fejlesztők egy könnyen kezelhető megoldás biztosít az Azure-fiók a belül az IntelliJ integrált fejlesztési környezeti (IDE) storage-fiók kezeléséhez.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>A storage-fiók létrehozása az intellij-t

A storage-fiók létrehozása az Azure-kezelővel használatával, tegye a következőket:

1. Jelentkezzen be az Azure-fiók használatával a [Eclipse Azure eszköztára bejelentkezési utasítások]. 

2. Az a **Azure Explorer** megtekintéséhez bontsa ki a **Azure** csomópontot, kattintson a jobb gombbal **Tárfiókok**, és kattintson a **Storage-fiók létrehozása**.

   ![A Tárfiók parancs létrehozása][CS01]

3. Az a **Storage-fiók létrehozása** párbeszédpanelen adja meg a következő beállításokat:

   ![Hozzon létre új Tárfiók párbeszédpanel][CS02]

   * **Név**: az új tárfiók neve.

   * **Fiók kind**: határozza meg a tárfiók létrehozása (például "Blob-tároló"). További információkért lásd: [tudnivalók az Azure storage-fiókok]. 

   * **Teljesítmény**: Adja meg a használt tárfiók ajánlat (például "Prémium") a kijelölt közzétételi használatára. További információkért lásd: [az Azure storage méretezhetőségi és Teljesítménycélok]. 

   * **Replikációs**: Adja meg a tárfiók (például "Zónaredundáns") replikációját. További információkért lásd: [az Azure storage replikációs]. 

   * **Előfizetés**: Adja meg az új tárfiók használni kívánt Azure-előfizetéshez.

   * **Hely**: Adja meg a helyet, ahol a tárfiók létrejön-e (például "Nyugati US").

   * **Erőforráscsoport**: Adja meg a virtuális géphez tartozó erőforráscsoport. A következő lehetőségek közül:
      * **Új**: megadhatja, hogy kívánja-e egy új erőforráscsoport létrehozásához.
      * **Meglévő**: meghatározza, hogy az erőforráscsoportok az Azure-fiókjával társított listából kiválaszthatja.

4. Ha az előző közül az összes megadott, kattintson **OK**.

## <a name="create-a-storage-container-in-intellij"></a>A tároló létrehozása az intellij-t

A tároló létrehozása az Azure-kezelővel használatával, tegye a következőket:

1. Az Azure-kezelővel nézetben kattintson a jobb gombbal a tárfiókot, hol szeretné létrehozni a tárolót, és kattintson a **létrehozás blob tároló**.

   ![A blob-tároló parancs létrehozása][CC01]

2. Az a **létrehozás blob tároló** párbeszédpanelen adja meg a tároló a nevét, és kattintson **OK**. A tároló elnevezésével kapcsolatos további információkért lásd: [elnevezési és a tárolók, blobok és metaadatok hivatkozó].

   ![Hozzon létre tárolási tároló párbeszédpanel][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>Az intellij-t a tároló törlése

A tároló törlése az Azure Explorerrel, tegye a következőket:

1. Az Azure-kezelővel nézetben kattintson a jobb gombbal a tárolóra, és kattintson **törlése**.

   ![Tárolási tároló parancs törlése][DC01]

2. Kattintson a megerősítés ablakban **Igen**.

   ![Tárolási tároló ablak törlése][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>Az IntelliJ a tárfiók törlése

A tárfiók törlése az Azure-kezelővel használatával, tegye a következőket:

1. Az a **Azure Explorer** nézetben kattintson a jobb gombbal a tárfiók, és válassza ki **törlése**.

   ![Felhasználóifiók-menüjéből tároló törlése][DS01]

2. Kattintson a megerősítés ablakban **Igen**.

   ![Törölje a tárolási fiók ablak][DS02]

## <a name="next-steps"></a>Következő lépések
További információ az Azure storage-fiókok méretek és a díjszabás, lásd a következőket:

* [A Microsoft Azure Storage bemutatása]
* [tudnivalók az Azure storage-fiókok]
* Azure storage-fiókok méretének
  * [Windows Azure storage-fiók mérete]
  * [Az Azure storage-fiókok Linux mérete]
* Az Azure storage-fiók díjszabása
  * [A Windows storage-fiók díjszabása]
  * [Linux-tárfiók díjszabása]

A Java IDEs Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:

* [Eclipse Azure eszköztára]
  * [What's new in Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * [Eclipse Azure eszköztára bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in IntelliJ Azure eszköztára]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

[A Microsoft Azure Storage bemutatása]: /azure/storage/storage-introduction
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
