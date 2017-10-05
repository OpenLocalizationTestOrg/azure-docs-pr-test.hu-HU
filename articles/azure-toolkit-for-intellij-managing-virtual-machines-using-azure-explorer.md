---
title: "Virtuális gépek felügyelni az intellij-t az Azure-kezelővel használatával |} Microsoft Docs"
description: "Útmutató az Azure virtuális gépek felügyelni az intellij-t az Azure-kezelővel használatával."
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
ms.openlocfilehash: 9197580407b3509fbf9a842e1fee1e6348478c34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a>Az intellij-t az Azure-kezelővel használatával virtuális gépek kezelése

Az Azure-kezelővel, amely az IntelliJ Azure eszköztára része, Java fejlesztők egy könnyen kezelhető megoldás biztosít a felhőszolgáltatóknak a virtuális gépek a Azure fiók belül az IntelliJ integrált fejlesztési környezeti (IDE).

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a>Virtuális gép létrehozása az intellij-t

Virtuális gép létrehozása az Azure Explorerrel, tegye a következőket: 

1. A lépések az Azure-fiókjával jelentkezzen be a [bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ] cikk.

2. Az a **Azure Explorer** megtekintéséhez bontsa ki a **Azure** csomópontot, kattintson a jobb gombbal **virtuális gépek**, és kattintson a **hozzon létre virtuális gép**. 

   ![A virtuális gép létrehozása parancs][CR01]  
    A **hozzon létre új virtuális gépet** varázsló megnyílik.

3. Az a **válasszon egy előfizetést** ablakban jelölje ki az előfizetését, és kattintson **következő**. 

   ![A Válasszon egy előfizetés ablak][CR02]

4. Az a **válassza ki a virtuálisgép-lemezkép** ablak, írja be a következő adatokat:

   * **Hely**: határozza meg, ahol létrejön a virtuális gép (például *USA nyugati régiója*). 

   * **Kép ajánlott**: meghatározza, hogy fog választhat egy lemezképet egy gyakran használt képek rövidített listájának.

   * **Kép: egyéni**: meghatározza a fog választani egy egyéni lemezképet, adja meg a következő információkat:

      * **A Publisher**: a közzétevő a lemezképet a virtuális géphez használni kívánt létrehozott határozza meg (például *Microsoft*).

      * **Ajánlat**: Adja meg a virtuális gép használata a kijelölt közzétételi ajánlat (például *JDK*).

      * **SKU**: Adja meg a Raktározásiegység (SKU) a kijelölt ajánlat használata (például *JDK_8*).

      * **Verzió #**: Adja meg a kiválasztott Termékváltozat használandó verziójának.

   ![Válassza ki a virtuálisgép-lemezkép ablak][CR03]

5. Kattintson a **Tovább** gombra. 

6. Az a **virtuális gép alapbeállítások** ablak, írja be a következő adatokat:

   * **Virtuális gép neve**: Adja meg a nevet az új virtuális gépet, amely kizárólag betűvel kezdődhet, és csak betűket, számokat és kötőjeleket tartalmazhat.

   * **Méret**: Adja meg a magok száma és a virtuális gép lefoglalni memóriát.

   * **Felhasználónév**: Adja meg a rendszergazdai fiók kezeléséhez a virtuális gép létrehozásához.

   * **Jelszó** és **megerősítése**: a rendszergazdai fiók jelszavát adja meg.

   ![A virtuális gép alapbeállítások ablak][CR04]

7. Kattintson a **Tovább** gombra. 

