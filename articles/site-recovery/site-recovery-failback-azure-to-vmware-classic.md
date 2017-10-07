---
title: "aaaFail VMware virtuális gépek biztonsági Azure hello klasszikus portál |} Microsoft Docs"
description: "További információk a hátsó toohello helyszíni hely hiányában a VMware virtuális gépek és fizikai kiszolgálók tooAzure feladatátvételt követően."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 7ca86e21-7a5d-45ab-8f4b-e42da90f199a
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 80bc3ab2708a5df953c6532b353da19a4c44ac34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-toohello-on-premises-site-classic-portal"></a>Sikertelen a hátsó VMware virtuális gépek és fizikai kiszolgálók toohello helyszíni hely (klasszikus portál)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klasszikus Azure portál](site-recovery-failback-azure-to-vmware-classic.md)
> * [A klasszikus Azure portálon (örökölt)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Ez a cikk ismerteti, hogyan toofail biztonsági másolatot az Azure virtuális gépek Azure toohello a helyszíni helyről. Kövesse az ebben a cikkben, ha már készen áll a toofail hello utasításokat a VMware virtuális gépeket vagy windowsos/Linuxos fizikai kiszolgálók biztonsági után már átvette azokat a hello helyszíni hely tooAzure használó [oktatóanyag](site-recovery-vmware-to-azure-classic.md).

## <a name="overview"></a>Áttekintés
Ez az ábra az ebben a forgatókönyvben hello feladat-visszavétel architektúráját mutatja be.

Az architektúrák használatára, ha hello folyamatkiszolgáló a helyszíni és az ExpressRoute használ.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Az architektúrák használatára, ha hello folyamatkiszolgáló az Azure-on, és egy VPN vagy ExpressRoute kapcsolat van.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

az alábbi képen toohello tekintse meg a toosee hello portok és listája hello feladat-visszavétel architechture diagramja

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Hogyan működik a feladat-visszavétel itt található:

* Miután tooAzure már feladatátvételt, néhány lépésben nem hátsó tooyour helyszíni hely:
  * **1. szakasz**: hello Azure virtuális gépek állítsa az, hogy elindítja a helyszíni hely futtató hátsó tooVMware virtuális gépek replikálása. Ismételt védelem engedélyezésével hello VM áthelyezi a feladat-visszavétel védelmi csoport automatikusan létrehozott hello feladatátvételi védelmi csoport létrehozásakor. Ha a feladatátvételi védelmi csoport tooa helyreállítási terv hozzáadott majd hello feladat-visszavétel védelmi csoport is automatikusan toohello terv lett adva.  Védelem-újrabeállítási során megadhatja, hogyan tooplan toofail biztonsági másolatot.
  * **2. szakaszra**: után az Azure virtuális gépek tooyour helyszíni helyre replikál, futtassa a sikertelen toofail keresztül az Azure-ból.
  * **3. szakaszra**: követően az adatok visszaállítása sikertelen volt, állítsa hello a helyszíni virtuális gépek, a sikertelen biztonsági, így azok tooAzure replikálást indítani.


  [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failback/player]

### <a name="failback-toohello-original-or-alternate-location"></a>Feladat-visszavétel toohello eredeti helyére vagy máshová
Ha átadja a VMware virtuális gép hátsó toohello sikertelen lehet azonos forrás virtuális gép, ha még létezik a helyszíni. Ebben a forgatókönyvben csak hello változáskülönbözeteit fog megtörténni vissza. Vegye figyelembe:

* Ha átadja a fizikai kiszolgálókon, akkor a feladat-visszavétel értéke mindig tooa új VMware virtuális gép.
  * Vissza a fizikai gép sikertelen előtt vegye figyelembe, hogy
  * Védett fizikai gépek fog visszatérhet, amikor újból az Azure tooVMware feladatátvételt virtuális gépként
  * Győződjön meg arról, hogy legalább egy fő célkiszolgáló Server együtt hello szükséges ESX/ESXi-gazdagépek toowhich felfedezi toofailback van szüksége.
* Ha Ön a feladat-visszavételt toohello eredeti VM hello következő szükség:

  * Ha hello VM egy vCenter-kiszolgáló által felügyelt hello fő célkiszolgáló ESX-gazdagép hozzáférés toohello virtuális gépek datastore kell rendelkeznie.
  * Ha hello VM ESX-gazdagépen, de nem kezeli vCenter majd hello merevlemezén hello VM kell lennie a hello MT a gazdagép által elérhető adattárolót.
  * Ha a virtuális Gépet az ESX-gazdagépen, és nem használja a vCenter el kell végeznie az ESX-gazdagép hello a fő Célkiszolgáló hello felderítése ahhoz, hogy lássa el újból védelemmel. Ez igaz, ha a feladat-visszavétele vissza fizikai kiszolgálók túl.
  * Egy másik lehetőség (ha hello a helyszíni virtuális gép létezik) toodelete Ez a feladat-visszavétel előtt. Majd feladat-visszavétel fog majd új virtuális gép létrehozása az hello azonos, hello fő ESX-gazdagép üzemeltetésére.
* Ha feladat-visszavétel tooan másik helyre hello adatokat fog helyreállított toohello ugyanazon adattár és hello hello a helyszíni fő célkiszolgáló által használt azonos ESX-gazdagépet.

## <a name="prerequisites"></a>Előfeltételek
* Szüksége lesz a rendelés toofail vissza VMware virtuális gépek és fizikai kiszolgálók VMware környezetben. Hátsó tooa hiányában a fizikai kiszolgáló nem támogatott.
* A sorrend toofail vissza kell létrehozott Azure-hálózat védelmi kezdeti beállításakor. Feladat-visszavétel hello Azure VPN- vagy ExpressRoute kapcsolat van szüksége, amelyben hello Azure virtuális gépek hálózati található toohello helyszíni hely.
* Ha hello virtuális gépek toofail hátsó tooare vCenter-kiszolgáló által kezelt toomake meg arról, hogy a vCenter-kiszolgáló a virtuális gépek felderítése hello szükséges engedélyek lesz szüksége. [További információk](site-recovery-vmware-to-azure-classic.md).
* Ha a pillanatképek a virtuális gép ismételt védelem meghiúsul. Hello pillanatképekkel vagy hello lemezek törlése.
* Ahhoz, hogy a feladat-visszavételt toocreate összetevők számos lesz szüksége:
  * **A folyamat-kiszolgáló létrehozása az Azure-ban**. Ez az az Azure virtuális gép toocreate kell, és a feladat-visszavétel során tovább futnak. Hello gép feladat-visszavétel befejezése után törölheti.
  * **Hozzon létre egy fő célkiszolgáló**: hello fő célkiszolgáló adatokat küld és fogad feladat-visszavételre. a helyszíni létrehozott hello felügyeleti kiszolgálóhoz be a fő célkiszolgálón, alapértelmezés szerint telepítve van. Azonban sikertelen hátsó forgalom mennyiségét hello függően szükség lehet egy külön fő célkiszolgáló toocreate feladat-visszavételi.
  * Ha azt szeretné, toocreate egy további fő célkiszolgálón futó Linux, szüksége lesz tooset hello Linux virtuális gép mentése az alább ismertetett hello fő célkiszolgáló, telepítése előtt.
* Konfigurációs kiszolgáló szükségesek helyszíni, ha így tesz, a feladat-visszavételre. A feladat-visszavétel során hello virtuális gép hello konfigurációs adatbázishoz, mely feladat-visszavétel nem lesz sikeres végrehajtása léteznie kell. Ezért győződjön meg arról, hogy szánjon a kiszolgáló rendszeres ütemezett biztonsági mentést. Abban az esetben, ha az egy olyan vészhelyzet esetén, szüksége lesz a toorestore hello együtt ugyanazon IP-címek, hogy a feladat-visszavétel fog működni.

## <a name="set-up-hello-process-server-in-azure"></a>Az Azure-ban hello folyamat kiszolgáló beállítása
Így hello Azure virtuális gépek küldhet hello adatok hátsó tooon helyszíni fő célkiszolgáló szüksége tooinstall a folyamatkiszolgáló az Azure-ban.

1. Hello Site Recovery portálon > **konfigurációs kiszolgálók** válassza ki az új folyamatkiszolgáló tooadd.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)
2. Adja meg a folyamat-kiszolgáló nevét, és adjon meg egy nevet és jelszót tooconnect toohello Azure virtuális gép rendszergazdaként fogja használni. A **konfigurációs kiszolgáló** válassza ki hello helyszíni felügyeleti kiszolgálót, és adja meg a hello Azure hálózati mely hello a folyamatkiszolgáló kell telepíteni. Ez legyen hello hálózati hello Azure virtuális gépek találhatók. Adjon meg egy egyedi IP-címet a hello válassza alhálózatból és telepítésének megkezdéséhez.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

   Egy feladat toodeploy hello folyamatkiszolgáló indul

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

   Hello után folyamatkiszolgáló bejelentkezhet hello hitelesítő adatokat adja meg azt az Azure-ban van telepítve. hello első bejelentkezéskor hello folyamat kiszolgáló párbeszédpanelen fog futni. Hello típusú hello IP-címet a helyi felügyeleti kiszolgáló és a jelszót. Hagyja hello alapértelmezett 443-as porton értéket. Is hagyhatja hello alapértelmezett 9443 port adatreplikáció kivéve hello helyszíni felügyeleti kiszolgáló beállításakor kifejezetten módosítani ezt a beállítást.

   > [!NOTE]
   > hello kiszolgáló nem fog megjelenni a **virtuális gép tulajdonságok**. Értéke csak hello láthatóvá **kiszolgálók** lapján hello felügyeleti kiszolgáló toowhich van regisztrálva. Arról is igénybe vehet a hello folyamat server tooappear 10 – 15 perc.
   >
   >

