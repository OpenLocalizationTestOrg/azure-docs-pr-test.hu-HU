---
title: "a Site Recovery tárolójából tooan Azure Recovery Services-tároló aaaUpgrade"
description: "Ismerje meg, hogyan tooupgrade egy Azure Site Recovery-tárolóban tooa Recovery Services tároló"
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/31/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: a18a923ee3bad91873e654c9b9b5bf8b83acc123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-site-recovery-vault-tooan-azure-resource-manager-based-recovery-services-vault"></a>Frissítse a Site Recovery tárolójából tooan Azure Resource Manager-alapú Recovery Services-tároló

Ez a cikk ismerteti, hogyan tooupgrade Azure Site Recovery-tárolók tooAzure helyreállítási szolgáltatás Resource Manager-alapú tárolók a folyamatban lévő replikáció gyakorolt hatás nélkül. További információ az Azure erőforrás-kezelő szolgáltatásait és előnyeit: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Bevezetés
Recovery Services-tároló egy Azure Resource Manager-erőforrás natív módon hello felhőalapú biztonsági mentés és katasztrófa helyreállítási kezeléséhez. Egy egységes tárolóban, melyekkel hello új Azure-portálon, és azt a felváltja hello klasszikus biztonsági mentés és a Site Recovery-tárolók.

Helyreállítási szolgáltatások tárolók funkciót, beleértve a tömbje kínál:

* Az Azure Resource Manager támogatása: védeni, és az Azure Resource Manager-verembe feladatátvételt a virtuális gépek és fizikai gépek.

* Lemez kizárása: Ha ideiglenes fájlokat vagy nagy forgalom adatokat, hogy nem szeretné, hogy toouse a megfelelő sávszélesség, kötetek lehet kizárni a replikálásból. Ez a funkció jelenleg engedélyezve van *VMware tooAzure* és *Hyper-V tooAzure* és ki van bővítve tooother forgatókönyvek is.

* Támogatja a premium és helyileg redundáns tárolás: kiszolgálók most védelméhez a prémium szintű storage fiókokat, amelyek lehetővé teszik a felhasználói tooprotect alkalmazások magasabb bemeneti/kimeneti műveletek száma másodpercenként (IOPS). Ez a funkció be van kapcsolva *VMware tooAzure*.

* Zökkenőmentes élményt-bevezető: hello fokozott-bevezető élmény úgy tervezték, könnyen toomake vész-helyreállítási beállítása.

* Biztonsági mentés és helyreállítás management hello azonos tárolóban: most vész-helyreállítási kiszolgálók védelme vagy készítsen biztonsági mentést a hello azonos tárolóban, terhet jelentősen csökkentheti a felügyeleti.

