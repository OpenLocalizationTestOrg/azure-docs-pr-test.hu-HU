---
title: "aaaAutomate StorSimple fájlmegosztási vész-Helyreállítási az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Hello lépéseit és ajánlott eljárások a vész-helyreállítási megoldást az üzemeltetett Microsoft Azure StorSimple tároló fájlmegosztások létrehozásához ismerteti."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/09/2017
ms.author: vidarmsft
ms.openlocfilehash: fa3e8d4e77ca0f6a7b5f9bbb956a4de12547642e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Azure Site Recovery segítségével StorSimple üzemeltetett fájlmegosztások automatizált vész-helyreállítási megoldás
## <a name="overview"></a>Áttekintés
A Microsoft Azure StorSimple hibrid felhőalapú tárolási megoldás, amely címek hello gyakran társított fájlmegosztások strukturálatlan adatok bonyolultságára. StorSimple felhőbeli tárhelyét használja az hello kiterjesztése a helyszíni megoldás, és automatikusan tiers adatok között a helyszíni és felhőalapú tárolására. Az adatvédelem integrálva van a helyi és felhőalapú pillanatfelvételek, szükségtelenné teszi hello sprawling tároló-infrastruktúra.

[Az Azure Site Recovery](../site-recovery/site-recovery-overview.md) egy Azure-alapú szolgáltatás, amely a vész-helyreállítási képességeket biztosít replikáció, feladatátvétel és helyreállítási virtuális gépek megvalósításában. Az Azure Site Recovery számos olyan replikációs technológiák tooconsistently replikálás támogat, védheti meg és zökkenőmentesen átveheti a virtuális gépek és az alkalmazások tooprivate/nyilvános vagy a szolgáltatott felhők.

Az Azure Site Recovery, a virtuálisgép-replikációt és a StorSimple felhő pillanatképkezelési funkciókat, védelmet biztosíthat a hello teljes fájl kiszolgálói környezet. A szüneteltetése hello esetben használhatja a egyetlen kattintással toobring a fájlmegosztások online az Azure-ban csak néhány perc múlva.

Ez a dokumentum részletesen ismerteti, hogyan hozhat létre egy vész-helyreállítási megoldást a StorSimple tárolási üzemeltetett fájlmegosztások és végrehajtani a tervezett, nem tervezett, és feladatátvételi tesztek használ egy kattintással helyreállítási tervet. Lényegében azt illusztrálja, hogyan módosítható a helyreállítási terv hello az Azure Site Recovery-tárolóban tooenable StorSimple feladatátvételek során vészhelyreállítási forgatókönyvek. Ismerteti továbbá támogatott konfigurációk és előfeltételek. Jelen dokumentum céljából feltételezzük, hogy jártas Azure Site Recovery és a StorSimple architektúrák hello alapjait.

## <a name="supported-azure-site-recovery-deployment-options"></a>Támogatott Azure Site Recovery telepítési lehetőségek
Az ügyfelek fizikai kiszolgálóként vagy virtuális gépek (VM) futó Hyper-V vagy VMware telepíthetők, és majd faragottnak kívül StorSimple tároló kötetekről a fájlmegosztások létrehozását. Az Azure Site Recovery megvédheti mindkét fizikai és virtuális központi telepítések tooeither tooAzure vagy másodlagos hely. Ez a dokumentum ismerteti az Azure virtuális gép futó Hyper-V fájlkiszolgáló hello helyreállítási helyként, és a StorSimple tároló fájlmegosztások vész-Helyreállítási megoldás részletei. Egyéb forgatókönyvek, mely hello fájlkiszolgálón található a virtuális gép van, a VMware virtuális gép vagy fizikai gépen hasonlóképpen kell végrehajtani.

## <a name="prerequisites"></a>Előfeltételek
A következő előfeltételek hello egy Azure Site Recovery által üzemeltetett StorSimple tároló fájlmegosztások a egy kattintással vész-helyreállítási megoldást Végrehajtási rendelkezik:

* Helyszíni Hyper-V vagy VMware vagy fizikai gépen futó virtuális gép Windows Server 2012 R2 fájl kiszolgáló
* StorSimple tárolási helyszíni Eszközkezelési regisztrálva az Azure StorSimple manager
* StorSimple felhő készülék létrehozott hello Azure StorSimple manager (ez is meg kell őrizni, leállítást állapot)
* Hello készleteiből konfigurált hello StorSimple tárolóeszközön tárolt fájlmegosztások
* [Az Azure Site Recovery services-tároló](../site-recovery/site-recovery-vmm-to-vmm.md) a Microsoft Azure-előfizetés létrehozása

