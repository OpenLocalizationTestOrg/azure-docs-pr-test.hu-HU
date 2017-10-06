---
title: "Az Azure Backup: Állítsa vissza a virtuális gépek hello Azure-portál használatával |} Microsoft Docs"
description: "Azure virtuális gép visszaállíthatja őket egy helyreállítási pontból, Azure-portál használatával"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "biztonsági másolat visszaállításával lehetséges; Hogyan toorestore; helyreállítási pont;"
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: f4f75d1da73c7760d2952afe80ff94918a08351c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-toorestore-virtual-machines"></a>Az Azure portál toorestore virtuális gépek használata
> [!div class="op_single_selector"]
> * [Állítsa vissza a virtuális gépek a klasszikus portál](backup-azure-restore-vms.md)
> * [Állítsa vissza a virtuális gépek Azure-portálon](backup-azure-arm-restore-vms.md)
>
>

Pillanatképek készítése a adatait meghatározott időközönként védeni az adatokat. Ezeket a pillanatképeket nevezzük helyreállítási pontok, és a recovery services-tárolók tárolódnak. Ha, vagy ha szükséges toorepair vagy építse újra a virtuális gépek, a hello VM bármelyik mentett helyreállítási pontok hello is helyreállíthatja. Ha visszaállítja a helyreállítási pont, hozzon létre egy új virtuális Gépet, amely a biztonsági másolat VM időpontban ábrázolása vagy visszaállíthatja lemezek és hello sablon használata, amely rendelkezik, toocustomize hello visszaállítani a virtuális gép vagy egy egyéni fájl helyreállításával próbálkozik. Ez a cikk azt ismerteti, hogyan toorestore egy virtuális gép tooa új virtuális gép vagy a visszaállítási összes biztonsági másolat lemezeket. Az egyes fájlok helyreállítása, tekintse meg túl[fájlok helyreállítása Azure virtuális gép biztonsági mentése](backup-azure-restore-files-from-vm.md)

