---
title: "Áttekintés: Azure virtuális gépek védelme Recovery Services-tárolóval | Microsoft Docs"
description: "Megvédheti Azure virtuális gépeit egy Recovery Services-tárolóval. Használja az adatok az erőforrás-kezelő telepített virtuális gépek, a klasszikus telepített virtuális gépek és a prémium szintű tároló virtuális gépek, virtuális gépek titkosítva felügyelt lemezek tooprotect futó virtuális gépek biztonsági mentése. Létrehozhat és regisztrálhat egy Recovery Services-tárolót. Regisztrálhat virtuális gépeket, létrehozhat egy házirendet, és megvédheti virtuális gépeit az Azure-ban."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/15/2017
ms.author: markgal;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70e4700abb76e16e32e1ead06ce1dbe277e1f0e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-toorecovery-services-vaults"></a>Készítsen biztonsági másolatot az Azure virtuális gépek tooRecovery szolgáltatások tárolók
> [!div class="op_single_selector"]
> * [Virtuális gépek védelme Recovery Services-tárolóval](backup-azure-vms-first-look-arm.md)
> * [Virtuális gépek védelme Backup-tárolóval](backup-azure-vms-first-look.md)
>
>

Ez az oktatóanyag végigvezeti a recovery services-tároló létrehozása és biztonsági mentése egy Azure virtuális gép (VM) hello lépései. A Recovery Services-tárolók megvédik:

* Az Azure Resource Manager által telepített virtuális gépeket
* A klasszikus virtuális gépeket
* A Standard szintű tárolós virtuális gépeket
* A Prémium szintű tárolós virtuális gépeket
* A Managed Disksen futó virtuális gépek
* Az Azure Disk Encryption használatával titkosított virtuális gépeket, amelyek rendelkeznek BEK-kel és KEK-kel
* Windows rendszerű virtuális gépek alkalmazáskonzisztens biztonsági mentése VSS és Linux rendszerű virtuális gépekkel egyéni, pillanatkép előtti és utáni szkriptekkel

