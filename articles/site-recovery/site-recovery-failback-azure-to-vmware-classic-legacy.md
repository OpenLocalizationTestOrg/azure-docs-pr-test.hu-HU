---
title: "aaaFail VMware virtuális gépek biztonsági Azure hello klasszikus örökölt portálon |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toofail vissza, amelynek már VMware virtuális gépek replikálása az Azure Site Recovery tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: a63524bf-990c-42ee-8554-e017e6e67e68
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 5ef66b366dcdc43f3bc171e0ed1532216cc2ab89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-toovmware-with-azure-site-recovery-legacy"></a>Sikertelen a hátsó VMware virtuális gépek és fizikai kiszolgálók Azure tooVMware az Azure Site Recovery (örökölt)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klasszikus Azure portál](site-recovery-failback-azure-to-vmware-classic.md)
> * [A klasszikus Azure portálon (örökölt)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Ez a cikk ismerteti, hogyan toofail hátsó VMware virtuális gépek és a windowsos/Linuxos fizikai kiszolgálók Azure tooyour a helyszíni helyről után végzett replikálást, a helyszíni webhelyen tooAzure segítségével [Azure Site Recovery?](site-recovery-overview.md).

Ez a cikk egy régi konfigurációt, és nem használható új tárolók.

## <a name="architecture"></a>Architektúra
Ez az ábra hello feladatátvétel és a feladat-visszavételi forgatókönyv jelöli. kék hello sorai hello kapcsolatok feladatátvétel során. piros hello sorai feladat-visszavétel során használt hello kapcsolatok. hello sorok nyíllal vizsgálni hello internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Előkészületek
* Meg kell rendelkeznie a feladatátvételt a VMware virtuális gépek vagy fizikai kiszolgálók, és azok rendszerűnek kell lennie az Azure-ban.
* Vegye figyelembe, hogy a feladatvisszavétel csak hátsó VMware virtuális gépek és a windowsos/Linuxos fizikai kiszolgálók Azure tooVMware virtuális gépekről hello helyszíni elsődleges helyen.  Visszavétele esetén vissza a fizikai gép, feladatátvételi tooAzure konvertálja tooan Azure virtuális Gépen, és a feladat-visszavétel tooVMware konvertálja tooa VMware virtuális gép.

Itt látható, hogyan állíthatja be a feladat-visszavétel:

1. **Feladat-visszavétel összetevők beállítása**: tooset vContinuum-kiszolgáló lesz szüksége a helyszínen, majd az Azure-ban toohello konfigurációs kiszolgáló VM mutasson. Azt is be lesznek állítva a folyamatkiszolgáló, egy Azure virtuális gép toosend adatok hátsó toohello a helyszíni fő célkiszolgáló. Hello folyamatkiszolgáló regisztrálása hello feladatátvételi kezelt hello konfigurációs kiszolgáló. A helyszíni fő célkiszolgáló telepítése. Ha egy Windows fő célkiszolgáló szüksége van beállítva automatikusan vContinuum telepítésekor. Ha Linux szüksége lesz szüksége tooset azt be manuálisan egy külön kiszolgálón.
2. **Védelem és a feladat-visszavétel engedélyezése**: hello összetevők beállítása után a vContinuum lesz szüksége tooenable védelme az Azure virtuális gépek a feladatátvételt. Virtuális gépek hello készültségének ellenőrzéséhez lesz, és feladatátvételt az Azure tooyour a helyszíni helyről. Feladat-visszavétel befejezése után, állítsa a helyszíni gépeket, hogy azok tooAzure replikálást indítani.

## <a name="step-1-install-vcontinuum-on-premises"></a>1. lépés: A helyszíni vContinuum telepítése
Tooinstall vContinuum-kiszolgáló a helyszínen kell lesz, és irányítsa toohello konfigurációs kiszolgáló.

1. [Töltse le a vContinuum](http://go.microsoft.com/fwlink/?linkid=526305).
2. Ezután töltse le a hello [vContinuum frissítés](http://go.microsoft.com/fwlink/?LinkID=533813) verziója.
3. Telepítse a vContinuum hello legújabb verzióját. A hello **üdvözlő** kattintson **következő**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4. Hello hello varázsló első lapján adja meg a hello CX kiszolgáló IP-címe és hello CX kiszolgáló portja. Válassza ki **HTTPS használatára**.

   ![](./media/site-recovery-failback-azure-to-vmware/image3.png)
5. Hello konfigurációs kiszolgáló IP-címének a hello **irányítópult** lapon hello konfigurációs kiszolgáló Azure-ban.
   ![](./media/site-recovery-failback-azure-to-vmware/image4.png)
6. Hello konfigurációs kiszolgáló HTTPS hello a nyilvános port található **végpontok** lapon hello konfigurációs kiszolgáló Azure-ban.

   ![](./media/site-recovery-failback-azure-to-vmware/image5.png)
7. A hello **CS jelszót részletek** lap feljegyzett le hello konfigurációs kiszolgáló regisztrálásakor hello jelszót adja meg. Ha nem emlékszik azt ellenőrzi azt **C:\\Program Files (x86)\\InMage rendszerek\\titkos\\connection.passphrase** hello virtuális gép konfigurációs kiszolgálón.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)
8. A hello **rendeltetési hely kiválasztása** lapon adja meg, amelyen szeretné, hogy tooinstall hello vContinuum-kiszolgáló, és kattintson **telepítése**.

   ![](./media/site-recovery-failback-azure-to-vmware/image7.png)
9. A telepítést követően vContinuum indíthatja el.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

## <a name="step-2-install-a-process-server-in-azure"></a>2. lépés: A folyamatkiszolgáló telepítése az Azure-ban
Így hello Azure virtuális gépek küldhet hello adatok hátsó tooan a helyszíni fő célkiszolgáló szüksége tooinstall a folyamatkiszolgáló az Azure-ban.

1. A hello **konfigurációs kiszolgálók** egy új folyamatkiszolgáló az Azure, jelölje be tooadd lapon.

   ![](./media/site-recovery-failback-azure-to-vmware/image9.png)
2. Adja meg a folyamat kiszolgálót, és egy nevet és jelszót tooconnect toohello virtuális gép rendszergazdaként. Válassza ki a kívánt tooregister hello folyamatkiszolgáló hello konfigurációs kiszolgáló toowhich. Ez legyen hello tooprotect használ, és a virtuális gépeket ugyanazon a kiszolgálón.
3. Adja meg a hello Azure hálózati mely hello a folyamatkiszolgáló kell telepíteni. Hello azonosnak kell lennie hálózati hello konfigurációs kiszolgálóval. Adjon meg egy egyedi IP-címet a kijelölt alhálózat és telepítésének megkezdéséhez.

   ![](./media/site-recovery-failback-azure-to-vmware/image10.png)
4. Egy feladat kiváltott toodeploy hello folyamatkiszolgáló.

   ![](./media/site-recovery-failback-azure-to-vmware/image11.png)
5. Az Azure-ban hello folyamatkiszolgáló telepítése után az hello kiszolgálóra megadott hello hitelesítő adataival jelentkezhet. Regisztrálja a folyamatkiszolgálót hello a hello ugyanúgy hello regisztrálta a helyszíni folyamatkiszolgáló.

   ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

> [!NOTE]
> feladat-visszavétel során regisztrálva hello kiszolgálók nem fog megjelenni a virtuális gép tulajdonságok a Site Recovery szolgáltatásban. Csak azok a hello látható **kiszolgálók** lapján hello konfigurációs kiszolgáló toowhich azok van regisztrálva. Körülbelül 10 – 15 percig, amíg azok hello folyamat is igénybe vehet kiszolgáló hello lapon jelenik meg.
>
>

## <a name="step-3-install-a-master-target-server-on-premises"></a>3. lépés: Telepítse helyszíni fő célkiszolgáló
Attól függően, hogy a forrás virtuális gép operációs rendszere kell tooinstall egy Linux vagy helyszíni fő célkiszolgáló Windows.

### <a name="deploy-a-windows-master-target-server"></a>Egy Windows fő célkiszolgáló telepítése
A fő célkiszolgálón már mellékelhető vContinuum telepítő Windows. Hello vContinuum telepítésekor főkiszolgálóvá is telepíti a hello ugyanaz a gép és a regisztrált toohello konfigurációs kiszolgáló.

1. toobegin telepítést, hozzon létre egy üres hello ESX-gazdagépet, amelyre szeretné toorecover hello virtuális gépeket az Azure-ból a helyszíni gépre.
2. Győződjön meg arról, hogy van legalább két lemezt csatlakoztatott toohello VM – hello operációs rendszerhez használt egyik, és más hello hello adatmegőrzési meghajtó használható.
3. Hello operációs rendszer telepítéséhez.
4. Telepítse a hello vContinuum hello kiszolgálón. Ezzel is hello fő célkiszolgáló telepítése befejeződött.

### <a name="deploy-a-linux-master-target-server"></a>A Linux fő célkiszolgáló telepítése
1. toobegin telepítést, hozzon létre egy üres hello ESX-gazdagépet, amelyre szeretné toorecover hello virtuális gépeket az Azure-ból a helyszíni gépre.
2. Győződjön meg arról, hogy van legalább két lemezt csatlakoztatott toohello VM – hello operációs rendszerhez használt egyik, és más hello hello adatmegőrzési meghajtó használható.
3. Hello Linux operációs rendszer telepítéséhez. a legfelső szintű vagy az adatmegőrzési tárolóhelyek hello Linux fő rendszer ne használjon LVM. A fő célkiszolgáló Linux tooavoid LVM partíciók/lemezek felderítés alapértelmezés szerint konfigurálva.
4. Létrehozható partíciók:

   ![](./media/site-recovery-failback-azure-to-vmware/image13.png)
5. Hajthat végre a következő utáni telepítési lépések hello hello fő célkiszolgáló telepítése előtt.

#### <a name="post-os-installation-steps"></a>Az operációs rendszer telepítési lépéseket utáni
az egyes SCSI merevlemez egy Linux virtuális gépen a SCSI-eseményazonosítók tooget hello engedélyezése hello paraméter "lemezt. EnableUUID = TRUE "az alábbiak szerint:

1. Állítsa le a virtuális gépet.
2. Kattintson a jobb gombbal a hello VM bejegyzés hello bal oldali panelen > **beállításainak szerkesztése**.
3. Kattintson a hello **beállítások** fülre. Válassza ki **speciális\>általános elem** > **konfigurációs paraméterek**. Hello **konfigurációs paraméterek** beállítás csak érhető el, amikor hello gép le van állítva.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)
4. Ellenőrzi, hogy a sor **lemez. EnableUUID** létezik-e. Ha túl van beállítva**hamis** utána állítsa be túl**igaz** (nem kis-és nagybetűket). Ha létezik, és tootrue megadásához kattintson a **Mégse** és hello SCSI parancs hello vendég operációs rendszerében tesztelése után be. Ha nem létezik kattintson **Hozzáadás sor**.
5. Adja hozzá a lemezt. A hello EnableUUID **neve** oszlop. Az értékét állítsa TRUE. Ne adjon hello meghaladja értékeket idézőjelek együtt.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-hello-additional-packages"></a>Töltse le és telepítse a további hello csomagok
Megjegyzés: Ellenőrizze, hogy hello rendszer internetkapcsolattal rendelkező letöltése és hello további csomagok telepítése előtt.

\#yum -y xfsprogs perl lsscsi rsync wget kexec-eszközök telepítése

Ez a parancs letölti a 15 csomagok CentOS 6.6 tárházból, és telepíti azokat:

BC-1.06.95-1.el6.x86\_64. rpm

busybox-1.15.1-20.el6.x86\_64. rpm

elfutils-függvénytárak-0.158-3.2.el6.x86\_64. rpm

kexec-eszközök – 2.0.0-280.el6.x86\_64. rpm

lsscsi-0,23-2.el6.x86\_64. rpm

lzo-2.03-3.1.el6\_5.1.x86\_64. rpm

Perl-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-modul-moduláris-3.90-136.el6\_6.1.x86\_64. rpm

Perl-Pod-Kilépés-1.04-136.el6\_6.1.x86\_64. rpm

Perl-Pod-egyszerű-3.13-136.el6\_6.1.x86\_64. rpm

Perl-függvénytárak-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-verzió-0.77-136.el6\_6.1.x86\_64. rpm

rsync-3.0.6-os-12.el6.x86\_64. rpm

klassz kis 1.1.0-ás 1.el6.x86\_64. rpm

wget-1.12-5.el6\_6.1.x86\_64. rpm

Ha hello forrásgép Reiser vagy XFS filesystem hello gyökér- vagy rendszerindító eszközt használ, majd következő csomagokat kell kell letöltött és telepített Linux fő előzetes tooprotection.

\#CD /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#rpm - ivh kmod-reiserfs-0,0-1.el6.elrepo.x86\_64. rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. rpm

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#rpm - ivh xfsprogs-3.1.1-16.el6.x86\_64. rpm

#### <a name="apply-custom-configuration-changes"></a>Egyéni konfigurációs módosítások alkalmazása
A módosítások alkalmazása előtt győződjön meg arról, hogy elsajátította hello előző szakaszban, majd kövesse az alábbi lépéseket:

1. Másolja a hello RHEL 6-64 Unified Agent ügynököt bináris toohello az újonnan létrehozott az operációs rendszer.
2. Futtassa a parancsot toountar hello bináris: **bont - zxvf \<fájl neve\>**
3. Futtassa a parancsot toogive engedélyek: \# **chmod 755./ApplyCustomChanges.sh**
4. Hello parancsprogrammal:  **\# ./ApplyCustomChanges.sh**. Csak egyszer hello parancsfájl hello kiszolgálón fut. Indítsa újra a hello server hello parancsfájl futtatása után.

### <a name="install-hello-linux-server"></a>Hello Linux-kiszolgáló telepítése
1. [Töltse le](http://go.microsoft.com/fwlink/?LinkID=529757) hello telepítőfájlt.
2. Hello fájl toohello segédprogrammal az sftp ügyfél az Ön által választott Linuxos fő célkiszolgáló virtuális gép másolása. Felváltva hello Linuxos fő célkiszolgáló virtuális gép bejelentkezik, és wget toodownload hello telepítési csomag a megadott hivatkozás használatára
3. Hello Linux fő kiszolgáló virtuális gép használatával bejelentkezik egy ssh ügyfél az Ön által választott
4. Ha csatlakozott toohello Azure hálózat, amelyen a Linux fő célkiszolgáló egy VPN-kapcsolaton keresztül telepített, akkor a belső IP-címet használ, amely a virtuális gépen található hello-kiszolgálóhoz **irányítópult** fülre, és a port 22 tooconnect toohello Linuxos fő célkiszolgáló Secure Shell használatával.
5. Ha toohello Linux fő célkiszolgáló nyilvános internetkapcsolaton keresztül hello Linux fő célkiszolgáló nyilvános virtuális IP-cím használatára (hello virtuális gépekről **irányítópult** lapon) és a nyilvános végpontot hello létre ssh toologin toohello Linux servder.
6. Nyerje ki hello fájlok hello gzipped Linux fő Server telepítő bont archívumból való futtatásával: *"bont – xvzf Microsoft-ASR\_EE\_8.2.0.0\_bites RHEL6-64\"* hello könyvtárból, hello telepítőfájl tartalmazza.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)
7. Ha Ön kibontott hello telepítő fájlok tooa másik címtárhoz módosítása toohello könyvtár toowhich hello tartalma hello bont archívum könyvtárban találhatók. A könyvtár elérési útja, futtassa a "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)
8. Ha felszólító toochoose elsődleges szerepkör kiválasztása **2 (fő célkiszolgáló)**. Hello egyéb interaktív telepítés beállítása hagyja az alapértelmezett értékekre.
9. Várja meg a telepítési toocontinue és hello gazdagép konfigurációs felület tooappear. a gazdagép konfigurálása segédprogram hello Linux fő sarget Server hello parancssori segédprogram. Nem átméretezése hello ssh ügyfél segédprogram ablak. Használjon hello nyíl billentyűk tooselect hello **globális** beállítás, és nyomja le az adja meg a billentyűzeten.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)
10. Hello mezőben **IP** hello konfigurációs kiszolgáló hello belső IP-címét adja meg (az hello található **irányítópult** lapon hello konfigurációs kiszolgáló virtuális gép) nyomja le az ENTER BILLENTYŰT. A **Port** meg **22** nyomja le az ENTER BILLENTYŰT.
11. Hagyja **használja HTTPS** , **Igen**, és nyomja le az ENTER BILLENTYŰT.
12. Adja meg a hello hello konfigurációs kiszolgálón létrehozott jelszót. A PUTTY ügyfél egy Windows gép toossh toohello Linux virtuális gép használata a Shift + Insert toopaste hello hello vágólap tartalmát is használhatja. Hello jelszót toohello helyi vágólapra Ctrl-C használatával másolja, majd a Shift + Insert toopaste használja azt. Nyomja le az ENTER BILLENTYŰT.
13. Hello jobbra mutató nyílra kulcs toonavigate tooquit használja, és nyomja le az ENTER BILLENTYŰT. Várja meg a telepítési toocomplete.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Ha valamilyen okból sikertelen tooregister a Linux fő toohello konfigurációs kiszolgálók teheti úgy újra futtatásával az állomás konfigurációs segédprogram (először tooset hozzáférési engedélyek toothis /usr/local/ASR/Vx/bin/hostconfigcli Directory felügyelőként chmod futtatásával).

#### <a name="validate-master-target-registration"></a>A fő célkiszolgáló regisztráció érvényesítése
Ellenőrizheti, hogy hello fő célkiszolgáló regisztrálása sikeres volt toohello konfigurációs kiszolgáló Azure Site Recovery-tárolóban > **konfigurációs kiszolgáló** > **kiszolgálóadatok**.

> [!NOTE]
> Miután regisztrálása hello fő célkiszolgálót, ha, amely a virtuális gép hello konfigurációs hibák merülnek fel lehet, hogy törölték az Azure-ból vagy végpontok nincsenek megfelelően konfigurálva, ennek az az oka bár hello fő konfigurációs észlelésekor hello Azure dndpoints által hello fő célkiszolgálót, az Azure-ban való telepítésekor ez nem egy a helyszíni fő célkiszolgáló kiszolgáló helyszíni igaz. Ez a feladat-visszavétel nem befolyásolják, és ezek a hibák figyelmen kívül hagyhatja.
>
>

## <a name="step-4-protect-hello-virtual-machines-toohello-on-premises-site"></a>4. lépés: Hello virtuális gépek toohello helyszíni hely védelmét.
Ahhoz, hogy a feladat-visszavételt szüksége tooprotect virtuális gépek toohello helyszíni hely.

### <a name="before-you-begin"></a>Előkészületek
Ha egy virtuális Gépet a tooAzure feladatátvételt, egy extra ideiglenes meghajtón a lapozófájlt ad hozzá. Ez az egy extra meghajtót, amely általában nincs szükség szerint a feladatait átadó virtuális gép, mert előfordulhat, hogy már rendelkezik egy dedikált a lapozófájlt a meghajtót. Fordított hello virtuális gépek védelmének megkezdése előtt győződjön meg arról, hogy a meghajtó offline állapotba kerül, hogy nem get védeni kell. Ehhez az alábbiak szerint:

1. Nyissa meg a számítógép-kezelés, majd válassza a tárolók kezelése, így hello lemezek online és a csatolt toohello gép felsorolja.
2. Válassza ki a hello mennyiségű ideiglenes lemezes csatolt toohello gép, és válassza ki a toobring offline.

### <a name="protect-hello-vms"></a>Hello virtuális gépek védelme
1. Hello Azure-portálon ellenőrizze, amely éppen feladatátvételt hello virtuális gép tooensure hello állapotát.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)
2. Indítsa el a vContinuum a számítógépen.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)
3. Kattintson a **új védelmi** hello műveletet a rendszer típusa, és a
4. Hello új ablakban nyissa meg a select hello **operációsrendszer-típus** > **részletek beszerzése** hello virtuális gépek biztonsági toofail keresi. A **elsődleges kiszolgálóadatok**, azonosításához, és válassza ki a megjeleníteni kívánt tooprotect virtuális gépek hello. Virtuális gépek hello vCenter állomásnév, amely a feladatátvétel előtt voltak találhatók.
5. Ha kiválasztja a virtuális gép tooprotect (és már nem tudott tooAzure keresztül) előugró ablak biztosít két bejegyzés hello virtuális gép. Ennek az az oka hello konfigurációs kiszolgáló észleli hello regisztrált virtuális gépek tooit két példánya. A hello a helyszíni virtuális gép, így megvédheti hello megfelelő virtuális gép tooremove hello bejegyzést kell. tooidentify hello megfelelő Azure virtuális gép bejegyzés itt jelentkezhet be hello Azure virtuális Gépen, és lépjen tooC:\Program fájlok (x86) \Microsoft Azure Site Recovery\Application Data\etc. A hello fájl drscout.conf azonosítják a hello állomás azonosítója. Hello vContinuum párbeszédpanelen, amelynek hello Állomásazonosító megtalálható hello VM hello bejegyzés megőrzése. Összes bejegyzés törlése. tooselect hello javítsa ki a virtuális gép, olvassa el a tooits IP-címet. hello IP cím tartomány helyszíni a helyszíni virtuális gép lesz hello.

   ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image23.png)
