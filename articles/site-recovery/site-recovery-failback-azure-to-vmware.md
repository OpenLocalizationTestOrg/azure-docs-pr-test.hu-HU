---
title: "az Azure tooon helyszíni VMware virtuális gépek aaaFailback |} Microsoft Docs"
description: "További információk a hátsó toohello helyszíni hely hiányában a VMware virtuális gépek és fizikai kiszolgálók tooAzure feladatátvételt követően."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 5a47337f-89a9-43e8-8fdc-7da373c0fb0f
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: ruturajd
ms.openlocfilehash: 258f5a55252083135b2040e5a235fa1ffbf3b9d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-toohello-on-premises-site"></a>Sikertelen a hátsó VMware virtuális gépek és fizikai kiszolgálók toohello helyszíni hely


Ez a cikk ismerteti, hogyan toofailback Azure virtuális gépek Azure toohello a helyszíni helyről. Hello utasítások itt készen toofail állításakor a VMware virtuális gépeket vagy windowsos vagy Linuxos fizikai kiszolgálók után újból védetté a használó gépekhez [hivatkozás](site-recovery-how-to-reprotect.md).

>[!NOTE]
>Ha a klasszikus Azure portálon hello használ, tekintse meg az említett tooinstructions [Itt](site-recovery-failback-azure-to-vmware-classic.md) továbbfejlesztett VMware tooAzure architektúra és [Itt](site-recovery-failback-azure-to-vmware-classic-legacy.md) hello örökölt architektúra

## <a name="overview"></a>Áttekintés
hello ebben a szakaszban diagramokon hello feladat-visszavétel architektúra ehhez a forgatókönyvhöz.

Hello Folyamatkiszolgálót a helyszíni és az Azure ExpressRoute-kapcsolatot használ, amikor az architektúrák használatára:

![ExpressRoute architektúra diagramja](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Ha Azure, és hogy a Folyamatkiszolgáló hello rendelkeznek, VPN vagy ExpressRoute kapcsolat, a architektúrák használatára:

![VPN-architektúra ábrája](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Portok és hello feladat-visszavétel architektúra diagramja teljes listájáért tekintse meg a kép a következő toohello:

![Feladatátvétel-feladat-visszavétel összes portok listája](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Miután tooAzure már feladatátvételt, három lépésben nem hátsó tooyour helyszíni hely:

* **1. szakasz**: hello Azure virtuális gépek állítsa az, hogy elindítja a helyszíni hely futó hátsó toohello VMware virtuális gépek replikálása.
* **2. szakaszra**: követően Azure virtuális gépeken, a replikált tooyour helyszíni hely, a feladatátvételi toofail vissza futtassa az Azure-ból.
* **3. szakaszra**: követően az adatok visszaállítása sikertelen volt, állítsa hello a helyszíni virtuális gépek, a sikertelen biztonsági, így azok tooAzure replikálást indítani.

### <a name="fail-back-toohello-original-location-or-an-alternate-location"></a>Sikertelen hátsó toohello eredeti helyre vagy másik helyre
Miután a rendszer átadja a VMware virtuális gép, hátsó toohello sikertelen lehet azonos forrás virtuális gép, ha még létezik a helyszíni. Ebben a forgatókönyvben csak hello eltérések sikertelen vissza.

Fizikai kiszolgálók feladatátvételt, ha feladat-visszavétel mindig-e tooa új VMware virtuális gép. Mielőtt sikertelen vissza a fizikai gépen, vegye figyelembe, hogy:
* A védett fizikai gépek fog visszatérhet virtuális gépként keresztül újból az Azure tooVMware sikertelen. Windows Server 2008 R2 SP1 fizikai gépen, ha a védett, és átadja a feladatokat tooAzure, nem sikerült vissza. A Windows Server 2008 R2 SP1 gépek, fut-e, a helyszíni virtuális gép lesz képes toofail vissza.
* Győződjön meg arról, hogy a fő célkiszolgáló legalább egy felderíteni hello együtt, hogy kell-e toofail szükséges ESX/ESXi-gazdagépek vissza.

Ha Ön nem hátsó toohello eredeti VM hello következők szükségesek:

* Ha hello VM egy vCenter-kiszolgáló felügyeli, hello fő célkiszolgáló ESX-gazdagép kell hozzáférés toohello virtuális gépek adattároló.
* Ha hello VM ESX-gazdagépen, de nem felügyeli a vCenter, hello merevlemezén hello VM kell hello MT a gazdagép által elérhető adattárolót.
* Ha a virtuális Gépet az ESX-gazdagépen, és nem használja a vCenter, el kell végeznie az ESX-gazdagép hello a fő Célkiszolgáló hello felderítése ahhoz, hogy lássa el újból védelemmel. Ez igaz, ha a feladat-visszavétele vissza fizikai kiszolgálók túl.
* Egy másik lehetőség (ha hello a helyszíni virtuális gép létezik) toodelete Ez a feladat-visszavétel előtt. Feladat-visszavétel majd hoz létre egy új virtuális gép hello azonos, hello fő ESX-gazdagép üzemeltetésére.

Ha nem hátsó tooan másik helyre, hello adatai helyreállított toohello ugyanazon adattár és hello hello a helyszíni fő célkiszolgáló által használt azonos ESX-gazdagépet.

## <a name="prerequisites"></a>Előfeltételek
* toofail vissza VMware virtuális gépek és fizikai kiszolgálók, VMware környezetre van szükség. Hátsó tooa hiányában a fizikai kiszolgáló nem támogatott.
* toofail vissza kell létrehozott Azure-hálózat védelmi kezdeti beállításakor. Feladat-visszavétel hello Azure VPN- vagy ExpressRoute kapcsolat van szüksége, amelyben hello Azure virtuális gépek hálózati található toohello helyszíni hely.
* Ha hello toofail kívánt virtuális gépek biztonsági tooare vCenter-kiszolgáló felügyeli, győződjön meg arról, hogy rendelkezik-e a vCenter-kiszolgáló a virtuális gépek felderítése hello szükséges engedélyekkel. További információkért lásd: [replikálása VMware virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
* Ha a pillanatképek a virtuális gép jelen, ismételt védelem sikertelen lesz. Hello pillanatképekkel vagy hello lemezek törlése.
* Ahhoz, hogy feladat-visszavételt, hozza létre ezeket az összetevőket:
  * **A folyamat-kiszolgáló létrehozása az Azure-ban**. Ez az összetevő egy Azure virtuális gép létrehozása és feladat-visszavétel során tovább futnak. Hello virtuális gép feladat-visszavétel befejezése után törölheti.
  * **Hozzon létre egy fő célkiszolgáló**: hello fő célkiszolgáló adatokat küld és fogad feladat-visszavételre. hello felügyeleti kiszolgáló helyszíni létrehozott el a fő célkiszolgálón, amely alapértelmezés szerint telepítve van. Azonban attól függően, hogy nem sikerült visszaírt forgalom mennyiségét hello, szükség lehet egy külön fő célkiszolgáló toocreate feladat-visszavételre.
  * További fő célkiszolgálón futó Linux, toocreate beállítása hello Linux virtuális gép hello fő célkiszolgáló, később leírtak telepítése előtt.
* A konfigurációs kiszolgálón szükséges a helyszíni, ha így tesz, a feladat-visszavétel. A feladat-visszavétel során hello virtuális gép konfigurációs adatbázishoz hello léteznie kell. Hello konfigurációs adatbázishoz nincs a virtuális Gépet tartalmaz, ha a feladat-visszavétel nem lehet sikeres. Ezért győződjön meg arról, hogy elvégezte-e a kiszolgáló rendszeres ütemezett biztonsági mentések. Egy olyan vészhelyzet esetén toorestore kell hello azonos IP-cím így a feladat-visszavétel működik együtt.
* Hello disk.enableUUID=true beállítani **konfigurációs paraméterek** hello fő cél a VMware virtuális gép. Ha a sor nem létezik, adja hozzá. A beállítás nem szükséges tooprovide egy egységes univerzálisan egyedi azonosítóval (UUID) toohello virtuálisgép-lemez (VMDK) fájlt, hogy megfelelően csatlakoztatva van.
* Vegye figyelembe a "fő célkiszolgáló nem lehet tárolási vMotioned" feltétel, ami hello feladat-visszavétel toofail okozhat. virtuális gép hello nem kapja meg, mivel hello lemezek nem érhető el tooit.
* Adjon hozzá egy meghajtót, alakzatot hello fő célkiszolgáló adatmegőrzési meghajtó nevezik. Adjon hozzá egy lemezt, és formázza hello meghajtót.

## <a name="failback-policy"></a>Feladat-visszavételi házirend
tooreplicate hátsó tooon helyszíni, akkor kell egy feladatátvételi házirendhez. hello házirend automatikusan jön létre, ha előre házirendet hoz létre, és automatikusan társítva hello konfigurációs kiszolgáló. A rendszer nem szerkeszthető. hello-házirendben engedélyezve van a replikációs beállításokat a következő hello:

* Helyreállítási Időkorlát küszöbértéke = 15 perc
* Helyreállítási pontok megőrzésének ideje 24 óra =
* Alkalmazáskonzisztens pillanatkép gyakorisága = 60 perc

 ![Replikációs beállítások hello feladat-visszavétel házirend](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## <a name="set-up-a-process-server-in-azure"></a>Állítsa be a Folyamatkiszolgáló az Azure-ban
A Folyamatkiszolgáló telepítése az Azure-ban, hogy hello Azure virtuális gépek küldhet hello adatok hátsó toohello a helyszíni fő célkiszolgáló.

Ha szeretne védetté tenni a virtuális gépeket, mint a hagyományos erőforrások (Ez azt jelenti, az Azure-ban helyreállított virtuális gép hello egy virtuális Gépet, amely hello klasszikus telepítési modellel készült), a Folyamatkiszolgáló az Azure-ban van szüksége. Ha helyreállította hello virtuális gépeket az Azure Resource Manager hello központi telepítési típusként, hello Folyamatkiszolgáló hello központi telepítési típusként erőforrás-kezelő rendelkeznie kell. hello központi telepítési típus van kiválasztva hello Azure által központilag telepített virtuális hálózati hello Folyamatkiszolgálót a.

1. A **tároló** > **beállítások** > **Site Recovery-infrastruktúra** (alatt **kezelése**) > **Konfigurációs kiszolgálók** (alatt **a VMware és fizikai gépek**) elemre, jelölje be hello konfigurációs kiszolgáló.
2. Kattintson a **Server feldolgozni**.

  ![Folyamat Server gomb](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)
3. Válassza ki a toodeploy hello Folyamatkiszolgálót, mint a **központi telepítése egy feladat-visszavételi Folyamatkiszolgáló az Azure-ban**.
4. Válassza ki, hogy a helyreállított virtuális gép hello hello előfizetést.
5. Válassza ki a hello helyreállított virtuális gép hello Azure-beli hálózat. hello Folyamatkiszolgáló kell az azonos, hogy hello helyreállítani a virtuális gépek és hello folyamat kiszolgáló képes kommunikálni a hálózati hello toobe.
6. Ha kiválasztott egy *klasszikus üzembe helyezési modellel* hálózati, hozzon létre egy virtuális Gépet hello Azure piactéren keresztül, és telepítse azt hello Folyamatkiszolgáló.

 ![hello "A folyamat kiszolgáló hozzáadása" ablak](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)

 Hello Folyamatkiszolgáló hoz létre, mert nagy figyelmet toohello következő:
 * hello kép neve hello *Microsoft Azure Site helyreállítási folyamat kiszolgáló V2*. Válassza ki **klasszikus** hello telepítési modell.

       ![Select "Classic" as hello Process Server deployment model](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
 * Telepítse a hello Folyamatkiszolgáló függően toohello utasításait [replikálása VMware virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
7. Ha hello *erőforrás-kezelő* Azure hálózati, hello Folyamatkiszolgáló telepítése, adja meg a következő információ hello:

  * hello toodeploy hello kiszolgáló hello erőforráscsoport neve
  * hello hello kiszolgáló neve
  * Felhasználónév és jelszó toohello Server aláírásra
  * toodeploy hello kiszolgáló hello tárfiók
  * hello alhálózati és hello hálózati adaptert, hogy kívánja-e tooconnect tooit
   >[!NOTE]
   >Létre kell hoznia a saját [hálózati illesztő](../virtual-network/virtual-networks-multiple-nics.md) (NIC), és válassza ki azt a Folyamatkiszolgáló hello központi telepítésekor.

    ![Adja meg az adatokat hello "A folyamat kiszolgáló hozzáadása" párbeszédpanel](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)

8. Kattintson az **OK** gombra. Ez a művelet elindítja egy feladatot, amely létrehoz egy erőforrás-kezelő központi telepítési típus virtuális gépet hello Folyamatkiszolgáló telepítése során. tooregister hello server toohello a konfigurációs kiszolgálón belüli futtatási hello beállítása a virtuális gép hello hello utasításait követve [replikálása VMware virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery](site-recovery-vmware-to-azure-classic.md). Egy feladat toodeploy hello Folyamatkiszolgáló is elindul.

  hello Folyamatkiszolgáló szerepel a hello **konfigurációs kiszolgálók** > **tartozó kiszolgálók** > **folyamat kiszolgálók** fülre.

    ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

    > [!NOTE]
    > hello Folyamatkiszolgáló nem látható a **virtuális gép tulajdonságok**. Csak a hello látható **kiszolgálók** lapján hello felügyeleti kiszolgálót, hogy regisztrálva van. Hello Folyamatkiszolgáló tooappear 10 too15 percet is igénybe vehet.


## <a name="set-up-hello-master-target-server-on-premises"></a>Hello fő helyszíni server beállítása
hello fő célkiszolgáló kap hello feladat-visszavétel adatokat. hello kiszolgáló automatikusan hello a helyi felügyeleti kiszolgálón van telepítve, de visszavétele esetén vissza túl sok adat, akkor módosítania kell egy további fő célkiszolgáló fel tooset. tooset fel a fő cél helyszíni kiszolgáló, hello a következő:

> [!NOTE]
> Linux, a fő célkiszolgáló fel tooset toohello következő eljárással kihagyása. A fő célkiszolgáló az operációs rendszer hello csak CentOS 6.6 minimális méretű operációs rendszer használja.

1. Ha hello fő célkiszolgáló Windows beállítása, a hello gyors üzembe helyezési oldal megnyitása hello virtuális Gépet, amely a hello fő célkiszolgáló telepítése.
2. Töltse le a hello telepítési fájl hello Azure Site Recovery az egységes telepítő varázsló.
3. Futtassa a telepítőt hello és a **megkezdése előtt**, jelölje be **adja hozzá a központi telepítési ki további folyamat kiszolgálók tooscale**.
4. Teljes hello varázsló hello a megszokott módon mikor meg [hello kiszolgáló beállítása](site-recovery-vmware-to-azure-classic.md). A hello **konfigurációs kiszolgáló adatait** lapon adja meg a fő célkiszolgáló hello hello IP-címét, és adjon meg egy jelszót tooaccess hello virtuális gép.

### <a name="set-up-a-linux-vm-as-hello-master-target-server"></a>Fő célkiszolgáló hello beállított Linux virtuális gép
hello felügyeleti kiszolgáló hello fő célkiszolgálón fut egy Linux virtuális gép tooset hello CentOS 6.6 minimális méretű operációs rendszer telepítése. Ezután minden SCSI merevlemez hello SCSI-azonosítóinak beolvasása, néhány további csomagok telepítése, és néhány egyéni módosítások alkalmazásához.

#### <a name="install-centos-66"></a>CentOS 6.6 telepítése

1. Hello CentOS 6.6 minimális méretű operációs rendszer telepítése hello VM felügyeleti kiszolgálón. Egy DVD-meghajtóban lévő ISO hello megőrzése, és hello indítórendszerrel. Hagyja ki hello media tesztelése. Válassza ki **angol** hello nyelv kiválasztása **alapvető tárolóeszközök**, ellenőrizze, hogy a merevlemez-meghajtóról hello nem rendelkezik a fontos adatokat, kattintson **Igen**, és elveti az adatok. Adja meg a hello felügyeleti kiszolgáló hello állomásnevét, és válassza a hello kiszolgáló hálózati adaptere.  A hello **szerkesztése rendszer** párbeszédpanelen jelölje ki **automatikus csatlakozás**, majd adja hozzá egy statikus IP-címet, a hálózati és a DNS-beállítások. Adjon meg egy időzónát. tooaccess hello felügyeleti kiszolgáló, írja be a hello gyökér szintű jelszavát.
2. Amikor a rendszer megkérdezi, hogy milyen típusú telepítést szeretne, válassza ki a **hozzon létre egyéni elrendezés** hello partíció. Kattintson a **Tovább** gombra. Válassza ki **szabad**, és kattintson a **létrehozása**. Hozzon létre  **/** , **/var/összeomlási**, és **/home partíciók** rendelkező **FS típusa:** **ext4**. A partíció hello swap, **FS típusa: swap**.
3. Ha már meglévő eszközökön található, megjelenik egy figyelmeztető üzenet. Kattintson a **formátum** tooformat hello meghajtó hello partíció beállításokkal. Kattintson a **írási módosítása toodisk** tooapply hello partíció módosításokat.
4. Válassza ki **telepítés rendszertöltőt** > **következő** tooinstall hello rendszertöltő hello legfelső szintű partíción.
5. Ha hello telepítés befejeződött, kattintson a **újraindítás**.

#### <a name="retrieve-hello-scsi-ids"></a>Hello SCSI-azonosítókat beolvasása

1. Hello telepítése után minden virtuális gép hello SCSI merevlemez hello SCSI-azonosítóinak beolvasása. toodo tehát hello felügyeleti kiszolgáló virtuális gép leállítása. A virtuális gép tulajdonságai hello VMware-ben kattintson a jobb gombbal hello VM bejegyzés > **beállításainak szerkesztése** > **beállítások**.
2. Válassza ki **speciális** > **általános elem**, és kattintson a **konfigurációs paraméterek**. Ez a beállítás nem érhető el, hello gép futása közben. Hello beállítás toobe érhető el, a gép hello le kell állítani.
3. Hello alábbi módszerekkel:
 * Ha hello sor **lemez. EnableUUID** létezik, győződjön meg arról, hogy hello értéke túl**igaz** (kis-és nagybetűket). Ha hello érték tooTrue már be van állítva, szakítsa meg, és hello SCSI parancsot egy vendég operációs rendszerében tesztelése után.
 * Ha hello sor **lemez. EnableUUID** nem létezik, kattintson a **Hozzáadás sor**, majd vegye fel a hello **igaz** érték. Ne használjon dupla idézőjelek közé.

#### <a name="install-additional-packages"></a>További csomagok telepítése
Töltse le, és további csomagok telepítése.

1. Ellenőrizze, hogy hello fő célkiszolgálóhoz csatlakoztatott toohello Internet.
2. a parancs futtatásához toodownload és 15 telepítőcsomagok hello CentOS tárházból: `# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools`.
3. Ha hello forrásgépek védi egy Reiser Linux fut, vagy XFS hello gyökér- vagy rendszerindító eszköz fájlrendszer, töltse le és további csomagok telepítése az alábbiak szerint:

   * \#CD /usr/local
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * \#rpm – ivh kmod-reiserfs-0,0-1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * \#wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * \#rpm – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm
   * \#yum eszköz-leképező-többutas (szükséges tooenable hello fő célkiszolgáló többutas csomagok) telepítése

#### <a name="apply-custom-changes"></a>Egyéni módosítások alkalmazásához
Miután megadta a hello telepítés utáni lépéseket és a telepített hello csomagok, egyéni módosítások alkalmazásához hello következő tevékenységek végrehajtásával:

1. Hello RHEL 6-64 Unified Agent ügynököt bináris toohello VM másolja. toountar hello bináris, a parancs futtatásához: `tar –zxvf <file name>`.
2. toogive engedélyek, a parancs futtatásához: `# chmod 755 ./ApplyCustomChanges.sh`.
3. Futtatási hello következő parancsfájlt: `# ./ApplyCustomChanges.sh`. Csak egyszer futtatnia. Miután sikeresen lefutott, hello kiszolgáló újraindul.

## <a name="run-hello-failback"></a>Hello feladat-visszavétel futtatása
### <a name="reprotect-hello-azure-vms"></a>Lássa el újból védelemmel hello Azure virtuális gépeken
1. A **tároló**, a **replikált elemek**, kattintson a jobb gombbal a virtuális Gépet, amely során meghiúsult hello, és válassza **védelmének újbóli beállításához**.
2. Hello paneljén láthatja védelmi hello irányában **Azure tooOn helyszíni** már be van jelölve.
3. A **fő célkiszolgáló** és **Folyamatkiszolgáló**hello a helyszíni fő célkiszolgáló között, hello Azure VM Folyamatkiszolgáló.
4. SELECT hello datastore toorecover hello lemezek kívánt helyszíni. Használja ezt a beállítást, ha hello a helyszíni virtuális gép törlődik, és toocreate új lemez szükséges. Figyelmen kívül hello lehetőséget, ha hello lemezek már létezik, de továbbra is szükséges toospecify értéket.
5. Egy adatmegőrzési meghajtó toostop hello pontokat használja az időpontot, amikor a virtuális gép hello replikált hátsó tooon helyszíni. Az itt felsorolt adatmegőrzési meghajtó bizonyos feltételeknek. Ezek a feltételek nélkül hello meghajtó nem szerepel a fő célkiszolgáló hello.

  * Kötet használja (cél replikációs, és így tovább) más célra nem lehet.
  * A kötet nem lehet zárolási üzemmódban.
  * A kötet nem lehet gyorsítótárkötet. (hello fő telepítési nem létezik a köteten található. a Folyamatkiszolgáló és fő hello egyéni telepítési célkötet nem nem jogosult az adatmegőrzési kötet. Itt hello telepítve a Folyamatkiszolgáló és fő célkiszolgáló kötet hello gyorsítótárkötete hello fő cél.)
  * hello kötet fájlrendszerének nem lehet FAT és FAT32.
  * hello kötet kapacitása kell lennie nullánál.
  * a Windows hello alapértelmezett adatmegőrzési kötet R kötet.
  * hello alapértelmezett adatmegőrzési kötet Linux /mnt/retention.

6. hello feladatátvételi házirendhez automatikusan ki van jelölve.
7. Miután rákattintott **OK** toobegin ismételt védelem, a feladatok megkezdése tooreplicate hello VM Azure toohello a helyszíni helyről. Előrehaladásának hello a hello **feladatok** fülre.

Ha toorecover tooan másik helyre, jelölje be a hello adatmegőrzési meghajtó és a fő célkiszolgáló hello beállított adattároló. Hátsó toohello helyszíni hely sikertelen lesz, ha VMware virtuális gépek hello hello feladat-visszavétel védelmi terv használja hello ugyanazon adattár hello fő kiszolgálóként. Ha azt szeretné, hogy toorecover hello replika Azure virtuális gép toohello megegyezik a helyszíni virtuális gép, hello a helyszíni virtuális gép kell már kell a hello azonos adattároló mint hello fő célkiszolgáló. Ha nincs virtuális gép a helyszíni, egy új ismételt védelem alatt jön létre.

![Hello legördülő menüben válassza a "Azure tooon helyszíni"](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

A helyreállítási terv szinten is állítsa. Ha a replikációs csoport, csak a helyreállítási terv használatával is állítsa. Akkor állítsa a helyreállítási tervet, használja hello korábbi értéket minden védett gép.

> [!NOTE]
> Replikációs csoportok védendő vissza magát hello azonos fő célkiszolgálót. A védett másik fő célkiszolgáló kiszolgálókkal vissza, ha egy közös időpontig visszamenőleg nem lehet megállapítani, a számukra.

### <a name="run-a-failover-toohello-on-premises-site"></a>Futtassa a feladatátvételi toohello helyszíni hely
Állítsa hello VM, miután a feladatátvétel az Azure tooon helyszíni is kezdeményezhető.

1. Hello replikált elemek lapon kattintson a jobb gombbal a hello virtuális gépet, majd válassza ki **nem tervezett feladatátvétel**.
2. A **megerősítéséhez feladatátvétel**, ellenőrizze a hello feladatátvételi irányát (az Azure-ból), és válassza a hello helyreállítási pontot, amelyet az toouse hello feladatátvételi (legújabb hello vagy hello legújabb alkalmazáskonzisztens helyreállítási pont). Időben hello legutóbbi pont előtt történik az alkalmazáskonzisztens helyreállítási pontot, és az adatvesztést okoz.
3. A feladatátvétel során a Site Recovery hello Azure virtuális gépek leáll. Után ellenőrizheti, hogy feladat-visszavétel befejezése után várt módon, nézze meg, amely hello Azure virtuális gépek be lett zárva elvárt tooensure.

### <a name="reprotect-hello-on-premises-site"></a>Lássa el újból védelemmel hello helyszíni hely
Feladat-visszavétel befejezése után, a helyreállított Azure virtuális gépek hello véglegesítési hello virtuális gép tooensure törlődnek. toodo Igen, kattintson a jobb gombbal a védett hello elemet, és kattintson **véglegesítése**. Ez a művelet egy feladatot, amely eltávolítja a hello korábbi helyreállított virtuális gépek Azure-ban váltja ki.

Hello véglegesítési befejezése után az adatok biztonsági hello helyszíni hely szabad, de azt nem lehet védetté tenni. toostart replikációs tooAzure újra, hello a következő:

1. A **tároló**, a **beállítás** > **replikált elemek**, válassza ki a hello vissza sikertelen, és kattintson a virtuális gépek **védelménekújbólibeállításához**.
2. Adjon meg, amelyet a használt toobe toosend adatok hátsó tooAzure Folyamatkiszolgáló hello hello értékének.
3. Kattintson az **OK** gombra.

Hello ismételt védelem befejezése után hello VM hátsó tooAzure replikálja, és elvégezhető a feladatátvétel.

### <a name="resolve-common-failback-issues"></a>Feladat-visszavétel kapcsolatos gyakori problémák megoldása
* Ha egy csak olvasható felhasználói vCenter-kiszolgálók automatikus észlelését, és a virtuális gépek védelmére, ez sikeres, és feladatátvételi működik. Ismételt védelem, során feladatátvétel sikertelen lesz, mivel hello datastores nem lesz felderítve. A jelenség, mint ismételt védelem alatt felsorolt hello datastores nem láthatja. tooresolve a probléma egy megfelelő jogosultsággal rendelkező fiók hello vCenter hitelesítő adatot frissíteni, és próbálja meg újra hello feladatot. További információkért lásd: [replikálása VMware virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery szolgáltatással](site-recovery-vmware-to-azure-classic.md)
* Ha a feladat-visszavételt Linux virtuális gép, és futtassa a helyszíni, lásd: hello hálózatkezelő csomag hello machine el lett távolítva. Az Eltávolítás az oka, hogy hello hálózatkezelő csomagot el kell távolítani a virtuális gép hello helyreállításakor az Azure-ban.
* Ha egy virtuális gép statikus IP-címmel van konfigurálva, és tooAzure feladatátvételt, hello IP-cím szerezte be a DHCP. Amikor a rendszer átadja a hátsó tooon helyszíni, hello VM toouse DHCP tooacquire hello IP-cím továbbra is. Manuálisan toohello gép bejelentkezhet, és szükség esetén állítsa be a hello IP-cím hátsó tooa statikus cím.
* Vagy ESXi 5.5 ingyenes kiadásban, vagy vSphere 6 hipervizor ingyenes kiadás használatakor volna sikertelen volt, de nem sikerült a feladat-visszavétel lenne. tooenable feladat-visszavétel, frissítési tooeither program próbalicencre.
* A Folyamatkiszolgáló hello hello konfigurációs kiszolgáló nem érhető el, ha Ellenőrizze kapcsolat toohello konfigurációs kiszolgáló Telnet toohello konfigurációs kiszolgáló számítógépén a 443-as porton. Megpróbálhatja tooping hello konfigurációs kiszolgáló hello Folyamatkiszolgáló gépről is. A Folyamatkiszolgáló lehet szívverés csatlakoztatott toohello konfigurációs kiszolgáló esetén.
* Toofail hátsó tooan alternatív vCenter próbált meg, ha győződjön meg arról, hogy az új vCenter fel van derítve és adott hello fő célkiszolgáló is fel van derítve. Egy tipikus előfordulhat, hogy hello datastores nem elérhetők és hello látható **lássa el újból védelemmel** párbeszédpanel megnyitásához.
* Egy védett, mint egy fizikai a helyi számítógépen nem sikerült újból az Azure tooon helyszíni WS2008R2SP1 machine.

## <a name="fail-back-with-expressroute"></a>Az ExpressRoute-es vissza
Vissza a VPN-kapcsolaton keresztül, vagy egy ExpressRoute-kapcsolat használatával sikertelen lehet. Ha azt szeretné, hogy toouse ExpressRoute kapcsolat, vegye figyelembe a hello következőket:

* hello ExpressRoute-kapcsolatot kell beállítani a hello tooand feladatátvételt hello forrásgépek Azure-beli virtuális hálózat hol hello Azure virtuális gépek a hello feladatátvételt követően.
* A adata replikált tooan egy nyilvános végpontot az Azure storage-fiók. toouse ExpressRoute-kapcsolat beállítása nyilvános társviszony-létesítés az ExpressRoute hello cél adatközpont Site Recovery-replikációhoz.
