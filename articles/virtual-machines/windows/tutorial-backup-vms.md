---
title: "Azure-Windows-alapú virtuális gépek aaaBackup |} Microsoft Docs"
description: "A Windows virtuális gépek védelme készítésével Azure Backup segítségével."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1cd3e1940a83aacd160cba3c8613b63b6f3c11d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Készítsen biztonsági másolatot a Windows virtuális gépek Azure-ban

Adatai védelme érdekében érdemes rendszeres időközönként biztonság mentést végeznie. Az Azure Backup georedundáns helyreállítás tárolók tárolt helyreállítási pontokat hoz létre. Egy helyreállítási pontból állítsa vissza, ha visszaállíthatja hello az egész virtuális gép vagy csak adott fájlokat. Ez a cikk azt ismerteti, hogyan toorestore egyetlen fájl tooa virtuális gép Windows Server és az IIS futnak. Ha még nem rendelkezik a virtuális gép toouse, létrehozhat egy hello segítségével [Windows gyors üzembe helyezés](quick-create-portal.md). Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Készítsen biztonsági másolatot a virtuális gépek
> * Napi biztonsági mentés ütemezése
> * A fájl visszaállítása biztonsági másolatból




## <a name="backup-overview"></a>A biztonsági mentés áttekintése

Hello Azure Backup szolgáltatás a biztonsági mentési feladatot indít, amikor elindítja a hello tartalék mellék tootake időpontban pillanatkép. hello Azure Backup szolgáltatás által használt hello _VMSnapshot_ bővítmény. Ha hello virtuális gép fut, hello első virtuális gép biztonsági mentés során telepítve hello bővítmény. Hello virtuális gép nem fut, ha biztonsági mentési szolgáltatás hello pillanatképet készít a hello az alapul szolgáló tárolási (mert nem alkalmazás írási műveletek hello VM leállítása közben történik).

Windows virtuális gépek pillanatkép létrehozása van folyamatban, amikor hello biztonsági mentési szolgáltatás koordinálja a kötet árnyékmásolata szolgáltatás (VSS) tooget hello hello virtuális gépek lemezeit konzisztens pillanatképet. Amennyiben az Azure Backup szolgáltatás hello hello pillanatfelvételt, hello adata átvitt toohello tárolóban. toomaximize hatékonyságát, hello szolgáltatás azonosítja, és csak a hello korábbi biztonsági mentés óta megváltoztak adattípusnak hello blokkok átvitele.

Hello adatátvitel befejezésekor hello pillanatkép törlődik, és egy helyreállítási pontot hoz létre.


## <a name="create-a-backup"></a>Biztonsági mentés létrehozása
Hozzon létre egy egyszerű ütemezett napi biztonsági mentési tooa Recovery Services-tároló. 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello hello bal oldali menüben válasszon ki **virtuális gépek**. 
3. Hello listájából válassza ki a virtuális gép tooback fel.
4. Hello VM panelen, a hello **beállítások** kattintson **biztonsági mentés**. Hello **biztonságimásolat-készítés engedélyezése** panel nyílik meg.
5. A **Recovery Services-tároló**, kattintson a **hozzon létre új** , és adja meg az új tároló hello hello nevét. Egy új tároló létrehozása hello ugyanazt az erőforráscsoportot és helyet hello virtuális gépként.
6. Kattintson a **biztonsági mentési házirend**. Ehhez a példához hello alapértelmezett tartása, és kattintson **OK**.
7. A hello **biztonságimásolat-készítés engedélyezése** panelen kattintson a **biztonsági mentés engedélyezése**. Ezzel létrehozza a napi biztonsági mentés a hello alapértelmezett ütemezés szerint.
10. az első helyreállítási pont, a hello toocreate **biztonsági mentés** panelen kattintson **biztonsági mentés most**.
11. A hello **biztonsági mentés most** panelen hello naptár ikonra, használja a hello Naptár vezérlőelem tooselect hello utolsó napja a helyreállítási pont őrzi meg, és kattintson a **biztonsági mentés**.
12. A hello **biztonsági mentés** a virtuális gép paneljén láthatja hello helyreállítási pontok, amelyek a teljes száma.

    ![Helyreállítási pontok](./media/tutorial-backup-vms/backup-complete.png)
    
hello első biztonsági mentés körülbelül 20 percet vesz igénybe. A biztonsági mentés befejezése után a folytatáshoz toohello Ez az oktatóanyag következő része.

## <a name="recover-a-file"></a>A fájl helyreállításához