Frissített hello élmény és szolgáltatásokkal kapcsolatban további információkért lásd: hello [tároló, biztonsági mentési és helyreállítási blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Következő funkciói

* Ne legyen hatással a folyamatban lévő replikáció: folyamatban lévő replikáció során minden megszakadása nélkül, és frissítés utáni.

* További költség nélkül: egy frissített képességek teljes készlete beolvasása minden további költség nélkül.

* Adatvesztés nélküli: mivel ezt a folyamatot a frissítés és az áttelepítés nem, a meglévő replikációs helyreállítási pontok és beállításai változatlanok maradnak során, és hello frissítés után.


## <a name="what-happens-during-hello-vault-upgrade"></a>Mi történik, hello tároló frissítés során?

Hello frissítés közben nem hajtható végre műveletek, például egy új kiszolgáló vagy a replikáció a virtuális gép (VM). Műveletek, például az adatok olvasása vagy írása adatok toohello tárolóban, például a védett elemek toohello tároló, folyamatban lévő replikáció megszakításmentes folytatásához.

### <a name="changes-tooautomation-and-tooling-after-hello-upgrade"></a>Módosítások tooautomation és tooling hello frissítés után
Hello tároló típus hello klasszikus üzembe helyezési modell toohello Resource Manager üzembe helyezési modellben frissít, hello meglévő automation vagy tooling tooensure, hogy az továbbra is toowork hello frissítés után frissítse.

### <a name="prepare-your-environment-for-hello-upgrade"></a>Hello frissítéshez a környezet előkészítése

* [Telepítse a PowerShell vagy a verziófrissítésre azt tooversion 5-ös vagy újabb](https://www.microsoft.com/download/details.aspx?id=50395)
* [Hello Azure PowerShell MSI legfrissebb verziójának telepítése](https://github.com/Azure/azure-powershell/releases)
* [Hello Recovery Services-tároló frissítési parancsprogram letöltése](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Előfeltételek
a Site Recovery tooupgrade tárolók tooAzure helyreállítási szolgáltatás Resource Manager-alapú tárolók, hello követelményeknek kell megfelelniük:

* Minimális verziója: hello Azure Site Recovery Provider a kiszolgálón telepítve kell lennie 5.1.1700.0 vagy újabb.

* Támogatott konfiguráció: a tároló nem lehet konfigurálni a tárolóhálózat (SAN) vagy SQL Server AlwaysOn rendelkezésre állási csoportok. Minden más konfigurációk vannak támogatva.

    >[!NOTE]
    >Hello frissítés után tárolási leképezése csak a PowerShell segítségével is kezelheti.

* Támogatott központi telepítési forgatókönyv: A tároló nem lehet hello *VMware tooAzure* a hagyományos telepítési modellt. Mielőtt továbblépne, először helyezze át a toohello továbbfejlesztett üzembe helyezési modellben.

* A felhasználó által kezdeményezett nem aktív feladatok, például a felügyeleti műveletek sík: mivel az access toohello felügyeleti vezérlősík korlátozott frissítés során, hajtsa végre a felügyeleti vezérlősík műveletet indít el a hello frissítése előtt. Ez a folyamat nem tartalmazza a folyamatban lévő replikáció.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Ez a frissítés hatással van a folyamatban lévő replikáció?**

Nem. A folyamatban lévő replikáció továbbra is folyamatos, során, és hello frissítés után.

**Mi történik, toonetwork beállításokat, például a pont-pont VPN és IP-beállítások?**

hello frissítés nem érinti hello hálózati beállításait. Az összes Azure-– helyi kapcsolat változatlanok maradnak.

**Toomy tárolók mi történik, ha nem tervezze meg a jövőben közelében hello tooupgrade?**

Hello régi Azure-portálon a Site Recovery-tároló támogatása szeptember 2017 indítása megszűnnek. Határozottan javasoljuk, hogy hello frissítési funkció toomove toohello új portált használja.

**Mi az áttelepítési terv jelent a saját meglévő eszközt használunk erre?**  

A tooling toohello Resource Manager üzembe helyezési modellben frissítése az egyik hello figyelembe kell venni a frissítési csomagok a legfontosabb módosításait. Recovery Services-tárolók hello hello Resource Manager üzembe helyezési modellben alapulnak. 

**Mennyi ideig nem hello felügyeleti-vezérlősík állásidő utolsó?**

hello frissítés általában körülbelül 15 too30 percet vesz igénybe, és eltarthat tooa legfeljebb egy óra.

**I vissza lehet vonni a frissítés után?**

Nem. FIM hello erőforrások sikeresen frissítette a rollback utasítás nem támogatott.

**Azt is szeretnék ellenőrzi az előfizetés vagy az erőforrások toosee e tudja frissíteni?**

Igen. Hello platform által támogatott frissítési lehetőség hello frissítés hello első lépése, hogy hello erőforrások képesek-e a frissítésre toovalidate. Hello érvényesítés meghiúsul, ha megfelelő hibaüzeneteket és figyelmeztetéseket fog kapni.

**Hogyan jelenthetem a problémát hello frissítést?**

Esetleges hibákat tapasztal hello frissítés során, vegye figyelembe a hello műveleti azonosító szerepel-e hello hiba. Microsoft Support proaktív hello probléma megoldását fog működni. Forduljon a hello támogatási csapatával az előfizetés-azonosító, a tároló neve és a művelet azonosítója. Támogatási tooresolve hello probléma minél gyorsabban fog működni. Nem próbálja meg újra hello művelet csak a kifejezetten utasításai toodo Igen, a Microsoft által.

## <a name="run-hello-script"></a>Hello parancsfájl futtatása

A PowerShellben futtassa a következő parancs hello:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* Előfizetés-azonosító: hello hello tárolóban, amely a frissítés társított előfizetés-azonosító.

* VaultName: hello tároló, amely a frissítés hello nevére.

* Helye: hello tárolóban, amely a frissítés hello helye.

* Indítása. ResourceType: A Site Recovery HyperVRecoveryManagerVault tárolók.

* TargetResourceGroupName: hello erőforráscsoport, amelybe hello elhelyezett tároló toobe frissítve. TargetResourceGroupName lehet egy meglévő erőforráscsoportot az Azure Resource Manager vagy egy újat. Ha hello megadott TargetResourceGroupName nem létezik, létrejön hello verziófrissítés részeként a hello azonos hello tárolóval helyen. További információ a hello "Erőforráscsoportok" című szakaszában talál [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md#resource-groups).

    >[!NOTE]
    >Tulajdonos toocertain megkötések erőforrás csoport elnevezési. tooprevent tároló frissítése sikertelen, gondosan kell, hogy tooobserve hello elnevezési konvenció.
    >
    >Példa:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - SubscriptionId 1234-54123-354354-56416-8645 - VaultName gen2dr-helyre "Észak-Európa" - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc

Másik lehetőségként a következő parancsfájl hello is futtathatja. Adja meg a hello értékeket hello kötelező paraméterekhez.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for hello following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. PowerShell parancsfájl hello meg tooenter a hitelesítő adatok megadását kéri. Kétszer meg kell adnia őket, egyszer hello klasszikus telepítési modell fiókhoz tartozó, egyszer pedig hello Azure Resource Manager-fiókot.

2. Után a megadott hitelesítő adatait, hello parancsfájl egy ellenőrzés toodetermine fut, hogy az infrastruktúra-beállítás megfelel-e hello azt már korábban említettük követelmények.

3. Miután hello Előfeltételek be van jelölve, és megerősítette, hello tároló frissítést felszólító tooproceed áll. hello frissítési folyamat elindul, a tároló frissítése. hello teljes frissítés 15 too30 perc toocomplete is igénybe vehet.

4. Miután hello frissítése sikeresen befejeződött, hello frissített tároló hello új Azure-portálon végezheti el.

## <a name="post-upgrade-vault-management"></a>A frissítés utáni tároló kezelése

### <a name="replicate-by-using-azure-site-recovery-in-hello-recovery-services-vault"></a>A Recovery Services-tároló hello Azure Site Recovery segítségével replikálása

* Most egy régió tartozik tooanother védheti az Azure virtuális gépeken. További információkért lásd: [Azure virtuális gépek replikálása az Azure Site Recovery régiók közötti](site-recovery-azure-to-azure.md).

* VMware virtuális gépek tooAzure replikálása kapcsolatos további információkért lásd: [Site Recovery szolgáltatással replikálja a VMware virtuális gépek tooAzure](vmware-walkthrough-overview.md).

* Replikálása (VMM nélkül) a Hyper-V virtuális gépek tooAzure kapcsolatos további információkért lásd: [replikálásához a Hyper-V virtuális gépek (VMM nélkül) tooAzure](hyper-v-site-walkthrough-overview.md).

* Hyper-V virtuális gépek (VMM) tooAzure replikálása kapcsolatos további információkért lásd: [replikálásához a Hyper-V virtuális gépek használata a Site Recovery VMM-felhők tooAzure hello Azure-portálon](vmm-to-azure-walkthrough-overview.md).

* További információ a Hyper-virtuális gépek (VMM) tooa másodlagos helyre replikál: [replikálásához a Hyper-V virtuális gépek a VMM felhők tooa másodlagos VMM hely használatával hello Azure-portálon](site-recovery-vmm-to-vmm.md).

* További információ a VMware virtuális gépek tooa másodlagos helyre replikál: [replikálja a helyszíni VMware virtuális gépek vagy fizikai kiszolgálók tooa másodlagos hely a klasszikus Azure portálon hello](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>A replikált elemek megtekintése

hello következő kép bemutatja hello Recovery Services tároló irányítópult megjelenítő lapon kulcs entitások hello tároló. hello tárolóban, védett entitások listájának tooview válasszon **Site Recovery** > **replikált elemek**.


![Replikált elemek](./media/upgrade-site-recovery-vaults/replicateditems.png)

hello következő kép bemutatja a replikált elemek és hello megtekintésére hello munkafolyamat **feladatátvételi** parancsot a feladatátvétel kezdeményezése.

![Replikált elemek](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>A replikációs beállítások megtekintése

Hello Site Recovery-tárolóban mindegyik védelmi csoporthoz úgy van konfigurálva, a Másolás gyakoriságát, a helyreállítási pontok megőrzésének ideje, a alkalmazás alkalmazáskonzisztens pillanatképek gyakorisága és a más replikációs beállítások. Replikációs házirend hello Recovery Services-tároló, ezek a beállítások vannak konfigurálva. hello házirend hello neve az üdvözlő védelmi csoportot vagy hello hello név *primarycloud_Policy*.

További információ a replikációs házirendhez: [VMware tooAzure replikációs házirendjének kezeléséhez](site-recovery-setup-replication-settings-vmware.md).