6. Hello vCenter Server hello virtuális gép leállítása. Hello virtuális gépek helyszíni is törölheti.
7. Hello a helyszíni fő Célkiszolgáló server toowhich azt szeretné, hogy a virtuális gépek tooprotect hello adja meg. toodo, toohello vCenter server toowhich toofail vissza szeretne csatlakozni.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
8. Jelölje be hello fő célkiszolgáló hello állomás toowhich alapján azt szeretné, hogy a virtuális gép toorecover hello.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
9. Adja meg az egyes virtuális gépek hello hello replikációs beállítás. toodo ezt kell tooselect hello helyreállítási ügyféloldali datastore toowhich hello állítja helyre a virtuális gépek. hello táblázat hello tooprovide szükséges az egyes virtuális gépek különböző beállításokat foglalja össze.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

   | **A beállítás** | **Javasolt érték beállítás** |
   | --- | --- |
   |  Folyamatok kiszolgáló IP-címe |Az Azure-ban telepített hello folyamatkiszolgáló kijelölése |
   |  Megőrzési mérete MB-ban | |
   |  Megőrzési idő értékét |1 |
   |  Nap, óra |Nap |
   |  Konzisztencia időköz |1 |
   |  Válassza ki a cél adattárhoz |hello hello helyreállítási helyen elérhető adattárolót. hello adattár kell elegendő területtel rendelkezik-e, és amelyen toorestore hello virtuális gép használni kívánt elérhető toohello ESX-gazdagép. |