## <a name="set-up-hello-master-target-server-on-premises"></a>Hello fő helyszíni server beállítása
hello fő célkiszolgáló kap hello feladat-visszavétel adatokat. A fő célkiszolgáló automatikusan hello a helyi felügyeleti kiszolgálón van telepítve, de visszavétele esetén vissza nagy mennyiségű adat szükség lehet a tooset további fő célkiszolgáló fel. Ehhez az alábbiak szerint:

> [!NOTE]
> Ha azt szeretné, hogy a fő célkiszolgáló Linux tooinstall, követésével hello hello következő eljárásban.
>
>

1. A Windows hello fő célkiszolgáló telepítése, nyissa meg a hello gyors kezdés lapon hello virtuális gép, amelyen hello fő célkiszolgáló telepítése, és töltse le a hello Azure Site Recovery az egységes telepítő varázsló hello telepítőfájlját.
2. Futtassa a telepítőt, és a **megkezdése előtt** válasszon **további folyamat kiszolgálók tooscale ki a központi telepítés hozzáadása**.
3. Teljes hello varázsló hello a megszokott módon mikor meg [hello kiszolgáló beállítása](site-recovery-vmware-to-azure-classic.md). A hello **konfigurációs kiszolgáló adatait** adja meg azokat a fő célkiszolgáló és egy jelszó tooaccess hello VM hello IP-címét.

