---
title: "Gyors üzembe helyezés: Az Egypéldányos SAP HANA Azure virtuális gépek manuális telepítésére |} Microsoft Docs"
description: "Gyors üzembe helyezési útmutató az Egypéldányos SAP HANA manuális telepítése Azure virtuális gépeken"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 57b58b8e07379eed5641f5f89d55b38f52c69e44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Gyors üzembe helyezés: Egypéldányos SAP HANA Azure virtuális gépeken manuális telepítése
## <a name="introduction"></a>Bevezetés
Ez az útmutató segítséget nyújt a SAP NetWeaver 7.5 és SAP HANA 1.0 SP12 telepítésekor manuálisan egy egypéldányos SAP HANA az Azure virtuális gépek (VM) beállítása. hello Ez az útmutató elsősorban az Azure-on SAP HANA üzembe helyezni. SAP dokumentáció nem helyettesíti. 

>[!Note]
>Ez az útmutató ismerteti a SAP HANA Azure virtuális gépeken történő központi telepítéséhez. SAP HANA HANA nagy példányok történő telepítésével kapcsolatos információkért lásd: [SAP használata az Azure virtuális gépek (VM)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató feltételezi, hogy Ön ismeri a szolgáltató (IaaS) alapjait, az ilyen infrastruktúra:
 * Hogyan toodeploy virtuális gépek vagy virtuális hálózatok keresztül hello Azure-portálon vagy a PowerShell használatával.
 * hello Azure platformfüggetlen parancssori felület (CLI), beleértve a hello beállítás toouse JavaScript Object Notation (JSON) sablonok.

Ez az útmutató feltételezi, hogy jártas:
* SAP HANA és SAP NetWeaver és hogyan tooinstall helyszíni őket.
* Telepítése, és működési SAP HANA és az Azure-on az SAP alkalmazáspéldányok.
* hello következő fogalmak és eljárások:
   * SAP központi telepítésének tervezésében az Azure, beleértve az Azure virtuális hálózat tervezése és az Azure Storage használatáért. Lásd: [SAP NetWeaver az Azure virtuális gépek (VM) - tervezési és megvalósítási útmutató](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Központi telepítési alapelvei és módon toodeploy virtuális gépek Azure-ban. Lásd: [Azure virtuális gépek telepítése az SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Magas rendelkezésre állású SAP NetWeaver Asc (ABAP SAP központi szolgáltatások), a SCS (SAP központi szolgáltatások) és az Azure-on (kiértékelése fogadását rendezésének) felhasználók. Lásd: [magas rendelkezésre állás a SAP NetWeaver Azure virtuális gépeken](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Megtudhatja, hogyan tooimprove hatékonyságának, ami egy ASC/SCS Azure multi-SID telepítését. Lásd: [SAP NetWeaver multi-SID-konfiguráció létrehozása](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * SAP NetWeaver futó alapelvek alapján Linux-alapú virtuális gépek Azure-ban. Lásd: [SAP NetWeaver Microsoft Azure SUSE Linux virtuális gépeken futó](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Ez az útmutató az Azure virtuális gépek és a részletek a Linux beállításait a hogyan tooproperly csatlakoztatása az Azure storage lemezek tooLinux virtuális gépeket.

Most Azure virtuális gépek hitelesített SAP által SAP HANA méretezett konfigurációkat. Az SAP HANA-munkaterhelések kibővített konfigurációkat még nem támogatottak. SAP HANA magas rendelkezésre állású azokban az esetekben a méretezett konfigurációk, lásd: [SAP HANA Azure virtuális gépek (VM) a magas rendelkezésre állású](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

Ha tooget egy SAP HANA-példány vagy az S/4HANA, vagy a BW/4HANA rendszert telepíteni nagyon gyorsan időben törekszik, érdemes lehet hello használata [felhő készülék teszteléshez](http://cal.sap.com). Találhat, például SAP-CAL az Azure-on keresztül az S/4HANA rendszer telepítésével kapcsolatos [Ez az útmutató](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Toohave szüksége az Azure-előfizetés és az egy SAP felhasználók felhő készülék teszteléshez regisztrálhatók.

## <a name="additional-resources"></a>További források
### <a name="sap-hana-backup"></a>SAP HANA biztonsági mentése
Azure virtuális gépeken SAP HANA-adatbázisok biztonsági mentésével információkért lásd:
* [Biztonsági mentési útmutató SAP Hana Azure virtuális gépeken](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [A fájl szintjén SAP HANA Azure biztonsági mentés](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [A storage-pillanatfelvételekkel alapján SAP HANA biztonsági mentése](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

### <a name="sap-cloud-appliance-library"></a>SAP felhő készülék könyvtára
A felhő készülék teszteléshez toodeploy S/4HANA vagy BW/4HANA információkért lásd: [telepítése SAP S/4HANA vagy a Microsoft Azure BW/4HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA-támogatott operációs rendszerek
SAP HANA-támogatott operációs rendszereken információkért lásd: [SAP támogatási Megjegyzés #2235581 - SAP HANA: támogatott operációs rendszerek](https://launchpad.support.sap.com/#/notes/2235581/E). Azure virtuális gépek támogatják az említett operációs rendszerektől csak egy részét. hello következő operációs rendszerek nem támogatott toodeploy SAP HANA Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

SAP termékkel kapcsolatos további dokumentáció SAP HANA és a különböző Linux operációs rendszert lásd:

* [SAP támogatási Megjegyzés #171356 - SAP szoftverek Linux: általános információk](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP támogatási Megjegyzés #1944799 - SAP HANA-irányelvek SLES operációs rendszer telepítéséhez](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [SAP támogatási Megjegyzés #2205917 - SAP HANA DB ajánlott az operációs rendszer beállításait a SLES 12 SAP-alkalmazásokból](https://launchpad.support.sap.com/#/notes/2205917/E)
* [SAP támogatási Megjegyzés #1984787 - SUSE Linux Enterprise Server 12: Telepítési jegyzetek](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP támogatási Megjegyzés #1391070 - Linux UUID megoldások](https://launchpad.support.sap.com/#/notes/1391070)
* [SAP támogatási Megjegyzés #2009879 - SAP HANA-irányelvek Red Hat Enterprise Linux (RHEL) operációs rendszerhez](https://launchpad.support.sap.com/#/notes/2009879)
* [2292690 - SAP HANA DB: RHEL 7 operációs rendszer ajánlott beállításai](https://launchpad.support.sap.com/#/notes/2292690/E)

### <a name="sap-monitoring-in-azure"></a>SAP figyelése az Azure-ban
Az Azure-ban figyelési SAP kapcsolatos információkért lásd:

* [SAP Megjegyzés 2191498](https://launchpad.support.sap.com/#/notes/2191498/E). Ez a Megjegyzés ismerteti, amelyek SAP "bővített figyelés" Linux virtuális gépek Azure-on. 
* [SAP Megjegyzés 1102124](https://launchpad.support.sap.com/#/notes/1102124/E). Megjegyzés vehető fel Linux SAPOSCOL kapcsolatos tudnivalókat ismerteti. 
* [SAP Megjegyzés 2178632](https://launchpad.support.sap.com/#/notes/2178632/E). Ez a Megjegyzés SAP alapvető figyelési metrikákat a Microsoft Azure ismerteti.

### <a name="azure-vm-types"></a>Az Azure Virtuálisgép-típusokon
Az Azure Virtuálisgép-típusokon és munkaterhelés SAP-támogatott forgatókönyvek SAP HANA használt ismertetett [SAP hitelesített IaaS platformok](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

SAP NetWeaver SAP által hitelesített vagy S/4HANA alkalmazásréteg hello Azure Virtuálisgép-típusokon ismertetett [SAP Megjegyzés 1928533 - Azure SAP-alkalmazásokból: támogatott termékek és az Azure virtuális gép típusok](https://launchpad.support.sap.com/#/notes/1928533/E).

>[!Note]
>SAP-Linux-Azure integrációs csak Azure Resource Manager és klasszikus telepítési modell nem hello támogatott. 

## <a name="manual-installation-of-sap-hana"></a>SAP HANA manuális telepítése
Ez az útmutató ismerteti, hogyan toomanually telepítése SAP HANA Azure virtuális gépeken két különböző módon:

* A NetWeaver elosztott hello "telepítés adatbázispéldány" lépés a telepítés részeként SAP szoftver kiépítés Manager (SWPM) használatával
* SAP HANA hello segítségével adatbázis-életciklus manager eszközt, HDBLCM és majd a NetWeaver telepítése

Is használhatja SWPM tooinstall lévő valamennyi összetevőnél (SAP HANA hello SAP alkalmazáskiszolgáló és hello ASC példány) egy egyetlen virtuális gép, ez a [SAP HANA blog közlemény](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Ez a beállítás nem gyors üzembe helyezési útmutatóban ismertetett, de figyelembe kell venni problémák hello hello azonos.

Telepítés megkezdése előtt javasoljuk, hogy olvassa el a hello "előkészítése Azure virtuális gépek SAP HANA manuális telepítéséhez" szakasz az útmutató későbbi részében. Ezzel megakadályozhatja több alapvető, csak egy alapértelmezett Azure Virtuálisgép-konfiguráció használatakor előforduló hibákat.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Fontos lépések az SAP SWPM használatakor SAP HANA-telepítés
Ez a rész felsorolja a hello fontos lépések egy egypéldányos, manuális SAP HANA-példányhoz, SAP SWPM tooperform egy elosztott SAP NetWeaver 7.5 telepítés. hello egyes lépéseket képernyőképeket az útmutató részletesen ismerteti.

1. Hozzon létre egy Azure virtuális hálózat, amely két teszt virtuális gépek tartalmazza.
2. Központi telepítése a két hello Azure virtuális gépeken futó operációs rendszerek (a példánkban SUSE Linux Enterprise Server (SLES) és az SAP alkalmazások 12 SP1 SLES), toohello Azure Resource Manager modell szerint.
3. Két Azure standard vagy prémium szintű storage lemezek (például 75 GB-os vagy 500 GB-os lemezeken) toohello alkalmazáskiszolgáló VM csatolni.
4. Prémium szintű storage lemezek toohello HANA DB server rendszerű virtuális Géphez csatolása. További információkért lásd: hello "Lemez beállítása" című útmutatóban.
5. Méret vagy átviteli követelményeitől függően több lemezt csatolni, és hozza létre a csíkozott kötetek belül hello VM hello az operációs rendszer szintjén logikai kötetkezelés vagy egy több-eszközök felügyeleti eszköz (MDADM) használatával.
6. Hozzon létre XFS fájlrendszerek hello csatolt lemezekhez vagy logikai kötetek.
7. Csatlakoztassa a hello új XFS fájlrendszerek hello az operációs rendszer szintjén. Egy fájlrendszer használata az összes hello SAP szoftvert. Használja például hello hello /sapmnt directory és a biztonsági mentések, más fájlrendszere. Hello SAP HANA-adatbázis-kiszolgálón csatlakoztassa hello XFS fájlrendszerek hello prémium szintű storage lemezek /hana és /usr/sap. Ez a folyamat szükséges tooprevent hello legfelső szintű fájlrendszer, amely nem Azure virtuális gépeken Linux, nagy kitöltse.
8. Adjon meg helyi IP-címek hello hello teszt virtuális gépek hello/etc/hosts fájlt.
9. Adja meg a hello **nofail** hello/etc/fstab fájl paramétere.
10. Használja a toohello Linux operációsrendszer-kiadásnak megfelelő Linux kernel paraméterek beállítása. További információkért lásd: hello megfelelő SAP tudomásul veszi, hogy milyen HANA és hello "Kernel paraméterek" című útmutatóban.
11. A lapozóterület bővítése.
12. Szükség esetén telepítse a grafikus asztali hello teszt virtuális gépek. Ellenkező esetben használja a távoli SAPinst telepítés.
13. Töltse le a hello SAP szolgáltatás piactér hello SAP szoftvert.
14. Hello SAP ASC példány telepíthető hello app server rendszerű virtuális Géphez.
15. Megosztott hello /sapmnt könyvtár között hello tesztelése a virtuális gépek NFS használatával. hello alkalmazáskiszolgáló VM hello NFS-kiszolgáló.
16. Hello adatbázispéldány HANA, beleértve a SWPM hello VM DB kiszolgálón telepítse.
17. Telepítsen hello elsődleges alkalmazáskiszolgálót (szolgáltatói CÍMEKET) hello alkalmazáskiszolgálón virtuális gép.
18. Indítsa el az SAP Management Console (SAP MC). Csatlakozzon például SAP grafikus felhasználói felületen vagy HANA Studio.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Fontos lépések az SAP HANA-telepítés HDBLCM használatakor
Ez a rész felsorolja a hello fontos lépések egy egypéldányos, manuális SAP HANA-példányhoz, SAP HDBLCM tooperform egy elosztott SAP NetWeaver 7.5 telepítés. hello egyes lépéseket részletesen további képernyőfelvételeken ebben az útmutatóban.

1. Hozzon létre egy Azure virtuális hálózat, amely két teszt virtuális gépek tartalmazza.
2. Operációs rendszerek (a példánkban SLES és az SAP alkalmazások 12 SP1 SLES) két Azure virtuális gépek telepítése toohello Azure Resource Manager modellt alapján történik.
3. Két Azure standard vagy prémium szintű storage lemezek (például 75 GB-os vagy 500 GB-os lemezeken) toohello app server rendszerű virtuális Géphez csatolása.
4. Prémium szintű storage lemezek toohello HANA DB server rendszerű virtuális Géphez csatolása. További információkért lásd: hello "Lemez beállítása" című útmutatóban.
5. Méret vagy átviteli követelményeitől függően több lemez csatolja, és hozzon létre csíkozott kötetek vagy logikai kötetkezelés, vagy egy több-eszközök felügyeleti eszköz (MDADM) belül hello VM hello az operációs rendszer szintjén.
6. Hozzon létre XFS fájlrendszerek hello csatolt lemezekhez vagy logikai kötetek.
7. Csatlakoztassa a hello új XFS fájlrendszerek hello az operációs rendszer szintjén. Egy fájlrendszer használata az összes hello SAP szoftvert, és használja hello egy másikra hello /sapmnt directory és a biztonsági mentések, például. Hello SAP HANA-adatbázis-kiszolgálón csatlakoztassa hello XFS fájlrendszerek hello prémium szintű storage lemezek /hana és /usr/sap. A folyamat be nem szükséges toohelp hello legfelső szintű fájlrendszer, amely nem nagy Linux Azure virtuális gépeken, nehogy betelőben.
8. Adjon meg helyi IP-címek hello hello teszt virtuális gépek hello/etc/hosts fájlt.
9. Adja meg a hello **nofail** hello/etc/fstab fájl paramétere.
10. Használja a toohello Linux operációsrendszer-kiadásnak megfelelő kernel paraméterek beállítása. További információkért lásd: hello megfelelő SAP tudomásul veszi, hogy milyen HANA és hello "Kernel paraméterek" című útmutatóban.
11. A lapozóterület bővítése.
12. Szükség esetén telepítse a grafikus asztali hello teszt virtuális gépek. Ellenkező esetben használja a távoli SAPinst telepítés.
13. Töltse le a hello SAP szolgáltatás piactér hello SAP szoftvert.
14. Hozzon létre egy csoportot, sapsys, hello HANA DB kiszolgáló virtuális gép azonosítója 1001 csoportja.
15. SAP HANA hello DB server virtuális gép telepítése HANA adatbázis életciklusát kezelő (HDBLCM) segítségével.
16. Hello SAP ASC példány telepíthető hello app server rendszerű virtuális Géphez.
17. Megosztott hello /sapmnt könyvtár között hello tesztelése a virtuális gépek NFS használatával. hello alkalmazáskiszolgáló VM hello NFS-kiszolgáló.
18. Hello adatbázispéldány HANA, beleértve a SWPM hello VM HANA DB kiszolgálón telepítse.
19. Telepítsen hello elsődleges alkalmazáskiszolgálót (szolgáltatói CÍMEKET) hello alkalmazáskiszolgálón virtuális gép.
20. Indítsa el a SAP MC. SAP grafikus felhasználói felületének vagy HANA Studio keresztül csatlakozni.

## <a name="preparing-azure-vms-for-a-manual-installation-of-sap-hana"></a>Azure virtuális gépek SAP HANA kézi telepítésének előkészítése
Ez a rész a következő témakörök hello:

* Operációs rendszer frissítése érdekében
* Lemez beállítása
* Kernel-paraméterek
* fájlrendszer
* hello/etc/hosts fájlt
* hello/etc/fstab fájl

### <a name="os-updates"></a>Operációs rendszer frissítése érdekében
Ellenőrizze a Linux operációs rendszert futtató frissítések és javítások, további szoftverek telepítése előtt. A javítás telepítésével is képes tooavoid egy hívás toohello támogatási szolgálat.

Győződjön meg arról, hogy használ:
* SUSE Linux Enterprise Server SAP-alkalmazásokból.
* Red Hat Enterprise Linux az SAP-alkalmazásokból vagy a Red Hat Enterprise Linux SAP Hana. 

Ha még nem tette meg, regisztráljon hello az operációs rendszer telepítése a Linux-előfizetés hello Linux szállítótól. Vegye figyelembe, hogy SUSE OS képek SAP-alkalmazások, amelyek már tartalmaznak, és amelynek automatikusan regisztrálva.

Íme egy példa hello segítségével ellenőrzése a rendelkezésre álló javítások SUSE Linux **zypper** parancs:

 `sudo zypper list-patches`

Attól függően, hogy a probléma hello típusú kategória és súlyosság szerint besorolt javítások. A gyakran használt kategória értékei: **biztonsági**, **ajánlott**, **választható**, **szolgáltatás**, **dokumentum**, vagy **yast**.
A gyakran használt súlyossági értékei: **kritikus**, **fontos**, **mérsékelt**, **alacsony**, vagy **Meghatározatlan**.

Hello **zypper** parancs csak hello frissítés, amelyet a telepített csomagok kell keres. Például használhatja ezt a parancsot:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Hello paraméter is hozzáadhat `--dry-run` tootest hello frissítés hello rendszer ténylegesen frissítése nélkül.


### <a name="disk-setup"></a>Lemez beállítása
hello legfelső szintű fájlrendszert Azure Linux virtuális gép mérete korlátozás van érvényben. Ezért ez azért szükséges tooattach további szabad terület tooan Azure virtuális gép SAP futtatásához. A SAP application server Azure virtuális gépeken hello Azure standard szintű tárolást használatát is elegendő lehet. Azonban SAP HANA DBMS Azure virtuális gépek hello prémium szintű Azure Storage lemezek üzemi és tesztkörnyezetben megvalósítások használata kötelező.

Hello alapján [SAP HANA TDI tárhellyel kapcsolatos követelmények](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), prémium szintű Azure Storage-konfiguráció a következő hello javasolt: 

| VIRTUÁLIS GÉP TERMÉKVÁLTOZAT | RAM |  / hana/adatainak és naplókönyvtárainak/hana / <br /> csíkozott LVM vagy MDADM | / hana/megosztott | / root kötet | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

A hello javasolt lemezkonfiguráció hello HANA adatmennyiség és a naplózási kötet kerülnek, azonos beállítása a prémium szintű Azure storage lemezek LVM vagy MDADM csíkozott hello. Már nem szükséges toodefine bármely RAID redundancia szinthez, mivel a prémium szintű Azure Storage tartja a három képek hello lemezek a redundancia érdekében. toomake meg arról, hogy konfigurálja-e elegendő szabad lemezterület, tekintse át a hello [SAP HANA TDI tárhellyel kapcsolatos követelmények](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) és [SAP HANA-kiszolgáló telepítési és frissítési útmutató](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Is érdemes lehet hello különböző virtuális merevlemez (VHD) átviteli mennyiségű hello különböző a prémium szintű Azure storage lemezek dokumentált módon [prémium szintű Storage nagy teljesítményű és a virtuális gépek felügyelt lemezek](https://docs.microsoft.com/azure/storage/storage-premium-storage). 

Hozzáadhat további prémium szintű storage toohello HANA adatbázis-kezelő virtuális gépek lemezek adatbázis vagy a tranzakciós napló biztonsági mentések tárolására.

Hello használt két fő eszközök tooconfigure szétosztott kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [Szoftveres RAID Linux konfigurálása](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [A Linux virtuális gép az Azure-ban LVM konfigurálása](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

További információk a kapcsolódó lemezeken tooAzure virtuális gépeken Linux futtató egy vendég operációs rendszer, a következő témakörben: [adja hozzá a lemezt tooa Linux virtuális gép](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Prémium szintű Storage lehetővé teszi a gyorsítótárazást módok toodefine lemez. Csíkozott hello állítsa /hana/data és /hana/log lemez gyorsítótárazás le kell tiltani. A hello más kötetek (lemezeket) hello gyorsítótárazás módot azelőtt kell beállítani túl**ReadOnly**.

További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../../storage/common/storage-premium-storage.md).

toofind minta JSON sablonok létrehozásához a virtuális gépek, nyissa meg túl[Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates).
hello vm-egyszerű-sles sablon egy sablont. Ez magában foglalja a tárolás részén, az adatok 100 GB-os-lemezzel. Ez a sablon alapjául használható. Hello sablon tooyour konfigurációs módosíthatja.

>[!Note]
>Fontos tooattach hello Azure tároló lemez UUID segítségével dokumentált módon [SAP NetWeaver futtató Azure virtuális gépeken Microsoft SUSE Linux](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hello tesztkörnyezetben két Azure standard tárolólemezek csatolt toohello SAP alkalmazások kiszolgálói virtuális gép, volt, ahogy az alábbi képernyőfelvétel a hello. Egy lemez tárolt összes hello SAP szoftver (beleértve a NetWeaver 7.5, a SAP grafikus felhasználói felület és az SAP HANA) telepítéséhez. hello második lemez biztosítani, hogy elég szabad terület áll rendelkezésre további követelményeket (például a biztonsági mentés és a vizsgálati adatokat), és hello /sapmnt könyvtár (Ez azt jelenti, hogy SAP-profilok) toobe toohello tartozó összes virtuális gép között meg van osztva a azonos SAP fekvő.

![SAP HANA app kiszolgáló lemezek ablakot, két adatlemezek és azok mérete](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Kernel-paraméterek
SAP HANA szükséges adott Linux kernel beállítások, amelyek nem hello szabványos Azure katalógusában képek részét képezik, és manuálisan kell beállítani. Attól függően, hogy a hogy SUSE vagy a Red Hat hello paraméterek eltérő lehet. hello SAP megjegyzések korábban felsorolt biztosítják ezeket a paramétereket kapcsolatos információkat. Hello képernyőfelvételeken látható a SUSE Linux 12 SP1 lett megadva. 

Az SAP alkalmazások 12 GA SLES és az SAP alkalmazások 12 SP1 SLES rendelkezik egy új eszköz **beállított adm**, hogy cserél hello régi **sapconf** eszköz. A speciális SAP HANA-profil érhető el **beállított adm**. tootune hello rendszer SAP Hana, legfelső szintű felhasználóként hello alábbi beírásával:

   `tuned-adm profile sap-hana`

További információ **beállított adm**, lásd: hello [SUSE dokumentációjában az beállított adm](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

A következő képernyőkép hello, hogy hogyan **beállított adm** módosított hello `transparent_hugepage` és `numa_balancing` értékek szerint toohello kötelező SAP HANA-beállítások.

![hello beállított adm eszköz módosítja értékek szerint toorequired SAP HANA-beállítások](./media/hana-get-started/image005.jpg)

toomake hello SAP HANA kernel beállítások véglegesítéséhez, használjon **grub2** a SLES 12 rendszert. További információ **grub2**, toohello lépjen [konfigurációs fájlstruktúra](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) hello SUSE dokumentáció szakasza.

hello alábbi képernyőfelvételen bemutatja, hogyan hello kernel beállítások lettek hello konfigurációs fájl módosult és majd lefordítják azt használó **grub2-mkconfig**:

![Kernel beállítások hello konfigurációs fájl módosult, és fordítva grub2-mkconfig használatával](./media/hana-get-started/image006.jpg)

Másik lehetőség is toochange hello beállítások YaST és hello segítségével **Rendszertöltőt** > **Kernel paraméterek** beállítások:

![hello Kernel paraméterek beállítások lapján YaST Rendszertöltőt](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>fájlrendszer
hello következő képernyőfelvétel a hello SAP alkalmazáskiszolgálón VM fölött hello két csatolt Azure standard tárolólemezek létrehozott két fájlrendszerek. Mindkét fájlrendszerek XFS típusú és csatlakoztatott túl/sapdata és /sapsoftware.

Ez nem kötelező toostructure a fájlrendszerek, így van. Lehetősége van más hello lemezterület rendszerezésére szolgál. hello egyik legfontosabb szempont, hogy tooprevent hello legfelső szintű fájlrendszer kevés a szabad hely.

![Két fájlrendszerek kiszolgálón hello SAP app virtuális gép létrehozása](./media/hana-get-started/image008.jpg)

Hello SAP HANA DB VM adatbázis telepítés esetén SAPinst (SWPM) használatakor és hello **tipikus** telepítésen, minden /hana és /usr/sap telepítve van. hello alapértelmezett hello SAP HANA-napló biztonsági mentési helye /usr/sap alatt. Újra mert fontos tooprevent hello legfelső szintű fájlrendszer kevés a tárolási hely, győződjön meg arról, hogy van elég szabad hely a /hana és /usr/sap SAP HANA SWPM használatával telepítése előtt.

Egy SAP HANA-hello szabványos fájlrendszer elrendezését ismertetését lásd: hello [SAP HANA-kiszolgáló telepítési és frissítési útmutató](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![További fájlrendszerek kiszolgálón hello SAP app virtuális gép létrehozása](./media/hana-get-started/image009.jpg)

Amikor SAP NetWeaver telepít egy szabványos SLES/SLES SAP alkalmazások 12 Azure gyűjtemény lemezkép, egy üzenet jelenik meg, amely szerint a nincs swap lemezterület, ahogy az alábbi képernyőfelvétel a hello. toodismiss az üzenetre, kézzel is felveheti a lapozófájl használatával **nn**, **mkswap**, és **swapon**. toolearn hogyan, keresse meg "Lapozófájl kézi hozzáadása" hello a [Using hello YaST particionáló](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) hello SUSE dokumentáció szakasza.

Lehetősége tooconfigure lapozóterület hello Linux Virtuálisgép-ügynök használatával. További információkért lásd: hello [Azure Linux ügynök felhasználói útmutató](../../linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

![Előugró üzenet tájékoztatja, hogy nincs-e elegendő lapozóterület](./media/hana-get-started/image010.jpg)


### <a name="hello-etchosts-file"></a>hello/etc/hosts fájlt
Tooinstall SAP megkezdése előtt győződjön meg arról hello/etc/hosts fájl hello állomásnevet és hello SAP virtuális gépek IP-címét tartalmazza. Központi telepítése az összes hello SAP virtuális gépet egy Azure virtuális hálózaton belül, és a hello belső IP-címek, ahogy az itt látható:

![Hello/etc/hosts fájlban felsorolt állomásnevet és IP-címek hello SAP virtuális gépek](./media/hana-get-started/image011.jpg)

### <a name="hello-etcfstab-file"></a>hello/etc/fstab fájl

Hasznos tooadd hello **nofail** toohello fstab paraméterfájl. Így ha probléma merül fel hello lemezek hello virtuális gép nem lefagy hello rendszerindítási folyamat során. De ne feledje, hogy a megfelelő mennyiségű lemezterületet nem feltétlenül érhető el, és a folyamatok hello legfelső szintű fájlrendszer lehet feltöltve. Ha /hana hiányzik, a SAP HANA nem indul el.

![Hello nofail paraméter toohello fstab fájl hozzáadása](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Grafikus GNOME asztalt a SLES 12/SLES az SAP alkalmazások 12
Ez a rész a következő témakörök hello:

* Az SAP alkalmazások 12 SLES 12/SLES hello GNOME asztali és xrdp telepítését
* Java-alapú SAP MC Firefox segítségével SLES 12/SLES az SAP alkalmazások 12 futnak

Alternatív eszközökbe, például Xterminal vagy VNC (a jelen útmutató nem ismerteti) is használhatja.

### <a name="installing-hello-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Az SAP alkalmazások 12 SLES 12/SLES hello GNOME asztali és xrdp telepítését
Ha Windows háttér, könnyen hello SAP Linux virtuális gépek toorun Firefox, SAPinst, SAP grafikus felhasználói Felülettel, SAP MC vagy HANA Studio belül közvetlenül egy grafikus asztal, szabadon toohello VM hello Remote Desktop Protocol (RDP) Windows rendszerű számítógépről keresztül csatlakozni. Függő hozzáadásával kapcsolatban a vállalati házirendeknek grafikus felhasználói felületek tooproduction és nem éles Linux-alapú rendszerek, érdemes tooinstall GNOME a kiszolgálón. tooinstall hello GNOME asztalt a egy Azure SLES 12/SLES SAP alkalmazások 12 virtuális gép számára:

1. Hello GNOME asztali telepítéséhez írja be a következő parancs (például a PuTTY ablak) hello:

   `zypper in -t pattern gnome-basic`

2. Egy kapcsolat toohello virtuális gép RDP keresztül xrdp tooallow telepítése:

   `zypper in xrdp`

3. /Etc/sysconfig/windowmanager szerkesztheti, és állítsa be a hello alapértelmezett ablak manager tooGNOME:

   `DEFAULT_WM="gnome"`

4. Futtatás **chkconfig** toomake meg arról, hogy xrdp automatikusan elindul a rendszer újraindítása után:

   `chkconfig -level 3 xrdp on`

5. Ha távoli ASZTAL kapcsolaton keresztül hello problémát, próbálja toorestart (ablakból PuTTY, például):

   `/etc/xrdp/xrdp.sh restart`

6. Ha újra kell indítani az xrdp említett hello előző lépésben nem működik, ellenőrizze, hogy .pid fájl:

   `check /var/run` 

   Keressen `xrdp.pid`. Ha található, távolítsa el, és próbálkozzon újra a toorestart.

### <a name="starting-sap-mc"></a>SAP MC indítása
Hello GNOME asztali telepítése után grafikus hello indítása SAP MC Java-alapú a Firefox, az Azure SLES 12/SLES SAP alkalmazások 12 virtuális gép futtatása közben megjelenítheti hiba miatt hello hiányzik a Java-böngésző beépülő modul.

hello URL-cím toostart hello SAP MC van `<server>:5<instance_number>13`.

További információkért lásd: [webalapú SAP felügyeleti konzol indítása hello](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

hello alábbi képernyőfelvételen látható hello hibaüzenet jelenik meg, ha a Java-böngésző beépülő modul hello hiányzik:

![Hiányzó Java-böngésző beépülő modul utaló hibaüzenetet jelenít meg](./media/hana-get-started/image013.jpg)

Egyirányú toosolve hello probléma a hiányzó beépülő modul használatával YaST, ahogy az alábbi képernyőfelvétel a hello tooinstall hello:

![YaST tooinstall hiányzik a beépülő modul használatával](./media/hana-get-started/image014.jpg)

Újra meg hello SAP felügyeleti konzol URL-cím, egy üzenet jelenik meg azzal a kérdéssel, tooactivate hello beépülő modult:

![Beépülő modul aktiválást párbeszédpanel](./media/hana-get-started/image015.jpg)

Akkor is megjelenik egy hibaüzenet, a hiányzó fájlok javafx.properties. Ez az SAP grafikus felhasználói Felülettel 7.4 a kapcsolódó toohello Oracle Java 1.8 szükséglete. (Lásd: [SAP Megjegyzés 2059429](https://launchpad.support.sap.com/#/notes/2059424).) Nem hello IBM Java-verziót, és nem érkezik a SLES/SLES az SAP alkalmazások 12 hello openjdk csomag tartalmazza az hello szükséges javafx.properties fájlt. hello megoldás toodownload, és az Oracle Java használata 8 telepítése.

OpenSUSE openjdk hasonló hibával kapcsolatos információkért lásd: hello vitafórum szál [SAPGui 7.4 Java a openSUSE 42.1 Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Az SAP HANA manuális telepítésére: SWPM
Ebben a szakaszban példaként bemutató képernyőképeket láthat hello sorozatát hello fontos lépések az SAP NetWeaver 7.5-ös és az SAP HANA SP12 telepítése SWPM (SAPinst) használatakor jeleníti meg. A NetWeaver 7.5 telepítésének részeként SWPM is telepíthet hello HANA-adatbázisból egy példányban.

Egy minta tesztkörnyezetben telepítettük a csak egy speciális üzleti alkalmazás programozási (ABAP) alkalmazások kiszolgálói. Ahogy az alábbi képernyőfelvétel a hello, használtuk hello **rendszer elosztott** tooinstall hello ASC és egy Azure virtuális gép és SAP HANA elsődleges alkalmazáskiszolgáló-példányok hello adatbázis rendszert egy másik Azure virtuális gép lehetőséget.

![Asc és telepítik hello rendszer elosztott lehetőséget elsődleges alkalmazáskiszolgáló-példányok](./media/hana-get-started/image012.jpg)

Után hello ASC példány hello VM app kiszolgálón van telepítve és beállítva túl "zöld" hello SAP Management Console (a következő képernyőkép hello látható), a hello /sapmnt könyvtár (beleértve a hello SAP profil könyvtár) meg kell osztani hello SAP HANA-adatbázis-kiszolgálóval VM. hello DB telepítési lépés szükséges elérni a toothis adatait. hello legjobb módja tooprovide hozzáférés toouse NFS-SZEL YaST használatával állítható be.

![SAP felügyeleti konzol ábrázoló hello ASC példány hello VM app kiszolgálón telepített, és állítsa be a túl "zöld"](./media/hana-get-started/image016.jpg)

A hello alkalmazáskiszolgálón VM, hello/sapmnt directory kell megosztott NFS keresztül hello segítségével **rw** és **no_root_squash** beállítások. hello azok **ro** és **root_squash**, amely vezethet tooproblems hello adatbázispéldány telepítésekor.

![Hello /sapmnt könyvtár NFS keresztül megosztása hello rw és no_root_squash beállítások használatával](./media/hana-get-started/image017b.jpg)

Hello következő képernyőfelvételen látható, mert hello /sapmnt megosztás hello alkalmazások kiszolgálói virtuális gép be kell állítani hello VM SAP HANA-adatbázis kiszolgálón használatával **NFS-ügyfél** (és YaST).

![NFS-ügyfél segítségével konfigurált hello /sapmnt megosztás](./media/hana-get-started/image018b.jpg)

telepítés egy elosztott NetWeaver 7.5 tooperform (**adatbázispéldány**), a következő képernyőfelvételen látható hello, toohello SAP HANA DB server Virtuálisgép jelentkezni, és indítsa el a SWPM.

![Bejelentkezés toohello SAP HANA DB server rendszerű virtuális Géphez, majd SWPM elindításával adatbázispéldány telepítése](./media/hana-get-started/image019.jpg)

Miután kiválasztotta a **tipikus** telepítési és hello elérési toohello telepítési adathordozóról, adja meg, DB SID, hello állomásnév, hello példányszámának, és hello DB rendszer rendszergazdai jelszót.

![SAP HANA adatbázis rendszer rendszergazdai bejelentkezés hello lap](./media/hana-get-started/image035b.jpg)

Hello DBACOCKPIT séma hello jelszó:

![hello DBACOCKPIT séma hello jelszó-beviteli mezője](./media/hana-get-started/image036b.jpg)

Adjon meg egy kérdést, hello SAPABAP1 séma jelszót:

![Adjon meg egy kérdést hello SAPABAP1 séma jelszót](./media/hana-get-started/image037b.jpg)

Minden tevékenység befejezése után egy zöld pipa hello DB telepítési folyamat következő tooeach fázisában jelenik meg. üdvözlőüzenetére "végrehajtási a... Az adatbázis-példány befejeződött"jelenik meg.

![A feladat befejeződött ablak megerősítést kérő üzenet](./media/hana-get-started/image023.jpg)

A sikeres telepítés után a hello SAP felügyeleti konzol kell hello DB példányához, "zöld" megjelenítése és is hello SAP HANA folyamatokat (hdbindexserver, hdbcompileserver, és így tovább) teljes listáját jeleníti meg.

![SAP-felügyeleti konzol ablakot SAP HANA-folyamatok listája](./media/hana-get-started/image024.jpg)

hello alábbi képernyőfelvételen látható hello fájlstruktúra hello /hana/shared könyvtárban SWPM hello HANA telepítés során létrehozott hello részeit. Mivel nincs lehetőség toospecify egy másik elérési utat, akkor fontos toomount további lemezterületet hello /hana könyvtárában SAP HANA-telepítés hello előtt SWPM használatával. Ez megakadályozza, hogy hello legfelső szintű fájl rendszer fut, nincs elég szabad lemezterület.

![hello /hana/shared fájl könyvtárstruktúrát HANA telepítés során létrehozott](./media/hana-get-started/image025.jpg)

A képernyőfelvételen látható hello fájlstruktúra hello /usr/sap könyvtár:

![hello /usr/sap directory fájlstruktúra](./media/hana-get-started/image026.jpg)

elosztott hello ABAP telepítésének utolsó lépésében hello tooinstall hello elsődleges kiszolgáló alkalmazáspéldány:

![ABAP telepítési ábrázoló elsődleges kiszolgáló alkalmazáspéldány hello utolsó lépésként](./media/hana-get-started/image027b.jpg)

Miután hello elsődleges application server-példány és az SAP grafikus felhasználói Felülettel van telepítve, a hello **DBA Vezérlőpult** megfelelően befejezte a tranzakció tooconfirm, amely SAP HANA-telepítés hello:

![Erősítse meg a sikeres telepítés DBA vezérlőpultja ablakban](./media/hana-get-started/image028b.jpg)

Utolsó lépésként akkor előfordulhat, hogy szeretné, hogy toofirst telepítéshez HANA Studio hello SAP alkalmazások kiszolgálói virtuális gép, és csatlakoztassa hello VM DB kiszolgálón futó toohello SAP HANA-példányok:

![SAP HANA Studio hello SAP alkalmazások kiszolgálói virtuális gép telepítése](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Az SAP HANA manuális telepítésére: HDBLCM
Továbbá tooinstalling SAP HANA SWPM használatával egy elosztott telepítés részeként, a telepítése hello HANA önálló először HDBLCM használatával. SAP NetWeaver 7.5, például Ezután telepítheti. hello képernyőképeket ebben a szakaszban jelenjen meg ez a folyamat működéséről.

Hello HANA HDBLCM eszközzel kapcsolatos további információkért lásd:

* [Javítsa ki a tevékenység SAP HANA HDBLCM választhatja hello](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [SAP HANA életciklus felügyeleti eszközök](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)
* [SAP HANA-kiszolgáló telepítési és frissítési útmutató](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

az alapértelmezett érték tooavoid problémák csoport hello azonosító beállítása `\<HANA SID\>adm user` (hello HDBLCM eszköz által létrehozott), adjon meg egy új csoportot nevű `sapsys` Csoportazonosító használatával `1001` SAP HANA keresztül HDBLCM telepítése előtt:

![Új csoport "sapsys" segítségével meghatározott csoport azonosítója 1001](./media/hana-get-started/image030.jpg)

HDBLCM hello indításakor először egy egyszerű start menüben jelenik meg. Válassza ki a cikket 1, **telepítése új rendszer**, ahogy az alábbi képernyőfelvétel a hello:

!["Az új rendszer telepítése" beállítást hello HDBLCM start ablakban](./media/hana-get-started/image031.jpg)

hello alábbi képernyőfelvételen megjeleníti, hogy a korábban kiválasztott összes kulcs hello-beállítások.

> [!IMPORTANT]
> HANA napló, az adatkötetek, valamint hello telepítési útvonalat (/ hana/Ez a példa megosztott) és /usr/sap, nevű könyvtár nem lehet hello legfelső szintű fájl rendszer része. Ezeket a könyvtárakat, amelyek csatlakoztatott toohello (hello "lemez setup" szakaszban leírt) virtuális gép toohello Azure adatlemezek tartozik. Ez a megközelítés megakadályozza, hogy hello legfelső szintű fájlrendszer futtatásának nincs elég lemezterület. A következő képernyőkép hello, hogy adott hello HANA rendszergazda felhasználói Azonosítóval rendelkezik `1005` és hello része `sapsys` csoport (azonosító `1001`), amely hello telepítés előtt lett definiálva.

![Korábban kiválasztott összes fő SAP HANA összetevők listáját](./media/hana-get-started/image032.jpg)

Ellenőrizheti a hello `\<HANA SID\>adm user` (`azdadm` a következő képernyőkép hello) hello/etc/passwd directory részletes adatai:

![HANA \<HANA SID\>hello/etc/passwd directory felsorolt adm felhasználó részletei](./media/hana-get-started/image033.jpg)

Telepítése után SAP HANA HDBLCM használatával, hello fájlstruktúra SAP HANA Studio, láthatja, ahogy az alábbi képernyőfelvétel a hello. hello SAPABAP1 séma, hello SAP NetWeaver minden olyan táblát tartalmaz, amelyek még nem érhető el.

![Az SAP HANA Studio SAP HANA-fájlstruktúra](./media/hana-get-started/image034.jpg)

Miután telepítette a SAP HANA, a SAP NetWeaver utasítást is telepítheti. A következő képernyőfelvételen látható hello, mint hello telepítése történik, egy elosztott telepítés SWPM (hello előző szakaszban leírtak) használatával. Hello adatbázispéldány SWPM használatával történő telepítésekor meg hello ugyanazokat az adatokat (például állomásnév HANA SID és példányszámának) HDBLCM használatával. SWPM majd hello meglévő HANA telepítését használja, és hozzáadja a sémák.

![Egy elosztott telepítés SWPM használatával történik](./media/hana-get-started/image035b.jpg)

hello alábbi képernyőfelvételen látható hello SWPM telepítési lépés ahol hello DBACOCKPIT séma adatait adja meg:

![Ha DBACOCKPIT séma adatok hello SWPM telepítési lépés](./media/hana-get-started/image036b.jpg)

Adja meg a hello SAPABAP1 séma adatait:

![Hello SAPABAP1 séma kapcsolatos adatok megadása](./media/hana-get-started/image037b.jpg)

A rendszer hello SWPM adatbázis-példány telepítése után hello SAPABAP1 séma SAP HANA Studio látható:

![az SAP HANA Studio hello SAPABAP1 séma](./media/hana-get-started/image038b.jpg)

Végezetül hello SAP alkalmazások kiszolgálói és az SAP grafikus felhasználói felülettel módú telepítés befejezése után ellenőrizheti hello HANA DB példány hello segítségével **DBA Vezérlőpult** tranzakció:

![hello HANA DB példány ellenőrizni a hello DBA Vezérlőpult tranzakció](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>SAP szoftver letöltése
Ahogy az alábbi képernyőképek hello szoftver is letölthető hello szolgáltatás piactér SAP.

Töltse le a NetWeaver 7.5 Linux/Hana:

 ![SAP-szolgáltatás telepítése és frissítése ablakának NetWeaver 7.5 letöltése](./media/hana-get-started/image001.jpg)

Töltse le a HANA SP12 Platform Edition:

 ![SAP-szolgáltatás telepítése és frissítése ablakának HANA SP12 Platform Edition letöltése](./media/hana-get-started/image002.jpg)