Emellett, ha Azure a helyreállítási hely, futtassa a hello [Azure virtuális gép Readiness Assessment eszközt](http://azure.microsoft.com/downloads/vm-readiness-assessment/) a virtuális gépek tooensure, hogy kompatibilisek, az Azure virtuális gépek és az Azure Site Recovery services.

tooavoid késési problémák (ami a magasabb költségű), győződjön meg arról, hogy a StorSimple felhő készüléknek, automation-fiók létrehozása, és a tárfiókok hello ugyanabban a régióban.

## <a name="enable-dr-for-storsimple-file-shares"></a>StorSimple-fájlmegosztások vész-Helyreállítási engedélyezése
Minden összetevője hello helyszíni kell védett toobe tooenable befejezni a replikációt és helyreállítási környezetet. Ez a szakasz ismerteti, hogyan:

* (Opcionális) Active Directory és a DNS-replikáció beállítása
* Az Azure Site Recovery tooenable protectionben hello fájlkiszolgáló méretű VM
* A StorSimple-kötetek védelmének engedélyezése
* Hello hálózat konfigurálása

### <a name="set-up-active-directory-and-dns-replication-optional"></a>(Opcionális) Active Directory és a DNS-replikáció beállítása
Ha azt szeretné, tooprotect hello tooexplicitly kell, hogy elérhetők a hello vész-Helyreállítási helyen Active Directory és a DNS rendszert futtató gépek, a védelmüket (így hello fájlkiszolgálók hitelesítéssel átkapcsolás után érhetők el). Két módon ajánlott hello az ügyfél helyszíni környezetben hello összetettsége alapján.

#### <a name="option-1"></a>1. lehetőséget
Ha hello vevő kis számú alkalmazást, hello teljes egyetlen tartományvezérlővel rendelkezik helyszíni hely és fog kell feladatátadásának hello teljes webhelyet, akkor azt javasoljuk, Azure Site Recovery replikációs tooreplicate hello tartomány a tartományvezérlő számítógép tooa másodlagos hely (Ez az a pont-pont és a webhely-Azure alkalmazható).

#### <a name="option-2"></a>2. lehetőséget
Ha hello felhasználói alkalmazások nagy számú, az Active Directory-erdőt fut, és néhány alkalmazások keresztül egyszerre lesz sikertelen, akkor azt javasoljuk, hogy egy további tartományvezérlőt hello vész-Helyreállítási helyen beállítása (vagy egy másodlagos helyre vagy az Azure-ban).

Tekintse meg a túl[automatikus vész-Helyreállítási megoldást nyújt az Active Directory és az Azure Site Recovery segítségével DNS](../site-recovery/site-recovery-active-directory.md) útmutatást, amikor elérhetővé teszi a tartományvezérlő hello vész-Helyreállítási helyen. Ez a dokumentum hátralévő hello indulunk ki hello vész-Helyreállítási helyen rendelkezésre áll egy tartományvezérlő.

### <a name="use-azure-site-recovery-tooenable-protection-of-hello-file-server-vm"></a>Az Azure Site Recovery tooenable protectionben hello fájlkiszolgáló méretű VM
Ebben a lépésben használatához hello helyszíni fájl kiszolgálói környezet előkészítése, létrehozása és előkészítése az Azure Site Recovery tárolójából, majd engedélyezi hello VM fájl védelmét.

#### <a name="tooprepare-hello-on-premises-file-server-environment"></a>tooprepare hello helyszíni fájl kiszolgálói környezet
1. Set hello **felhasználói fiókok felügyelete** túl**nincs értesítés**. Ez azért szükséges, hogy az Azure automation parancsfájlok tooconnect hello iSCSI-tárolók után sikertelen Azure Site Recovery által használható.

   1. Nyomja le az ENTER hello Windows billentyű + Q, és keresse meg a **UAC**.
   2. Válassza ki **módosítás felhasználói fiókok felügyelete beállításainak**.
   3. Eszközterület-toohello felfelé húzza hello **nincs értesítés**.
   4. Kattintson a **OK** majd **Igen** megjelenésekor.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png)
