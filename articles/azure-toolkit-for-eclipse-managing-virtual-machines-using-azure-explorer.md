---
title: "aaaManage virtuális gépek Azure-kezelőjét az Eclipse hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure virtuális gépek használatával hello Azure-kezelőjét az eclipse-ben."
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
ms.openlocfilehash: aa83ec7546a0a8514842723b51ce8a5af81f98f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-eclipse"></a>Virtuális gépek felügyelni Eclipse hello Azure Explorer használatával

hello Azure-kezelővel, amely hello Azure eszköztára Eclipse része, Java-fejlesztők számára, egy könnyen használható megoldást biztosít a felhőszolgáltatóknak a virtuális gépeket az Azure-fiók a hello Eclipse integrált fejlesztési környezeti (IDE) belül.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a>Virtuális gép létrehozása az eclipse-ben

a virtuális gép hello Azure Explorer használatával toocreate hello a következő:

1. Jelentkezzen be Azure-fiók tooyour hello [hello Azure eszköztára Eclipse bejelentkezési utasítások].

2. A hello **Azure Explorer** megtekintéséhez bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **virtuális gépek**, és kattintson a **hozzon létre virtuális gép**.

   ![Hozzon létre virtuális gép parancs hello][CR01]  
   Hello **hozzon létre új virtuális gépet** varázsló megnyílik.

3. A hello **válasszon egy előfizetést** ablakban jelölje ki az előfizetését, és kattintson **következő**.

   ![Válasszon egy előfizetés ablak hello][CR02]

4. A hello **válassza ki a virtuálisgép-lemezkép** ablak, írja be a következő információ hello:

   * **Hely**: határozza meg, ahol létrejön a virtuális gép (például *USA nyugati régiója*).

   * **A Publisher**: hello publisher fogjuk toocreate a virtuális gép hello kép létrehozott határozza meg (például *Microsoft*).

   * **Ajánlat**: hello virtuális gép toouse ajánlat hello kijelölt közzétételi határozza meg (például *JDK*).

   * **SKU**: hello raktározási egységhez toouse hello kijelölt ajánlat az határozza meg (például *JDK_8*).

   * **Verzió #**: Adja meg a kiválasztott hello verziójának SKU toouse.

    ![hello válassza ki a virtuálisgép-lemezkép ablak][CR03]

5. Kattintson a **Tovább** gombra.

6. A hello **virtuális gép alapbeállítások** ablak, írja be a következő információ hello:

   * **Virtuális gép neve**: hello neve, az új virtuális gépen, amely kizárólag betűvel kezdődhet, és csak betűket, számokat és kötőjeleket tartalmazhat.

   * **Méret**: hello magok száma és a virtuális gép memória tooallocate határozza meg.

   * **Felhasználónév**: hello rendszergazdai fiók toocreate kezelését a virtuális gép határozza meg.

   * **Jelszó** és **megerősítése**: az Ön rendszergazdai fiókjának hello jelszót ad meg.

    ![hello virtuális gép alapbeállítások ablak][CR04]

7. Kattintson a **Tovább** gombra.

8. A hello **új Tárfiók létrehozása** ablak, írja be a következő információ hello:

   * **Erőforráscsoport**: hello erőforrás csoportot a virtuális gép határozza meg. Válasszon egyet az alábbi beállítások hello:
      * **Új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.
      * **Meglévő**: megadhatja, hogy kívánja-e tooselect egy erőforráscsoport, amely már társítva van az Azure-fiókjával.

      ![hello új Tárfiók létrehozása párbeszédpanel][CR05]

   * **A tárfiók**: hello tárolási fiók toouse a virtuális gép tárolásához adja meg. Meglévő tárfiók használata, vagy hozzon létre egy új fiókot.

   * **Virtuális hálózati** és **alhálózati**: hello virtuális hálózat és az alhálózatot, amely a virtuális gép csatlakozni fog. Használhat egy meglévő hálózati és alhálózati, vagy létrehozhat egy új hálózati és alhálózati. Ha **hozzon létre új**, hello a következő párbeszédpanel jelenik meg:

      ![hello új virtuális hálózat létrehozása párbeszédpanel][CR06]

9. A hello **kapcsolódó erőforrások** ablak, írja be a következő információ hello:

   * **Nyilvános IP-cím**: egy külső hálózat irányába néző IP-cím a virtuális géphez. Választhat toocreate egy új IP-címet, vagy ha a virtuális gép nem fog működni a nyilvános IP-cím, választhat **(nincs)**.

   * **Hálózati biztonsági csoport**: Adja meg a virtuális gép egy választható hálózati tűzfal. Kiválaszthatja, hogy egy meglévő tűzfal, vagy ha a virtuális gép nem használja a hálózati tűzfal, akkor választhat **(nincs)**.

   * **A rendelkezésre állási csoport**: határozza meg, opcionális rendelkezésre állási készlet, hogy a virtuális gép tartozhatnak. Válasszon egy meglévő rendelkezésre állási csoportot, vagy hozzon létre egy új rendelkezésre állási készletet, vagy ha a virtuális gép nem fog tartozni tooan rendelkezésre állási csoportot, válassza **(nincs)**.

   ![hello kapcsolódó erőforrások ablak][CR07]

9. Kattintson a **Befejezés** gombra.  
  Az új virtuális gép hello Azure Explorer eszköz ablakban jelenik meg.

   ![Új virtuális gép][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a>A virtuális gép újraindításához az eclipse-ben

az eclipse-ben hello Azure Explorer használatával egy virtuális gép toorestart hello a következő:

1. A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **indítsa újra a**.

   ![virtuális gép újraindítási parancsnak hello][RE01]

2. Hello ablak, kattintson **Igen**.

   ![hello újraindítás ablak][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a>Az eclipse-ben a virtuális gép leállítása

hello Azure Explorer használatával az eclipse-ben futó virtuális gép le tooshut hello a következő:

1. A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **leállítási**.

   ![hello virtuális gép leállítási parancs][SH01]

2. Hello ablak, kattintson **Igen**.

   ![virtuális gép leállítási hello ablak][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a>Az eclipse-ben a virtuális gép törlése

az eclipse-ben hello Azure Explorer használatával egy virtuális gép toodelete hello a következő:

1. A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **törlése**.

   ![hello virtuális gép Delete parancs esetében][DE01]

2. Hello ablak, kattintson **Igen**.

   ![hello törölni a virtuális gépet ablak][DE02]

## <a name="next-steps"></a>Következő lépések
Az Azure virtuális gépek méretét és a díjszabással kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* Az Azure virtuális gépek méretét
  * [Az Azure-ban a Windows virtuális gépek méretei]
  * [Az Azure Linux virtuális gépek méretei]
* Az Azure virtuális gépek – díjszabás
  * [Windows virtuális gépek – díjszabás]
  * [Linux virtuális gépek – díjszabás]

A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:

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

[Az Azure-ban a Windows virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-windows-sizes
[Az Azure Linux virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows virtuális gépek – díjszabás]: /pricing/details/virtual-machines/windows/
[Linux virtuális gépek – díjszabás]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
