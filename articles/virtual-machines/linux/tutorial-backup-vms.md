---
title: "Az Azure Linux virtuális gépek biztonsági mentése |} Microsoft Docs"
description: "A Linux virtuális gépek védelme készítésével Azure Backup segítségével."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7c00392d5185a2f067f2ee2717529dcbde1e71f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-linux--virtual-machines-in-azure"></a>Készítsen biztonsági másolatot a Linux virtuális gépek Azure-ban

Adatai védelme érdekében érdemes rendszeres időközönként biztonság mentést végeznie. Az Azure Backup georedundáns helyreállítás tárolók tárolt helyreállítási pontokat hoz létre. Egy helyreállítási pontból állítsa vissza, ha visszaállíthatja hello az egész virtuális gép vagy csak adott fájlokat. Ez a cikk azt ismerteti, hogyan toorestore egyetlen fájl tooa Linux virtuális gép futó nginx. Ha még nem rendelkezik a virtuális gép toouse, létrehozhat egy hello segítségével [Linux gyors üzembe helyezés](quick-create-cli.md). Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Készítsen biztonsági másolatot a virtuális gépek
> * Napi biztonsági mentés ütemezése
> * A fájl visszaállítása biztonsági másolatból



## <a name="backup-overview"></a>A biztonsági mentés áttekintése

Hello Azure biztonsági mentési szolgáltatás biztonsági kezdeményez, amikor elindítja a hello tartalék mellék tootake időpontban pillanatkép. hello Azure Backup szolgáltatás által használt hello _VMSnapshotLinux_ Linux bővítményt. Ha hello virtuális gép fut, hello első virtuális gép biztonsági mentés során telepítve hello bővítmény. Hello virtuális gép nem fut, ha biztonsági mentési szolgáltatás hello pillanatképet készít a hello az alapul szolgáló tárolási (mert nem alkalmazás írási műveletek hello VM leállítása közben történik).

Alapértelmezés szerint Azure biztonsági mentési időt vesz igénybe a fájlrendszer konzisztens biztonsági mentése a Linux virtuális gép, de lehet konfigurált tootake [alkalmazás konzisztens biztonsági mentését parancsfájl előtti és utáni parancsfájl keretrendszerrel](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). Amennyiben az Azure Backup szolgáltatás hello hello pillanatfelvételt, hello adata átvitt toohello tárolóban. toomaximize hatékonyságát, hello szolgáltatás azonosítja, és csak a hello korábbi biztonsági mentés óta megváltoztak adattípusnak hello blokkok átvitele.

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

## <a name="restore-a-file"></a>Fájl visszaállítása

Ha véletlenül törli vagy módosításokat tooa fájl, a fájlok helyreállítása toorecover hello fájlt használhat a biztonsági mentési tárolóból. A fájlok helyreállítása a virtuális gép, hello futó parancsfájlt használ toomount hello helyreállítási pont helyi meghajtóként. Ezek a meghajtók marad csatlakoztatott 12 óra, hogy a fájlok másolását hello helyreállítási pont, és állítsa vissza őket toohello VM.  

Ebben a példában megmutatjuk, hogyan toorecover hello alapértelmezett nginx weblap /var/www/html/index.nginx-debian.html. Ebben a példában a virtuális gép hello nyilvános IP-címe *13.69.75.209*. Hello IP-címet a virtuális gép használatával található:

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. A helyi számítógépen nyisson meg egy böngészőt, és írja be hello nyilvános IP-címet a virtuális gép toosee hello alapértelmezett nginx weblap.

    ![Alapértelmezett nginx-weblap](./media/tutorial-backup-vms/nginx-working.png)

1. SSH-ból a virtuális Gépet.

    ```bash
    ssh 13.69.75.209
    ```
2. /Var/www/html/index.nginx-debian.html törlése.

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. A helyi számítógépen, frissítheti hello böngésző szerezze meg a CTRL + F5 toosee, amely alapértelmezett nginx oldal nem.

    ![Alapértelmezett nginx-weblap](./media/tutorial-backup-vms/nginx-broken.png)
    
1. A helyi számítógépen jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
6. Hello hello bal oldali menüben válasszon ki **virtuális gépek**. 
7. Hello listáról válassza ki a virtuális gép hello.
8. Hello VM panelen, a hello **beállítások** kattintson **biztonsági mentés**. Hello **biztonsági mentés** panel nyílik meg. 
9. Hello panel felső hello hello menüben válasszon ki **fájlhelyreállítás**. Hello **fájlhelyreállítás** panel nyílik meg.
10. A **1. lépés: válassza ki a helyreállítási pont**, a hello legördülő listán válasszon egy helyreállítási pontot.
11. A **2. lépés: Töltse le a parancsfájl toobrowse és a fájlok helyreállítása**, hello kattintson **végrehajtható fájl letöltése** gombra. Mentse a hello letöltött fájl tooyour helyi számítógépen.
7. Kattintson a **parancsprogram letöltése** toodownload hello parancsfájl helyileg.
8. Nyissa meg a Bash, írja be hello követően, hogy *Linux_myVM_05-05-2017.sh* hello megfelelő elérési út és fájlnév hello parancsfájl letöltött, *azureuser* hello a felhasználónevet használva a virtuális gép hello és *13.69.75.209* hello nyilvános IP-cím a virtuális gép számára.
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. A helyi számítógépen nyisson meg egy SSH-kapcsolat toohello virtuális gép.

    ```bash
    ssh 13.69.75.209
    ```
    
10. A virtuális gépen, vegye fel az engedélyek toohello parancsfájl végrehajtása.

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. A virtuális Gépet, a fájlrendszer hello parancsfájl toomount hello helyreállítási pontot futtató.

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. hello kimenetét hello parancsfájl ad meg hello hello csatlakozási pont elérési útját. hello kimeneti hasonló toothis néz ki:

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting toorecovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of hello recovery point toothis machine...
                         
    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

12. Másolja a virtuális gép hello nginx alapértelmezett weblap hello csatlakoztatási pont hátsó toowhere a hello fájl törölve.

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. A helyi számítógépen, ahol csatlakoznak hello böngészőlapon nyílik hello VM hello nginx alapértelmezett lapot ábrázoló toohello IP-címét. Használja a CTRL + F5 toorefresh hello böngésző lap. Most látnia kell, hogy hello alapértelmezett oldal ismét működik.

    ![Alapértelmezett nginx-weblap](./media/tutorial-backup-vms/nginx-working.png)

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