![3-ways-restore-from-VM-Backup](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk ismerteti, hello telepített hello Resource Manager modellt használó virtuális gépek visszaállítására.
>
>

A virtuális gép vagy az összes lemez visszaállítása a virtuális gép biztonsági mentése két lépésből áll:

1. A visszaállítási visszaállítási pont kiválasztása
2. Hello kiválasztása állítani típus - hozzon létre egy új virtuális Gépet, vagy állítsa vissza a lemezek és adja meg a szükséges paramétereket. 

## <a name="select-restore-point-for-restore"></a>Válasszon visszaállítási pontot a visszaállításhoz
1. Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com/)
2. A hello Azure menüben, kattintson **Tallózás** és a szolgáltatások hello listájának megtekintéséhez írja be **Recovery Services**. szolgáltatások listája hello beírta toowhat módosítja. Amikor látja **Recovery Services-tárolók**, válassza ki azt.

    ![Nyissa meg a Recovery Services-tároló](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    hello hello előfizetésben tárolók listája jelenik meg.

    ![Helyreállítási szolgáltatások listája tárolók](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. Hello listából válassza hello tárolóhoz társított hello toorestore kívánt virtuális gép. Ha hello tároló gombra kattint, az irányítópult megnyitása.

    ![Helyreállítási szolgáltatások listája tárolók](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Most, hogy Ön hello tároló irányítópulton. A hello **biztonsági mentés elemek** csempére, kattintson a **Azure virtuális gépek** toodisplay hello virtuális gépek társított hello tárolóban.

    ![tároló irányítópult](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    Hello **biztonsági mentés elemek** panel megnyitása és az Azure virtuális gépek hello listáját jeleníti meg.

    ![a tárolóban található virtuális gépek listájára](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. Hello listából válasszon ki egy virtuális gép tooopen hello irányítópultot. hello VM irányítópult megnyitása toohello figyelés, amely hello visszaállítási pontok csempe tartalmazza.

    ![a tárolóban található virtuális gépek listájára](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. Hello VM irányítópult menüjének **visszaállítása**

    ![a tárolóban található virtuális gépek listájára](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    hello visszaállítás panel nyílik meg.

    ![visszaállítás panel](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. A hello **visszaállítása** panelen kattintson a **visszaállítási pont** tooopen hello **válasszon visszaállítási pontot** panelen.

    ![visszaállítás panel](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    Alapértelmezés szerint hello párbeszédpanelt jelenít meg hello a helyreállítási pontokat utolsó 30 nap. Használjon hello **szűrő** tooalter hello időtartománya hello visszaállítási pontok jelenik meg. Alapértelmezés szerint az összes konzisztencia visszaállítási pontok jelennek meg. Módosítsa **összes visszaállítása mutat** tooselect visszaállítási pontok egy adott konzisztenciájának szűréséhez. A helyreállítási pont típusonkénti kapcsolatos további információkért lásd: hello magyarázata [adatkonzisztenciát](backup-azure-vms-introduction.md#data-consistency).  

   * **Állítsa vissza a pont konzisztencia** lekerül erről a listáról válassza ki:
     * Összeomlás-konzisztens visszaállítási pontok
     * Alkalmazáskonzisztens visszaállítási pontok,
     * Fájlrendszerkonzisztens visszaállítási pontok
     * Minden visszaállítási pontok.  
8. Válasszon visszaállítási pontot, és kattintson a **OK**.

    ![Válasszon visszaállítási pontot](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    Hello **visszaállítása** panelen látható hello visszaállítási pont van beállítva.

    ![visszaállítási pont beállítása](./media/backup-azure-arm-restore-vms/recovery-point-set.png)
9. A hello **visszaállítása** panelen **konfiguráció visszaállítása** visszaállítási pont megadása után automatikusan megnyílik.

## <a name="choosing-a-vm-restore-configuration"></a>A virtuális gép visszaállítási konfigurációjának kiválasztása
Most, hogy a kijelölt hello visszaállítási pontot, válassza ki a helyreállítási virtuális gép konfigurációját. Hello konfigurálásához a választott visszaállítani a virtuális gép toouse van: az Azure portál vagy a PowerShell.

1. Ha nem már létezik, nyissa meg toohello **visszaállítása** panelen. Győződjön meg arról a [visszaállítási pont kiválasztva](#select-restore-point-for-restore), és kattintson a **konfiguráció visszaállítása** tooopen hello **helyreállítási konfiguráció** panelen.

    ![helyreállítási konfigurációja varázsló beállítása](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
2. A hello **konfiguráció visszaállítása** panelen, két lehetősége van:
   * Állítsa vissza a teljes virtuális gép
   * Biztonsági másolat lemezek visszaállítása

Portál egy gyors létrehozás lehetőséget biztosít a visszaállított virtuális Géphez. Ha azt szeretné, hogy toocustomize hello Virtuálisgép-konfiguráció, vagy részeként létrehozott hello erőforrások nevei, hozzon létre egy új virtuális gép választott, használja a PowerShell vagy a portál toorestore biztonsági másolat lemezek és a PowerShell-parancsok tooattach használja őket toochoice VM konfigurációval vagy -sablon, amely előre visszaállításával lemezek toocustomize hello visszaállítani a virtuális gép. Lásd: [visszaállítása a virtuális gép és a speciális hálózati konfigurációk](#restoring-vms-with-special-network-configurations) kapcsolatos részletes tudnivalókért toorestore virtuális gép, amely több hálózati adapter vagy a belső terheléselosztót. Ha a Windows virtuális gép használ [HUB licencelési](../virtual-machines/windows/hybrid-use-benefit-licensing.md), toorestore lemez szükséges, és a megadott alábbi toocreate hello VM PowerShell/sablont használ és győződjön meg arról, hogy ad meg LicenseType "Windows_Server" virtuális gép tooavail HUB létrehozása közben a visszaállított virtuális Géphez előnyeit. 
 
## <a name="create-a-new-vm-from-restore-point"></a>Hozzon létre egy új virtuális gép visszaállítási pont
Ha nem már létezik, [válasszon ki egy helyreállítási pontot](#restoring-vms-with-special-network-configurations) előtt eljárás toocreating egy új virtuális Gépet a helyreállítási pont. Miután a visszaállítási pont ki van jelölve, a hello **konfiguráció visszaállítása** panelen adja meg vagy válassza ki az értékeket az egyes mezők a következő hello:

* **Állítsa vissza a típusú** -virtuális gép létrehozása.
* **Virtuális gép neve** -adjon meg egy nevet a virtuális gép hello. hello név egyedi toohello erőforráscsoport (a Resource Manager telepített virtuális gép) vagy a felhőszolgáltatás (az egy klasszikus virtuális) kell lennie. Ha már létezik a hello előfizetés hello virtuális gép nem cserélhető le.
* **Erőforráscsoport** – használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat. A klasszikus virtuális gépek állítja vissza, ha egy új felhőalapú szolgáltatást a mező toospecify hello nevét használja. Ha hoz létre egy új erőforrás csoport/felhőalapú szolgáltatás, hello nevének globálisan egyedinek kell lennie. Hello felhőszolgáltatás neve általában társítva van egy nyilvánosan elérhető URL - pl.: [cloudservice]. cloudapp.net. Ha toouse hello felhő erőforrás csoport/felhőalapú szolgáltatás, amely már használatban van egy nevet, az Azure rendel hello erőforrás csoport/felhőalapú szolgáltatás hello ugyanazzal a névvel, a virtuális gép hello. Azure erőforrás-csoportok/felhőszolgáltatások és virtuális gépek nem társított bármely affinitáscsoportok jeleníti meg. További információkért lásd: [hogyan toomigrate az Affinitáscsoportok tooa regionális virtuális hálózatot (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
* **Virtuális hálózati** – Itt adhatja meg hello virtuális hálózathoz (VNET) létrehozásakor hello virtuális gép. hello mező hello előfizetéshez tartozó összes Vnetek biztosít. A virtuális gép hello erőforráscsoport zárójelben jelenik meg.
* **Alhálózati** -alhálózatok hello VNET e hello első alhálózat alapértelmezettként van beállítva. Ha további alhálózatokat, válassza ki a kívánt alhálózat hello.
* **A tárfiók** -a listában tárfiókok hello hello megtalálható ugyanazon a helyen hello Recovery Services-tároló. Zónaredundáns tárolás fiókok nem támogatottak. Ha nem a tárfiókok hello hello és ugyanazon a helyen Recovery Services tároló, akkor létre kell hoznia egy hello visszaállítási művelet indítása előtt. hello tárolási fiók replikációtípust zárójel szerepel.

![visszaállítás konfigurációs varázsló beállítása](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

> [!NOTE]
> 1. A Resource Manager telepített virtuális gépek állítja vissza, ha meg kell adnia egy virtuális hálózatot (VNET). Egy virtuális hálózatot (VNET) nem kötelező megadni egy klasszikus virtuális Gépet.
> 2. Virtuális gépek visszaállításakor felügyelt lemezzel, győződjön meg arról, hogy kijelölve tárfiók nincs engedélyezve a tárolás szolgáltatás Encryption(SSE) élettartama során.
> 3. Hello tároló tárfióknak típusa kiválasztva (prémium és standard) alapuló, vissza az összes lemez lesz vagy a prémium szintű, vagy a standard lemezek. Jelenleg nem támogatjuk lemezek vegyes üzemmódú visszaállításakor.  
>
>

A hello **konfiguráció visszaállítása** panelen kattintson a **OK** toofinalize hello konfiguráció visszaállítása. A hello **visszaállítása** panelen kattintson a **visszaállítása** tootrigger hello visszaállítási műveletet.

## <a name="restore-backed-up-disks"></a>Biztonsági másolat lemezek visszaállítása
Ha azt szeretné, hogy toocustomize hello virtuális gép szeretné toocreate a biztonsági másolat, mint jelen visszaállítási konfigurációs panelen válassza a lemezek **lemezek visszaállítása** regisztrációja, mivel értékének **visszaállítása típus**. Ez a választás kér egy tárfiókot, ahol a biztonsági mentésekből lemezek kerülnek. Ha egy tárfiókot, válassza ki a fiókot, hogy a megosztások, hello Recovery Services-tároló hello ugyanazon a helyen. Zónaredundáns tárolás fiókok nem támogatottak. Ha nem a tárfiókok hello hello és ugyanazon a helyen Recovery Services tároló, akkor létre kell hoznia egy hello visszaállítási művelet indítása előtt. hello tárolási fiók replikációtípust zárójel szerepel.

Visszaállítási művelet befejezése után végezhetők el:
* [Használjon sablont toocustomize hello visszaállítani virtuális gép](#use-templates-to-customize-restore-vm)
* [Használjon hello visszaállított lemezek tooattach tooan meglévő virtuális gépet](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Hozzon létre egy új virtuális gép PowerShell visszaállított lemezekről.](./backup-azure-vms-automation.md#restore-an-azure-vm)

A hello **konfiguráció visszaállítása** panelen kattintson a **OK** toofinalize hello konfiguráció visszaállítása. A hello **visszaállítása** panelen kattintson a **visszaállítása** tootrigger hello visszaállítási műveletet.

![Helyreállítási konfiguráció befejeződött](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-hello-restore-operation"></a>Hello visszaállítási művelet nyomon követése
Hello visszaállítási műveletet indít el, ha a biztonsági mentési szolgáltatás hello létrehoz egy feladatot az követési hello visszaállítási műveletet. biztonsági mentési szolgáltatás hello is létrehoz, és átmenetileg hello értesítést jelenít meg a portál értesítési területén. Ha nem látja hello értesítési, mindig kattinthat hello értesítések ikon tooview az értesítések.

![A visszaállítási elindítva](./media/backup-azure-arm-restore-vms/restore-notification.png)

tooview hello művelet feldolgozása közben, vagy tooview befejezése után nyissa meg a hello biztonsági mentési feladatok listáján.

1. A hello Azure menüben, kattintson **Tallózás** és a szolgáltatások hello listájának megtekintéséhez írja be **Recovery Services**. szolgáltatások listája hello beírta toowhat módosítja. Amikor látja **Recovery Services-tárolók**, válassza ki azt.

    ![Nyissa meg a Recovery Services-tároló](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    hello hello előfizetésben tárolók listája jelenik meg.

    ![Helyreállítási szolgáltatások listája tárolók](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. Hello listából válassza hello tároló társított hello visszaállított virtuális gép. Ha hello tároló gombra kattint, az irányítópult megnyitása.
3. Hello tároló irányítópult a hello **biztonsági mentési feladatok** csempére, kattintson a **Azure virtuális gépek** hello tárolóhoz társított toodisplay hello feladatok.

    ![tároló irányítópult](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    Hello **biztonsági mentési feladatok** panel nyílik meg, és hello feladatok listáját jeleníti meg.

    ![a tárolóban található virtuális gépek listájára](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="use-templates-toocustomize-restore-vm"></a>Használja a sablonok toocustomize visszaállítási vm
Egyszer [lemezek visszaállítás befejezése](#Track-the-restore-operation), visszaállítási művelet toocreate erőforrások biztonsági mentési konfiguráció vagy toocustomize neve eltér konfigurálása egy új virtuális gép részeként is használhatja a generált hello sablon Hozzon létre egy új virtuális gép visszaállítási pont létrehozása. 

> [!NOTE]
> Sablonok lemezek visszaállítása után 2017. március 1-jétől. a helyreállítási pontok részeként lesz hozzáadva. Azok a nem titkosított, és nem kezelt lemez virtuális gépek alkalmazhatók. Titkosított virtuális gépek és kezelt lemez virtuális gépek támogatása hamarosan a jövőbeli kiadásokban. 
>
>

a visszaállítási lemezek beállítás részeként létrehozott tooget hello sablon

1. Nyissa meg a megfelelő toohello feladat toorestore feladat részleteit. 
2. A hello visszaállítási feladat részletei képernyőn, kattintson a *sablon telepítése* tooinitiate sablon-üzembehelyezés gombra. 

     ![Állítsa vissza a feladat részletes](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
   
Hello telepítés sablon paneljén egyéni központi telepítés használata sablon-üzembehelyezés túl[szerkesztése és hello sablon üzembe helyezése](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) vagy hozzáfűző további testreszabásai [sablont készít](../azure-resource-manager/resource-group-authoring-templates.md) központi telepítése előtt. 

   ![sablon-üzembehelyezés betöltése](./media/backup-azure-arm-restore-vms/loading-template.png)
   
Után írja be a szükséges hello értékeket, fogadja el a hello *feltételek és kikötések* , majd kattintson a **beszerzési**.

   ![sablon-üzembehelyezés elküldése](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Visszaállítás utáni lépéseket
* Biztonsági okokból például Ubuntu, Linux-alapú felhő-init terjesztési használ, ha blokkolva van-e a jelszó visszaállítás utáni. Adja a VMAccess bővítmény a hello használata visszaállítani virtuális gép túl[jelszó-átállítási hello](../virtual-machines/linux/classic/reset-access.md). A feladás egy vagy több helyreállítási jelszó alaphelyzetbe állítása terjesztéseket tooavoid az SSH-kulcsok használatát javasoljuk.
* Bővítmények hello biztonsági mentési konfiguráció során települ, azonban nem engedélyezett. Telepítse újra a bővítmények, ha bármilyen probléma. 
* Ha hello biztonsági másolat virtuális gép van statikus IP-címe, feladás egy vagy több restore, visszaállított virtuális Géphez nem lesz a dinamikus IP-tooavoid ütközés esetén létrehozása visszaállítani a virtuális gép. További tudnivalókért, hogyan zajlik a [adja hozzá a statikus IP-toorestored méretű VM](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm)
* Visszaállított virtuális Géphez nincs rendelkezésre állási értéket. Visszaállítási lemezek beállítás használatát javasoljuk és [rendelkezésre állási csoport hozzáadása](../virtual-machines/windows/tutorial-availability-sets.md) amikor vissza a virtuális gép létrehozása a PowerShell vagy a sablonok használatával a lemezeket. 


## <a name="backup-for-restored-vms"></a>A visszaállított virtuális gépek biztonsági mentése
Ha már visszaállította VM toosame erőforráscsoport az azonos nevű eredeti biztonsági másolatba mentett virtuális gép hello, biztonsági mentés továbbra is a hello VM utáni visszaállítást. Ha visszaállította a virtuális gép tooa másik erőforráscsoport vagy egy másik nevet a visszaállított virtuális Géphez megadott rendelkezik, ez a rendszer egy új virtuális Gépet, és visszaállított virtuális Géphez szükséges toosetup biztonsági mentés.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>A virtuális gépek Azure-adatközpontban vészhelyreállítás során visszaállítása
Azure biztonsági mentés lehetővé teszi, hogy a biztonsági másolat visszaállítása virtuális gépek toohello párosított adatközpont abban az esetben hello elsődleges az adatközpont, ahol virtuális gépek futnak a feladatait katasztrófa és biztonsági mentési tároló toobe georedundáns konfigurálta. Ilyen esetben során tooselect egy tárfiókot, amely párosított adatközpontban található van szüksége, és ki hello visszaállítási folyamat ugyanaz marad. Azure biztonsági mentés párosított földrajzi toocreate hello visszaállított virtuális gépet számítási szolgáltatást használja. További információ [Azure Data center rugalmasság](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Tartomány a tartományvezérlő virtuális gépek helyreállítása
Tartományvezérlőn (DC) virtuális gépek biztonsági mentése támogatott lehetőség az Azure Backup szolgáltatással. Azonban gondot kell fordítani hello visszaállítási folyamat során. hello megfelelő helyreállítási folyamatot hello tartományi szerkezete hello függ. Hello legegyszerűbb esetben egyetlen tartományvezérlő egyetlen tartományban van. Gyakrabban üzemi terhelés esetén a fog egyetlen tartományt a több tartományvezérlők, lehet, hogy az egyes tartományvezérlők a helyszíni. Végül előfordulhat, hogy több tartomány erdő. 

Az Active Directory perspektíva hello az Azure virtuális gép olyan, mint bármely más virtuális gép modern támogatott hipervizor alkalmazáson. hello helyszíni hipervizorok a fő különbség az, hogy nincs virtuális gép konzol az Azure-ban érhető el. A konzol bizonyos forgatókönyvekben, például az operációs rendszer nélküli helyreállítást (BMR) típusú biztonsági mentéssel helyreállítása szükség. Virtuális gép visszaállítási hello biztonsági mentési tárolóból azonban a teljes operációs rendszer nélküli Helyreállítás váltja fel. Active Directory helyreállító módjának (DSRM) is rendelkezésre áll, ezért az összes Active Directory-helyreállítási forgatókönyvek életképes. Háttér kapcsolatos további információkért tekintse meg [biztonsági mentése és visszaállítása a virtualizált tartományvezérlők szempontjai](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) és [tervezése az Active Directory-erdő helyreállítási](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Egyetlen tartományvezérlő egyetlen tartományban
hello VM visszaállíthatóak (mint bármely más virtuális gép) hello Azure portál vagy a PowerShell használatával.

### <a name="multiple-dcs-in-a-single-domain"></a>Ugyanazon tartományba több tartományvezérlők
Hello keresztül elérhető ugyanabban a tartományban, más tartományvezérlők hálózati hello, hello DC lehet visszaállítani, mint bármely virtuális gép. Ha fennmaradó tartomány utolsó Tartományvezérlőjének a hello hello, vagy egy elkülönített hálózatot visszaállítás történik, egy erdő helyreállítási eljárást kell követni.

### <a name="multiple-domains-in-one-forest"></a>Több tartomány egy erdőben
Hello keresztül elérhető ugyanabban a tartományban, más tartományvezérlők hálózati hello, hello DC lehet visszaállítani, mint bármely virtuális gép. Azonban minden más esetben egy erdő helyreállítási ajánlott.

## <a name="restoring-vms-with-special-network-configurations"></a>Virtuális gépek speciális hálózati konfigurációjának visszaállítása
Ez nem lehetséges tooback fel és hello speciális hálózati konfigurációt a következő virtuális gépek a visszaállítást. Ezek a beállítások azonban keresztül hello visszaállítási folyamat során egy különös figyelmet igényel.

* A terheléselosztó (belső és külső) virtuális gépek
* Virtuális gépek több foglalt IP-cím
* Több hálózati adapterrel rendelkező virtuális gépek

> [!IMPORTANT]
> Hello speciális hálózati konfigurációt a virtuális gépek létrehozásakor a PowerShell toocreate virtuális gépek hello lemezekből vissza kell használnia.
>
>

toofully hozza létre újra hello virtuális gépek toodisk, visszaállítása után kövesse az alábbi lépéseket:

1. Hello lemezek visszaállíthatók egy helyreállítási szolgáltatások tároló használatával [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm)
2. Hello Virtuálisgép-konfiguráció szükséges a terheléselosztó létrehozása vagy több hálózati adapter/több foglalt IP használata hello PowerShell-parancsmagok és használni kívánt konfigurációról VM toocreate hello.

   * Hozzon létre virtuális Gépet a felhőszolgáltatás [belső terheléselosztó](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Hozzon létre virtuális gép tooconnect túl[internetre irányuló terheléselosztót](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * A virtuális gép létrehozása [több hálózati adapter](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * A virtuális gép létrehozása [több foglalt IP-címek](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Következő lépések
Most, hogy a virtuális gépek próbál visszaállítani, tekintse meg a hello információt a virtuális gépek gyakori hibák elhárítása. Emellett olvassa el a feladatokat a virtuális gépek kezelésére hello cikk.

* [Kapcsolatos hibák elhárítása](backup-azure-vms-troubleshoot.md#restore)
* [Virtuális gépek kezelése](backup-azure-manage-vms.md)