10. Hello a virtuális gép tulajdonságok arányban fogják beszerezni után feladatátvételi tooon helyszíni hely hello konfigurálása. hello tulajdonságok hello a következő táblázat foglalja össze.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    | **Tulajdonság** | **Részletek** |
    | --- | --- |
    | Hálózati konfiguráció |Az egyes hálózati adapterekhez azt észlelte, jelölje ki, majd kattintson a **módosítás** tooconfigure hello feladat-visszavétel IP-cím hello virtuális géphez. |
    | Hardverkonfiguráció |Adja meg a hello Processzor- és a virtuális gép hello hello. Beállítások lehet alkalmazott tooall hello tooprotect kívánt virtuális gépeket. tooidentify hello hello CPU és memória tartozó helyes értékeket, tekintse meg a toohello IAAS virtuális gépek szerepkör méretét, és tekintse meg a hello magok száma és rendelt memória. |
    | Megjelenített név |Feladat-visszavétel tooon helyszíni, miután átnevezheti hello virtuális gépek formában fog hello vCenter készlet. hello alapértelmezett értéke hello virtuális gép számítógép állomásneve. tooidentify hello virtuális gép nevét, olvassa el a toohello VM lista hello védelmi csoportban. |
    | Hálózati Címfordítás beállításait |Részletes információkat az alábbi |

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>NAT-beállítások konfigurálása
1. a hello virtuális gépek védelmének tooenable, két kommunikációs csatorna létrehozott toobe kell. hello első csatorna hello virtuális gép és a folyamatkiszolgáló hello közé esik. Ez a csatorna hello adatokat gyűjti össze az hello virtuális gép, és elküldi azt melyik majd küld hello adatok toohello fő célkiszolgáló toohello folyamatkiszolgáló. Ha hello folyamatkiszolgáló és a védett hello virtuális gép toobe hello van azonos Azure virtuális hálózatban, akkor nem kell toouse NAT-beállításokat. Ellenkező esetben adja meg a NAT-beállításokat. Nézet hello nyilvános IP-cím hello folyamatkiszolgáló az Azure-ban.

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)
2. hello második csatorna hello folyamatkiszolgáló és fő célkiszolgáló hello közé esik. beállítás toouse NAT hello, vagy nem függ, hogy vannak-alapú VPN-kapcsolattal, vagy -en keresztüli kommunikációra hello internet. NAt ne válassza ki, ha VPN használ, de csak akkor, ha az internetkapcsolat használata.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)
3. Ha még nem törölt hello a helyszíni virtuális gépek meghatározott, és ha hello datastore hibás hátsó toostill hello régi VMDK tartalmaz, akkor kell tooensure adott hello feladat-visszavétel VM lekérdezi létre új helyen. toodo a select hello **speciális** beállításait, és adja meg egy másik mappa toorestore tooin **mappa beállításai**. Hagyja hello egyéb beállításokat, az alapértelmezett beállításokkal. Hello mappa beállításai tooall hello névkiszolgálók alkalmazni.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)
4. A készenléti ellenőrzés hello virtuális gépeknek kell lenniük készen toobe tooensure védett hátsó tooon helyszíni.

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)
5. Várjon, amíg az toocomplete. Ha az összes virtuális gép sikeresen kerül hello védelmi terv nevet is megadhat. Kattintson a **védelme** toobegin.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)
6. A folyamat állapotát a vContinuum figyelheti.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)
7. Hello lépés után **védelmi terv aktiválása** befejeződik a figyelheti a virtuális gépek védelmét hello Site Recovery portálon.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)
8. Hello pontos állapotát, ehhez kattintson a virtuális gép hello és a telepítés előrehaladását figyelési tekintheti meg.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-hello-failback-plan"></a>Hello feladat-visszavétel terv készítése
A feladat-visszavétel tervet, a vContinuum, így hello alkalmazás lehet a hibás hátsó toohello helyszíni hely bármikor előkészítheti. A helyreállítási terv olyan nagyon hasonló toohello helyreállítási tervek a Site Recovery szolgáltatásban.