Prémium szintű storage virtuális gépek védelméről további információkért lásd: hello cikk [biztonsági mentése és visszaállítása a prémium szintű Storage virtuális gépek](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup). További információ a felügyelt lemezeken található virtuális gépek támogatásáról: [ Virtuális gépek biztonsági mentése és visszaállítása felügyelt lemezeken](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). A Linux rendszerű virtuális gépek biztonsági mentéseinek szkript előtti és utáni keretrendszerével kapcsolatos további információért lásd: [Linux rendszerű virtuális gépek alkalmazáskonzisztens biztonsági mentése szkript előtti és utáni keretrendszerrel] (https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Tekintse meg a toofind több mi is meg biztonsági másolat, és mit nem, [Itt](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

> [!NOTE]
> Ez az oktatóanyag azt feltételezi, hogy már rendelkezik egy virtuális Gépet az Azure-előfizetéshez, és, hogy elvégezte-e az intézkedések tooallow hello biztonsági mentési szolgáltatás tooaccess hello virtuális gép.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

Attól függően, hogy a virtuális gépek száma hello meg tooprotect, elkezdheti a különböző kezdőpontja. Ha szeretne egy művelettel több virtuális gépek tooback, nyissa meg toohello Recovery Services-tároló és [biztonsági mentési feladat indítása hello hello tároló irányítópultról](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-recovery-services-vault). Ha azt szeretné, hogy egyetlen virtuális gépek tooback, hello biztonsági mentési feladat a Virtuálisgép-felügyelet panel is kezdeményezhető.

## <a name="configure-hello-backup-job-from-hello-vm-management-blade"></a>Hello biztonsági mentési feladat a Virtuálisgép-felügyelet panel hello konfigurálása

Hello lépéseket tooconfigure hello biztonsági mentési feladat követően – hello virtuális gép felügyelet panel az Azure-portálon hello használata. Ezeket a lépéseket a klasszikus portálon hello toohello virtuális gépek nem vonatkoznak.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello központ menüben kattintson a **több szolgáltatások** hello szűrő párbeszédpanelen írja be a **virtuális gépek**. Megadásakor hello erőforrások szűrők listája. Amikor meglátja a Virtuális gépet, kattintson rá.

  ![A központ menüben kattintson a további szolgáltatások tooopen párbeszédpanelen, és írja be a virtuális gépek](./media/backup-azure-vms-first-look-arm/open-vm-from-hub.png)

  virtuális gépek (VM) hello előfizetésben hello listája jelenik meg.

  ![hello hello előfizetésben található virtuális gépek listája jelenik meg.](./media/backup-azure-vms-first-look-arm/list-of-vms.png)

3. Hello listájából válassza ki a virtuális gép tooback fel.

  ![hello hello előfizetésben található virtuális gépek listája jelenik meg.](./media/backup-azure-vms-first-look-arm/list-of-vms-selected.png)

  Hello méretű kiválasztásakor virtuális gépek listájának hello átvált toohello balra, és hello virtuálisgép-felügyelet panel és hello virtuális gép irányítópult, nyissa meg. </br>
 ![Virtuális gép kezelése panel](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)

4. Hello VM felügyeleti panelen, a hello **beállítások** kattintson **biztonsági mentés**. </br>

  ![Backup beállítás a Virtuális gép kezelése panelen](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

  hello engedélyezése biztonsági mentési panel nyílik meg.

  ![Backup beállítás a Virtuális gép kezelése panelen](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. Hello Recovery Services-tároló, kattintson a **válasszon meglévő** és hello legördülő listából válassza ki a hello tárolóban.

  ![Biztonsági mentés engedélyezése varázsló](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

  Ha nincs Recovery Services-tárolók, vagy ha azt szeretné, hogy egy új tároló toouse, kattintson a **hozzon létre új** , és adja meg az új tároló hello hello nevét. Egy új tároló létrehozása hello ugyanazt az erőforráscsoportot és hello virtuális gép ugyanazon a helyen. Ha azt szeretné, hogy a Recovery Services-tároló, eltérő értékű toocreate, című hello hogyan túl[a recovery services-tároló létrehozása](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm).

6. a biztonsági mentési házirend, hello tooview hello részletekért kattintson **biztonsági mentési házirend**.

  Hello **biztonsági mentési házirend** panel nyílik meg, és kijelölt hello házirend hello részleteit. Ha vannak más házirendek, használja a hello legördülő menü toochoose egy másik biztonsági mentési házirend. Ha azt szeretné, hogy toocreate egy házirendet, válassza ki a **hozzon létre új** hello legördülő menüből. A biztonsági mentési házirendek meghatározását segítő utasításokat itt találja: [Biztonsági mentési házirend meghatározása](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). toosave hello módosítások toohello a biztonsági mentési házirend, és visszatérési toohello engedélyezése biztonsági mentési paneljén kattintson **OK**.

  ![Biztonsági mentési házirend kiválasztása](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new-2.png)

7. Hello engedélyezése biztonsági mentési paneljén kattintson **biztonsági mentés engedélyezése** toodeploy hello házirend. Hello szabályzat telepítése társítja azt hello tárolóban és hello virtuális gépek.

  ![Biztonsági mentés engedélyezése gomb](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. Előrehaladásának hello konfigurációs keresztül hello értesítések, amelyek hello portálon jelennek meg. hello a következő példa bemutatja, hogy elindult-e a központi telepítés.

  ![Biztonsági mentési értesítések engedélyezése](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. A Virtuálisgép-felügyelet panel hello, hello konfigurációs folyamat befejezése után kattintson **biztonsági mentés** tooopen hello biztonsági másolati elem panelt, és tekintse meg a hello részleteit.

  ![Virtuális gép biztonsági mentési elemei nézet](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

  Hello kezdeti biztonsági mentés befejeződött, amíg **utolsó biztonsági mentés állapotának** jeleníti meg, mint a **figyelmeztetés (függőben lévő kezdeti biztonsági mentési)**. Amikor hello a biztonsági mentési feladat következő ütemezett toosee következik be, a **biztonsági mentési házirend** kattintson hello hello házirend nevét. hello biztonsági mentési házirend panel nyílik meg, és hello ütemezett biztonsági mentés hello időt jeleníti meg.

10. toorun egy biztonsági mentési feladat és hello első helyreállítási pont létrehozása, a biztonsági mentés hello tároló panelen kattintson **biztonsági mentés most**.

  ![a biztonsági mentés most toorun hello kezdeti biztonsági mentés gombra](./media/backup-azure-vms-first-look-arm/backup-now.png)

  hello biztonsági mentés most panel nyílik meg.

  ![hello biztonsági mentés most panelen látható](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

11. Hello biztonsági mentés most paneljén kattintson hello naptár ikonra, használja a hello Naptár vezérlőelem tooselect hello utolsó napja a helyreállítási pont őrzi meg, és kattintson a **biztonsági mentés**.

  ![hello utolsó nap hello biztonsági mentés most őrzi meg a helyreállítási pont beállítása](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Központi telepítés értesítések lehetővé teszik, hogy hello biztonsági mentési feladat lett elindítva, és hogy kísérheti hello hello feladat hello biztonsági mentési feladatok lapján.

## <a name="configure-hello-backup-job-from-hello-recovery-services-vault"></a>Hello biztonsági mentési feladata a Recovery Services-tároló hello konfigurálása
tooconfigure hello biztonsági mentési feladat, befejezte a következő lépéseket hello.  

1. Hozzon létre egy Recovery Services-tárolót egy virtuális géphez.
2. Az Azure portál tooselect esetén használjon hello beállított biztonsági mentési házirend és az elemek tooprotect azonosítja.
3. Hello kezdeti biztonsági mentés futtatására.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Létrehoz egy Recovery Services-tárolót egy virtuális géphez.
Recovery Services-tároló olyan entitás, amely hello biztonsági mentések és adott idő alatt létrehozott helyreállítási pontokat tárolja. hello Recovery Services-tároló is tartalmaz hello alkalmazza a biztonsági mentési házirend toohello védett virtuális gépeket.

> [!NOTE]
> A virtuális gépek biztonsági mentése egy helyi folyamat. Nem készíthet biztonsági másolatot virtuális gépeket az egyik helyen tooa Recovery Services-tárolónak egy másik helyen. Igen minden Azure-beli hely, amely rendelkezik a virtuális gépek toobe biztonsági másolat, legalább egy Recovery Services-tároló léteznie kell az adott helyre.
>
>

Recovery Services-tároló toocreate:

1. Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) használata az Azure-előfizetéshez.
2. Hello központ menüben kattintson a **további szolgáltatások** hello szűrő párbeszédpanelen írja be a **Recovery Services**. Megadásakor hello erőforrások szűrők listája. Amikor megjelenik a Recovery Services-tárolók hello listában, kattintson rá.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Ha nincsenek Recovery Services-tárolók hello az előfizetést, hello tárolók vannak felsorolva.

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-azure-vms-first-look-arm/list-of-rs-vault.png)
3. A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.

    ![Recovery Services-tároló létrehozása – 3. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. hello nevének kell toobe egyedi hello Azure-előfizetés esetében. Írjon be egy 2–50 karakter hosszúságú nevet. Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.

5. A hello **előfizetés** területen hello legördülő menü toochoose hello Azure-előfizetés használatára. Ha csak egyetlen előfizetéssel, előfizetés megjelenő használja, és tovább toohello a lépést kihagyhatja. Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés. Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.

6. A hello **erőforráscsoport** szakasz:

    * Válassza ki **hozzon létre új** Ha azt szeretné, hogy toocreate egy erőforráscsoportot.
    Vagy
    * Válassza ki **meglévő** kattintson hello legördülő menü toosee hello elérhető erőforráscsoportok listáját.

  Az erőforráscsoportok, tanulmányozza hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).

7. Kattintson a **hely** tooselect hello földrajzi régióban hello tároló. Ez a beállítás meghatározza, hogy hello földrajzi régiót, ahol a biztonsági mentési adatokat küldi el.

  > [!IMPORTANT]
  > Ha biztos benne, hogy hello helyét, amelyben a virtuális gép található, hello tároló létrehozása párbeszédpanel bezárásához, és válassza a toohello hello portál virtuális gépek listáját. Ha több régióban rendelkezik virtuális gépekkel, minden régióban hozzon létre egy Recovery Services-tárolót. Mielőtt továbblép a következő helyre toohello hello első helyen hello tárolóban hozza létre. Nem kell toospecify hello tárfiókok használt toostore hello biztonsági mentési adatok – hello Recovery Services-tároló és a hello Azure Backup szolgáltatás automatikusan hello-tároló kezeléséhez.
  >

8. A Recovery Services-tároló panel hello hello alján kattintson **létrehozása**.

    Recovery Services-tároló létrehozása toobe hello több percet is igénybe vehet. Hello állapot értesítések hello felső jobb területen hello portál figyelése. A tároló létrehozása után hello Recovery Services-tárolók listája jelenik meg. Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.

    ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    A tároló a Recovery Services-tárolók hello listája jelenik meg, ha készen áll a tooset hello adattároló redundanciája, amely áll.

Most, hogy létrehozta a tárolót, megtudhatja, hogyan tooset hello tárolóreplikálást.

### <a name="set-storage-replication"></a>Tárreplikáció beállítása
hello tárolási replikációs beállítás lehetővé teszi toochoose georedundáns tárolás és a helyileg redundáns tárolás között. Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik. Az elsődleges biztonsági mentés esetén a Recovery Services-tároló hello hagyja hello tárolási replikációs beállítás set toogeo redundáns tárolás. Ha egy olcsóbb, rövidebb élettartamú megoldást szeretne, válassza a helyileg redundáns tárolást. Tudjon meg többet az [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolásáról: hello [Azure Storage replikáció – áttekintés](../storage/common/storage-redundancy.md).

tooedit hello tárolási replikációs beállítását:

1. A hello **Recovery Services-tárolók** panelen, jelölje be hello új tárolóba.

  ![Válassza ki az új tároló hello a hello Recovery Services-tároló](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

  Hello tároló kiválasztásakor hello beállítások panel (*hello tároló neve tartalmaz hello felső*) és hello tároló Részletek panel megnyitása.

  ![Hello új tároló tárolási konfiguráció megtekintése](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. Hello új tároló-beállítások panelen hello függőleges diák tooscroll le toohello kezelése szakasz használja, és kattintson **biztonsági infrastruktúra**.
    hello biztonsági infrastruktúra panel nyílik meg.
3. Hello biztonsági infrastruktúra paneljén kattintson **biztonsági mentési konfigurációhoz** tooopen hello **biztonsági mentési konfigurációhoz** panelen.

    ![Új tároló hello tárolási konfiguráció beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Válassza ki a hello megfelelő tárolási replikációs beállítás a tároló számára.

    ![a tároló konfigurálásának lehetőségei](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik. Azure biztonságimásolat-tároláshoz-végpontként használatakor, továbbra is toouse **georedundáns**. Ha nem adja meg Azure biztonságimásolat-tároláshoz-végpontként, majd válasszon **helyileg redundáns**, amely csökkenti a hello Azure storage költségei. A [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és a [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolási lehetőségekről többet olvashat ebben a [Tárhely-redundancia áttekintésben](../storage/common/storage-redundancy.md).


## <a name="select-a-backup-goal-set-policy-and-define-items-tooprotect"></a>Biztonsági mentési cél, házirend, és adja meg az elemek tooprotect
Mielőtt regisztrálná a virtuális gép egy tárolóban, futtassa a hello felderítési folyamat tooensure, hogy minden új virtuális gépek toohello előfizetés hozzáadott azonosítja. hello folyamat lekérdezések Azure hello hello az előfizetést, valamint további információkat a virtuális gépek listája például a felhőalapú szolgáltatás- és hello régió hello. Hello Azure-portálon, a forgatókönyv tooput hello recovery services-tároló be fog toowhat hivatkozik. A házirend olyan hello milyen gyakran és mikor készít-e a helyreállítási pontok ütemezését. Házirend hello megőrzési időtartam hello helyreállítási pontok esetében is.

1. Ha már van egy helyreállítási szolgáltatások nyissa meg a tároló, a folytatáshoz toostep 2. Ellenkező esetben hello központ menüben kattintson **további szolgáltatások** hello az erőforrások listájához, írja be a **Recovery Services** kattintson **Recovery Services-tárolók**.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    helyreállítási szolgáltatások tárolók hello listája jelenik meg.

    ![Helyreállítási szolgáltatások hello ábrázolása tárolók listája](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    A recovery services-tárolók hello listáról válassza ki a tároló tooopen az irányítópulton.

     ![Tároló panelének megnyitása](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Hello tároló irányítópult menüjének **biztonsági mentés** tooopen hello biztonsági mentés panelen.

    ![Biztonsági mentés panel megnyitása](./media/backup-azure-arm-vms-prepare/backup-button.png)

    hello biztonsági mentési és a biztonsági mentési cél panel nyílik meg.

    ![Forgatókönyv panel megnyitása](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)
3. Hello biztonsági mentési cél panelen, a hello **a számítási feladatok futtató** legördülő menüben válassza ki az Azure. A hello **miről szeretne toobackup** legördülő, válassza ki a virtuális gépet, majd kattintson a **OK**.

    Ezek a műveletek hello Virtuálisgép-bővítmény hello tárolóhoz regisztrált. hello panel bezárása után a biztonsági másolat cél és hello **biztonsági mentési házirend** panel nyílik meg.

    ![Forgatókönyv panel megnyitása](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Hello biztonsági szabályzat paneljén válassza hello tooapply toohello tároló kívánt biztonsági mentési házirendet.

    ![Biztonsági mentési házirend kiválasztása](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    hello alapértelmezett házirend hello adatait hello legördülő menüjében találhatók. Ha azt szeretné, hogy toocreate egy házirendet, válassza ki a **hozzon létre új** hello legördülő menüből. A biztonsági mentési házirendek meghatározását segítő utasításokat itt találja: [Biztonsági mentési házirend meghatározása](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Kattintson a **OK** tooassociate hello biztonsági mentési házirend hello tárolóban.

    Biztonsági szabályzat panel bezárása és hello hello **válassza ki a virtuális gépek** panel nyílik meg.
5. A hello **válassza ki a virtuális gépek** panelen válassza ki a hello virtuális gépek tooassociate hello a megadott házirend, és kattintson a **OK**.

    ![Számítási feladat kiválasztása](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    hello kijelölt virtuális gép van hitelesítve. Ha nem látja a hello virtuális gépekhez, amelyek toosee, ellenőrizze, hogy léteznek a várt hello azonos Azure-beli hely, hello Recovery Services-tároló. Recovery Services-tároló hello hello helye hello tároló irányítópulton látható.

6. Most, hogy a beállított hello tárolóban, az összes beállítás hello biztonsági mentés panelen, kattintson a **biztonsági mentés engedélyezése** toodeploy hello házirend toohello tárolóban, és a virtuális gépek hello. Hello biztonsági mentési házirend központi telepítése nem hoz létre hello első helyreállítási pont hello virtuális géphez.

    ![Biztonsági mentés engedélyezése](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Miután sikeresen engedélyezte a hello biztonsági mentés, a biztonsági mentési házirend végrehajtja a ütemezés szerint. A folytatáshoz azonban tooinitiate hello első biztonsági mentési feladat.

## <a name="initial-backup"></a>Kezdeti biztonsági mentés
Ha a biztonsági mentési házirend lett telepítve a hello virtuális gép számára, amely nem jelenti azt hello adatok biztonsági másolat készült. Alapértelmezés szerint hello első ütemezett biztonsági mentést (meghatározott módon a biztonsági mentési házirend hello) az hello kezdeti biztonsági másolatot. Amíg nem történik hello kezdeti biztonsági másolatot, a hello hello utolsó biztonsági mentés állapotának **biztonsági mentési feladatok** panelt jeleníti meg, mint a **figyelmeztetés (függőben lévő kezdeti biztonsági másolatot)**.

![Biztonsági mentés függőben](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Kivéve, ha a kezdeti biztonsági másolatot esedékes toobegin hamarosan, javasoljuk, hogy **biztonsági másolat készítése**.

toorun hello kezdeti biztonsági mentési feladat:

1. Az hello tároló irányítópultján kattintson a hello szám alatt **biztonsági mentés elemek**, vagy kattintson a hello **biztonsági mentés elemek** csempe. <br/>
  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Hello **biztonsági mentés elemek** panel nyílik meg.

  ![Elemek biztonsági mentése](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. A hello **biztonsági mentés elemek** panelen, jelölje be hello elemet.

  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Hello **biztonsági mentés elemek** listában megnyílik. <br/>

  ![Biztonsági mentési feladat elindul](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. A hello **biztonsági mentés elemek** listában, kattintson a hello folytatást jelző pontokra **...**  tooopen hello helyi menü.

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  hello helyi menü megjelenik.

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Hello helyi menüben, kattintson a **biztonsági mentés most**.

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  hello biztonsági mentés most panel nyílik meg.

  ![hello biztonsági mentés most panelen látható](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Hello biztonsági mentés most paneljén kattintson hello naptár ikonra, használja a hello Naptár vezérlőelem tooselect hello utolsó napja a helyreállítási pont őrzi meg, és kattintson a **biztonsági mentés**.

  ![hello utolsó nap hello biztonsági mentés most őrzi meg a helyreállítási pont beállítása](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Központi telepítés értesítések lehetővé teszik, hogy hello biztonsági mentési feladat lett elindítva, és hogy kísérheti hello hello feladat hello biztonsági mentési feladatok lapján. Attól függően, hogy a virtuális gép mérete hello hello kezdeti biztonsági másolatot készít eltarthat egy ideig.

6. hello első biztonsági hello tároló irányítópult hello tooview vagy követése hello állapotának **biztonsági mentési feladatok** csempén kattintson **folyamatban**.

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  hello biztonsági mentési feladatok panel nyílik meg.

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  A hello **biztonsági mentési feladatok** panelen láthatja, hogy az összes feladat hello állapota. Ellenőrizze, ha még folyamatban van a virtuális gép biztonsági mentési feladata hello, vagy befejeződött. Ha egy biztonsági mentési feladat befejeződött, hello állapota *befejezve*.

  > [!NOTE]
  > Hello biztonsági mentési művelet részeként hello Azure Backup szolgáltatás kibocsát egy parancs toohello tartalék mellék minden virtuális gép tooflush ír, és egységes pillanatképet készít a.
  >
  >


[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Hello Virtuálisgép-ügynök telepítése hello virtuális gépen
Ez az információ szükség esetén mellékelve van. hello Azure Virtuálisgép-ügynök hello hello biztonsági mentés bővítmény toowork Azure virtuális gép telepítve kell lennie. Azonban ha a virtuális Gépet az Azure katalógusában hello jött létre, majd hello Virtuálisgép-ügynök már telepítve hello virtuális gépet. Virtuális gépek, amelyek áttelepítése a helyszíni adatközpontját a volna nem hello Virtuálisgép-ügynök telepítve. Ebben az esetben hello Virtuálisgép-ügynök telepítve toobe van szüksége. Ha problémába ütközik hello Azure virtuális gép biztonsági mentéséről, ellenőrizze, hogy a hello Azure Virtuálisgép-ügynök megfelelően telepítve van a hello virtuális gépen (lásd a következő táblázat hello). Ha létrehoz egy egyéni virtuális Gépet, [hello biztosítása **Virtuálisgép-ügynök telepítése hello** jelölőnégyzet be van jelölve](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) hello virtuális gép kiépítése előtt.

További tudnivalók: hello [Virtuálisgép-ügynök](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) és [hogyan tooinstall azt](../virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

a következő táblázat hello VM ügynök a Windows hello és a Linux virtuális gépek további információkat tartalmaz.

| **Művelet** | **Windows** | **Linux** |
| --- | --- | --- |
| Hello Virtuálisgép-ügynök telepítése |<li>Töltse le és telepítse a hello [ügynök MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége. <li>[Hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van. |<li> Telepítse legújabb hello [Linux-ügynök](https://github.com/Azure/WALinuxAgent) a Githubról. Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége. <li> [Hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van. |
| Hello Virtuálisgép-ügynök frissítése |Frissítési hello Virtuálisgép-ügynök újratelepítését hello egyszerűen [Virtuálisgép-ügynök bináris](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Győződjön meg arról, hogy fut-e biztonsági mentési művelet, hello Virtuálisgép-ügynök frissítése közben. |Hello utasításokat kövesse a megjelenő [Linux Virtuálisgép-ügynök frissítése hello](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Győződjön meg arról, hogy fut-e biztonsági mentési művelet során hello Virtuálisgép-ügynök frissítése folyamatban van. |
| Hello Virtuálisgép-ügynök telepítésének ellenőrzése |<li>Keresse meg a toohello *C:\WindowsAzure\Packages* hello Azure virtuális gép mappájában. <li>Keresse meg hello WaAppAgent.exe fájl található.<li> Kattintson a jobb gombbal a hello fájlt, nyissa meg túl**tulajdonságok**, majd válassza ki a hello **részletek** külön-külön hello termékverzió mező lehet 2.6.1198.718 vagy újabb. |N/A |

### <a name="backup-extension"></a>Backup bővítmény
Virtuálisgép-ügynök telepítve van a hello virtuális gépen, hello Azure Backup szolgáltatás hello egyszer hello tartalék mellék toohello Virtuálisgép-ügynök telepítése. hello Azure Backup szolgáltatás zökkenőmentesen frissíti, és javításokkal hello tartalék mellék további felhasználói beavatkozás nélkül.

hello biztonsági mentési szolgáltatás hello tartalék mellék, telepíti, akkor is, ha nem fut a virtuális gép hello. A virtuális gép hello legnagyobb esély arra az alkalmazáskonzisztens helyreállítási pontot biztosít. Azonban hello Azure Backup szolgáltatás továbbra is fel hello VM tooback még akkor is, ha ki van kapcsolva, és hello bővítmény telepítése sikertelen volt. Ilyen típusú biztonsági mentést is ismert, offline állapotba a virtuális gép, és hello helyreállítási pont *összeomlás-konzisztens*.

## <a name="troubleshooting-information"></a>Hibaelhárítási információ
Ha problémába ütközik ebben a cikkben hello feladatokat parancsokról, tekintse meg a [hibaelhárítási útmutatás](backup-azure-vms-troubleshoot.md).

## <a name="pricing"></a>Díjszabás
Azure virtuális gépek biztonsági mentéséről hello költségét védett példányok száma hello alapul. A védett példányok definíciójáért lásd [a fogalmat ismertető szakaszt](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). A virtuális gépek biztonsági mentéséről hello költségét kiszámítása példáért lásd: [védett példányok kiszámítási módját](backup-azure-vms-introduction.md#calculating-the-cost-of-protected-instances). Tekintse meg az Azure Backup árairól hello kapcsolatos információ lapon [biztonsági mentés árazásának](https://azure.microsoft.com/pricing/details/backup/).

## <a name="questions"></a>Kérdései vannak?
Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).
