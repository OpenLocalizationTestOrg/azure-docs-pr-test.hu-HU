---
title: "aaaStorSimple Snapshot Manager és a kötetek |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul tooview hello és kötetek és tooconfigure biztonsági mentések kezelése."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: b8ce102d082b7c773d667a9d3ec747be9e1d3064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-volumes"></a>StorSimple Snapshot Manager tooview használja, és a kötetek kezelése
## <a name="overview"></a>Áttekintés
Használhatja a StorSimple Snapshot Manager hello **kötetek** csomópont (a hello **hatókör** ablaktáblán) tooselect kötetek és a rájuk vonatkozó információk megtekintése. hello kötetek, amelyek megfelelnek a hello állomás által csatlakoztatott toohello kötetek meghajtóként jelennek meg. Hello **kötetek** csomópontban jelennek meg a helyi kötet és a StorSimple, beleértve a kötetek hello használata az iSCSI- és eszköz által feltárt által támogatott kötet típusok. 

További információ a támogatott köteteket, nyissa meg túl[többféle kötet támogatása](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Az eredmények ablaktábláján kötet listája](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Hello **kötetek** csomópont emellett lehetővé teszi a futtasson újbóli keresést vagy kötetek törlése után a StorSimple Snapshot Manager deríti fel őket. 

Ez az oktatóanyag azt ismerteti, hogyan csatlakoztatása, inicializálása, és a kötetek formázásának és majd használni a StorSimple Snapshot Manager:

* Kötetek adatainak megtekintése 
* Kötetek törlése
* Kötetek újraellenőrzése 
* Egyszerű kötet konfigurálása és biztonsági másolatot készíteni
* A dinamikus tükrözött kötetek konfigurálása és biztonsági másolatot készíteni

> [!NOTE]
> Az összes hello **kötet** csomópont műveletek is rendelkezésre állnak a hello **műveletek** ablaktáblán.
> 
> 

## <a name="mount-volumes"></a>Kötet csatlakoztatása
Használjon hello következő eljárás toomount, inicializálja és formázza a StorSimple-köteteket. Ez az eljárás használja a Lemezkezelés alkalmazásban merevlemezek és a megfelelő kötetek hello vagy a partíciók kezelésére szolgál. További információ a Lemezkezelés eszközben lépjen túl[Lemezkezelés](https://technet.microsoft.com/library/cc770943.aspx) hello Microsoft TechNet webhelyen.

#### <a name="toomount-volumes"></a>toomount kötetek
1. A gazdaszámítógépen indítsa el a hello Microsoft iSCSI-kezdeményező.
2. Adjon meg egy hello illesztő IP-címek hello tárolókapu, vagy felderítési IP-címet, és csatlakoztassa toohello eszközt. Hello eszköz csatlakoztatása után hello kötetek lesznek elérhető tooyour Windows rendszert. Hello Microsoft iSCSI-kezdeményező használatával kapcsolatos további információkért keresse meg a "Csatlakozás tooan iSCSI-tárolóeszköz" toohello szakasz [telepítése és konfigurálása a Microsoft iSCSI-kezdeményező][1].
3. Használja a következő beállítások toostart Lemezkezelés hello valamelyikét:
   
   * Írja be a Diskmgmt.msc hello **futtatása** mezőbe.
   * Indítsa el a Kiszolgálókezelőt, bontsa ki a hello **tárolási** csomópont, és válassza **Lemezkezelés**.
   * Start **felügyeleti eszközök**, bontsa ki a hello **számítógép-kezelés** csomópont, és válassza **Lemezkezelés**. 
     
     > [!NOTE]
     > Rendszergazdai jogosultságok toorun Lemezkezelés kell használnia.
     > 
     > 
4. Hello kötet(ek) online érvénybe:
   
   1. A Lemezkezelés beépülő modulban kattintson a jobb gombbal bármely megjelölt kötet **Offline**.
   2. Kattintson a **lemez újraaktiválása**. hello lemez kell megjelölni **Online** hello lemez újraaktiválásakor után.
5. Hello kötet(ek) inicializálása:
   
   1. Kattintson a jobb gombbal felderített hello kötetek.
   2. A hello menüben válassza **lemez inicializálása**.
   3. A hello **lemez inicializálása** párbeszédpanel megnyitásához, jelölje be hello lemezek tooinitialize szeretne, és kattintson **OK**.
6. Egyszerű kötetek formázása:
   
   1. Kattintson a jobb gombbal egy kötetet, amelyet az tooformat.
   2. A hello menüben válassza **új egyszerű kötet**.
   3. Hello új egyszerű kötet varázslóban tooformat hello kötet használható:
      
      * Adja meg a hello kötet méretét.
      * Adjon meg egy meghajtóbetűjelet.
      * Válassza ki a hello az NTFS fájlrendszerhez.
      * Adjon meg 64 KB-os lemezfoglalásiegység-méretet.
      * Hajtson végre egy gyorsformázást.
7. Több partíció kötetek formázása. Útmutatás, keresse meg a "Partíciók és kötetek" szakaszban toohello [Lemezkezelés végrehajtási](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>A kötetek adatainak megtekintése
A következő eljárás tooview információt a helyi és Azure StorSimple-köteteket hello használata.

#### <a name="tooview-volume-information"></a>tooview kötetinformációi
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager. 
2. A hello **hatókör** ablaktáblán kattintson a hello **kötetek** csomópont. Helyi és csatlakoztatott köteteket, beleértve az összes Azure StorSimple-köteteket, megjelennek a hello **eredmények** ablaktáblán. hello oszlopai hello **eredmények** ablaktáblán is konfigurálható. (Kattintson a jobb gombbal hello **kötetek** csomópontban jelölje ki **nézet**, majd válassza ki **oszlopok hozzáadása/eltávolítása**.)
   
    ![Hello oszlopok konfigurálása](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Eredmények oszlop | Leírás |
   |:--- |:--- |
   |  Név |Hello **neve** oszlop hello meghajtó-betűjelet felderített tooeach kötet tartalmaz. |
   |  Eszköz |Hello **eszköz** oszlop hello eszköz csatlakoztatott toohello fogadó számítógép hello IP-címét tartalmazza. |
   |  Eszköz kötet neve |Hello **kötet eszköznév** oszlop hello eszköz kötet toowhich hello kijelölt hello nevét tartalmazza kötet tartozik. Ez az Azure-portálon, hogy a megadott köteten hello definiált hello kötetneve. |
   |  Elérési utak |Hello **elérési utak** oszlop hello hozzáférés elérési toohello kötet jeleníti meg. Ez az hello meghajtó betűjele vagy csatlakozási pont, mely hello kötet hello állomáson elérhető. |

## <a name="delete-a-volume"></a>Kötet törlése
A következő eljárás toodelete egy kötetet a StorSimple Snapshot Manager hello használata.

> [!NOTE]
> A kötet nem törölhető, ha valamelyik kötet csoport része. (hello törlési beállítás nem érhető el, amelyek egy kötet csoport tagjai kötetek.) Hello teljes kötet csoport toodelete hello kötet kell törölnie.

#### <a name="toodelete-a-volume"></a>a kötet toodelete
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a hello **kötetek** csomópont. 
3. A hello **eredmények** ablaktáblában kattintson a jobb gombbal hello kötet, amelyet az toodelete.
4. Hello menüjének **törlése**. 
   
    ![Kötet törlése](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. Hello **kötet törlése** párbeszédpanel jelenik meg. Típus **megerősítése** hello szövegmezőbe, és kattintson a **OK**.
6. Alapértelmezés szerint a StorSimple Snapshot Manager biztonsági mentést készít egy kötet törlése előtt. Ez okokból is védve adatvesztés Ha hello törlése nem volt szándékos. StorSimple Snapshot Manager megjelenít egy **automatikus pillanatkép** állapotüzenetet, amíg azt készít biztonsági másolatot hello kötet. 
   
    ![Automatikus pillanatkép üzenet](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Kötetek újraellenőrzése
A következő eljárás toorescan hello kötetek csatlakoztatott tooStorSimple Snapshot Manager hello használata.

#### <a name="toorescan-hello-volumes"></a>toorescan hello kötetek
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a jobb gombbal **kötetek**, és kattintson a **ismételt vizsgálat kötetek**.
   
    ![Kötetek újraellenőrzése](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Ezt az eljárást a StorSimple Snapshot Manager hello kötet lista szinkronizálja. A módosításokat, például az új kötetekre vagy törölt kötetek hello eredmények fog szerepelni.

## <a name="configure-and-back-up-a-basic-volume"></a>Konfigurálja, és készítsen biztonsági másolatot alaplemezek
Használja a következő eljárás tooconfigure alaplemezek, biztonsági másolatot hello majd azonnal a biztonsági mentést elindítani, vagy ütemezett biztonsági mentések házirendet hozhat létre.

### <a name="prerequisites"></a>Előfeltételek
Előzetes teendők

* Győződjön meg arról, hogy a StorSimple eszköz hello és a gazdaszámítógép helyesen vannak konfigurálva. További információ: túl[a helyszíni StorSimple eszköz üzembe helyezése](storsimple-deployment-walkthrough-u2.md).
* Telepítse és konfigurálja a StorSimple Snapshot Manager. További információ: túl[StorSimple Snapshot Manager telepítése](storsimple-snapshot-manager-deployment.md).

#### <a name="tooconfigure-backup-of-a-basic-volume"></a>egyszerű kötet tooconfigure biztonsági másolata
1. Hozzon létre egy egyszerű kötet hello StorSimple eszközön.
2. Csatolásához, inicializálásához és formázásához hello leírtak [csatlakoztassa köteteket](#mount-volumes). 
3. Kattintson a hello StorSimple Snapshot Manager ikonra az asztalon. hello StorSimple Snapshot Manager ablak. 
4. A hello **hatókör** ablaktáblában kattintson a jobb gombbal hello **kötetek** csomópont, és válassza **ismételt vizsgálat kötetek**. Hello vizsgálat befejezését követően hello meg kell jelennie a köteteket **eredmények** ablaktáblán. 
5. A hello **eredmények** ablaktáblán kattintson a jobb gombbal a hello kötet, és válassza ki **kötet csoport létrehozása**. 
   
    ![Kötet csoport létrehozása](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. A hello **kötet csoport létrehozása** párbeszédpanelen írja be a hello kötet csoport nevét, rendelje hozzá a kötetek tooit, és kattintson **OK**.
7. A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópont. hello új kötet csoport meg kell jelennie a hello **kötet csoportok** csomópont. 
8. Kattintson a jobb gombbal hello kötet csoport nevét.
   
   * Kattintson egy interaktív (igény) biztonsági mentési feladat toostart **érvénybe biztonsági mentési**. 
   * Kattintson az automatikus biztonsági mentés tooschedule **biztonsági mentési házirend létrehozása**. A hello **általános** lapon, válassza ki a kötet hello listából. A hello **ütemezés** lapján írja be a hello ütemezési adatait. Ha elkészült, kattintson a **OK**. 
9. amely a biztonsági mentési feladat hello tooconfirm elindult, bontsa ki a hello **feladatok** hello csomópontja **hatókör** ablaktáblán, és kattintson a hello **futtató** csomópont. jelenleg futó feladatok hello listája jelenik meg hello **eredmények** ablaktáblán. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfigurálja, és készítsen biztonsági másolatot a dinamikus tükrözött kötetek
Hajtsa végre a következő lépéseket tooconfigure dinamikus tükrözött kötet biztonsági mentését hello:

* 1. lépés: A Lemezkezelés toocreate dinamikus tükrözött kötet. 
* 2. lépés: Használja a StorSimple Snapshot Manager tooconfigure biztonsági mentés.

### <a name="prerequisites"></a>Előfeltételek
Előzetes teendők

* Győződjön meg arról, hogy a StorSimple eszköz hello és a gazdaszámítógép helyesen vannak konfigurálva. További információ: túl[a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md).
* Telepítse és konfigurálja a StorSimple Snapshot Manager. További információ: túl[StorSimple Snapshot Manager telepítése](storsimple-snapshot-manager-deployment.md).
* Konfigurálhatja a két kötet hello StorSimple eszközön. (A hello példákban hello elérhető kötetek vannak **lemez 1** és **Disk 2**.) 

### <a name="step-1-use-disk-management-toocreate-a-dynamic-mirrored-volume"></a>1. lépés: A Lemezkezelés toocreate dinamikus tükrözött kötetek
A Lemezkezelés eszköz szolgál merevlemezek és hello kötetek vagy partíciókat tartalmaz. További információ a Lemezkezelés eszközben lépjen túl[Lemezkezelés](https://technet.microsoft.com/library/cc770943.aspx) hello Microsoft TechNet webhelyen.

#### <a name="toocreate-a-dynamic-mirrored-volume"></a>a dinamikus tükrözött kötetek toocreate
1. Használja a következő beállítások toostart Lemezkezelés hello valamelyikét: 
   
   * Megnyitás hello **futtatása** mezőbe írja be **Diskmgmt.msc**, és nyomja le az ENTER billentyűt.
   * Indítsa el a Kiszolgálókezelőt, bontsa ki a hello **tárolási** csomópont, és válassza **Lemezkezelés**. 
   * Start **felügyeleti eszközök**, bontsa ki a hello **számítógép-kezelés** csomópont, és válassza **Lemezkezelés**. 
2. Győződjön meg arról, hogy rendelkezik-e elérhető két kötet hello StorSimple eszközön. (Hello példában hello elérhető kötetek vannak **lemez 1** és **Disk 2**.) 
3. Hello Lemezkezelőben, hello jobb oldali oszlopban található hello alsó panelén kattintson a jobb gombbal **lemez 1** válassza **új tükrözött kötet**. 
   
    ![Új tükrözött kötetek](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. A hello **új tükrözött kötet** varázslólap, kattintson a **következő**.
5. A hello **lemez kiválasztása** lapon, válassza ki **Disk 2** a hello **kijelölt** ablaktáblán kattintson **Hozzáadás**, és kattintson a **tovább**. 
6. A hello **meghajtóbetűjel hozzárendelése vagy az elérési út** lapon fogadja el hello alapértelmezett beállításokat, és kattintson a **következő**. 
7. A hello **kötet formázásának** lap hello **foglalásiegység-méret** mezőben válassza **64K**. Jelölje be hello **gyorsformázást** jelölőnégyzetet, majd kattintson a **következő**. 
8. A hello **befejezése hello új tükrözött kötet** lapon tekintse át a beállításokat, és kattintson a **Befejezés**. 
9. Egy üzenet jelenik meg, hogy az alaplemez hello tooindicate konvertált tooa dinamikus lemez lesz. Kattintson a **Yes** (Igen) gombra.
   
    ![Dinamikus lemez átalakítás üzenet](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. A Lemezkezelés eszközben ellenőrizze, hogy lemez 1 és lemez 2 rendszer dinamikus tükrözött kötetek. (**Dinamikus** kell hello állapot oszlop jelenik meg, és hello kapacitás színe toored, a tükrözött kötetek jelző kell módosítani.) 
    
    ![Lemez tükrözött felügyeleti dinamikus lemezek](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-tooconfigure-backup"></a>2. lépés: Használja a StorSimple Snapshot Manager tooconfigure biztonsági mentése
Használja a következő eljárás tooconfigure dinamikus tükrözött kötetek hello majd azonnal a biztonsági mentést elindítani, vagy ütemezett biztonsági mentések házirendet hozhat létre.

#### <a name="tooconfigure-backup-of-a-dynamic-mirrored-volume"></a>a dinamikus tükrözött kötetek tooconfigure biztonsági másolata
1. Kattintson a hello StorSimple Snapshot Manager ikonra az asztalon. hello StorSimple Snapshot Manager ablak. 
2. A hello **hatókör** ablaktáblában kattintson a jobb gombbal hello **kötetek** csomópont, és válassza **ismételt vizsgálat kötetek**. Hello vizsgálat befejezését követően hello meg kell jelennie a köteteket **eredmények** ablaktáblán. csak egy kötet hello dinamikus tükrözött kötet szerepel. 
3. A hello **eredmények** ablaktáblán kattintson a jobb gombbal a hello dinamikus tükrözött kötet, és kattintson a **kötet csoport létrehozása**. 
4. A hello **kötet csoport létrehozása** párbeszédpanelen írja be a hello kötet csoport nevét, rendelje hello dinamikus tükrözött kötet toothis csoportot, és kattintson **OK**. 
5. A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópont. hello új kötet csoport meg kell jelennie a hello **kötet csoportok** csomópont. 
6. Kattintson a jobb gombbal hello kötet csoport nevét. 
   
   * Kattintson egy interaktív (igény) biztonsági mentési feladat toostart **érvénybe biztonsági mentési**. 
   * Kattintson az automatikus biztonsági mentés tooschedule **biztonsági mentési házirend létrehozása**. A hello **általános** lapot, jelölje be hello kötet csoport hello listából. A hello **ütemezés** lapján írja be a hello ütemezési adatait. Ha elkészült, kattintson a **OK**. 
7. Figyelheti hello biztonsági mentési feladat futtatása közben. A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontra, majd **futtató**, hello feladat részletei megjelennek a hello **eredmények** ablaktáblán. Ha hello biztonsági mentési feladat befejeződött, hello részletek-e az átvitt toohello **utolsó 24** óra feladat listája. 

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* Ismerje meg, hogyan túl[StorSimple Snapshot Manager toocreate használatát és kezelését a kötet csoportok](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