1. Indítsa el a vContinuum, és válassza ki **tervek kezelése** > **helyreállítani.** Az összes hello csomagok, amelyek a virtuális gépek keresztül lettek használt toofail listája látható. Használhatja a hello toorecover tervek azonos.

   ![](./media/site-recovery-failback-azure-to-vmware/image37.png)
2. Válassza ki a hello védelmi terv és az összes hello toorecover azt a kívánt virtuális gépeket. Minden méretű kiválasztásakor további információt talál, többek között a hello cél ESX-kiszolgálói és hello VM forráslemez tekintheti meg. Kattintson a **következő** toobegin hello helyreállítása varázsló, és válassza ki a kívánt toorecover hello virtuális gépeket.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)
3. Visszaállíthat több beállítások alapján, de ajánlott **legújabb címke** válassza **alkalmaz a virtuális gép esetében** , hogy a legfrissebb adatok hello virtuális gépről hello tooensure fogja használni.
4. Futtassa a hello **készültségének ellenőrzése.** Ha hello jobb paraméterei konfigurált tooenable virtuális gép helyreállítási ezzel beadja. Kattintson a **következő** hello az ellenőrzés sikeres esetén. Ha nem hello a naplóban található, és hello hibák.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)
5. A **Virtuálisgép-konfiguráció** győződjön meg arról, hogy hello helyreállítási beállításai helyesen vannak-e beállítva. Ha szeretné hello virtuális gép beállításait módosíthatja.

   ![](./media/site-recovery-failback-azure-to-vmware/image40.png)