### <a name="set-up-a-linux-vm-as-hello-master-target-server"></a>Fő célkiszolgáló hello beállított Linux virtuális gép
hello felügyeleti kiszolgáló hello fő célkiszolgálón fut egy Linux virtuális gép tooinstall hello centes kell tooset) S 6.6 minimális méretű operációs rendszer, egyes SCSI merevlemez hello SCSI-azonosítóinak beolvasása, néhány további csomagok telepítése, és néhány egyéni módosítások alkalmazásához.

#### <a name="install-centos-66"></a>CentOS 6.6 telepítése
1. Hello CentOS 6.6 minimális méretű operációs rendszer telepítése hello VM felügyeleti kiszolgálón. Egy DVD-ről meghajtó és rendszerindító hello rendszer hello ISO ne. Kihagyás hello media tesztelése, válassza ki angol hello nyelvi, jelölje be a **alapvető tárolóeszközök**, ellenőrizze, hogy a merevlemez-meghajtóról hello nem fontos adatokat, majd kattintson a **Igen**, minden az adatok elvetése. Adja meg a hello felügyeleti kiszolgáló hello állomásnevét, és válassza ki a hello kiszolgáló hálózati adaptere.  A hello **szerkesztése rendszer** párbeszédpanelen válasszon ** automatikus csatlakozás **, és adja hozzá egy statikus IP-címet, a hálózati és a DNS-beállításait. Adjon meg egy időzóna, és egy jelszó tooaccess hello felügyeleti gyökérkiszolgáló.
2. Ha kérni hello típusú telepítést szeretné, hogy válasszon **hozzon létre egyéni elrendezés** hello partíció. Miután rákattintott **következő** válasszon **szabad** , és kattintson a Létrehozás gombra. Hozzon létre  **/** , **/var/összeomlási** és **/home partíciók** rendelkező **FS típusa:** **ext4**. Hozzon létre hello swap partion, **FS típusa: swap**.
3. Ha már meglévő eszközök találhatók, ekkor megjelenik egy figyelmeztető üzenet. Kattintson a **formátum** tooformat hello meghajtó hello partíció beállításokkal. Kattintson a **írási módosítása toodisk** tooapply hello partíció módosításokat.
4. Válassza ki **telepítés rendszertöltőt** > **következő** tooinstall hello rendszertöltő hello legfelső szintű partíción.
5. Kattintson a telepítés befejeztével **újraindítás**.

