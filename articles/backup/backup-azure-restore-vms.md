---
title: "a virtuális gépek biztonsági másolatból aaaRestore |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestore egy Azure virtuális géphez a helyreállítási pont"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "biztonsági másolat visszaállításával lehetséges; Hogyan toorestore; helyreállítási pont;"
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: trinadhk; jimpark;
ms.openlocfilehash: 44c25f3248784accd1c2beaabb2c9a4dca3232d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-virtual-machines-in-azure"></a>Virtuális gépek visszaállítása az Azure-ban
> [!div class="op_single_selector"]
> * [Állítsa vissza a virtuális gépek Azure-portálon](backup-azure-arm-restore-vms.md)
> * [Állítsa vissza a virtuális gépek a klasszikus portál](backup-azure-restore-vms.md)
>
>

Egy virtuális gép tooa lépések új virtuális Gépet a következő hello egy Azure Backup-tárolóban tárolt hello biztonsági másolatból állítsa vissza.

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> **2017. október 15**, akkor nem fog tudni toouse PowerShell toocreate mentési tárolókban. <br/> **2017. november 1-től kezdődően**:
>- A többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="restore-workflow"></a>Munkafolyamat visszaállítása
### <a name="step-1-choose-an-item-toorestore"></a>1. lépés: Az elem toorestore kiválasztása
1. Keresse meg a toohello **védett elemek** lapra, és válassza ki a hello toorestore tooa kívánt virtuális gép új virtuális Gépet.

    ![Védett elemek](./media/backup-azure-restore-vms/protected-items.png)

    Hello **helyreállítási pont** hello oszlopa **védett elemek** lap megtudhatja, hogy hello egy virtuális géphez a helyreállítási pontok száma. Hello **legújabb helyreállítási pont** oszlop mutatja meg hello hello legutóbbi biztonsági másolat készítésének idején, amelyből a virtuális gépek állíthatók vissza.