8. Az a **kapcsolódó erőforrások** ablak, írja be a következő adatokat:

   * **Erőforráscsoport**: Adja meg a virtuális géphez tartozó erőforráscsoport. A következő lehetőségek közül:
      * **Új**: megadhatja, hogy kívánja-e egy új erőforráscsoport létrehozásához.
      * **Meglévő**: meghatározza, hogy szeretné-e az erőforráscsoportok az Azure-fiókjával társított listájáról.

       ![A kapcsolódó erőforrások ablak][CR07]

   * **A tárfiók**: Adja meg a tárfiók a virtuális gép tárolására használandó. Válasszon egy meglévő tárfiókot használ, vagy hozzon létre egy új fiókot. Ha úgy dönt, **hozzon létre új**, a következő párbeszédpanel jelenik meg:

      ![A Storage-fiók létrehozása párbeszédpanel][CR05]

   * **Virtuális hálózati** és **alhálózati**: Adja meg a virtuális hálózat és az alhálózatot, amely a virtuális gép csatlakozni fog. Használhat egy meglévő hálózati és alhálózati, vagy létrehozhat egy új hálózati és alhálózati. Ha **hozzon létre új**, a következő párbeszédpanel jelenik meg:

      ![A virtuális hálózat létrehozása párbeszédpanel][CR06]

   * **Nyilvános IP-cím**: egy külső hálózat irányába néző IP-cím a virtuális géphez. Ha szeretné, hozzon létre egy új IP-címet, vagy ha a virtuális gép nem fog működni a nyilvános IP-cím, választhat **(nincs)**. 

   * **Hálózati biztonsági csoport**: Adja meg a virtuális gép egy választható hálózati tűzfal. Kiválaszthatja, hogy egy meglévő tűzfal, vagy ha a virtuális gép nem használja a hálózati tűzfal, akkor választhat **(nincs)**. 

   * **A rendelkezésre állási csoport**: határozza meg, opcionális rendelkezésre állási készlet, hogy a virtuális gép tartozhatnak. Válassza ki a rendelkezésre állási készlet, létrehozhat egy új rendelkezésre állási csoport vagy, ha a virtuális gép nem fog tartozni egy rendelkezésre állási csoportot, válassza ki a **(nincs)**.

9. Kattintson a **Befejezés** gombra.  
    Az új virtuális gépet az Azure Explorer eszköz ablakban jelenik meg. 

   ![Új virtuális gépet az Azure-kezelővel nézetben][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a>Az intellij-t egy virtuális gép újraindítása

Az intellij-t az Azure-kezelővel használatával egy virtuális gép újraindításához tegye a következőket:

1. Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **indítsa újra a**.

   ![A virtuális gép újraindítási parancsnak][RE01]

2. Kattintson a megerősítés ablakban **Igen**. 

   ![A virtuális gép újraindítása ablak][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a>Az intellij-t egy virtuális gép leállítása

Az intellij-t az Azure-kezelővel használatával állítsa le a futó virtuális gépek, tegye a következőket:

1. Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **leállítási**.

   ![A virtuális gép leállítási parancs][SH01]

2. Kattintson a megerősítés ablakban **Igen**. 

   ![A virtuális gép ablak Leállítás][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a>Az intellij-t egy virtuális gép törlése

Törölje a virtuális gép az intellij-t az Azure-kezelővel használatával, tegye a következőket:

1. Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **törlése**.

   ![A virtuális gép Delete parancs esetében][DE01]

2. Kattintson a megerősítés ablakban **Igen**. 

   ![A virtuális gép törlése ablak][DE02]

## <a name="next-steps"></a>Következő lépések
Az Azure virtuális gépek méretét és a díjszabással kapcsolatos további információkért lásd a következőket:

* Az Azure virtuális gépek méretét
  * [Az Azure-ban a Windows virtuális gépek méretei]
  * [Az Azure Linux virtuális gépek méretei]
* Az Azure virtuális gépek – díjszabás
  * [Windows virtuális gépek – díjszabás]
  * [Linux virtuális gépek – díjszabás]

A Java IDEs az Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:

* [Eclipse Azure eszköztára]
  * [What's new in Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * [Az Eclipse Azure eszköztára bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in IntelliJ Azure eszköztára]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Az Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

[Az Azure-ban a Windows virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-windows-sizes
[Az Azure Linux virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows virtuális gépek – díjszabás]: /pricing/details/virtual-machines/windows/
[Linux virtuális gépek – díjszabás]: /pricing/details/virtual-machines/linux/


<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