#### <a name="retrieve-hello-scsi-ids"></a>Hello SCSI-azonosítókat beolvasása
1. A telepítés után minden virtuális gép hello SCSI merevlemez hello SCSI-azonosítóinak beolvasása. toodo ez leállítása hello felügyeleti kiszolgáló virtuális gép, a virtuális gép hello tulajdonságok VMware-ben kattintson a jobb gombbal hello VM bejegyzés > **beállításainak szerkesztése** > **beállítások**.
2. Válassza ki **speciális** > **általános elem** kattintson **konfigurációs paraméterek**. Ez a beállítás el de-active hello gép futása közben. toomake informatikai aktív hello gépen le kell állítani.
3. Ha hello sor **lemez. EnableUUID** létezik ellenőrizze, hogy hello értéke túl**igaz** (kis-és nagybetűket). Ha már van szakítsa meg, és tesztelje a hello SCSI parancsot egy vendég operációs rendszerében után.
4. Ha hello sor nem létező kattintson **Hozzáadás sor** –, és adja hozzá hello **igaz** érték. Ne használjon dupla idézőjelet.

#### <a name="install-additional-packages"></a>További csomagok telepítése
Toodownload szüksége lesz, és néhány további csomagok telepítése.

1. Győződjön meg arról, hello fő célkiszolgálóhoz csatlakoztatott toohello internet.
2. Ez a parancs toodownload futtatásához és 15 csomagok telepítéséhez a CentOS adattárból hello: **# yum – y xfsprogs perl lsscsi rsync wget kexec-eszközök telepítése**.
3. Ha lát el védelemmel hello forrásgépek futnak Linux p Reiser, illetve XFS hello gyökér-fájlrendszer vagy rendszerindító eszköz, akkor le kell töltenie és további csomagok telepítése az alábbiak szerint:

   * # <a name="cd-usrlocal"></a>CD /usr/local
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>rpm – ivh kmod-reiserfs-0,0-1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>rpm – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Egyéni módosítások alkalmazásához
Hajtsa végre a következő tooapply egyéni módosításokat, Miután megismerte a teljes hello telepítés utáni lépéseket és a telepített hello csomagok hello:

1. Hello RHEL 6-64 Unified Agent ügynököt bináris toohello VM másolja. Futtassa a parancsot toountar hello bináris: **bont – zxvf<file name>**
2. Futtassa a parancsot toogive engedélyek: **# chmod 755./ApplyCustomChanges.sh**
3. Hello parancsprogrammal: **#./ApplyCustomChanges.sh**. Hello parancsfájl egyszer csak futtasson. Hello kiszolgáló újraindul, miután hello parancsprogram sikeresen lefut.

## <a name="run-hello-failback"></a>Hello feladat-visszavétel futtatása
### <a name="reprotect-hello-azure-vms"></a>Lássa el újból védelemmel hello Azure virtuális gépeken
1. Hello Site Recovery portálon > **gépek** válassza ki hello virtuális Gépet, amely a feladatátvétel megtörtént, és kattintson a **védelmének újbóli beállításához**.
2. A **fő célkiszolgáló** és **Folyamatkiszolgáló** válassza ki a helyszíni fő célkiszolgáló hello és hello Azure virtuális gép folyamatkiszolgáló.
3. Válassza ki a csatlakozáshoz a virtuális gép toohello konfigurált hello fiókot.
4. Válassza ki a hello feladat-visszavétel verziót hello védelmi csoport. Például ha a virtuális gép hello PG1 védett, akkor tooselect PG1_Failback telepítenie kell.
5. Ha toorecover tooan másik helyre, jelölje be a hello adatmegőrzési meghajtó és adattároló hello fő célkiszolgáló konfigurálva. Ha nem sikerül hátsó toohello helyszíni hely hello VMware virtuális gépek hello feladat-visszavétel védelmi terv hello fogja használni, hello fő célkiszolgáló ugyanazon adattár. Ha azt szeretné, hogy toorecover hello replika Azure virtuális gép toohello megegyezik a helyszíni virtuális gép ezután hello a helyszíni virtuális gép már kell kell a hello azonos adattároló mint hello fő célkiszolgáló. Ha nincs virtuális gép helyszíni ismételt védelem alatt jön létre egy újat.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)
6. Miután rákattintott **OK** toobegin ismételt védelemmel ellátni azt a feladatok megkezdése tooreplicate hello VM Azure toohello a helyszíni helyről. Előrehaladásának hello a hello **feladatok** fülre.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-toohello-on-premises-site"></a>Futtassa a feladatátvételi toohello helyszíni hely
Ismételt védelem hello után a virtuális Gépet áthelyezett toohello feladat-visszavétel verziója, a védelmi csoportból, és automatikusan fel lesz véve toohello helyreállítási terv hello feladatátvételi tooAzure ha van ilyen használja.