2. Virtuálisgép-ügynök hello hello fájlkiszolgáló virtuális gépeket telepíthet. Ez azért szükséges, hogy az Azure automation-parancsfájlok futtathatók a virtuális gépek a feladatátvételt hello.

   1. [Hello-ügynök letöltése](http://aka.ms/vmagentwin) túl`C:\\Users\\<username>\\Downloads`.
   2. Nyissa meg a Windows PowerShell rendszergazdai módban (Futtatás rendszergazdaként), és írja be a következő parancs toonavigate toohello letöltési helye hello:

      `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

      > [!NOTE]
      > hello fájlnév hello verziójától függően változhatnak.
      >
      >
3. Kattintson a **Tovább** gombra.
4. Fogadja el a hello **feltételeket a szerződés** majd **következő**.
5. Kattintson a **Befejezés** gombra.
6. StorSimple tárolási kívül faragottnak köteteket használó fájlmegosztásokat létrehozni. További információkért lásd: [hello StorSimple Manager szolgáltatás toomanage köteteket](storsimple-manage-volumes.md).

   1. A helyszíni virtuális gépeken, nyomja le az ENTER hello Windows billentyű + Q, és keresse meg a **iSCSI**.
   2. Válassza ki **iSCSI-kezdeményező**.
   3. Jelölje be hello **konfigurációs** fülre, és másolja hello kezdeményező neve.
   4. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
   5. Jelölje be hello **StorSimple** fülre, majd válassza ki hello StorSimple Manager szolgáltatás, amely tartalmazza a hello fizikai eszköz.
   6. Kötet tárolója létrehozása, és majd a kötetek létrehozása. (Ezek a kötetek szolgálnak hello fájl megosztásainak hello fájlkiszolgáló virtuális gépeken). Másolja a hello kezdeményező neve, és nevezze el egy megfelelő hozzáférés-vezérlési rekordokat hello a hello kötetek létrehozásakor.
   7. Jelölje be hello **konfigurálása** lapra, és jegyezze fel a hello eszköz hello IP-címét.
   8. A helyszíni virtuális gépeken, lépjen a toohello **iSCSI-kezdeményező** újra és írja be a hello IP hello gyors csatlakozás szakasz. Kattintson a **gyors csatlakozás** (hello eszköz most kell csatlakoztatni).
   9. Nyissa meg hello Azure-portálon, és válassza hello **kötetek és eszközök** fülre. Kattintson a **automatikus konfigurálása**. újonnan létrehozott hello kötet megjelenjen-e.
   10. Hello portálon, válassza ki a hello **eszközök** fülre, majd jelölje **hozzon létre egy új virtuális eszközt.** (A virtuális eszköz alkalmazva lesznek, ha a feladatátvételt hajt végre). Az új virtuális eszköz is meg kell őrizni, az offline állapotban tooavoid további költségek. tootake hello virtuális eszköz offline állapotban, lépjen toohello **virtuális gépek** hello Portal a szakaszt, és leállítani.
   11. Lépjen vissza toohello a helyszíni virtuális gépeket, és nyissa meg a Lemezkezelés (nyomja meg a hello Windows billentyű + X, és válassza ki **Lemezkezelés**).
   12. Megfigyelheti, hogy néhány további lemezek (attól függően, hogy létrehozott kötetek hello száma). Kattintson a jobb gombbal hello első, válassza a **lemez inicializálása**, és válassza ki **OK**. Kattintson a jobb gombbal hello **Unallocated** szakaszban jelölje be **új egyszerű kötet**, rendelje hozzá meghajtóbetűjelhez és hello varázsló befejezéséhez.
   13. Az összes hello lemez ismételje meg a l. Most már megtekintheti az összes hello lemez **ez PC** a Windows Explorer hello.
   14. Hello fájl- és tárolási szolgáltatások szerepkör toocreate fájlmegosztások használja ezeken a köteteken.

#### <a name="toocreate-and-prepare-an-azure-site-recovery-vault"></a>toocreate és készítse elő az Azure Site Recovery-tároló
Tekintse meg a toohello [Azure Site Recovery dokumentáció](../site-recovery/site-recovery-hyper-v-site-to-azure.md) tooget lépések az Azure Site Recovery védelmének hello fájlkiszolgáló virtuális gép.

#### <a name="tooenable-protection"></a>tooenable védelme
1. Válassza le a hello iSCSI cél(ok) hello a helyszíni virtuális gépek, amelyet az Azure Site Recovery segítségével tooprotect:

   1. Nyomja le a Windows billentyű + Q, és keresse meg a **iSCSI**.
   2. Válassza ki **iSCSI-kezdeményező beállítása**.
   3. Válassza le a korábban csatlakoztatott hello StorSimple eszközt. Alternatív megoldásként válthat ki hello fájlkiszolgáló néhány percig a védelem engedélyezésekor.

   > [!NOTE]
   > Ennek hatására hello fájl megosztások toobe átmenetileg nem érhető el.
   >
   >
2. [A virtuális gép védelmének engedélyezése](../site-recovery/site-recovery-hyper-v-site-to-azure.md) hello fájlkiszolgáló VM hello Azure Site Recovery portálról.
3. Hello kezdeti szinkronizálás kezdődik, amikor újra hello cél újra. Nyissa meg az iSCSI-kezdeményező toohello, válassza ki a StorSimple eszköz hello, és kattintson **Connect**.
4. Hello szinkronizálás befejeztével, miután hello hello virtuális gép állapota **védett**, válassza ki a virtuális gép hello, válassza ki a hello **konfigurálása** lapot, és ennek megfelelően frissülnek hello hello VM-hálózata (Ez a hello hálózati a feladatátvételt a virtuális gép van hello fog tartozni). Ha hello hálózati nem jelenik meg, az azt jelenti, hogy hello szinkronizálási van még mindig folyamatban.

### <a name="enable-protection-of-storsimple-volumes"></a>A StorSimple-kötetek védelmének engedélyezése
Ha nincs kiválasztva hello **engedélyezése ehhez a kötethez alapértelmezett biztonsági mentés** választás, hello StorSimple-köteteket, nyissa meg túl**biztonsági mentési házirendek** hello a StorSimple Manager szolgáltatás, és megfelelő biztonsági mentés létrehozása a házirend az összes hello kötet. Azt javasoljuk, hogy állítsa a biztonsági mentések toohello helyreállításipont-célkitűzés (RPO), hogy szeretné-e hello alkalmazás toosee hello gyakoriságát.

### <a name="configure-hello-network"></a>Hello hálózat konfigurálása
Hello fájlkiszolgáló virtuális gép, hálózati beállítások konfigurálása a Azure Site Recovery, amelyek a Virtuálisgép-hálózatok hello csatolt toohello megfelelő vész-Helyreállítási hálózati feladatátvételt követően.

Hello VM igény szerint a hello **replikált elemek** tooconfigure hello hálózati beállításai lapon a hello a következő ábrán látható módon.

![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása
Az ASR tooautomate hello feladatátvételi folyamat hello fájlmegosztások helyreállítási tervet is létrehozhat. A szüneteltetése akkor fordul elő, ha van lehetősége hello fájlmegosztások csupán egyetlen kattintással néhány perc múlva. tooenable ezt az automatizálást szüksége lesz egy Azure automation-fiók.

#### <a name="toocreate-an-automation-account"></a>toocreate Automation-fiók
1. Nyissa meg az Azure portál toohello &gt; **Automation** szakasz.
2. Kattintson a **+ Hozzáadás** gomb, megnyílik a panel alatt.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)

   * Name,-adja meg az új automation-fiók
   * Előfizetés - előfizetés kiválasztása
   * Resource group - új/válasszon meglévő erőforráscsoport létrehozása
   * Hely - hely kiválasztásához, tartsa hello azonos földrajzi vagy régió mely hello a StorSimple felhő készüléket és a Storage-fiókok létrejöttek.
   * Hozzon létre válassza ki az Azure-beli futtató fiók - **Igen** lehetőséget.

3. Nyissa meg toohello Automation-fiókot, kattintson a **Runbookok** &gt; **Tallózás gyűjtemény** összes hello tooimport szükséges runbookokat hello automation figyelembe.
4. Adja hozzá a runbookok keresse meg a következő hello **vész-helyreállítási** címke hello gyűjteményben:

   * StorSimple-köteteket a teszt feladatátvételi (TFO) után tisztítása
   * Feladatátvételi StorSimple kötettárolók
   * Csatlakoztassa a köteteket a StorSimple eszközön a feladatátvételt követően
   * Távolítsa el az egyéni parancsprogramok futtatására szolgáló bővítmény Azure virtuális gépen
   * Indítsa el a StorSimple virtuális készülék

     ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)

5. Összes hello parancsfájl közzététele hello automation-fiókban hello runbook kiválasztásával, és kattintson a **szerkesztése** &gt; **közzététel** , majd **Igen** toohello ellenőrzése üzenet. Ez a lépés után hello **Runbookok** lapon fog megjelenni az alábbiak szerint:

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)

6. Hello automation-fiókban, válassza ki a hello **eszközök** lapon &gt; kattintson **változók** &gt; **változó hozzáadása** , és adja hozzá a következő változók hello. Választhat tooencrypt ezeknek az eszközöknek. Ezek a változók a helyreállítási terv-specifikus. Ha a helyreállítási terv (amely a következő lépésben hello létrehozhat) neve TestPlan, akkor a változók csal, TestPlan-StorSimRegKey TestPlan-AzureSubscriptionName és így tovább.

   * *RecoveryPlanName***- StorSimRegKey**: hello regisztrációs kulcsot az hello StorSimple Manager szolgáltatás.
   * *RecoveryPlanName***- AzureSubscriptionName**: hello hello Azure-előfizetés nevét.
   * *RecoveryPlanName***- ResourceName**: hello hello StorSimple eszköz StorSimple erőforrást hello nevét.
   * *RecoveryPlanName***- DeviceName**: hello-eszköz toobe átadja a feladatokat.
   * *RecoveryPlanName***- VolumeContainers**: kötettárolók vesszővel elválasztott karakterlánc megtalálható, amelyeket nem sikerült a több mint, például toobe hello eszköz volcon1, volcon2, volcon3.
   * *RecoveryPlanName***- TargetDeviceName**: hello StorSimple felhő készülék tárolók mely hello toobe szerepelnek a feladatátvételt.
   * *RecoveryPlanName***- TargetDeviceDnsName**: hello céleszköz hello szolgáltatás nevét (Ez hello található **virtuális gép** szakasz: hello szolgáltatás neve van hello ugyanaz, mint hello DNS-név).
   * *RecoveryPlanName***- StorageAccountName**: hello tárfiók mely hello parancsfájlban szereplő neve (amely a hello toorun feladatátadása megtörtént VM) tárolja. A storage-fiók, amely ideiglenesen rendelkezik némi lemezterület toostore hello parancsfájl is lehet.
   * *RecoveryPlanName***- StorageAccountKey**: hello fent tárfiók hello a hozzáférési kulcsot.
   * *RecoveryPlanName***- ScriptContainer**: hello tároló mely hello parancsfájl tárolva lesznek hello felhőben hello neve. Ha hello tároló nem létezik, a rendszer létrehozza.
   * *RecoveryPlanName***- VMGUIDS**: után a virtuális gépek védelméhez, Azure Site Recovery minden virtuális gép egyedi-Azonosítót rendel hozzá, amelyek a virtuális gép a feladatátvételt hello hello részletezi. tooobtain hello VMGUID, jelölje be hello **Recovery Services** fülre, és kattintson **védett elem** &gt; **védelmi csoportok** &gt; **Gépek** &gt; **tulajdonságok**. Ha több virtuális géphez, majd adja hozzá hello GUID egy vesszővel elválasztott karakterlánc.
   * *RecoveryPlanName***- AutomationAccountName** – hello hello forgatókönyve és eszköze hello hozzáadta, amelyben hello automation-fiók nevét.

  Például akkor, ha hello hello helyreállítási terv neve nem fileServerpredayRP, akkor a **hitelesítő adatok** & **változók** lapon meg kell jelennie az alábbiak szerint az összes hello eszköz hozzáadása után.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

7. Nyissa meg toohello **Recovery Services** szakasz és a korábban létrehozott válassza hello Azure Site Recovery-tárolóban.
8. Jelölje be hello **helyreállítási tervek (helyreállítás)** parancsát **kezelése** csoportban, és hozzon létre új helyreállítási terv az alábbiak szerint:

   a.  Kattintson a **+ a helyreállítás terv** gomb, megnyílik a panel alatt.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)

   b.  Adja meg a helyreállítási terv nevét, válassza ki a forrás, a cél és a központi telepítési modell értékeket.

   c.  Válassza ki a hello virtuális gépek hello védelmi csoportból, amelyet az tooinclude hello helyreállítási tervet, és kattintson a **OK** gombra.

   d.  Jelölje ki azt a korábban létrehozott helyreállítási terv **Testreszabás** tooopen hello helyreállítási terv testreszabási megtekintése gombra.

   e.  Kattintson a jobb gombbal **összes csoportok leállítási** kattintson **előtti művelet hozzáadása**.

   f.  Megnyitja Insert művelet panelen, adjon meg egy nevet, válassza ki **elsődleges ügyféloldali** ahol toorun lehetőséget, válassza ki az Automation-fiók (hozzá runbookokat hello), és válassza a hello beállítása  **Feladatátvétel-StorSimple-kötet-tárolók** runbook.

   g.  Kattintson a jobb gombbal **csoport 1: Start** kattintson **Hozzáadás védett elemek** lehetőséget, majd jelölje ki a hello virtuális gépek, amelyek hello helyreállítási tervet, és kattintson a védett toobe **Ok** gombra. Nem kötelező, ha már van kiválasztva a virtuális gépek.

   h.  Kattintson a jobb gombbal **csoport 1: Start** kattintson **művelet utáni** lehetőséget, majd adja hozzá a következő parancsfájlok összes hello:

   * Runbook elindítása-StorSimple-virtuális-készülék
   * Over-StorSimple-kötet-tárolók runbook sikertelen
   * Runbook csatlakoztatási-kötetek-után-feladatátvétel
   * Távolítsa el – egyéni-parancsfájl-kiterjesztés runbook

   i.  A manuális műveletet hozzáadása után ugyanaz a hello parancsfájlok hello fent 4 **csoport 1: utáni lépéseket** szakasz. Ez a művelet akkor hello pontot, amellyel ellenőrizheti, hogy minden helyesen működik. Ez a művelet csak feladatátvételi teszt részeként hozzáadott toobe kell (így csak select hello **feladatátvételi teszt** jelölőnégyzet).

   j.  Hello manuális műveletet, miután hozzáadása hello **tisztítás** parancsfájl használatával ugyanazt az eljárást, a hello hello más runbookokat. **Mentés** hello helyreállítási terv.

    > [!NOTE]
    > Feladatátvételi teszt futtatásakor Ellenőrizze minden hello a manuális műveletet lépésben mert kellett lett klónozása hello céleszközön hello StorSimple-köteteket törli a hello karbantartás részeként hello manuális művelet befejeződése után.
    >

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Feladatátvételi teszt végrehajtása
Tekintse meg a toohello [Active Directory vész-Helyreállítási megoldás](../site-recovery/site-recovery-active-directory.md) útmutatója a szempontok adott tooActive Directory hello teszt feladatátvétele során. hello helyszíni beállítása nem zavarják minden hello a feladatátvételi teszt esetén. StorSimple-köteteket lenne csatlakoztatva hello toohello a helyszíni virtuális gép klónozott toohello Azure StorSimple felhő készülék. Tesztelési célból egy virtuális Gépet az Azure-ban nem válik, és hello klónozott kötetek csatolt toohello virtuális gép.

#### <a name="tooperform-hello-test-failover"></a>tooperform hello feladatátvételi teszthez
1. Hello Azure-portálon válassza ki a site recovery-tárolóban.
2. Kattintson a létrehozott hello fájlkiszolgáló VM hello helyreállítási terv.
3. Kattintson a **feladatátvételi teszt**.
4. Válassza ki a hello Azure-beli virtuális hálózat toowhich Azure virtuális gépek csatlakoznak feladatátvételt követően.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
5. Kattintson a **OK** toobegin hello feladatátvételi. Előrehaladásának parancsával hello VM tooopen tulajdonságát, vagy a hello **tesztfeladat feladatátvételt** a tároló neve &gt; **feladatok** &gt; **SiteRecovery-feladatok**.
6. Hello feladatátvétel befejezése után meg kell jelennie toosee hello replika Azure machine hello Azure-portálon jelenik meg &gt; **virtuális gépek**. Végezheti el az érvényesítést.
7. Miután hello érvényesítést végzett, kattintson **érvényesítést teljes**. Ez lesz tisztítás hello StorSimple-köteteket és leállítási hello StorSimple felhő készüléket.
8. Amikor elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. Megjegyzések rekord, és mentse el hello kapcsolatos megfigyelések feljegyzéséhez a feladatátvételi teszt. Ezzel a lépéssel törli a hello virtuális gép teszt feladatátvétele során létrehozott.

## <a name="perform-a-planned-failover"></a>Végezzen el egy tervezett feladatátvételt
   Egy tervezett feladatátvétel során a helyszíni hello fájlkiszolgáló VM szabályosan leállítása és a StorSimple eszköz hello kötetek biztonsági mentési pillanatképet készít felhő. StorSimple-köteteket hello toohello virtuális eszköz, a replika virtuális gép állapotba kerül a Azure-feladatátvétel történt és hello kötetek csatolt toohello virtuális gép.

#### <a name="tooperform-a-planned-failover"></a>a tervezett feladatátvétel tooperform
1. Hello Azure-portálon, válassza ki **helyreállítási szolgáltatások** tároló &gt; **helyreállítási tervek (helyreállítás)** &gt; **recoveryplan_name** létre hello fájlkiszolgáló virtuális gép.
2. Hello helyreállítási terv paneljén kattintson **további** &gt; **tervezett feladatátvétel**.  

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
3. A hello **tervezett feladatátvétel megerősítése** panelen válassza ki a hello forrás és a célhelyek és válassza ki a cél hálózati, és kattintson a hello négyzet ikon ✓ toostart hello feladatátvételi folyamat.
4. A replika virtuális gépek létrehozása után is a Függőben állapotba. Kattintson a **véglegesítése** toocommit hello feladatátvételi.
5. Replikáció befejezése után hello virtuális gépek indítása hello másodlagos helyen.

## <a name="perform-a-failover"></a>Végezzen el egy feladatátvételt
Egy nem tervezett feladatátvétel során toohello virtuális eszköz, a replika VM kerül a Azure-feladatátvétel történt hello StorSimple-köteteket és hello kötetek csatolt toohello virtuális gép.

#### <a name="tooperform-a-failover"></a>a feladatátvétel tooperform
1. Hello Azure-portálon, válassza ki **helyreállítási szolgáltatások** tároló &gt; **helyreállítási tervek (helyreállítás)** &gt; **recoveryplan_name** létre hello fájlkiszolgáló virtuális gép.
2. Hello helyreállítási terv paneljén kattintson **további** &gt; **feladatátvételi**.  
3. A hello **megerősítéséhez feladatátvétel** panelen hello forrás kiválasztása, és a cél helyét.
4. Válassza ki **virtuális gépek leállítása és hello legfrissebb adatok szinkronizálása** toospecify, hogy a Site Recovery próbálja tooshut le hello védett virtuális gépet és szinkronizálás hello hello hello adatok legújabb verziója lesz a feladatátvételt.
5. Hello feladatátvétel után hello virtuális gép állapota függőben van. Kattintson a **véglegesítése** toocommit hello feladatátvételi.


## <a name="perform-a-failback"></a>A feladat-visszavételt végrehajtani
A feladat-visszavétel során StorSimple kötettárolók feladatátvétel történt hátsó toohello fizikai eszköz után a biztonsági mentés történik.

#### <a name="tooperform-a-failback"></a>a feladat-visszavétel tooperform
1. Hello Azure-portálon, válassza ki **helyreállítási szolgáltatások** tároló &gt; **helyreállítási tervek (helyreállítás)** &gt; **recoveryplan_name** létre hello fájlkiszolgáló virtuális gép.
2. Hello helyreállítási terv paneljén kattintson **további** &gt; **tervezett feladatátvétel**.  
3. Válassza ki a hello forrása és célja helyek, a megfelelő adatszinkronizálási válassza hello és a virtuális gép létrehozásának beállításai.
4. Kattintson a **OK** toostart hello feladat-visszavétel folyamat gombra.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>Ajánlott eljárások
### <a name="capacity-planning-and-readiness-assessment"></a>A kapacitás tervezésével és a készültségi értékelés
#### <a name="hyper-v-site"></a>Hyper-V-hely
Használjon hello [felhasználói kapacitás planner eszköz](http://www.microsoft.com/download/details.aspx?id=39057) toodesign hello server, a tároló és a hálózati infrastruktúra, a Hyper-V replika környezetnek.

#### <a name="azure"></a>Azure
Hello futtatása [Azure virtuális gép Readiness Assessment eszközt](http://azure.microsoft.com/downloads/vm-readiness-assessment/) a virtuális gépek tooensure, hogy kompatibilisek, az Azure virtuális gépek és az Azure Site Recovery Services. hello Készültségfelmérő eszköz ellenőrzi a Virtuálisgép-konfigurációk, és figyelmeztetést küld, ha a konfiguráció nem kompatibilisek az Azure-ral. Például kapcsolatos figyelmeztetés Ha a C: meghajtó 127 GB-nál nagyobb.

Kapacitástervezés alkotják, legalább két fontos folyamat:

* Leképezési helyszíni Hyper-V virtuális gépek tooAzure Virtuálisgép-méretek (például A6, A7, A8 és a9-es).
* Internetes sávszélesség hello meghatározása szükséges.

## <a name="limitations"></a>Korlátozások
* Jelenleg csak az 1 StorSimple eszköz feladatátvételre (tooa egyetlen StorSimple felhő készülék). egy fájlkiszolgáló több StorSimple eszközt magában foglaló hello forgatókönyv jelenleg nem támogatott.
* Ha a virtuális gépek védelmének engedélyezésekor hibaüzenetet kap, győződjön meg arról, hogy a kapcsolat bontása hello iSCSI-tárolók.
* Az összes, egy biztonsági mentési házirendek kötettárolók átnyúlva miatt csoportban hello kötet tároló átvétele fog megtörténni együtt.
* Minden kiválasztott hello kötettárolók hello kötet átvétele fog megtörténni.
* Toomore egyezzen, mint 64 TB-ot nem sikertelen keresztül, mert egy egyetlen StorSimple felhő készülék hello maximális kapacitását 64 TB-os köteteket.
* Ha hello tervezett/nem tervezett feladatátvétel sikertelen lesz, és hello virtuális gépek jönnek létre az Azure-ban, majd végezzen nem tisztítása hello virtuális gépeket. Ehelyett hajtsa végre egy feladat-visszavételre. Ha törli a hello virtuális gépek majd hello a helyszíni virtuális gépek újra nem kapcsolható be.
* Egy feladatátvétel után Ha nem sikerül toosee hello köteteket, nyissa meg toohello virtuális gépeket, nyissa meg a Lemezkezelés, hello lemezek újraellenőrzése és majd kapcsolásuk.
* Bizonyos esetekben hello meghajtóbetűjelek hello vész-Helyreállítási hely eltérhet hello betűket helyszíni. Ilyen esetben szüksége lesz toomanually megfelelő hello probléma hello feladatátvétel befejezése után.
* A multi-factor authentication hello Azure automation-fiók hello eszközként megadott hitelesítő le kell tiltani. A hitelesítés nem le van tiltva, ha a parancsfájlok nem engedélyezett toorun automatikusan, és hello helyreállítási terv sikertelen lesz.
* Feladatátvételi feladat időtúllépése: hello StorSimple parancsfájl időtúllépést okoz, ha kötettárolók hello feladatátvétele hello Azure Site Recovery felső határ az egyes parancsfájl (jelenleg 120 perc) több időt vesz igénybe.
* Biztonsági mentési feladat időtúllépése: hello StorSimple parancsfájlt időtúllépés Ha hello biztonsági mentése a kötetek hello Azure Site Recovery felső határ az egyes parancsfájl (jelenleg 120 perc) több időt vesz igénybe.

  > [!IMPORTANT]
  > Futtassa manuálisan hello biztonsági mentés hello Azure-portálon, és futtassa újból a hello helyreállítási terv.

* Klónozza a feladat időtúllépése: hello StorSimple parancsfájlt időtúllépés történik, ha tovább tart a kötetek klónozása hello mint hello Azure Site Recovery felső határ az egyes parancsfájl (jelenleg 120 perc).
* Szinkronizálási hiba ideje: hello StorSimple parancsfájlok hibák meg arról, hogy hello biztonsági mentése sikertelen volt-e annak ellenére, hogy hello biztonsági mentés sikeres hello portálon. Ennek oka lehet, hogy hello StorSimple készülék idő lehet szinkronban hello hello időzónában aktuális idő.

  > [!IMPORTANT]
  > Hello készülék szinkronizáláskor hello az aktuális idő hello időzónában.

* Készülék feladatátvételi hiba: hello StorSimple parancsfájl sikertelen lehet, ha van egy készülék feladatátvételt, ha a hello helyreállítási terv fut-e.

  > [!IMPORTANT]
  > Futtassa újra a hello helyreállítási terv hello készülék feladatátvételi befejeződése után.


## <a name="summary"></a>Összefoglalás
Azure Site Recovery segítségével, a fájlkiszolgáló virtuális gép teljes automatizált vészhelyreállítási tervet hozhat létre a StorSimple tárolón tárolt fájlmegosztások rendelkező. Hello feladatátvételi bárhonnan másodpercen belül is kezdeményezhető a hello eseményeket, a szüneteltetése és hello alkalmazás első lépéseivel, néhány perc múlva.
