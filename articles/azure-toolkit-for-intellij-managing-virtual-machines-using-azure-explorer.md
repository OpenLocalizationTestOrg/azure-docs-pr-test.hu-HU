---
title: "IntelliJ aaaManage virtuális gépek használatával hello Azure-kezelővel |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure virtuális gépek használatával hello Azure-kezelőjét az intellij-t."
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
ms.openlocfilehash: a73dd4f73b311dd3413f6712e3b76c36ee464de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-intellij"></a>Virtuális gépek felügyelni az IntelliJ hello Azure Explorer használatával

hello Azure Explorer, az IntelliJ Azure eszköztára hello részét képező Java fejlesztők egy könnyen kezelhető megoldás biztosít a felhőszolgáltatóknak a virtuális gépek az Azure-fiók a hello IntelliJ integrált fejlesztési környezeti (IDE) belül.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a>Virtuális gép létrehozása az intellij-t

a virtuális gép hello Azure Explorer használatával toocreate hello a következő: 

1. Jelentkezzen be Azure-fiók tooyour hello lépéseket a hello [bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ] cikk.

2. A hello **Azure Explorer** megtekintéséhez bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **virtuális gépek**, és kattintson a **hozzon létre virtuális gép**. 

   ![Hozzon létre virtuális gép parancs hello][CR01]  
    Hello **hozzon létre új virtuális gépet** varázsló megnyílik.

3. A hello **válasszon egy előfizetést** ablakban jelölje ki az előfizetését, és kattintson **következő**. 

   ![Válasszon egy előfizetés ablak hello][CR02]

4. A hello **válassza ki a virtuálisgép-lemezkép** ablak, írja be a következő információ hello:

   * **Hely**: határozza meg, ahol létrejön a virtuális gép (például *USA nyugati régiója*). 

   * **Kép ajánlott**: meghatározza, hogy fog választhat egy lemezképet egy gyakran használt képek rövidített listájának.

   * **Kép: egyéni**: meghatározza a fog választani egy egyéni lemezképet, adja meg a következő információ hello:

      * **A Publisher**: Adja meg a virtuális géphez használni kívánt kép hello létrehozott hello közzétevő (például *Microsoft*).

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

8. A hello **kapcsolódó erőforrások** ablak, írja be a következő információ hello:

   * **Erőforráscsoport**: hello erőforrás csoportot a virtuális gép határozza meg. Válasszon egyet az alábbi beállítások hello:
      * **Új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.
      * **Meglévő**: megadhatja, hogy kívánja-e tooselect az erőforráscsoportok az Azure-fiókjával társított listájából.

       ![hello kapcsolódó erőforrások ablak][CR07]

   * **A tárfiók**: hello tárolási fiók toouse a virtuális gép tárolásához adja meg. Válasszon egy meglévő tárfiókot használ, vagy hozzon létre egy új fiókot. Ha úgy dönt, **hozzon létre új**, hello a következő párbeszédpanel jelenik meg:

      ![hello Storage-fiók létrehozása párbeszédpanel][CR05]

   * **Virtuális hálózati** és **alhálózati**: hello virtuális hálózat és az alhálózatot, amely a virtuális gép csatlakozni fog. Használhat egy meglévő hálózati és alhálózati, vagy létrehozhat egy új hálózati és alhálózati. Ha **hozzon létre új**, hello a következő párbeszédpanel jelenik meg:

      ![hello virtuális hálózat létrehozása párbeszédpanel][CR06]

   * **Nyilvános IP-cím**: egy külső hálózat irányába néző IP-cím a virtuális géphez. Választhat toocreate egy új IP-címet, vagy ha a virtuális gép nem fog működni a nyilvános IP-cím, választhat **(nincs)**. 

   * **Hálózati biztonsági csoport**: Adja meg a virtuális gép egy választható hálózati tűzfal. Kiválaszthatja, hogy egy meglévő tűzfal, vagy ha a virtuális gép nem használja a hálózati tűzfal, akkor választhat **(nincs)**. 

   * **A rendelkezésre állási csoport**: határozza meg, opcionális rendelkezésre állási készlet, hogy a virtuális gép tartozhatnak. Válassza ki a rendelkezésre állási készlet, létrehozhat egy új rendelkezésre állási csoport vagy, ha a virtuális gép nem fog tartozni tooan rendelkezésre állási csoportot, válassza ki a **(nincs)**.

9. Kattintson a **Befejezés** gombra.  
    Az új virtuális gép hello Azure eszköz ablak jelenik meg. 

   ![Új virtuális gép hello Azure Explorer megtekintése][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a>Az intellij-t egy virtuális gép újraindítása

az intellij-t, hello Azure Explorer használatával egy virtuális gép toorestart hello a következő:

1. A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **indítsa újra a**.

   ![virtuális gép újraindítási parancsnak hello][RE01]

2. Hello ablak, kattintson **Igen**. 

   ![hello indítsa újra a virtuális gép ablak][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a>Az intellij-t egy virtuális gép leállítása

le a futó virtuális gépek IntelliJ, hello Azure Explorer használatával tooshut hello a következő:

1. A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **leállítási**.

   ![hello virtuális gép leállítási parancs][SH01]

2. Hello ablak, kattintson **Igen**. 

   ![Állítsa le a virtuális gép ablak hello][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a>Az intellij-t egy virtuális gép törlése

az intellij-t, hello Azure Explorer használatával egy virtuális gép toodelete hello a következő:

1. A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **törlése**.

   ![hello virtuális gép Delete parancs esetében][DE01]

2. Hello ablak, kattintson **Igen**. 

   ![hello törlése a virtuális gép ablak][DE02]

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
  * [Hello Azure eszköztára Eclipse bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in hello IntelliJ Azure eszköztára]
  * [Az IntelliJ hello Azure eszközkészlet telepítése]
  * [bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Hello Azure eszköztára Eclipse bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

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