1. A hello **helyreállítási tervek** lapon jelölje be hello helyreállítási tervet tartalmazó hello feladat-visszavétel csoport, és kattintson **feladatátvételi** > **nem tervezett feladatátvétel**.
2. A **megerősítéséhez feladatátvétel** hello feladatátvételi irányát (tooAzure) ellenőrizze, és válassza ki a kívánt toouse (legújabb) hello feladatátvételhez hello helyreállítási pontot. Ha engedélyezte a **virtuális Gépre kiterjedő** replikációs tulajdonságok konfigurálása során is helyreállíthatja toohello legújabb alkalmazás vagy összeomlás-konzisztens helyreállítási pontot. Kattintson a hello pipa toostart hello feladatátvételi.
3. A feladatátvétel során a Site Recovery hello Azure virtuális gépeken le fog állni. Ellenőrzése meg a várt módon, hogy feladat-visszavétel befejezése után is ellenőrizheti, hogy hello Azure virtuális gépek be lett zárva várt módon.

### <a name="reprotect-hello--on-premises-site"></a>Lássa el újból védelemmel hello helyszíni hely
Feladat-visszavétel befejezése után az adatok vissza a hello helyszíni helyen, de nem lehet védetté tenni. toostart replikációs tooAzure újra hello a következő:

1. Hello Site Recovery portálon > **gépek** válassza hello virtuális gépek ismét sikertelen, és kattintson a lap **védelmének újbóli beállításához**.
2. Miután ellenőrizte, hogy replikációs tooAzure a várt módon működik, az Azure-ban törölheti hello Azure virtuális gépeken (jelenleg nem fut) visszaállítása sikertelen volt.

### <a name="common-issues-in-failback"></a>A feladat-visszavétel kapcsolatos gyakori hibák
1. Csak olvasási jogosultsággal rendelkező felhasználói vCenter-kiszolgálók automatikus észlelését, és a virtuális gépek védelmére, ha ez sikeres, és feladatátvételi működik. A védelem-Újrabeállítási hello időpontjában meghiúsul óta hello datastores nem lesz felderítve. A jelenség, nem látják hello datastores felsorolt közben ismételt védelmével. tooresolve, frissíteni hello vCenter hitelesítő adatokat a megfelelő engedélyekkel rendelkező fiókot, és próbálja meg újra hello feladatot. [További információ](site-recovery-vmware-to-azure-classic.md)
2. Ha a Linux virtuális gép feladatátvételi, és futtassa a helyszínen, látni fogja a hello hálózatkezelő csomag eltávolítása hello gépről. Ennek az az oka a virtuális gép hello helyreállításakor az Azure-ban hello hálózatkezelő csomag törlődik.
3. Ha egy virtuális gép statikus IP-címmel van konfigurálva, és tooAzure feladatátvételt, hello IP-cím szerezte be a DHCP. Amikor a rendszer átadja a hátsó tooOn helyszínen, hello VM toouse DHCP tooacquire hello IP-cím továbbra is. Hello géppé toomanually bejelentkezési kell és hello IP-cím beállítása biztonsági tooStatic címet, ha szükséges.
4. Vagy ESXi 5.5 ingyenes kiadásban, vagy vSphere 6 hipervizor ingyenes kiadás használatakor volna sikertelen volt, de a feladat-visszavétel nem fog sikerülni. Ned tooupgrade tooeither Próbalicencre tooenable feladat-visszavétel lesz.

## <a name="failing-back-with-expressroute"></a>Az ExpressRoute meghibásodott vissza
Átveheti vissza egy VPN-kapcsolat vagy Azure expressroute-ot. Ha azt szeretné, hogy toouse ExpressRoute Megjegyzés hello következő:

* ExpressRoute hello Azure-beli virtuális hálózat toowhich forrás gépek sikertelenek keresztül, és amelyhez Azure virtuális gépek találhatók hello feladatátvételt követően kell beállítani.
* A adata replikált tooan egy nyilvános végpontot az Azure storage-fiók. Állítson be nyilvános társviszony-létesítés ExpressRoute a Site Recovery replikációs toouse ExpressRoute hello cél adatközpontot.
