---
title: vissza az Azure tooVMware aaaHow toofail |} Microsoft Docs
description: "A virtuális gépek tooAzure feladatátvétel után a feladat-visszavétel toobring virtuális gépek hátsó tooon helyszíni is kezdeményezhető. Ismerje meg, hogyan toofail biztonsági hello lépéseit."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: b82abf6b15db9dccab49edbd14298b121e9fdc6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-from-azure-tooan-on-premises-site"></a>A helyszíni hely Azure tooan ból

Ez a cikk ismerteti, hogyan toofail biztonsági másolatot a virtuális gépek Azure virtuális gépek toohello a helyszíni helyről. Ez a cikk toofail hello utasításait kövesse a VMware virtuális gépeket vagy windowsos/Linuxos fizikai kiszolgálók biztonsági után már átvette azokat a hello helyszíni hely tooAzure hello segítségével [replikálása VMware virtuális gépek és fizikai az Azure Site Recovery kiszolgálók tooAzure](site-recovery-vmware-to-azure-classic.md) oktatóanyag.

> [!WARNING]
> Ha rendelkezik [áttelepítésével kész](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), hello áthelyezett virtuális gép tooanother erőforrás csoport vagy törölt hello Azure virtuális gép, ezt követően a feladat-visszavétel nem lehet.

> [!NOTE]
> Ha VMware virtuális gépek átvette majd nem feladat-visszavétel tooa Hyper-v gazdagépen.

## <a name="overview-of-failback"></a>Feladat-visszavétel áttekintése
Ez feladat-visszavétel működése. Miután tooAzure átvette, néhány lépésben nem hátsó tooyour helyszíni hely:

1. [Lássa el újból védelemmel](site-recovery-how-to-reprotect.md) hello Azure virtuális gépeket, hogy a helyszíni hely tooreplicate tooVMware virtuális gépek indítása. Ez a folyamat részeként szükség:
    1. Egy helyszíni fő célkiszolgáló beállítása: Windows fő célkiszolgáló Windows virtuális gépek és [Linuxos fő célkiszolgáló](site-recovery-how-to-install-linux-master-target.md) Linux virtuális gépekhez.
    2. Állítson be egy [folyamatkiszolgáló](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Kezdeményezzen [lássa el újból védelemmel](site-recovery-how-to-reprotect.md). Ezzel hello a helyszíni virtuális gép kikapcsolása és szinkronizálása hello Azure virtuális gép adatait a hello a helyi lemezek.
5. Miután a virtuális gépek Azure-on tooyour helyszíni helyre replikál, elindította egy sikertelen keresztül Azure toohello a helyszíni helyről.

Után az adatok visszaállítása sikertelen volt, akkor állítsa hello a helyszíni virtuális gépek vissza a megbukott, hogy tooreplicate tooAzure elindítja.

A gyors áttekintését tekintse meg a következő videó bemutatja, hogyan toofail keresztül Azure tooan a helyszíni hely hello.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="fail-back-toohello-original-or-alternate-location"></a>Hátsó toohello eredeti helyére vagy máshová sikertelen

VMware virtuális gépek feladatátvételt, ha ugyanazon adatforrás a helyszíni virtuális gép, még ha létezik hátsó toohello sikertelen lehet. Ebben a forgatókönyvben csak hello változások lesznek replikálva vissza. Ebben a forgatókönyvben az eredeti helyre történő helyreállítást nevezik. Ha hello a helyszíni virtuális gép nem létezik, a hello forgatókönyv, egy másodlagos hely helyreállítását.

> [!NOTE]
> Beállíthatja, hogy csak a feladat-visszavétel toohello eredeti vCenter és a konfigurációs kiszolgáló. Nem telepíthet egy új konfigurációs kiszolgáló és a feladat-visszavétel használja azt. Is hogy nem adható hozzá egy új vCenter toohello a meglévő konfigurációs kiszolgáló és a feladat-visszavétel hello új vCenter.

#### <a name="original-location-recovery"></a>Eredeti helyre történő helyreállítás

Ha nem hátsó toohello eredeti virtuális gép, a következő feltételek hello szükség:
* Ha a hello virtuális gép vCenter-kiszolgáló kezeli, majd hello fő célkiszolgáló ESX-gazdagép szükséges hozzáférési toohello virtuális gép adattároló.
* Ha hello virtuális gép ESX-gazdagépen, de vCenter, nem felügyeli, majd adattárolót hello virtuális gép hello merevlemezén kell lennie, hogy hello fő célkiszolgáló gazdagép el tudja érni.
* Ha a virtuális gép ESX-gazdagépen, és nem használja a vCenter, majd el kell végeznie az ESX-gazdagép hello hello fő cél felderítése ahhoz, hogy lássa el újból védelemmel. Ez igaz, ha a feladat-visszavétele vissza fizikai kiszolgálók túl.
* Sikertelen lehet, hátsó tooa virtuális tárolóhálózat (vSAN) vagy egy lemezt, amely a nyers eszköz-hozzárendelési (RDM), ha hello lemezek már létezik és csatlakoztatott toohello a helyszíni virtuális gép alapján.

#### <a name="alternate-location-recovery"></a>Helyreállítás másik helyre
Ha a hello a helyszíni virtuális gép nem létezik előtt hello virtuális gép újbóli védelméhez, hello forgatókönyv neve egy másik helyre történő helyreállítást. hello védelem-újrabeállítási munkafolyamat újra létrehozza a hello a helyszíni virtuális gépet. Ennek hatására a teljes adatok letöltése.

* Ha nem sikerül hátsó tooan másik helyre, a hello virtuális gép lesz-e a helyreállított toohello azonos ESX-gazdagépet, mely hello fő célkiszolgálón telepítve van. hello által használt toocreate hello lemez datastore lesz hello ugyanazon adattár hello virtuális gép újbóli védelméhez kiválasztott.
* Vissza csak tooa virtuális gép fájl system (virtuálisgép-Fájlrendszereinek) datastore sikertelen lehet. Ha van, egy vsan-hoz vagy RDM, védelem-újrabeállítási és a feladat-visszavétel nem fog működni.
* Védelem-újrabeállítási magában foglalja egy nagy kezdeti adatátvitelt, amely hello módosításokat követi. Ez a folyamat létezik, mert hello virtuális gép nem létezik a helyszínen. hello teljes adatoknak kell toobe replikálja vissza. A védelem-újrabeállítási veszik az eredeti helyre történő helyreállítás több időt is.
* Nem utasíthat el vissza toovSAN vagy RDM-alapú lemezek. Új virtuálisgép-lemezeket (VMDKs) csak a virtuálisgép-Fájlrendszereinek adattárolót hozhatók létre.

A fizikai gép, ha a tooAzure, a feladatátvételt is sikertelen biztonsági csak VMware virtuális gépként (is hivatkozott tooas, P2A2V). Ez a folyamat hello másik helyre történő helyreállítást hatálya alá tartozik.

* Windows Server 2008 R2 SP1 fizikai kiszolgálók, ha a védett, és átadja a feladatokat tooAzure, nem sikerült vissza.
* Győződjön meg arról, hogy a legalább egy fő kiszolgálón és hello szükséges ESX/ESXi gazdagépek toowhich toofail vissza kell felderíteni.

## <a name="have-you-completed-reprotection"></a>Elvégezte a ismételt védelem?
Mielőtt továbblépne, teljes hello lássa el újból védelemmel lépéseket, hogy hello virtuális gépek replikált állapotban van, és a feladatátvételi hátsó tooan helyszíni hely is kezdeményezhető. További információkért lásd: [hogyan tooreprotect az Azure tooon helyszíni](site-recovery-how-to-reprotect.md).

## <a name="prerequisites"></a>Előfeltételek

* Ha így tesz, a feladat-visszavétel a konfigurációs kiszolgáló helyi szükséges. A feladat-visszavétel során hello virtuális gép léteznie kell a hello konfigurációs kiszolgáló adatbázisában, vagy nem sikerült a feladat-visszavétel. Így győződjön meg arról, hogy a rendszeresen ütemezett biztonsági mentések a kiszolgáló is. Hiba történt egy olyan vészhelyzet esetén, ha szüksége lesz a toorestore hello kiszolgáló hello feladat-visszavétel toowork azonos IP-címet.
* Ahhoz, hogy kiváltsa feladat-visszavétel hello fő célkiszolgáló nem rendelkezhet összes pillanatképet.

## <a name="steps-toofail-back"></a>Vissza a lépéseket toofail

> [!IMPORTANT]
> Feladat-visszavétel elindítása előtt győződjön meg arról, hogy végrehajtotta-e a hello virtuális gépek ismételt védelem. hello virtuális gépek védett állapotban kell lennie, és meg kell az állapotukat **OK**. tooreprotect hello virtuális gépek, olvassa el [hogyan tooreprotect](site-recovery-how-to-reprotect.md).

1. A hello replikált elemek lapon válasszon hello virtuális gép, majd kattintson a jobb gombbal az tooselect **nem tervezett feladatátvétel**.
2. A **megerősítéséhez feladatátvétel**, ellenőrizze a hello feladatátvételi irányát (az Azure-ból), és válassza a hello helyreállítási pont (legújabb vagy hello legújabb alkalmazáskonzisztens), amelyet az toouse hello feladatátvételhez. hello alkalmazás konzisztens pont mögött hello legújabb pont időben, ami adatvesztést.
3. A feladatátvétel során a Site Recovery hello virtuális gépeket az Azure leáll. Után ellenőrizheti, hogy feladat-visszavétel a várt módon befejeződött, ellenőrizheti, hogy hello virtuális gépek Azure-be lett zárva.

### <a name="toowhat-recovery-point-can-i-fail-back-hello-virtual-machines"></a>helyreállítási pont toowhat vagyok meghiúsulhat hátsó hello virtuális gépek?

A feladat-visszavétel során, hogy két beállítások toofail hátsó hello virtuális gép vagy helyreállítási terv.

Ha legújabb feldolgozott hello pont időben választ, minden virtuális gép időben tootheir legújabb elérhető pont átvétele fog megtörténni. Ha egy replikációs csoport helyreállítási terv hello belül van, majd hello replikációs csoport minden egyes virtuális gép hajt végre feladatátvételt tooits független legújabb pont időben.

Nem utasíthat el újra a virtuális gépek amíg rá nem kényszerül legalább egy helyreállítási pontot. Nem utasíthat el újra a helyreállítási terv mindaddig, amíg az összes virtuális gép rendelkezik legalább egy helyreállítási pontot.

> [!NOTE]
> A legutóbbi helyreállítási pontot az összeomlás-konzisztens helyreállítási pontot.

Ha hello alkalmazáskonzisztens helyreállítási pontnak, egyetlen virtuális gép feladat-visszavétel tooits legutóbbi elérhető alkalmazáskonzisztens helyreállítási pontot állítja helyre. A replikációs csoport helyreállítási tervet hello esetben minden replikációs csoport tooits közös elérhető helyreállítási pontot állítja helyre.
Ne feledje, hogy időben alkalmazáskonzisztens helyreállítási pontokra lehet mögött, előfordulhat adatvesztés adatokat.

### <a name="what-happens-toovmware-tools-post-failback"></a>Mi történik, tooVMware eszközök utáni feladat-visszavétel?

Feladatátvételi tooAzure során hello Azure virtuális gép hello VMware-eszközök nem futnak. Egy Windows rendszerű virtuális gép esetén a ASR hello VMware-eszközök letiltja a feladatátvétel során. Linux virtuális gép esetén az ASR hello VMware-eszközök eltávolítása a feladatátvétel során.

Hello Windows virtuális gép feladat-visszavétel során hello VMware-eszközök újbóli engedélyezéséig a feladat-visszavétel. Ehhez hasonlóan a linux virtuális gép hello VMware-eszközök újratelepíti hello gépen feladat-visszavétel során.

## <a name="next-steps"></a>Következő lépések

Feladat-visszavétel befejezése után toocommit kell helyreállítani a virtuális gépek Azure-ban hello virtuális gép tooensure törlődnek.

### <a name="commit"></a>Véglegesítés
Érvényesítés szükséges tooremove hello átadja a feladatokat az Azure-ból a virtuális gép.
Kattintson a jobb gombbal a védett hello elemet, és kattintson **véglegesítése**. A feladat eltávolítja az Azure virtuális gépek a feladatátvételt hello.

### <a name="reprotect-from-on-premises-tooazure"></a>Állítsa a helyszíni tooAzure

Véglegesítési befejezése után a virtuális gép vissza a hello helyszíni helyen, de azt nem lehet védetté tenni. toostart tooreplicate tooAzure újra, hello a következő:

1. A **tároló** > **beállítás** > **replikált elemek**, válassza ismét sikertelen, és kattintson a virtuális gépek hello  **Ismételt védelemmel**.
2. Adjon meg, amelyet a használt toobe toosend adatok hátsó tooAzure hello folyamatkiszolgáló hello értékének.
3. Kattintson a **OK** toobegin hello védelem-újrabeállítási feladat.

> [!NOTE]
> Miután egy helyszíni virtuális gép elindul, csak bizonyos idő hello ügynök tooregister hátsó toohello konfigurációs kiszolgáló (felfelé too15 perc). Ebben az időszakban állítsa be az sikertelen lesz, és értéket ad eredményül, hibaüzenetet kap, amelyet hello ügynöke nincs telepítve. Várjon néhány percet, és próbálkozzon újra az védelem-újrabeállítási.

Miután hello lássa el újból védelemmel feladat befejeződik, hello virtuális gép replikálódik hátsó tooAzure és elvégezhető a feladatátvétel.

## <a name="common-issues"></a>Gyakori problémák
Győződjön meg arról, hogy hello vCenter csatlakoztatott állapotban van, a feladat-visszavétel előtt. Ellenkező esetben lemez leválasztása és hátsó toohello csatolva a virtuális gép sikertelen lesz.
