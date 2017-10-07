---
title: "az Azure (nagy példányok) SAP HANA SAP HANA aaaInstall |} Microsoft Docs"
description: "Hogyan tooinstall SAP HANA a egy SAP HANA Azure (nagy példány)."
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b2fe242270a1166cabcfae2f9249a8dd70ff3b93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-sap-hana-large-instances-on-azure"></a>Hogyan tooinstall és SAP HANA (nagy példányok) konfigurálása az Azure-on

Az alábbiakban néhány fontos definíciókat tooknow Ez az útmutató olvasása előtt. A [SAP HANA (nagy példányok) – áttekintés és Azure architektúra](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) HANA nagy példány egység, amelyek két különböző osztályú bevezetett azt:

- S72, S72m, S144, S144m, S192 és S192m, amelyek azt tooas hello "Típusú i. osztály" SKU.
- S384, S384m, S384xm, S576, S768 és S960, amely az tooas hello "Típusú class" SKU.

hello osztály megadása folyamatos toobe hello HANA dokumentáció tooeventually tekintse meg a toodifferent lehetőségei és követelményei alapján HANA nagy példány termékváltozatok nagy példány használatban.

Más definíciók gyakran használjuk a következők:
- **Nagy példány stamp:** hardver infrastruktúra verem egy SAP HANA TDI van hitelesített, és a dedikált toorun SAP HANA-példányok Azure-ban.
- **SAP HANA Azure (nagy példány):** hello ajánlat Azure toorun HANA-példány az SAP HANA TDI hardver, amely telepítve van a különböző Azure-régiók nagy példány bélyegzők hitelesített hivatalos nevét. hello kapcsolódó kifejezés **HANA nagy példány** rövid a SAP HANA Azure (nagy példányokat), és széles körben használt műszaki telepítési útmutatóban.


SAP HANA hello telepítését a feladata, és megkezdheti a hello tevékenység után egy új SAP HANA (nagy példányok) Azure-kiszolgálón a handoff számára. És az Azure VNet(s) és hello HANA nagy példány egység(ek) hello összekapcsolását kapott létrehozása után. 

> [!Note]
> E házirend SAP SAP HANA hello telepítése tooperform SAP HANA-telepítések hitelesített személy által hajtható végre. Egy személy, aki hello hitelesített SAP technológia társítása vizsgálatához, SAP HANA telepítési hitelesítő vizsgálatához, a rendszer megfelelt vagy egy SAP hitelesített rendszert integráló (SI).