2. Kattintson a **visszaállítása** tooopen hello **visszaállítani az elemet** varázsló.

    ![Állítsa vissza egy elemet](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>2. lépés: Válassza ki a helyreállítási pont létrehozása
1. A hello **válasszon ki egy helyreállítási pontot** képernyő, visszaállíthatja hello legújabb helyreállítási pontról, vagy az egy korábbi időpontra időben. hello Amikor megnyílik a varázsló alapértelmezett beállítással van *legújabb helyreállítási pont*.

    ![Válasszon ki egy helyreállítási pontot](./media/backup-azure-restore-vms/select-recovery-point.png)
2. toopick egy korábbi időpontbeli időpontban, válassza ki a hello **dátum kiválasztása** hello legördülő lehetőséget, majd válassza ki a dátum a havinaptár-vezérlőben hello hello kattintva **naptár ikonra**. Hello vezérlőben minden dátumra, amelyek rendelkeznek a helyreállítási pontok egy világos szürke árnyalatát kitöltődnek és hello felhasználó által választható.

    ![Dátum kiválasztása](./media/backup-azure-restore-vms/select-date.png)

    Miután hello havinaptár-vezérlőben dátum gombra kattint, a hello helyreállítási pontok érhető el, hogy megjelenik-e a dátum a helyreállítási pontok az alábbi táblázatban. Hello **idő** oszlop, mely hello során létrehozott pillanatképen hello idejét jelzi. Hello **típus** oszlopában láthatók hello [konzisztencia](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) hello helyreállítási pont. hello táblázatfejlécet rendelkezésre álló helyreállítási pontok hello számát mutatja meg az adott napon zárójelek között.

    ![Helyreállítási pontok](./media/backup-azure-restore-vms/recovery-points.png)
3. Válassza ki a hello helyreállítási pontot a hello **helyreállítási pontok** táblázatban, majd kattintson a hello tovább nyílra toogo toohello következő képernyőn.

### <a name="step-3-specify-a-destination-location"></a>3. lépés: Adja meg a kívánt rendeltetési helyet
1. A hello **válassza ki a példány visszaállítása** képernyőn adja meg, ahol toorestore hello virtuális gép adatait.

   * Adja meg a virtuális gép neve hello: egy adott felhőalapú szolgáltatás, a hello virtuális gép nevének egyedinek kell lennie. Túlzott írása a meglévő virtuális gép nem támogatott.
   * Válasszon egy felhőalapú szolgáltatást a virtuális gép hello: Ez az megadása kötelező a virtuális gép létrehozása. Válasszon tooeither használjon egy meglévő felhőalapú szolgáltatást, vagy hozzon létre egy új felhőalapú szolgáltatás.

        Bármilyen felhőszolgáltatás neve nek globálisan egyedinek kell lennie. Általában hello felhőszolgáltatás neve lekérdezi egy nyilvánosan elérhető URL-címet [cloudservice] hello formájában társítva. cloudapp.net. Azure nem lehetővé teszi egy új felhőalapú szolgáltatás toocreate Ha hello neve már használatban van. Ha úgy dönt, toocreate új felhőalapú szolgáltatás, akkor lesz azonos nevet hello virtuális gépként – a mely eset hello VM hello megadott kivételezett neve alkalmazott elég egyedi toobe tartozó toohello felhőszolgáltatás kell lennie.

        Jelenleg csak jelenítse meg a felhőalapú szolgáltatásokat, és virtuális hálózatokat, amelyek nem kapcsolódnak az hello affinitás csoportok visszaállítása példány részletei. [További](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. Válasszon egy tárfiókot, a virtuális gép hello: Ez az kötelező hello virtuális gép létrehozásához. Igény szerint a meglévő tárfiókok a hello azonos régióban legyen, mint a hello Azure Backup-tárolóban. Storage-fiókok, amelyek a zóna redundáns vagy a prémium szintű storage típusa nem támogatott.

    Támogatott konfigurációval nem tárfiókok esetén hozzon létre egy tárfiókot, a támogatott konfigurációkra előzetes toostarting visszaállítási műveletet.

    ![Válasszon virtuális hálózatot](./media/backup-azure-restore-vms/restore-sa.png)
3. Válasszon olyan virtuális hálózatot: hello virtuális gép létrehozásának hello időpontban kell kiválasztani a hello VM hello virtuális hálózathoz (VNET). hello állítsa vissza a felhasználói felület megjelenítése használhat az előfizetésen belüli összes hello Vnetek. Megadása nem kötelező tooselect egy VNETET az hello visszaállítani a virtuális gép – keresztül képes tooconnect toohello visszaállított virtuális gépet fog hello internet akkor is, ha hello virtuális hálózat nem alkalmazható.

    Hello felhőalapú szolgáltatás a kiválasztott akkor társítva van egy virtuális hálózatot, majd hello virtuális hálózat nem módosítható.

    ![Válasszon virtuális hálózatot](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. Válassza ki az alhálózatot: abban az esetben hello VNET rendelkezik alhálózatokkal, alapértelmezés szerint hello első alhálózat lesz kiválasztva. Az Ön által választott hello alhálózati hello legördülő lehetőségek közül választhat. Az alhálózati adatokat, nyissa meg a bővítmény tooNetworks hello [portál kezdőlapján](https://manage.windowsazure.com/), nyissa meg túl**virtuális hálózatok** és select hello virtuális hálózati és a konfigurálás toosee alhálózati részleteinek részletezés.

    ![Válasszon alhálózatot](./media/backup-azure-restore-vms/select-subnet.png)
5. Kattintson a hello **Submit** hello varázsló toosubmit hello részleteit, és hozzon létre egy visszaállítási feladat.

## <a name="track-hello-restore-operation"></a>Hello visszaállítási művelet nyomon követése
Miután bemeneti hello helyreállítása varázsló összes hello adatait és elküldve az Azure Backup szolgáltatás megpróbál toocreate feladat tootrack hello a visszaállítási művelet.

![Helyreállítási feladat létrehozása](./media/backup-azure-restore-vms/create-restore-job.png)

Ha hello feladat létrehozása sikeres, megjelenik egy bejelentési értesítés arról, hogy hello feladat jön létre. Hello kattintva kaphat további részleteket **feladat megtekintése** gomb, amellyel a rendszer ekkor túl**feladatok** fülre.

![A visszaállítási feladat létrehozása](./media/backup-azure-restore-vms/restore-job-created.png)

Hello visszaállítási művelet befejeződése után lesz megjelölve a befejezettként **feladatok** fülre.

![A visszaállítási feladat befejezése](./media/backup-azure-restore-vms/restore-job-complete.png)

Után szükség lehet a meglévő telepítés toore hello bővítmények a virtuális gép helyreállítása hello hello az eredeti virtuális gép és [hello végpontok módosítása](../virtual-machines/windows/classic/setup-endpoints.md) hello virtuális gép hello Azure-portálon.

## <a name="post-restore-steps"></a>Visszaállítás utáni lépéseket
Ha például Ubuntu, Linux-alapú felhő-init terjesztési biztonsági okokból használ, jelszó le lesz tiltva visszaállítás utáni. Adja a VMAccess bővítmény a hello használata visszaállítani virtuális gép túl[jelszó-átállítási hello](../virtual-machines/linux/classic/reset-access.md). A feladás egy vagy több helyreállítási jelszó alaphelyzetbe állítása terjesztéseket tooavoid az SSH-kulcsok használatát javasoljuk.

## <a name="backup-for-restored-vms"></a>A visszaállított virtuális gépek biztonsági mentése
Ha már visszaállította hello azonos nevű eredeti biztonsági másolatba mentett virtuális gép virtuális gép toosame felhőszolgáltatás, biztonsági mentés továbbra is a hello VM utáni helyreállítás. Ha visszaállította a virtuális gép tooa másik felhőalapú szolgáltatást vagy más nevet a visszaállított virtuális Géphez megadott rendelkezik, ennek tekintendők egy új virtuális Gépet, és visszaállított virtuális Géphez szükséges toosetup biztonsági mentés.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>A virtuális gépek helyreállítása Azure DataCenter vészhelyreállítás során
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

<!--- WK: this following original supportability statement is incorrect, taking it out.
hello challenge arises because DSRM mode is not present in Azure. So toorestore such a VM, you cannot use hello Azure portal. hello only supported restore mechanism is disk-based restore using PowerShell.

> [!WARNING]
> For Domain Controller VMs in a multi-DC environment, do not use hello Azure portal for restore! Only PowerShell based restore is supported
>
>

Read more about hello [USN rollback problem](https://technet.microsoft.com/library/dd363553) and hello strategies suggested toofix it.
--->

## <a name="restoring-vms-with-special-network-configurations"></a>Virtuális gépek speciális hálózati konfigurációjának visszaállítása
Azure biztonsági mentés a következő speciális hálózati konfigurációk a virtuális gépek biztonsági mentését is támogatja.

* A terheléselosztó (belső és külső) virtuális gépek
* Virtuális gépek több foglalt IP-cím
* Több hálózati adapterrel rendelkező virtuális gépek

Ezek a konfigurációk engedélyezhetik a következő szempontok őket visszaállítása közben.

> [!TIP]
> Használjon PowerShell-alapú folyamat toorecreate hello speciális hálózati konfiguráció visszaállítása a virtuális gépek post visszaállítás.
>
>

### <a name="restoring-from-hello-ui"></a>A felhasználói felület hello visszaállítása:
A felhasználói Felületről, visszaállítása közben **mindig válasszon új felhőszolgáltatást**. Vegye figyelembe, hogy mivel portál csak akkor kötelező paraméterek visszaállítási folyamat, virtuális gépek visszaállítani a felhasználói felület használata során elvész a hello speciális hálózati konfiguráció rendelkeznek. Ez azt jelenti, a helyreállítás virtuális gépek lesz normál virtuális gépek nélkül a terheléselosztót vagy több hálózati adapter vagy több foglalt IP-cím.

### <a name="restoring-from-powershell"></a>A PowerShell visszaállítása:
PowerShell hello képességét toojust hello virtuális gépek lemezei állítsa vissza biztonsági másolatból, és hozza létre hello virtuális gép van. Ez akkor hasznos, ha a fent említett speciális hálózati konfigurációk igénylő virtuális gépek visszaállításakor.

Ahhoz, toofully hozza létre újra a hello virtuális gép post lemezek visszaállítása, kövesse az alábbi lépéseket:

1. A mentési tároló hello lemezek visszaállítása [Azure biztonsági mentési PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)
2. Hello virtuális gép konfigurációs szükséges terheléselosztó létrehozása vagy több hálózati adapter/több foglalt IP használata hello PowerShell-parancsmagok és használni kívánt konfigurációról VM toocreate hello.

   * Hozzon létre virtuális Gépet a felhőszolgáltatás [belső terheléselosztó](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Hozzon létre virtuális gép tooconnect túl[internetre irányuló terheléselosztót](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * A virtuális gép létrehozása [több hálózati adapter](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * A virtuális gép létrehozása [több foglalt IP-címek](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Következő lépések
* [Kapcsolatos hibák elhárítása](backup-azure-vms-troubleshoot.md#restore)
* [Virtuális gépek kezelése](backup-azure-manage-vms.md)