6. Tekintse át a virtuális gépek, amelyek állítja helyre, és adjon meg egy helyreállítási sorrendben hello listáját. Vegye figyelembe, hogy a virtuális gépek találhatók, hello gazdaszámítógép-név használatával. Előfordulhat, hogy nehéz toomap hello számítógép állomás neve toohello virtuális gép.
   toomap hello nevek, nyissa meg toohello virtuális gépek **irányítópult** az Azure és az ellenőrzés hello virtuális gép állomásnevét.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)
7. Adja meg a csomag nevét, és válassza ki **későbbi helyreállítás**. Ajánlott toorecover később óta hello kezdeti védelem hiányosak lehetnek.
8. Kattintson a **helyreállítása** toosave hello terv vagy eseményindító hello helyreállítási most és később nem toorecover beállítás esetén. Ha hello terv által mentett hello helyreállítási állapot toosee ellenőrizheti.

   ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Virtuális gépek helyreállítása
Miután hello terv által létrehozott, akkor helyreállíthatja hello virtuális gépek. Ahhoz, hogy ellenőrizze, hogy virtuális hello gépek szinkronizálás sikeresen befejeződött. Ha a replikációs állapota OK hello védelmi befejeződött, és hello helyreállítási Időkorlát küszöbértéke teljesülnek. Állapotfigyelő hello virtuális gép tulajdonságai ellenőrizheti.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Kapcsolja ki a hello Azure virtuális gépek hello helyreállítási elindítása előtt. Ez biztosítja, nem maszkolt van, és, hogy a felhasználók csak érik el hello alkalmazás egy példánya.