Ha véletlenül törli vagy módosításokat tooa fájl, a fájlok helyreállítása toorecover hello fájlt használhat a biztonsági mentési tárolóból. A fájlok helyreállítása a virtuális gép, hello futó parancsfájlt használ toomount hello helyreállítási pont helyi meghajtóként. Ezek a meghajtók marad csatlakoztatott 12 óra, hogy a fájlok másolását hello helyreállítási pont, és állítsa vissza őket toohello VM.  

Ebben a példában megmutatjuk, hogyan toorecover hello képfájl IIS használt hello alapértelmezett weblapon. 

1. Nyisson meg egy böngészőt, és csatlakozzon a hello VM tooshow hello alapértelmezett IIS lap toohello IP-címét.

    ![Alapértelmezett IIS-weblap](./media/tutorial-backup-vms/iis-working.png)

2. Csatlakoztassa a virtuális gép toohello.
3. Nyissa meg a virtuális gép hello, **Fájlkezelőben** , és keresse meg a too\inetpub\wwwroot, és törölje a hello fájlt **iisstart.png**.
4. A helyi számítógépen frissítési hello böngésző toosee hello hello alapértelmezett IIS-lapot a lemezkép nem.

    ![Alapértelmezett IIS-weblap](./media/tutorial-backup-vms/iis-broken.png)

5. A helyi számítógépen nyisson meg egy új lapra, és lépjen hello hello [Azure-portálon](https://portal.azure.com).
6. Hello hello bal oldali menüben válasszon ki **virtuális gépek** és select hello VM űrlap hello listáját.
8. Hello VM panelen, a hello **beállítások** kattintson **biztonsági mentés**. Hello **biztonsági mentés** panel nyílik meg. 
9. Hello panel felső hello hello menüben válasszon ki **fájlhelyreállítás**. Hello **fájlhelyreállítás** panel nyílik meg.
10. A **1. lépés: válassza ki a helyreállítási pont**, a hello legördülő listán válasszon egy helyreállítási pontot.
11. A **2. lépés: Töltse le a parancsfájl toobrowse és a fájlok helyreállítása**, hello kattintson **végrehajtható fájl letöltése** gombra. Mentse a fájlt tooyour hello **letölti** mappát.
12. Nyissa meg a helyi számítógép **Fájlkezelőben** , és keresse meg a tooyour **letölti** mappát, másolja hello letöltött .exe-fájl. hello fájlnév fog szerepelnie, hogy a virtuális gép nevét. 
13. A virtuális gépen (keresztül a távoli ASZTAL kapcsolaton keresztül hello) illessze be a hello .exe fájl toohello a virtuális gép asztalát. 
14. Keresse meg a virtuális gép asztalát toohello, és kattintson duplán arra a hello .exe. Ez elindítani a parancssort, és csatlakoztassa hello helyreállítási pont, egy fájlmegosztást, amely érheti el. Amikor befejeződött hello megosztás létrehozása, írja be a **q** tooclose hello parancssort.
15. Nyissa meg a virtuális gép **Fájlkezelőben** , és keresse meg a hello fájlmegosztás használt toohello meghajtóbetűjelet.
16. Keresse meg a too\inetpub\wwwroot, és másolja **iisstart.png** hello fájlból megosztani, és illessze be \inetpub\wwwroot. Például F:\inetpub\wwwroot\iisstart.png másolja és illessze be c:\inetpub\wwwroot toorecover hello fájl.
17. A helyi számítógépen, ahol csatlakoznak hello böngészőlapon nyílik hello VM hello az IIS alapértelmezett lapot ábrázoló toohello IP-címét. Használja a CTRL + F5 toorefresh hello böngésző lap. Most látnia kell, hogy hello kép vissza lett állítva.
18. A helyi számítógépen, lépjen vissza toohello böngészőlapon hello Azure-portál és a **3. lépés: hello lemez leválasztása a helyreállítás után** hello kattintson **lemez leválasztása** gombra. Ha elfelejti toodo ezt a lépést, hello kapcsolat toohello a csatlakoztatási pont az automatikusan Bezárás 12 óra elteltével. E 12 óra elteltével kell egy új parancsfájl toocreate egy új csatlakozásipont toodownload.


## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Készítsen biztonsági másolatot a virtuális gépek
> * Napi biztonsági mentés ütemezése
> * A fájl visszaállítása biztonsági másolatból

Virtuális gépek figyelésével kapcsolatos útmutató toolearn következő toohello előzetes.

> [!div class="nextstepaction"]
> [Virtuális gépek figyelése](tutorial-monitoring.md)