Ellenőrizze újra, különösen akkor, ha a tervezési tooinstall HANA 2.0 [SAP támogatási Megjegyzés #2235581 - SAP HANA: támogatott operációs rendszerek](https://launchpad.support.sap.com/#/notes/2235581/E) sorrendben toomake meg arról, hogy hello támogatott operációs rendszer hello SAP HANA a kiadási meg tooinstall lezárását. A támogatott operációs rendszer HANA 2.0 több korlátozódik, mint a hello HANA 1.0 támogatott operációs rendszer hello valósíthat meg. 

## <a name="first-steps-after-receiving-hello-hana-large-instance-units"></a>Első lépések hello HANA nagy példány egység(ek) fogadása után

**Első lépés** fogadását követően hello HANA nagy példány pedig megállapítva hozzáférési és toohello példányok, tooregister hello OS hello példány a az operációs rendszer szolgáltatónál. Ez a lépés a SUSE Linux operációs rendszert futtató, amelyekre szüksége van az Azure virtuális gépen telepített toohave SUSE SMT példányának regisztrálása tartalmazhat. hello HANA nagy példány egység kapcsolódhatnak toothis SMT példány (lásd a jelen dokumentációban később). Vagy a RedHat OS toobe hello Red Hat előfizetés Manager való tooconnect kell regisztrálni kell. Lásd: is megjegyzések ezen [dokumentum](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ez a lépés is nem szükséges toobe képes toopatch hello az operációs rendszer. Ez a feladat hello felelőssége hello ügyfél van. A SUSE, dokumentáció tooinstall található, és konfiguráljon SMT [Itt](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**A második lépésben** toocheck új javítások és javításokat hello adott operációs rendszer kiadási vagy verzió van. Ellenőrizze, hogy hello HANA nagy példány hello javítási szintje a hello aktuális állapotát. A javítás/kiadások, és a változások toohello rendszerképbe Microsoft telepítheti időzítési alapul, néhány esetben előfordulhat ahol hello legújabb javítások nem kerülhet. Ezért fontos kötelező lépés után egy HANA nagy példány egység toocheck tart, hogy biztonsági, funkciókat, rendelkezésre állásának és teljesítményének a szükséges javítások időközben hello bizonyos Linux szállító által kiadott és toobe alkalmazni kell.

**Harmadik lépés** telepítési és konfigurálási SAP HANA-verzión hello adott operációs rendszer kiadási vagy az SAP vonatkozó megjegyzések toocheck hello ki van. Lejáró toochanging ajánlások vagy módosításokat tooSAP megjegyzések vagy egyedi telepítési forgatókönyvek függő konfigurációk, a Microsoft nem mindig lesz képes toohave tökéletesen konfigurált HANA nagy példány egység. Ezért fontos kötelező az Ön ügyfélként, tooread hello SAP megjegyzések kapcsolódó tooSAP HANA a pontos Linux kiadására. Is hello konfigurációk az operációs rendszer hello/kiadása szükséges, és hello konfigurációs beállítások alkalmazására, ha ezt még nem tette.

A specifikus ellenőrizze a következő paramétereket, és végül igazítva hello:

- NET.Core.rmem_max = 16777216
- NET.Core.wmem_max = 16777216
- NET.Core.rmem_default = 16777216
- NET.Core.wmem_default = 16777216
- NET.Core.optmem_max = 16777216
- NET.IPv4.tcp_rmem 65536 16777216 16777216 =
- NET.IPv4.tcp_wmem 65536 16777216 16777216 =

RHEL 7.2 és SLES12 SP1-től kezdődően ezeket a paramétereket kell állítani a konfigurációs fájlban hello /etc/sysctl.d könyvtárban. Például hello neve 91-NetApp-HANA.conf konfigurációs fájl kell létrehozni. A régebbi SLES és RHEL kiadásai ezek a paraméterek beállítása in/etc/sysctl.conf kell lennie.

Minden RHEL kiadott és hello SLES12 verziótól kezdődően 
- sunrpc.tcp_slot_table_entries = 128

a paraméter in/etc/modprobe.d/sunrpc-local.conf kell beállítani. Ha hello fájl nem létezik, akkor létre kell hozni hello a következő bejegyzés hozzáadásával: 
- beállítások sunrpc tcp_max_slot_table_entries = 128

**Negyedik lépés** toocheck hello rendszeridő HANA nagy példány egységének van. a rendszer időzóna, amelyek megfelelnek a hello Azure-régiót hello HANA nagy példány Stamp található hello helyét hello példányok telepítik. Jelenleg szabad toochange hello rendszeridő vagy időzóna hello példányok Ön a tulajdonosa. Ennek során, és a bérlői további példányokat rendelés, készüljön, hogy kell-e tooadapt hello időzónájának hello példányok újonnan kézbesíteni. A Microsoft operations nem állít be hello osztályt után hello átadási hello időzónával betekintést rendelkezik. Ezért újonnan telepített példányok előfordulhat, hogy nem állítható be hello azonos időzóna szerint hello egyik értékre módosult. Emiatt feladata az ügyfél toocheck szerint, és szükség esetén alkalmazkodnak hello időzóna átadná hello előfordulás. 

**Ötödik** toocheck etc/hosts van. Hello paneleken átadná lekérni, mint rendelkeznek, különböző IP-címtartományból többféle célra (lásd a következő szakaszt). Ellenőrizze a hello etc/hosts fájlt. Azokban az esetekben, ahol egységek hozzáadja egy meglévő bérlői nem várt toohave stb/gazdagépeken az újonnan telepített hello rendszerek karban korábbi kézbesített rendszerek hello IP-címmel. Ezért az kerül, ügyfél toocheck hello megfelelő beállításaival, hogy egy újonnan telepített példányt interaktívan és a korábban telepített egységek a bérlő hello neveinek feloldásához. 

## <a name="networking"></a>Hálózat
Feltételezzük, hogy követték-e az Azure Vnetekhez tervezése, és csatlakoztatja a Vnetek toohello HANA nagy példányok, ezeket a dokumentumokat a hello javaslatait:

- [SAP HANA (nagy példány) – áttekintés és az Azure-architektúra](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP HANA (nagy példányok) infrastruktúra és az Azure-kapcsolat](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Néhány érdemes toomention hello egyetlen egységek hello hálózatokkal kapcsolatos részletek érhetők el. Minden HANA nagy példány egység két vagy három tootwo vagy hello egység három hálózati portok rendelt IP-címeket tartalmaz. Három IP-címek HANA kibővített konfigurációk és hello HANA replikációs forgatókönyvek szerepelnek. Hello IP-címek egyikére hozzárendelt hálózati hello egység hello hello ismertetett kiszolgáló IP-címkészlet nincs toohello [SAP HANA (nagy példány) – áttekintés és Azure architektúra](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

az egységek két IP-címek hozzárendelve hello terjesztési hasonlóan kell kinéznie:

- eth0.xx kell egy hozzárendelt IP-címet, amely a kiszolgáló IP-készlet címtartománya, hogy Ön által küldött tooMicrosoft hello kívül esik. Az IP-cím alkalmazzák az operációs rendszer hello/etc/hosts kezelésével.
- eth1.xx rendelkeznie kell egy hozzárendelt IP-címet, amely kommunikációs tooNFS szolgál. Ezért ezeknél a címeknél tegye **nem** toobe stb/gazdagépek rendelés tooallow példány tooinstance forgalom belül hello bérlői karbantartani kell.

Két IP-címek hozzárendelve a panel-konfiguráció nem megfelelő HANA replikációs vagy HANA kibővített esetekben. Ha rendelt csak két IP-címekre, és meg van toodeploy ilyen konfiguráció, forduljon az Azure szolgáltatásfelügyelet tooget SAP HANA egy külső IP-címek hozzárendelve VLAN harmadik. A nagy példány HANA egységek három IP-címtartományból három hálózati portokra, hello következő használati szabályok vonatkoznak:

- eth0.xx kell egy hozzárendelt IP-címet, amely a kiszolgáló IP-készlet címtartománya, hogy Ön által küldött tooMicrosoft hello kívül esik. Ezért az IP-címet kell nem használható az operációs rendszer hello/etc/hosts kezelésével.
- eth1.xx rendelkeznie kell egy hozzárendelt IP-címet, amely kommunikációs tooNFS tárolására szolgál. Ezért az ilyen típusú cím nem fenn kell tartani a etc/hosts.
- eth2.xx kizárólag használt toobe őrzi meg a stb/állomások kommunikációhoz hello különböző példányai között kell lennie. Ezek a címek is használhatók lesznek az hello IP-címeket toobe őrzi meg a kibővített HANA konfigurációk IP-cím HANA hello csomópontok közötti konfigurációs használ.



## <a name="storage"></a>Storage

hello tárolási elrendezés SAP Hana Azure (nagy példányok) be van állítva az Azure szolgáltatásfelügyeleti ajánlott útmutatás, ahogy SAP keresztül SAP HANA [SAP HANA tárhellyel kapcsolatos követelmények](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) találhatók meg. a különböző HANA nagy példányok termékváltozatok a kapott részletes ismertetését lásd: hello hello más köteteket a nyers méretét hello [SAP HANA (nagy példány) – áttekintés és Azure architektúra](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

a következő táblázat hello hello elnevezési konvenciókat hello tároló kötetek listáját:

| Tárhely kihasználtsága | Csatlakoztatási neve | Kötet neve | 
| --- | --- | ---|
| HANA adatok | /Hana/Data/SID/mnt0000<m> | Tárolási IP: / hana_data_SID_mnt00001_tenant_vol |
| HANA-napló | /Hana/log/SID/mnt0000<m> | Tárolási IP: / hana_log_SID_mnt00001_tenant_vol |
| HANA napló biztonsági mentése | /Hana/log/backups | Tárolási IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| Megosztott HANA | /Hana/Shared/SID | Tárolási IP: / hana_shared_SID_mnt00001_tenant_vol/megosztott |
| usr/sap | /usr/SAP/SID | Tárolási IP: / hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Ha SID hello HANA példány Rendszerazonosító = 

És a bérlői egy belső számbavételi műveletek = a bérlő központi telepítésekor.

Ahogy látja, megosztott HANA és usr/sap megosztása hello ugyanazon a köteten. hello elnevezéseket a hello akkor csatlakozási hello Rendszerazonosító hello HANA példányok, valamint a hello csatlakoztatási számát tartalmazza. Méretezett telepítések esetén csak van egy csatlakoztatási, például a mnt00001. Kibővített telepítésben mutatunk be annyi csatlakoztatások, mivel vannak olyan munkavégző és fő csomópontok. Hello kibővített környezet, az adatok, a napló, a napló biztonsági mentési köteteken megosztott és csatolt tooeach csomópont hello kibővített konfigurációban. A konfigurációk több SAP példánya fut egy másik olyan kötetek készlete nem létezik és csatolt toohello HAN nagy példány egység.

Hello dokumentumban olvasható, és keresse meg a HANA nagy példány egység, akkor vegye figyelembe, hogy hello egységek kapható helyett korlátozó lemezkötetet HANA/adatok és, hogy a kötet HANA/naplómásolat van. hello miért hello HANA/adatok nagy méretű-e azt oka az, hogy az ügyfélként használ fel hello storage-pillanatfelvételekkel hello azonos lemezköteten. Ez azt jelenti, hogy hello további storage-pillanatfelvételekkel hajt végre, a kiosztott köteteket a pillanatképek fel több helyet hello. hello HANA/naplómásolat lemez nem gondolat toobe hello kötet tooput adatbázis a biztonsági másolatok. Akkor használja a biztonsági mentési kötet hello HANA tranzakciónapló biztonsági mentései méretű toobe. A jövőben hello tároló verziói pillanatfelvétel önkiszolgáló, azt az adott kötet toohave további által megcélzott gyakori pillanatképeket. És az, hogy több gyakran használják replikációk toohello vész helyreállítási hely ha megfelel toooption-hello HANA nagy példány infrastruktúra által biztosított hello vész-helyreállítási funkcióhoz. A részleteket a [SAP HANA (nagy példányok) magas rendelkezésre állás és vészhelyreállítás Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Ezenkívül toohello tárolási megadott, vásárolhat további tárolási kapacitás 1 TB-os lépésekben. A további tárhely, új kötetek tooa HANA nagy példányok lehet hozzáadni.

Bevezetés az Azure szolgáltatásfelügyelet az SAP HANA, során hello ügyfél határozza meg a felhasználói azonosító (UID) és a csoport azonosító (CSOPORTAZONOSÍTÓ) hello sidadm felhasználói és sapsys csoport (pl.: 1000,500) szükséges, hogy hello SAP HANA-rendszer telepítéskor ezek azonos értékeket fogja használni. Kívánt toodeploy több HANA példányt egy egységbe, több példányban kötetek (minden példány egy set) kap. Ennek eredményeképpen a központi telepítéskor kell toodefine:

- hello SID hello különböző HANA-példányok (sidadm származtatott belőle).
- Hello HANA példány a memória méretét. Mivel a hello memória mérete példányok száma minden egyes kötetet készletet definiálja a hello hello kötetek méretét.

A tárolási szolgáltató javaslatok hello alábbi csatlakozási beállítások vannak konfigurálva az összes csatlakoztatott kötet alapján (nem tartalmazza a rendszerindító LUN):

- NFS-RW lemezt használ, illesztők = 4, nehéz, timeo = 600, rsize = 1048576, wsize = 1048576, megszakítás, noatime, zárolása 0 0

A csatlakoztatási pontok vannak konfigurálva az/etc/fstab például a következő grafikus látható hello:

![a csatlakoztatott kötetek HANA nagy példány egység fstab](./media/hana-installation/image1_fstab.PNG)

hello kimeneti hello parancs df - h egy S72m HANA nagy példány egységen alábbihoz hasonlóan fog kinézni:

![a csatlakoztatott kötetek HANA nagy példány egység fstab](./media/hana-installation/image2_df_output.PNG)


hello tárolóvezérlő és hello nagy példány bélyegzők csomópontja a szinkronizált tooNTP kiszolgálók. Szinkronizálás hello SAP HANA Azure (nagy példányok) egységek és az Azure virtuális gépek NTP-kiszolgálóval együtt nincs jelentős mennyiségű időt eltéréseket azonban hello infrastruktúra és az Azure vagy nagy példány stamp hello számítási egység között kell lennie.

Rendelés toooptimize alatt használt SAP HANA-toohello tároló, a következő SAP HANA-konfigurációs paraméterek hello is be kell állítani:

- 128 max_parallel_io_requests
- a async_read_submit
- a async_write_submit_active
- minden async_write_submit_blocks
 
A SAP HANA 1.0-s verzió tooSPS12 fel, ezek a paraméterek állítható be hello SAP HANA-adatbázisból, hello telepítése során a [SAP Megjegyzés #2267798 - SAP HANA-adatbázisból hello konfigurálása](https://launchpad.support.sap.com/#/notes/2267798)

Emellett konfigurálhatja hello paramétereit hello SAP HANA-adatbázis telepítése után hello hdbparam keretrendszer használatával. 

A SAP HANA 2.0 hello hdbparam keretrendszer elavult. Ennek eredményeképpen hello paramétert kell beállítani az SQL-parancsok használatával. További információkért lásd: [SAP Megjegyzés #2399079: HANA 2 hdbparam felszámolása](https://launchpad.support.sap.com/#/notes/2399079).


## <a name="operating-system"></a>Operációs rendszer

Az operációsrendszer-lemezképek kézbesíteni hello lapozóterület értéke toohello szerint too2 GB [SAP támogatási Megjegyzés #1999997 - gyakran ismételt kérdések: SAP HANA memória](https://launchpad.support.sap.com/#/notes/1999997/E). Bármely másik beállítást kívánt igények toobe Ön által megadott ügyfélként.

[SUSE Linux Enterprise Server 12 SP1 SAP-alkalmazásokból](https://www.suse.com/products/sles-for-sap/hana) hello terjesztési Linux SAP Hana Azure (nagy példányok) telepítve van. Az adott terjesztési SAP-specifikus képességeket biztosít &quot;hello kezdő verzióról&quot; (beleértve az SAP futó SLES hatékonyan előre beállított paramétereinek).

Lásd: [erőforrás könyvtár-/ tanulmányok](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) hello SUSE webhelyen és [SAP SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) a hello SAP közösségi hálózati Állapotváltozás-számos hasznos források kapcsolódó toodeploying SLES (többek között a következőket hello beállításról a SAP HANA Magas rendelkezésre állású, biztonsági korlátozási adott tooSAP műveleteket, és több).

További és hasznos SAP SUSE kapcsolódó hivatkozások:

- [SAP HANA SUSE Linux webhelyen](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Az ajánlott eljárás az SAP: sorba helyezni replikációs – SAP NetWeaver a SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – SAP SLES védelmet](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (beleértve a SLES 12 rendszert az SAP-alkalmazások).

SAP támogatási megjegyzések alkalmazható tooimplementing SAP HANA a SLES 12:

- [SAP támogatási Megjegyzés #1944799 – SAP HANA-irányelvek SLES operációs rendszer telepítéséhez](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [SAP támogatási Megjegyzés #2205917 – SAP HANA DB ajánlott az operációs rendszer beállításait a SLES 12 SAP-alkalmazásokból](https://launchpad.support.sap.com/#/notes/2205917/E).
- [SAP támogatási Megjegyzés #1984787 – SUSE Linux Enterprise Server 12: Telepítési jegyzetek](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP támogatási Megjegyzés #171356 – SAP szoftverek Linux: általános információk](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP támogatási Megjegyzés #1391070 – Linux UUID megoldások](https://launchpad.support.sap.com/#/notes/1391070).

[Red Hat Enterprise Linux SAP Hana](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) egy másik ajánlat SAP HANA futtatási nagy HANA-példányokon. RHEL 6.7 és 7.2 kiadásaiban érhetők el. 

Red Hat kapcsolódó hivatkozások további és hasznos SAP:
- [SAP HANA a Red Hat Linux hely](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

SAP támogatási megjegyzések alkalmazható tooimplementing SAP HANA a Red Hat:

- [SAP támogatási Megjegyzés #2009879 - SAP HANA-irányelvek a Red Hat Enterprise Linux (RHEL) operációs rendszer](https://launchpad.support.sap.com/#/notes/2009879/E).
- [SAP támogatási #2292690 - SAP HANA DB Megjegyzés: az operációs rendszer ajánlott beállítások RHEL 7](https://launchpad.support.sap.com/#/notes/2292690).
- [SAP támogatási #2247020 - SAP HANA DB Megjegyzés: Ajánlott operációs rendszer beállításait RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [SAP támogatási Megjegyzés #1391070 – Linux UUID megoldások](https://launchpad.support.sap.com/#/notes/1391070).
- [SAP támogatási Megjegyzés #2228351 - Linux: SAP HANA-adatbázis szervizcsomag-verzió 11 változat 110 (vagy magasabb) RHEL 6 vagy SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [SAP támogatási #2397039 - gyakran ismételt kérdések Megjegyzés: Az RHEL SAP](https://launchpad.support.sap.com/#/notes/2397039).
- [SAP támogatási Megjegyzés #1496410 - Red Hat Enterprise Linux 6.x: telepítés és frissítés](https://launchpad.support.sap.com/#/notes/1496410).
- [SAP támogatási Megjegyzés #2002167 - Red Hat Enterprise Linux 7.x: telepítés és frissítés](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Időszinkronizálás

Hello SAP NetWeaver architektúra épülő SAP-alkalmazások esetében az időeltérést-és nagybetűket, a hello különböző összetevőből épül fel hello SAP rendszer. SAP ABAP rövid kiírja a hello hiba címe ZDATE\_nagy\_idő\_Különbözeti nem valószínűleg ismerős, mivel ezek rövid memóriaképek jelennek meg, ha hello rendszerideje különböző kiszolgálókra vagy a virtuális gépek felé sodródik túl távolságot.

Az Azure (nagy példányokat), az Azure nem &#39; időszinkronizálást SAP Hana t toohello számítási egység hello nagy példány bélyegzők alkalmazni. A szinkronizálás nem alkalmazható SAP-alkalmazásokból natív Azure virtuális gépeken futó Azure biztosítja a rendszer & #39, s idő megfelelően szinkronizálva. Emiatt egy külön idő server kell beállítani, amelyek segítségével Azure virtuális gépeken futó SAP alkalmazáskiszolgálók és hello SAP HANA-adatbázis használati ideje HANA nagy példányok futó példányok. hello tároló-infrastruktúra nagy példány bélyegzők az az idő szinkronizálva az NTP-kiszolgáló.

## <a name="setting-up-smt-server-for-suse-linux"></a>SUSE Linux SMT kiszolgáló beállítása
SAP HANA nagy példányok nem rendelkezik Internet közvetlen kapcsolatot toohello. Ezért ez nem egy egyszerű folyamat tooregister hello OS szolgáltató és toodownload ilyen egység és javításokat. SUSE Linux hello esetben egy megoldás lehet egy SMT kiszolgálót egy Azure virtuális gép tooset. Mivel hello Azure virtuális gép toobe üzemeltetett egy Azure virtuális hálózatot, amely csatlakoztatott toohello HANA nagy példány. Ilyen SMT kiszolgálóval hello HANA nagy példány egység sikerült regisztrálni és létesít a javítókészletek letöltéséhez. 

SUSE Ez a témakör egy nagyobb útmutató a [előfizetés felügyeleti eszköz a SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Egy SMT kiszolgáló, amely megfelel a hello feladat HANA nagy példány hello telepítésének előfeltétele, mint kell:

- Egy Azure virtuális hálózatot, amely csatlakoztatott toohello HANA nagy példány ER körön.
- Egy szervezet társított SUSE fiókot. Mivel a szervezet hello kellene toohave néhány érvényes SUSE előfizetés.

### <a name="installation-of-smt-server-on-azure-vm"></a>Az Azure virtuális gépen SMT kiszolgáló

Ebben a lépésben hello SMT kiszolgáló egy Azure virtuális gép telepítése. hello első mérték adattípusa a toohello toolog [SUSE ügyfél Center](https://scc.suse.com/)

Mivel van bejelentkezve, nyissa meg tooOrganization--> a szervezet hitelesítő adataival. Ezen részében keresse meg, amelyek a szükséges tooset hello SMT kiszolgáló hello hitelesítő adatokat.

hello harmadik lépése tooinstall hello Azure virtuális hálózatot az SUSE Linux virtuális gép. toodeploy hello virtuális gép, egy Azure SLES 12 SP2 gyűjtemény képe igénybe vehet. Hello központi telepítési folyamat során ne adja meg egy DNS-nevet, és ne használjon statikus IP-címek létrehozása ezen a képernyőn látható módon

![a virtuálisgép-telepítéshez SMT kiszolgáló](./media/hana-installation/image3_vm_deployment.png)

hello telepített virtuális gép egy kisebb méretű és kapott hello belső IP-címet a hello 10.34.1.4 az Azure virtuális hálózatot. A volt smtserver hello virtuális gép neve. Hello telepítést követően hello kapcsolat toohello HANA nagy példány egység(ek) be lett jelölve. Függ, hogyan vannak rendszerezve névfeloldás szükség lehet hello HANA nagy példány egység stb/állomás hello Azure virtuális gép tooconfigure feloldását. Vegyen fel egy további szabad toohello virtuális Gépet, amely toobe használt toohold hello javítások lesz. lehet, hogy hello rendszerindító lemezzel túl kicsi. Egy hello esetben hello lemez csatlakoztatott túl/srv/www/htdocs kapott, ahogy az alábbi képernyőfelvétel a hello. A 100 GB-os lemezenként elegendőnek kell lennie.

![a virtuálisgép-telepítéshez SMT kiszolgáló](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Jelentkezzen be toohello HANA nagy példány egység(ek), / etc/hosts karbantartása, és ellenőrizze, hogy hello Azure virtuális Gépet, amely toorun hello SMT server hello hálózaton keresztül kellene érhető el.

Ezt az ellenőrzést sikeresen megtörtént, után kell toohello Azure virtuális Gépet, amely hello SMT kiszolgálón fusson a toolog. Putty toolog toohello VM használ, ha szüksége tooexecute a parancssorrend a bash ablakban:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Ezek a parancsok végrehajtása után indítsa újra a bash tooactivate hello beállításait. Majd indítsa el a YAST.

YAST lépjen a karbantartási tooSoftware és smt keresése. Válassza ki a smt, amely automatikusan tooyast2-smt alább látható módon

![A yast SMT](./media/hana-installation/image5_smt_in_yast.PNG)


Fogadja el a hello smtserver telepítésre hello elemet. A telepítést követően nyissa meg toohello SMT kiszolgálókonfiguráció, és adjon meg hello szervezeti hitelesítő adatok hello SUSE ügyfél Center korábban kapott. Is adja meg az Azure virtuális gép állomásnevet hello SMT URL-címe. Ebben a bemutatóban volt https://smtserver hello következő grafikus megjelenő.

![SMT kiszolgáló konfigurációja](./media/hana-installation/image6_configuration_of_smtserver1.png)

Következő lépésként kell tootest e hello kapcsolat toohello SUSE ügyfél Center működik. Ahogy a következő grafikus hello bemutató esetben hello látni az működött.

![Teszt csatlakozás tooSUSE ügyfél Center](./media/hana-installation/image7_test_connect.png)

Egyszer hello SMT telepítő indul el, meg kell tooprovide jelszót. Mivel ez egy új telepítés, kell toodefine hello következő grafikus látható módon, hogy a jelszó.

![Az adatbázis jelszó megadása](./media/hana-installation/image8_define_db_passwd.PNG)

hello következő lépést akkor, ha egy tanúsítvány jön létre. Hello párbeszédpaneléről lépjen tovább látható, és hello lépést kell tenni.

![SMT server tanúsítvány létrehozása](./media/hana-installation/image9_certificate_creation.PNG)

Előfordulhat, hogy néhány percig, hello konfigurációs hello végén a "Futtatás szinkronizálásának ellenőrzése" hello lépésben töltött. Hello telepítéshez és konfiguráláshoz hello SMT kiszolgáló után hello directory tárház hello csatlakoztatási pont /srv/www/htdocs alatt keresse meg vagy és a tárház néhány alkönyvtárat. 

Indítsa újra a hello SMT kiszolgálót és a kapcsolódó szolgáltatások, a következő parancsokkal.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Töltse le a csomagok SMT-kiszolgálóra

Ha minden hello szolgáltatás újraindul, válassza ki a megfelelő csomagok használatával Yast SMT felügyeleti hello. hello csomag kijelölése hello operációsrendszer-lemezképek hello nagy HANA-példány kiszolgálójának és nem a SLES kiadási hello vagy hello virtuális gép futó hello SMT server verziója függ. Hello kiválasztására szolgáló képernyő megjelenítéséhez példát alább láthatók.

![Csomagok kiválasztása](./media/hana-installation/image10_select_packages.PNG)

Miután elkészült hello csomag választás, kell toostart hello kezdeti másolat hello válassza csomagok toohello SMT kiszolgáló állít be. Ezt a másolatot hello rendszerhéj használatával hello parancs smt-tükör alább látható módon elindul


![Csomagok tooSMT kiszolgáló letöltése](./media/hana-installation/image11_download_packages.PNG)

Fent látható, hello csomagok kell beolvasni értékeit másolja a program hello könyvtárak hello csatlakoztatási pont /srv/www/htdocs alatt hozott létre. A folyamat eltarthat egy ideig. Függ a kiválasztott csomagok száma, eltarthat tooone óráig vagy tovább.
Mivel ez a folyamat befejezése után kell toomove toohello SMT ügyféltelepítés. 

### <a name="set-up-hello-smt-client-on-hana-large-instance-units"></a>Hello SMT ügyfél HANA nagy példány egységeken beállítása

hello fogadják ebben az esetben olyan hello HANA nagy példány egységet. hello SMT kiszolgálóbeállítás hello parancsfájl clientSetup4SMT.sh másol hello Azure virtuális Gépen. Másolja a parancsfájlt, a parancsfájl toohello HANA nagy példány egység tooconnect tooyour SMT kiszolgálón keresztül. Indítsa el a hello parancsfájl hello -h lehetőséggel, és adjon neki a SMT kiszolgáló paraméter hello neveként. Az ebben a példában smtserver.

![SMT-ügyfél konfigurálása](./media/hana-installation/image12_configure_client.PNG)

Előfordulhat, hogy egy olyan forgatókönyvet, ahol hello hello ügyfél hello kiszolgálóról hello tanúsítvány betöltése sikerült, de hello regisztrálása sikertelen volt a lent látható módon.

![Ügyfél-regisztráció sikertelen lesz.](./media/hana-installation/image13_registration_failed.PNG)

Ha hello regisztrálása sikertelen volt, olvassa el ezt [SUSE támogatja a dokumentum](https://www.suse.com/de-de/support/kb/doc/?id=7006024) és hello lépéseit nem hajtható végre.

> [!IMPORTANT] 
> Kiszolgáló neve van szüksége a nagybetűk smtserver hello teljesen minősített tartománynevét nélkül a virtuális gép, hello tooprovide hello nevét. Csak hello virtuális gép neve működik. 

A lépések végrehajtása után kell-e a parancs következő hello HANA nagy példány egységen tooexecute hello

```
SUSEConnect –cleanup
```

> [!Note] 
> A tesztelés azt mindig kellett toowait adott lépés után néhány perc múlva. hello azonnali végrehajtás clientSetup4SMT.sh hello javító intézkedéseket ismertetett hello SUSE cikk, Miután befejeződött hello tanúsítvány nem lesz érvényes még üzeneteket. Általában 5 – 10 percig várakozik, és clientSetup4SMT.sh végrehajtása befejeződött, a sikeres ügyfél-konfigurációval.

Ha hello probléma van, akkor szükséges, hogy hello lépéseket hello SUSE cikk alapján toofix ütközött, újra kell toorestart clientSetup4SMT.sh hello HANA nagy példány egységben. Most azt sikeresen be kell fejeződnie alább látható módon.

![Ügyfél-regisztráció sikeres](./media/hana-installation/image14_finish_client_config.PNG)

Ebben a lépésben az hello SMT ügyfél hello HANA nagy példány egység tooconnect hello SMT kiszolgálón telepítette hello Azure virtuális Gépen, akkor a konfigurált. Most "zypper be" vagy "zypper az" tooinstall OS javítások végrehajtása tooHANA nagy példányok vagy további csomagok telepítése. Megértése, hogy csak kaphat a javítások hello SMT kiszolgálón letöltött előtt.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Egy SAP HANA-telepítést HANA nagy példányok – példa
Ez a szakasz bemutatja, hogyan tooinstall SAP HANA tartozó HANA nagy példány egységre. hello indítási állapotban van a következőhöz hasonló:

- A megadott Microsoft összes hello adatok toodeploy meg egy SAP HANA nagy példányát.
- A Microsoft hello SAP HANA nagy példány kapott.
- Létrehozott egy Azure virtuális hálózatot, amely csatlakoztatott tooyour helyi hálózat.
- Hello ExpressRotue áramkör HANA nagy példányok toohello a csatlakoztatott azonos Azure virtuális hálózatot.
- Telepítette az Azure virtuális gép HANA nagy példányok jump dobozban használják.
- Végzett meg arról, hogy csatlakozhat hello jump lista tooyour HANA nagy példány egysége, és ez fordítva is igaz.
- Hogy be van jelölve, hogy az összes szükséges csomagok hello és javítások vannak telepítve.
- Elolvasta hello SAP megjegyzések és a dokumentációra vonatkozó HANA hello használ, és ellenőrizze, hogy hello HANA kiadási választott hello az operációs rendszer kiadása támogatott operációs rendszer telepítését.

Mi megjelenik-e a következő feladatütemezések hello hello letöltési hello HANA telepítési csomagok toohello jump mezőben, a Windows operációs rendszer, hello csomagok toohello HANA nagy példány egység hello példányát és hello beállítása hello sorozatát ebben az esetben futó virtuális gép.

### <a name="download-of-hello-sap-hana-installation-bits"></a>Töltse le a hello SAP HANA telepítési bitek
Mivel hello HANA nagy példány egységek nincs közvetlen kapcsolat toohello internet, nem közvetlenül letöltött hello telepítőcsomagok SAP toohello HANA nagy példány VM. tooovercome hello hiányzó közvetlen internetkapcsolattal, meg kell hello jump mezőbe. Hello csomagok toohello jump mezőben VM letöltése.

Rendelés toodownload hello HANA telepítési csomagokat kell az SAP-felhasználó vagy más felhasználó, amely lehetővé teszi, hogy Ön tooaccess hello SAP piactér. Ebben a sorozatban képernyők végrehajtania a bejelentkezés után:

Nyissa meg túl[SAP szolgáltatás piactér](https://support.sap.com/en/index.html) > kattintson a szoftver letöltése > telepítés és frissítés > által alfabetikus > a H – SAP HANA Platform Edition > SAP HANA Platform Edition 2.0 > telepítési > letöltési hello fájlok

![Töltse le a HANA telepítése](./media/hana-installation/image16_download_hana.PNG)

Hello bemutató esetben SAP HANA 2.0 telepítőcsomagok töltöttük le. Hello Azure Ugrás mezőbe virtuális gép, bontsa ki hello önkicsomagoló archívumokat hello könyvtárba, alább látható módon.

![Bontsa ki a HANA telepítése](./media/hana-installation/image17_extract_hana.PNG)

Mivel hello archívum kibontása, másolja a hello kivonással hello esetben 51052030, toohello HANA nagy példány egység hello /hana/shared kötet egy olyan könyvtárba, létrehozta a fentiekben létrehozott hello könyvtárat.

> [!Important]
> Nem hello telepítőcsomagok átmásolja hello gyökér- vagy rendszerindító LUN óta terület korlátozva, és használják, valamint egyéb folyamatok toobe kell.


### <a name="install-sap-hana-on-hello-hana-large-instance-unit"></a>SAP HANA telepíthető hello HANA nagy példány egység
Az order tooinstall SAP HANA toolog a felhasználó legfelső szintű kell. Csak a legfelső szintű rendelkezik elegendő engedélyekkel tooinstall SAP HANA.
hello elsőként toodo szüksége tooset engedélyeinek hello directory be/hana/megosztott keresztül másolja. hello engedélyeket kell például tooset

```
chmod –R 744 <Installation bits folder>
```

Ha azt szeretné, hogy tooinstall SAP HANA grafikus hello segítségével a telepítő, hello gtk2 csomag igények toobe hello HANA nagy példányok telepítve. Ellenőrizze, hogy van-e telepítve a hello paranccsal

```
rpm –qa | grep gtk2
```

A további lépések azt is, amely tartalmazza hello SAP HANA-telepítő hello grafikus felhasználói felülettel. Következő lépésként hello telepítőkönyvtár állapotba, és keresse meg hello HDB_LCM_LINUX_X86_64 sub könyvtárba. Indítás

```
./hdblcmgui 
```
kívül könyvtárhoz. Most már első végigvezeti képernyők sorozatát ahol tooprovide hello adatokat kell hello telepítéséhez. Bemutatott hello esetben hello SAP HANA-kiszolgáló és a hello SAP HANA ügyféloldali összetevői telepíti azt. Ezért az érték a "SAP HANA-adatbázisból" alább látható módon

![Válassza ki a HANA telepítés](./media/hana-installation/image18_hana_selection.PNG)

A következő képernyőn hello hello lehetőséget választja "Új rendszer telepítéséhez"

![Válassza ki az új HANA-telepítés](./media/hana-installation/image19_select_new.PNG)

Ebben a lépésben után tooselect között számos további összetevők, továbbá telepítve kell toohello SAP HANA adatbázis-kiszolgáló.

![További HANA összetevők kiválasztása](./media/hana-installation/image20_select_components.PNG)

A jelen dokumentáció hello célra azt hello SAP HANA-ügyfél választott és hello SAP HANA Studio. Azt is telepíti a méretezett példánya. ezért a következő képernyőn hello kell toochoose "Egy állomásra rendszer" 

![Válassza ki a méretezett telepítése](./media/hana-installation/image21_single_host.PNG)

A következő képernyőn hello kell tooprovide néhány adat

![Adja meg az SAP HANA SID](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Mint HANA rendszer azonosítója (SID), szüksége tooprovide hello SID AZONOSÍTÓVAL, Microsoft ismertetett, amikor hello HANA nagy példány telepítési rendelt. Eltérő SID AZONOSÍTÓVAL kiválasztása révén hello telepítési hello különböző köteteken tooaccess engedély problémák miatt sikertelen

Telepítési könyvtár: használhatja hello /hana/shared könyvtár. Hello a következő lépésben szüksége tooprovide hello helyek hello HANA az adatfájlok és hello HANA naplófájlok


![Adja meg a HANA naplófájl helye](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Meg kell határozni, adatokat és a naplófájlok hello mellékelt már hello csatlakoztatási pontok hello hello képernyő kiválasztása előtt erre a képernyőre kiválasztott SID tartalmazó köteteket. Ha hello SID eltérés van a megadott, az üdvözlő képernyőt, mielőtt egy hello lépjen vissza, és állítsa be a hello SID toohello értéke hello csatlakozási pontjain lévő.

Hello a következő lépésben tekintse át a hello állomásnév, és végül javítható ki. 

![Tekintse át a gazdagép neve](./media/hana-installation/image24_review_host_name.PNG)

Hello a következő lépésben szükség tooMicrosoft megadott, amikor hello HANA nagy példány telepítési rendelt tooretrieve adatokat. 

![Adja meg a rendszer felhasználói sémabővítményét](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Tooprovide kell hello azonos felhasználói azonosító és felhasználói csoport azonosítója, meghatározott Microsoft akkor hello egység telepítési sorrend szerint. Ha Ön nem toogive hello nagyon ugyanazokat az azonosítókat, hello HANA nagy példány egységen hello SAP HANA telepítése sikertelen lesz.

Hello következő két képernyőn, amely jelenleg nem láthatók a jelen dokumentációban, tooprovide hello jelszó szükséges hello SAP HANA-adatbázisból és a hello jelszó hello sapadm, hello részeként telepíti SAP Állomásügynök használt hello rendszer felhasználója hello SAP HANA-adatbázispéldány.

Hello jelszót határozza meg, miután egy visszaigazoló képernyő jelenik meg. minden hello adat szerepel, és hello telepítésének ellenőrzése. A folyamatban lévő képernyőn, amely a dokumentumok hello telepítési folyamat, például a hello alatt

![Ellenőrizze a telepítési folyamat](./media/hana-installation/image27_show_progress.PNG)

Hello telepítés befejezése után, akkor például hello egyet a következő kép

![Telepítés befejeződött](./media/hana-installation/image28_install_finished.PNG)

Ezen a ponton hello SAP HANA-példányt kell működik és fut, és kész használatra. Az SAP HANA Studio képes tooconnect tooit kell lennie. Győződjön meg arról is, hogy ellenőrizze a legújabb javítások hello az SAP HANA, és ezeket a javításokat.
























































 







 