1. Indítsa el a terv mentett hello. Válassza ki a vContinuum **figyelő** hello csomagok. A parancs megjeleníti az összes hello terveket tartalmazhatnak, amelyek futtatták.

   ![](./media/site-recovery-failback-azure-to-vmware/image45.png)
2. A SELECT hello terv **helyreállítási** kattintson **Start**. Helyreállítási figyelheti. Miután a virtuális gépek van kapcsolva a csatlakoztathatja a vCenter toothem.

   ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-tooazure-again-after-failback"></a>Feladat-visszavétel után újra tooAzure védelme
Feladat-visszavétel befejezése után érdemes tooprotect hello virtuális gépeket újra. Ehhez az alábbiak szerint:

1. Ellenőrizze, hogy hello virtuális gépek helyszíni dolgozik, és hogy érhetők el alkalmazások.
2. Hello Azure Site Recovery portálon hello virtuális gépeket, és törölje őket. Válassza ki a toodisable hello hello virtuális gépek védelmét. Ez biztosítja, hogy hello virtuális gépek több védett.
3. Az Azure-ban az Azure virtuális gépek a feladatátvételt hello törlése
4. Törölje a régi virtuális gép hello vSpehere. Ezek a hello virtuális gépeket, amely korábban feladatátvételt tooAzure.
5. Hello Site Recovery portálon hello nemrég átadja a virtuális gépek védelmét. Miután, hogy védettek felveheti azokat tooa helyreállítási terv.

## <a name="next-steps"></a>Következő lépések
* [További információ a](site-recovery-vmware-to-azure-classic.md) VMware virtuális gépek és fizikai kiszolgálók tooAzure fokozott hello központi telepítéssel végez replikációt.
