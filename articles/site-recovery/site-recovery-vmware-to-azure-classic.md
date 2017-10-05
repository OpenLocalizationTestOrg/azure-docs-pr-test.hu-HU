---
title: "VMware virtuális gépek és fizikai kiszolgálók replikálása az Azure a klasszikus portálon |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Site Recovery-bA a replikáció, feladatátvétel és a helyszíni VMware virtuális gépek és a windowsos/Linuxos fizikai kiszolgálók Azure-ba történő központi telepítéséről."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a9022c1f-43c1-4d38-841f-52540025fb46
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-vmware-to-azure
ms.openlocfilehash: 73c3fb5cf4056ddb9554f598ec7f173d81802f17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>VMware virtuális gépek és fizikai kiszolgálók replikálása Azure-bA az Azure Site Recovery szolgáltatással
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [A klasszikus portál](site-recovery-vmware-to-azure-classic.md)
> * [A klasszikus portálon (örökölt)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Az Azure Site Recovery szolgáltatás replikáció, feladatátvétel és helyreállítási virtuális gépek és fizikai kiszolgálók replikálásával hozzájárul az üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégiát. Gépek Azure-bA vagy egy másodlagos helyszíni adatközpontba replikálhatja. A gyors áttekintését lásd: [Mi az Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan:

* **VMware virtuális gépek replikálása az Azure-bA**: Site Recovery telepítése replikációs, feladatátvétel és helyreállítás a helyszíni VMware virtuális gépeket az Azure storage koordinálására.
* **Fizikai kiszolgálók replikálása az Azure-bA**: Azure Site Recovery koordinálja a replikációt, feladatátvételével és helyreállításával a helyszíni fizikai Windows és Linux kiszolgálók az Azure-ba történő telepítése.

> [!NOTE]
> Ez a cikk ismerteti az Azure-bA replikálni. Ha a kívánt VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók replikálása másodlagos adatközpontba, lásd: [hely helyreállítási VMware és VMware közötti](site-recovery-vmware-to-vmware.md).
>
>

Ez a cikk vagy a alsó megjegyzések vagy kérdések utáni a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Továbbfejlesztett üzemelő
Ez a cikk egy továbbfejlesztett üzemelő útmutatást tartalmaz a klasszikus Azure portálon. Azt javasoljuk, hogy az új telepítésére vonatkozó jelenlegi verzióját használja. Ha már telepítése után a korábbi régebbi verzióval, azt javasoljuk, hogy telepítse át az új verziót. Áttelepítési kapcsolatban bővebben lásd: [hely helyreállítási VMware-ből az Azure klasszikus örökölt](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

A továbbfejlesztett üzemelő a fő frissítés. Ez a fejlesztéseket hajtottunk összefoglalása:

* **Az Azure virtuális gépek infrastruktúra nélkül**: közvetlenül az Azure-tárfiók replikálja az adatokat. Ezenkívül nincs szükség semmilyen infrastruktúra beállításához virtuális gépek (például a konfigurációs kiszolgáló vagy a fő célkiszolgáló) replikációs és feladatátvételi a korábbi központi telepítés volt szükség szerint.  
* **Telepítési egyesített**: egyetlen telepítés egyszerű beállítás és méretezhetőséget biztosít a helyszíni összetevőket.
* **Üzembe**: az összes forgalom titkosítva van, és a replikációs felügyeleti üzeneteket küld HTTPS 443-as porton keresztül.
* **Helyreállítási pontok**: összeomlási és alkalmazáskonzisztens helyreállítási pontokat Windows és Linux környezetek támogatása, és mindkét egyetlen virtuális gép és a virtuális Gépre kiterjedő konzisztens konfigurációk támogatása.
* **Feladatátvételi teszt**: Azure befolyásolja a munkakörnyezetet vagy replikáció felfüggesztése nélkül feladatátvételi teszt nem zavaró támogatása.
* **Nem tervezett feladatátvétel**: nem tervezett feladatátvételt az Azure-ba, hogy automatikusan feladatátvétel előtt állítsa le a virtuális gépek kibővített támogatása.
* **Feladat-visszavétel**: csak a változási különbözeteket replikálja a a helyszíni hely integrált feladat-visszavétel.
* **a vSphere 6.0**: korlátozott mértékben támogatja a VMware vSphere 6.0 központi telepítéseket.

## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Hogyan segíti a Site Recovery védelmét a virtuális gépek és fizikai kiszolgálók?
* VMware-rendszergazdák konfigurálhatják a telephelytől távoli védelmet üzleti számítási feladatok és a VMware virtuális gépeken futó alkalmazások Azure védelméhez. Kiszolgáló kezelők az Azure-bA replikálhatja fizikai, helyszíni Windows és Linux-kiszolgálókon.
* Az Azure Site Recovery konzolján egyszerű üzembe helyezési és replikálásának, feladatátvételének és helyreállítási folyamatok kezelését egy központi helyen biztosítja.
* A vCenter-kiszolgáló által felügyelt VMware virtuális gépeket replikálja, ha a Site Recovery virtuális gépek automatikusan képes felderíteni. Ha gép ESXi-állomáson, a Site Recovery felderíti a virtuális gépek a gazdagépen.
* Egyszerű feladatátvétel a helyszíni infrastruktúra futtatja az Azure-ba, ha sikertelen biztonsági (visszaállítás) az Azure-ból a helyszíni hely kiszolgálóján VMware virtuális gép.
* Alkalmazások és szolgáltatások, amelyek több számítógépen is rétegzett csoport helyreállítási tervek konfigurálhatja. E terveket feladatátvételt, ha a Site Recovery a úgy, hogy ugyanazt a munkafolyamatot futtató gépek állíthatók helyre együtt konzisztens adatpont biztosít a virtuális Gépre kiterjedő konzisztencia.

## <a name="supported-operating-systems"></a>Támogatott operációs rendszerek
### <a name="windows-64-bit-only"></a>Windows (csak a 64 bites)
* Windows Server 2008 R2 SP1 +
* Windows Server 2012
* Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (csak a 64 bites)
* Red Hat Enterprise Linux 7.2, 6.7 és 7.1
* CentOS 6.5, 6.6, 6.7, 7.0, 7.1-es és 7.2
* Oracle Enterprise Linux 6.4 és a Red Hat kompatibilis kernel vagy az szoros vállalati Kernel Release 3 (UEK3) futtató 6.5
* SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Forgatókönyv-architektúra
A forgatókönyv összetevőket:

* **Egy helyszíni felügyeleti kiszolgáló**: A felügyeleti kiszolgáló futtatja a Site Recovery-összetevőkhöz:
  * **Konfigurációs kiszolgáló**: kommunikáció koordinálását, és kezeli az adatokat replikálás és helyreállítási folyamatok.
  * **Folyamatkiszolgáló**: replikációs átjáróként. Adatokat fogad védett forrásgépek; optimalizálja a gyorsítótárazás, tömörítés és titkosítás; és az Azure storage replikációs adatokat küld. Kezeli a védett gépekre a mobilitási szolgáltatás leküldéses telepítéséhez és a VMware virtuális gépek automatikus felderítést hajt végre.
  * **Fő célkiszolgáló**: az Azure-ból történő feladat-visszavétel során kezeli a replikációs adatokat.
    A felügyeleti kiszolgáló, amely csak kiszolgálóként működik a folyamat a telepítés méretezésére is telepíthet.
* **A mobilitási szolgáltatás**: Ez az összetevő minden gépet (VMware virtuális gép vagy fizikai kiszolgálón), az Azure-bA replikálni kívánt van telepítve. A gépen végbemenő adatírásokat, és továbbítja őket a folyamatkiszolgálónak.
* **Azure**: nem kell létrehozni a replikációs és feladatátvételi bármely Azure virtuális gépeken. A Site Recovery szolgáltatás adatkezelés kezeli, és közvetlenül az Azure storage replikálja az adatokat. A replikált Azure virtuális gépek vannak hoz létre automatikusan csak akkor, ha az Azure-bA feladatátvételt hajt végre. Azonban ha meg szeretné tudni az Azure-ból a helyszíni hely, akkor hozzon létre egy Azure virtuális gép folyamat kiszolgálója.

(Henry Robalino által létrehozott) a következő ábra bemutatja, hogyan működnek együtt ezeket az összetevőket:

![Összetevő-architektúra](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

## <a name="capacity-planning"></a>Kapacitástervezés
Amikor arra készül kapacitását, és itt meg kell gondolniuk:

* **A forráskörnyezettel**: kapacitástervezés vagy a VMware infrastructure és a forrás gép követelményeinek.
* **A felügyeleti kiszolgáló**: a Site Recovery összetevőt futtató helyszíni felügyeleti kiszolgálók tervezése.
* **Hálózati sávszélesség forrás és cél**: a forrás és az Azure közötti replikáció szükséges hálózati sávszélesség tervezése.

### <a name="source-environment-considerations"></a>Forrás környezettel kapcsolatos kérdések
* **Legfeljebb napi adatváltozási sebesség**: A védett gép csak egy folyamat-kiszolgálót is használhat. A folyamat egyetlen kiszolgáló kezelni tud a akár 2 TB naponta változását. Ebből kifolyólag 2 TB az adatok maximális napi adatváltozási sebessége, a védett gépek által támogatott.
* **Maximális átviteli sebesség**: egy tárfiókot az Azure-ban A replikált gép is tartozhatnak. Standard szintű tárfiók kezelni tud a legfeljebb 20 000 kérelmek / másodperc, és azt javasoljuk, hogy maradjon 20 000 IOPS száma a forrásgép között. Például ha a forrásgép 5 lemezzel rendelkezik, és egyes lemezek hoz létre a forrás 120 IOPS (8 KB-os méret), lesz lemez IOPS-korlát az 500-as Azure-ban. A szám a tárfiókok szükséges összes forrás IOPs/20 000 =.

### <a name="management-server-considerations"></a>Felügyeleti kiszolgáló kapcsolatos szempontok
A felügyeleti kiszolgálót a Site Recovery-összetevők, amelyek kezelik az adatok optimalizálása, replikációs és felügyeleti fut. Tudja kezelni a napi módosítási gyakorisága kapacitást az összes védett gépeken futó munkaterhelések között kell lennie, és folyamatosan replikálják az adatokat az Azure storage elegendő sávszélesség rendelkezik. Konkrétan:

* A folyamatkiszolgáló védett gépek kap replikációs adatokat, és optimalizálja a gyorsítótárazás, tömörítés és titkosítás Azure felé. A felügyeleti kiszolgáló elegendő erőforrással a feladatok végrehajtásához kell rendelkeznie.
* A folyamatkiszolgáló lemezalapú gyorsítótárban használja. Azt javasoljuk, hogy egy külön gyorsítótár lemez 600 GB vagy több kezelni a hálózati szűk vagy szolgáltatáskimaradás esetén tárolt adatok változásait. A telepítés során beállíthatja a gyorsítótár a meghajtón, amelyen legalább 5 GB tárterület érhető el, de 600 GB-os minimális ajánlott.
* Ajánlott eljárásként azt javasoljuk, hogy a felügyeleti kiszolgáló, a védeni kívánt gépek az ugyanazon a hálózaton és a LAN-szegmens található. Egy másik hálózaton található, de a védeni kívánt gépek 3. hálózati látható rá kell rendelkeznie.

Méretével kapcsolatos megfontolások a felügyeleti kiszolgálón az alábbi táblázat foglalja össze:

| **Felügyeleti kiszolgáló Processzora** | **Memória** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag) |16 GB |300 GB |500 GB vagy kevesebb |Ezekkel a beállításokkal a 100-nál kevesebb gépeket replikálhat a felügyeleti kiszolgáló központi telepítése. |
| 12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) |18 GB |600 GB |1 TB 500 GB |Ezekkel a beállításokkal a 100-150 gépeket replikálhat a felügyeleti kiszolgáló központi telepítése. |
| 16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag) |32 GB |1 TB |1 TB-os és 2 TB |Ezekkel a beállításokkal 150-200 gépeket replikálhat a felügyeleti kiszolgáló központi telepítése. |
| Egy másik folyamat-kiszolgáló központi telepítése | | |> 2 TB |Ha több mint 200 gépeket replikál, vagy ha napi módosítása aránya meghaladja a 2 TB további folyamat kiszolgálók telepítése |

Az elemek magyarázata:

* Minden forrásgép 100 GB, 3 lemezzel van konfigurálva.
* Összehasonlítási tárolása 8 SAS-meghajtókkal 10 000 fordulat/PERCES, RAID 10-es, gyorsítótár lemez mérések használtuk.

### <a name="network-bandwidth-from-source-to-target"></a>Hálózati sávszélesség forrás és cél
Ellenőrizze, hogy a sávszélesség, amelyek segítségével lenne szükséges, a kezdeti replikáció és a különbözeti replikálás számítása a [kapacitás planner eszköz](site-recovery-capacity-planner.md).

#### <a name="throttling-bandwidth-used-for-replication"></a>A replikáláshoz használt sávszélesség szabályozása
Azure-felhőbe replikált VMware forgalom végighalad egy adott folyamat kiszolgáló. A sávszélesség, amelyet a Site Recovery replikációs ezen a kiszolgálón az alábbiak szerint szabályozhatja:

1. Nyissa meg a Microsoft Azure Backup szolgáltatás MMC beépülő modul a fő felügyeleti kiszolgálón vagy egy felügyeleti kiszolgálón futó további kiépített folyamat kiszolgálók. Alapértelmezés szerint a Microsoft Azure Backup parancsikonja az asztalon jön létre. Vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin találja.
2. Kattintson a beépülő modul **Tulajdonságok módosítása** elemére.

    ![Sávszélesség-szabályozási sávszélesség tulajdonságainak módosítása](./media/site-recovery-vmware-to-azure-classic/throttle1.png)
3. Az a **sávszélesség-szabályozási** lapra, adja meg a sávszélesség, a Site Recovery replikáció és a megfelelő ütemezés használható.

    ![Replikáció sávszélesség szabályozása](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Sávszélesség-szabályozás a PowerShell használatával is beállíthatja. Íme egy példa:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>A sávszélesség-használat maximalizálva
A replikáció az Azure Site Recovery által használt sávszélesség növelése érdekében módosítani szeretné egy beállításkulcs megadásával.

A következő kulcs szabályozza / lemez replikálásához replikálása esetén használt szálak száma:

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 Ez a beállításkulcs egy szükségesnél több erőforrással ellátott hálózatban kell módosítható az alapértelmezett értékeit. Legfeljebb 32 támogatott.  

További információk a részletes kapacitásának megtervezéséről lásd: [Site Recovery kapacitás planner](site-recovery-capacity-planner.md).

### <a name="additional-process-servers"></a>További folyamat kiszolgálók
Ha több mint 200 gépek védeni kíván, vagy a napi adatváltozási sebesség nagyobb 2 TB vehet fel további kiszolgálókat a terhelés kezelésére. Horizontális, a következőket teheti:

* A felügyeleti kiszolgálók számának növeléséhez. Például védelmet biztosíthat a két felügyeleti kiszolgálóval legfeljebb 400 gépek.
* Adjon hozzá további folyamat kiszolgálókat, és használja, ezek helyett (vagy kívül) forgalmat fog kezelni a felügyeleti kiszolgálón.

Ez a táblázat egy olyan forgatókönyvet, amelyben:

* Az eredeti kiszolgáló beállítása csak egy konfigurációs kiszolgálóként használni.
* További folyamat kiszolgáló beállítása.
* Beállíthatja a további folyamatkiszolgálót szeretne használni a védett virtuális gépet.
* Minden védett forrásgép 100 GB három lemez van konfigurálva.

| **Eredeti felügyeleti kiszolgáló**<br/><br/>(konfigurációs kiszolgáló) | **További folyamatkiszolgáló** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB RAM |4 Vcpu (2 sockets * @ 2,5 GHz-es 2 mag), 8 GB RAM |300 GB |250 GB vagy kevesebb |85 vagy kevesebb gépek replikálhatja. |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB RAM |8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 12 GB RAM |600 GB |250 GB és 1 TB |Replikálhatja a 85-150 gépek. |
| 12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag), 18 GB RAM |12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) 24 GB RAM |1 TB |1 TB-os és 2 TB |150-225 gépek replikálhatja. |

A kiszolgálók méretezni hogyan attól függ, hogy egy méretezett és kibővített modell igény szerint. Vertikális felskálázás néhány nagy teljesítményű felügyeleti és folyamat-kiszolgálók üzembe helyezésével, vagy kevesebb erőforrást további kiszolgálók üzembe helyezésével kiterjesztése. Példa: Ha 220 gépek védelmére van szüksége, az alábbiak valamelyikét teheti:

* Konfigurálja az eredeti felügyeleti kiszolgáló 12 Vcpu és 18 GB RAM-MAL. Állítson be egy további folyamat kiszolgálót a 12 Vcpu és 24 GB RAM-MAL. Konfigurálja a védett gépek csak a további folyamatkiszolgálót szeretne használni.
* (2 darab 8 Vcpu, 16 GB RAM) két felügyeleti kiszolgáló és két további folyamat-kiszolgáló (1 x 8 Vcpu és 4vCPUs x 1 135 + 85 (220) gépek kezelésének) konfigurálása. Védett gépek csak a folyamat további kiszolgálók használatára konfigurálja.

Kövesse az utasításokat a [további folyamat-kiszolgálót telepíthet](#deploy-additional-process-servers) egy további folyamatkiszolgáló beállításához.

## <a name="before-you-start-deployment"></a>Mielőtt elkezdené a telepítést
A következő táblázat összefoglalja az előfeltételeket a forgatókönyv üzembe helyezését.

### <a name="azure-prerequisites"></a>Azure-előfeltételek
| **Előfeltétel** | **Részletek** |
| --- | --- |
| **Azure-fiók** |Szüksége van egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. További információ a Site Recovery díjszabásáról: [Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
| **Azure Storage** |A replikált adatok tárolásához Azure-tárfiókra van szükség. A replikált adatok az Azure storage tárolja, és az Azure virtuális gépeket hoz létre, ha feladatátvétel történik. <br/><br/>Ehhez [standard georedundáns tárfiókot kell használnia](../storage/storage-redundancy.md#geo-redundant-storage). A fiók a Site Recovery szolgáltatásnak ugyanabban a régióban legyen, és társítható ugyanahhoz az előfizetéshez. Prémium szintű tárfiókokba történő replikációt jelenleg nem támogatott, és használatuk kerülendő.<br/><br/>Nem támogatjuk áthelyezése storage-fiókok használatával létrehozott a [Azure-portálon](../storage/storage-create-storage-account.md) erőforráscsoportok között. További információkért lásd: [Microsoft Azure Storage bemutatása](../storage/storage-introduction.md).<br/><br/> |
| **Azure-hálózat** |Szüksége van egy, az Azure virtuális gépek tudnak csatlakozni feladatátvétel esetén az Azure virtuális hálózat. Az Azure virtuális hálózatnak és a Site Recovery-tárolónak ugyanabban a régióban kell elhelyezkednie.<br/><br/>VPN-en kell sikertelen vissza Azure feladatátvétel után, kapcsolat (vagy Azure ExpressRoute) beállítása a az Azure-hálózatot a helyszíni helyre. |

### <a name="on-premises-prerequisites"></a>Helyszíni előfeltételek
| **Előfeltétel** | **Részletek** |
| --- | --- |
| **Felügyeleti kiszolgáló** |Egy virtuális gép vagy fizikai kiszolgálón fut a helyi Windows 2012 R2 kiszolgálóra van szüksége. Összes az helyszíni Site Recovery-összetevők telepítve van a felügyeleti kiszolgálón.<br/><br/> Azt javasoljuk, hogy a kiszolgálón, a VMware virtuális gép magas rendelkezésre állású telepít. A feladat-visszavétel a helyszíni hely Azure-hoz mindig VMware virtuális gépek függetlenül attól, hogy nem sikerült virtuális gépek vagy fizikai kiszolgálók. Ha nem adja meg a felügyeleti kiszolgáló a VMware virtuális gépként, meg kell beállítani egy külön fő célkiszolgáló VMware virtuális gép feladat-visszavétel forgalom fogadására.<br/><br/>A kiszolgáló nem lehet tartományvezérlő.<br/><br/>A kiszolgálójának rendelkeznie kell egy statikus IP-címet.<br/><br/>A kiszolgáló állomásnevét kell 15 karakter vagy kevesebb.<br/><br/>Az operációs rendszer területi beállítása csak angol nyelvű kell.<br/><br/>A felügyeleti kiszolgáló Internet-hozzáférésre van szüksége.<br/><br/>Kimenő hozzáférést a kiszolgáló a következőképpen kell: a Site Recovery összetevőt (MySQL letöltése); a telepítés során a 80-as HTTP ideiglenes hozzáférés a folyamatban lévő kimenő hozzáférést a 443-as HTTPS replikációkezelés; a folyamatban lévő kimenő hozzáférést HTTPS 9443 replikálási forgalma (Ez a port módosítható).<br/><br/> Gondoskodjon arról, hogy az URL-címek elérhető a felügyeleti kiszolgálóról: <br/>- \*.hypervrecoverymanager.windowsazure.com<br/>- \*.accesscontrol.windows.net<br/>- \*.backup.windowsazure.com<br/>- \*.blob.core.windows.net<br/>- \*.store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/archives/MySQL-5.5/MySQL-5.5.37-Win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Ha a kiszolgáló IP-cím alapuló tűzfalszabályok működnek, ellenőrizze, hogy a szabályok engedélyezik-e a kommunikációt az Azure-bA. Engedélyeznie kell az [Azure Datacenter IP-tartományait](https://www.microsoft.com/download/details.aspx?id=41653) és a HTTPS-portot (443). Engedélyezett IP-címtartományok az előfizetés az Azure-régió, valamint az USA nyugati is kell. Az URL-cím [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") a MySQL letöltés. |
| **VMware vCenter/ESXi-állomáson** |Van szüksége egy vagy több VMware vSphere ESX/ESXi hipervizorok kezeléséhez a VMware virtuális gépeket ESX/ESXi 6.0, 5.5 vagy 5.1 fut a legújabb frissítésekkel.<br/><br/> Azt javasoljuk, hogy telepít egy VMware vCenter-kiszolgálót az ESXi-gazdagépek kezelése. Akkor kell futtatnia 6.0 vagy 5.5 vCenter verziója a legújabb frissítésekkel.<br/><br/>Ne feledje, hogy a Site Recovery nem támogatja az új vCenter vSphere 6.0 szolgáltatások például a kereszt-vCenter vMotion, a virtuális kötetek és a tárolási DRS. Webhely-helyreállítási támogatás korlátozódik, melyeket is 5.5 verziójában elérhető funkciókat. |
| **Védett gépek** |**Azure**<br/><br/>A védeni kívánt gépek meg kell felelnie az [Azure-Előfeltételek](site-recovery-prereq.md) Azure virtuális gépek létrehozásához.<br><br/>Ha azt szeretné, a feladatátvételt követően az Azure virtuális gépeken való csatlakozáshoz, engedélyezze a távoli asztali kapcsolatokat a helyi tűzfal szeretné.<br/><br/>A védett gépek önálló lemezeinek kapacitása nem haladhatja meg az 1023 GB-t. Egy virtuális gépen legfeljebb 64 lemez működhet (ami 64 TB kapacitást jelent). Ha 1 TB-nál nagyobb lemezek, érdemes lehet például az SQL Server Always On vagy Oracle Data Guard adatbázis-replikáció.<br/><br/>Lemezterület a rendszermeghajtón összetevő telepítéséhez legalább 2 GB.<br/><br/>Közös lemezes vendégfürtök nem használhatók. Ha a fürtözött központi telepítéssel rendelkezik, érdemes lehet például az SQL Server Always On vagy Oracle Data Guard adatbázis-replikáció.<br/><br/>Unified Extensible Firmware Interface (UEFI) / EFI típusú rendszerindítás nem használható.<br/><br/>Gépnevek (betűket, számokat és kötőjeleket) 1 és 63 karakter között kell tartalmaznia. A nevét kell kezdődnie, betűvel vagy számmal, és betűvel vagy számmal végződhet. A gép védelme, után módosíthatja a Azure neve.<br/><br/>**VMware virtuális gépek**<br/><br>VMware vSphere PowerCLI 6.0 telepíteni szeretné. a felügyeleti kiszolgálón (konfigurációs kiszolgáló).<br/><br/>VMware virtuális gépek védeni kívánt VMware-eszközök telepítve és fut kell rendelkeznie.<br/><br/>Ha a forrás virtuális gép hálózati adapterek összevonása, akkor alakítja át egyetlen hálózati adapter az Azure-bA a feladatátvételt követően.<br/><br/>Ha védett virtuális iSCSI-lemez, a Site Recovery alakítja át a védett virtuális gép iSCSI-lemez VHD-fájl Ha a virtuális gép átadja az Azure-bA. Az Azure virtuális gép iSCSI-tároló elérhető, ha azok csatlakozni iSCSI-tároló, és két lemez tulajdonképpen lásd: a VHD lemez az Azure virtuális gép és a forrás iSCSI-lemez. Ebben az esetben kell válassza le az iSCSI-tároló, amely akkor jelenik meg, az átvevő Azure virtuális gépen.<br/><br/>A VMware felhasználói engedélyek kapcsolatos további információk a Site Recovery van szüksége, tekintse meg [VMware vCenter hozzáférési engedélyeinek](#vmware-permissions-for-vcenter-access).<br/><br/> **Windows Server-gépek (a VMware virtuális gép vagy fizikai kiszolgálón)**<br/><br/>A kiszolgálón futnia kell egy támogatott 64 bites operációs rendszer: Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2: legalább SP1.<br/><br/>Az operációs rendszer telepítéséhez a C meghajtó, és az operációs rendszer lemezének Windows alaplemeznek kell lenniük. (Az operációs rendszer nem telepíthető a Windows dinamikus lemezeket.)<br/><br/>A Windows Server 2008 R2 gépek kell .NET-keretrendszer 3.5.1-es verziója telepítve van.<br/><br/>Meg kell adni egy rendszergazdai fiókot (a Windows-számítógép helyi rendszergazdának kell lennie) a Windows rendszerű kiszolgálókon a mobilitási szolgáltatás leküldéses telepítése. Ha a megadott fiók nem tartományi fiók, tiltsa le a távoli felhasználói hozzáférés-vezérlés a helyi számítógépen szeretné. További információkért lásd: [telepíteni a mobilitási szolgáltatás leküldéses telepítéshez](#install-the-mobility-service-with-push-installation).<br/><br/>A Site Recovery támogatja a virtuális gépek RDM-lemeze. A feladat-visszavétel során a Site Recovery szeretné újrafelhasználni a RDM-lemeze Ha az eredeti forrás virtuális gép és az RDM-lemeze elérhető. Nem érhetők el, feladat-visszavétel során, a Site Recovery hoz létre egy új VMDK-fájl az egyes lemezek.<br/><br/>**Linux-gépek**<br/><br/>Egy támogatott 64 bites operációs rendszerre van szüksége: Red Hat Enterprise Linux 6.7; Centos 6.5, 6.6 vagy 6.7; Oracle Enterprise Linux 6.4 vagy a Red Hat kompatibilis kernel vagy az szoros vállalati Kernel Release 3 (UEK3); 6.5 SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts fájlokat a védett gépek bejegyzéseit, amelyek társított összes hálózati adapter IP-címek hozzárendelését a helyi állomás nevét kell tartalmaznia. <br/><br/>Ha azt szeretné, a Secure Shell-ügyfél (ssh) használatával a feladatátvételt követően Linux operációs rendszert futtató Azure virtuális géphez való kapcsolódáshoz, győződjön meg arról, hogy a védett számítógépen a Secure Shell szolgáltatás rendszerindításkor az automatikus indításra van állítva, és, hogy a tűzfalszabályok engedélyezik-e az ssh hozzá való csatlakozást.<br/><br/>Védelem csak akkor engedélyezhető, a Linux-gépek a következő tárolóval: fájlrendszer (EXT3, ETX4, ReiserFS, XFS); A többutas szoftver-eszköz leképező (többutas); Kötetkezelő (LVM2). HP CCISS vezérlő tároló fizikai kiszolgálók nem támogatottak. SUSE Linux Enterprise Server 11 SP3 csak támogatott ReiserFS fájlrendszert.<br/><br/>A Site Recovery támogatja a virtuális gépek RDM-lemeze. Feladat-visszavétel Linux, során a Site Recovery nem használja fel újra az RDM-lemeze. Ehelyett létrehoz egy új VMDK fájlt mindegyik megfelelő RDM-lemeze számára. |

Csak a Linux virtuális gép: Győződjön meg arról, hogy a konfigurációs paraméterek lévő VMware virtuális gép állítsa be a disk.enableUUID=true beállítást. Ha a sor nem létezik, adja hozzá. Ez szükséges, hogy helyesen csatlakoztatja, adjon meg konzisztens UUID a vmdk-fájl. Ha ez a beállítás feladat-visszavétel, akkor teljes letöltésére akkor is, ha a virtuális gép rendelkezésre álló helyszíni. Ez a beállítás hozzáadásával biztosítja, hogy csak a változási különbözeteket átkerülnek a feladat-visszavétel során vissza.

## <a name="step-1-create-a-vault"></a>1. lépés: A tároló létrehozása
1. Jelentkezzen be az [Azure Portalra](https://manage.windowsazure.com/).
2. Bontsa ki a **adatszolgáltatások** > **Recovery Services** kattintson **Site Recovery-tároló**.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **Név** mezőben adjon meg egy, a tárolót azonosító rövid nevet.
5. A **Régió** mezőben válassza ki a tároló földrajzi régióját. Annak ellenőrzéséhez, régiók, lásd: [Azure Site Recovery díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kattintson a **Tároló létrehozása** elemre.
    ![Tároló létrehozása](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Az állapotsorban ellenőrizheti, hogy a tároló sikeresen létrejött-e. A tároló tulajdonosaként **aktív** a fő **Recovery Services** lap.

## <a name="step-2-set-up-an-azure-network"></a>2. lépés: Az Azure-hálózat beállítása
Beállítása az Azure-hálózatot, hogy az Azure virtuális gépek csatlakoznak-e a hálózati feladatátvétel után, és, hogy a feladat-visszavétel a helyszíni hely is a várt módon működik.

1. Válassza ki az Azure-portálon **virtuális hálózat létrehozása** , és adja meg a hálózati név, IP-címtartomány és alhálózat neve.
2. Ha kell tennie a feladat-visszavétel, VPN vagy ExpressRoute hozzáadása a hálózathoz. VPN/ExpressRoute feladatátvétel után is felveheti a hálózathoz.

Azure-hálózatok kapcsolatos további információkért lásd: [virtuális hálózatok áttekintése](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> A Site Recovery üzembe helyezéséhez használt hálózatok esetében a [Migration of networks](../azure-resource-manager/resource-group-move-resources.md) (Hálózatok áttelepítése) művelet az egy előfizetésen belüli erőforráscsoportok között vagy előfizetések között nem támogatott.
>
>

## <a name="step-3-install-the-vmware-components"></a>3. lépés: A VMware-összetevők telepítéséhez
Ha szeretné replikálni a VMware virtuális gépeket, a felügyeleti kiszolgálón kövesse az alábbi lépéseket:

1. [Töltse le](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) és VMware vSphere PowerCLI 6.0 telepítése.
2. Indítsa újra a kiszolgálót.

## <a name="step-4-download-a-vault-registration-key"></a>4. lépés: A tárolóbeli regisztrációs kulcs letöltése
1. A felügyeleti kiszolgálóról az Azure-ban nyissa meg a Site Recovery konzolján. Az a **Recovery Services** a tároló megnyitásához kattintson a **gyors üzembe helyezés** lap. Is megnyithatja a **gyors üzembe helyezés** ikonra kattintva bármikor lap.

    ![Gyors üzembe helyezési ikonra](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)
2. Az a **gyors üzembe helyezés** kattintson **Célerőforrások előkészítése** > **a regisztrációs kulcs letöltése**. A regisztrációs fájl automatikusan jön létre. Érvénytelen, így ezt követően öt napig.

## <a name="step-5-install-the-management-server"></a>5. lépés: A felügyeleti kiszolgáló telepítése
> [!TIP]
> Gondoskodjon arról, hogy az URL-címek elérhető a felügyeleti kiszolgálóról:
>
> * *.hypervrecoverymanager.windowsazure.com
> * *.accesscontrol.windows.net
> * *.backup.windowsazure.com
> * *.blob.core.windows.net
> * *.store.core.windows.net
> * https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi
> * https://www.msftncsi.com/ncsi.txt
>
>



>[!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Setup-Registration/player]



1. Az a **gyors üzembe helyezés** lapon, az egyesített telepítőfájljának letöltése a kiszolgálóra.
2. Futtassa a telepítőfájlt telepítő elindításához a **Site Recovery az egységes telepítő** varázsló.
3. Az **Előkészületek** területen válassza **A konfigurációs kiszolgáló és a folyamatkiszolgáló telepítése** lehetőséget.

   ![Előkészületek](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. A **külső szoftverlicenc**, kattintson a **elfogadom** letöltése és telepítése a MySQL.

    ![Külső gyártótól származó szoftverek](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)
5. A **regisztrációs**, keresse meg és jelölje ki a regisztrációs kulcsot letöltött a tárolóból.

    ![Regisztráció](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)
6. A **Internetbeállítások**, adja meg, hogy a konfigurációs kiszolgálón futó provider fog csatlakozni az Azure Site Recovery az interneten keresztül.

   * Ha a gépen jelenleg beállított proxyval szeretne csatlakozni, válassza a **Csatlakozás a meglévő proxybeállításokkal** lehetőséget.
   * Ha azt szeretné, hogy a provider közvetlenül kapcsolódjon, válassza ki a **kapcsolódás proxy nélkül közvetlenül**.
   * Ha a meglévő proxy hitelesítést igényel, vagy ha egyéni proxyt a szolgáltatói kapcsolat, válassza ki a használni kívánt **kapcsolódás egyéni proxybeállításokkal**.
     * Ha egyéni proxyt használ, szüksége lesz a cím, port és hitelesítő adatok megadásához.
     * Ha a proxy használata esetén kell már engedélyezte, hogy a következő URL-címek:
       * *.hypervrecoverymanager.windowsazure.com    
       * *.accesscontrol.windows.net
       * *.backup.windowsazure.com
       * *.blob.core.windows.net
       * *.store.core.windows.net

    ![Tűzfal](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

1. A **szükséges előfeltételek ellenőrzése**, a telepítő futtatásának ellenőrzi, hogy győződjön meg arról, hogy futtatható-e a telepítés.

    ![Előfeltételek](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Ha megjelenik egy figyelmeztetés a **globális időszinkron ellenőrzéséről**, ellenőrizze, hogy a rendszeróra ideje (a **Dátum és idő** beállítások) megegyeznek-e az időzónával.

     ![Idő szinkronizálási problémája](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

1. A **MySQL konfigurációs**, hozzon létre a hitelesítő adatokat a MySQL-kiszolgálópéldány telepítendő való bejelentkezéskor.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)
2. A **Környezet részletei** területen válassza ki, hogy kívánja-e replikálni a VMware virtuális gépeket. Ha, a telepítő ellenőrzi, hogy telepítve van-e a PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)
3. A **Telepítés helye** területen válassza ki, hová szeretné telepíteni a bináris fájlokat, és hol kívánja tárolni a gyorsítótárat. Egy legalább 5 GB szabad lemezterülettel rendelkező meghajtóra is kiválaszthatja, de azt javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad lemezterület.

   ![Telepítés helye](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)
4. A **Hálózatválasztás**, adja meg a figyelő (hálózati adapter és SSL-port) amelyre a konfigurációs kiszolgáló küld és fogad adatokat. Módosíthatja az alapértelmezett port (9443). Ezt a portot mellett 443-as portot a kiszolgáló koordinálja a replikálási műveletek használják. Ne használja a 443-as replikációs forgalom fogadására.

    ![Hálózat kiválasztása](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



1. Az **Összefoglalás** területen ellenőrizze az adatokat, majd kattintson a **Telepítés** gombra. A telepítés után a rendszer létrehoz egy hozzáférési kódot. Ez szüksége engedélyezze a replikálást, ezért másolja és tartsa biztonságos helyen.

   ![Összefoglalás](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)


> [!WARNING]
> A Microsoft Azure Site Recovery szolgáltatás Agent proxy be kell állítania.
> A telepítés befejezése után indítsa el a Microsoft Azure Recovery Services rendszerhéj a Windows Start menüből. A parancssorba a futtassa a következő parancsok futtatásával állítsa be a proxykiszolgáló beállításait.
>
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
>


### <a name="run-setup-from-the-command-line"></a>Futtassa a telepítőt a parancssorból
Emellett az egyesített varázsló futtatása a parancssorból, az alábbiak szerint:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Az elemek magyarázata:

* /ServerMode: Kötelező. Megadja, hogy a telepítés telepítse a konfigurációs és folyamat-kiszolgálók vagy csak a folyamatkiszolgáló (további folyamatok kiszolgálók telepítéséhez használt). Bemeneti értékek: CS, PS.
* InstallDrive: kötelező. Adja meg a mappát, ahol telepítve vannak a összetevői.
* / MySQLCredFilePath. Kötelező. Adja meg a MySQL hitelesítő adatait tároló egy fájl elérési útját. A fájl létrehozása sablon beolvasása.
* / VaultCredFilePath. Kötelező. A tároló hitelesítési adatait tartalmazó fájlt helye.
* /EnvType. Kötelező. A telepítés típusát. Értékek: VMware, NonVMware.
* /PSIP és /CSIP. Kötelező. A folyamatkiszolgáló és a konfigurációs kiszolgáló IP-címe.
* /PassphraseFilePath. Kötelező. A jelszó-fájl helyét.
* / ByPassProxy. Választható. Adja meg a felügyeleti kiszolgáló, amely kapcsolódik az Azure proxy nélkül.
* /ProxySettingsFilePath. Választható. Határozza meg az egyéni proxy (alapértelmezett proxy hitelesítést igényel a kiszolgálón), vagy egyéni proxy beállításait.

## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>6. lépés: A hitelesítő adatait a vCenter-kiszolgáló beállítása
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Discovery/player]
>
>

A folyamatkiszolgáló automatikusan felderíthetők a VMware vCenter-kiszolgáló által kezelt virtuális gépek. A automatikus felderítést a Site Recovery szüksége lesz a fiók és a hitelesítő adatait a vCenter-kiszolgáló által elérhető. Ez a nem megfelelő, ha csak fizikai kiszolgálókat replikál.

A fiók és a hitelesítő adatok megadása

1. A vCenter-kiszolgáló szerepkör létrehozása (**Azure_Site_Recovery**) a vCenter szinten a [szükséges engedélyek](#vmware-permissions-for-vcenter-access).
2. Rendelje hozzá a **Azure_Site_Recovery** egy vCenter felhasználói szerepkört.

   > [!NOTE]
   > Egy vCenter-felhasználói fiókot, amely rendelkezik a csak olvasható szerepkör futtathatja a feladatátvevő védett forrásgépek leállítása nélkül. Ha azt szeretné, ezeket a gépeket le kell állítania a Azure_Site_Recovery szerepkör lesz szüksége. Ha most csak telepít virtuális gépeket VMware az Azure-ba, és a feladat-visszavételt nem szükséges, a csak olvasható szerepkör is használhatók.
   >
   >
3. Adja hozzá a fiókot, nyissa meg a **cspsconfigtool**. A [telepítés helye] \home\svsystems\bin mappában érhető el, egy parancsikont az asztalon és található.
4. A **Manage Accounts** (Fiókok kezelése) lapon kattintson az **Add Account** (Fiók hozzáadása) lehetőségre.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic/credentials1.png)
5. A **fiókadatok**, vegye fel a vCenter server eléréséhez használt hitelesítő adatait. A fiók neve megjelenik a portálon több mint 15 percet is igénybe vehet. Azonnali frissítéséhez kattintson **frissítése** a a **konfigurációs kiszolgálók** fülre.

    ![Részletek](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>7. lépés: A vCenter-kiszolgáló és az ESXi-gazdagépek hozzáadása
Ha VMware virtuális gépeket replikál, kell hozzáadnia a vCenter-kiszolgáló (vagy ESXi-állomáson).

1. Az a **kiszolgálók** > **konfigurációs kiszolgálók** lapon jelölje be **vCenter-kiszolgáló hozzáadása**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)
2. Vegye fel a vCenter-kiszolgáló vagy ESXi-gazdagép részleteit, a fiókot, amelyet az előző lépésben a vCenter-kiszolgáló és a folyamatkiszolgálót, felderítéséhez a vCenter-kiszolgáló által felügyelt VMware virtuális gépek eléréséhez megadott neve. A vCenter-kiszolgáló vagy ESXi-állomáson, és a kiszolgáló, amelyen telepítve van a folyamatkiszolgáló ugyanahhoz a hálózathoz kell elhelyezni.

   > [!NOTE]
   > Hozzáadása a vCenter-kiszolgáló vagy ESXi-állomáson, egy olyan fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal a vCenter vagy a gazdagép-kiszolgálón, győződjön meg arról, hogy a vCenter vagy ESXi-fiókok jogosultak e engedélyezve: Datacenter, Datastore, mappa, Jost, hálózati, erőforráshoz, Virtuális gép, és elosztott kapcsoló vSphere. A vCenter-kiszolgálót kell a tárolási nézetek jogosultság engedélyezve van.
   >
   >

    ![VCenter-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)
3. Felderítési befejezése után a vCenter-kiszolgáló jelenik meg a **konfigurációs kiszolgálók** fülre.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)

## <a name="step-8-create-a-protection-group"></a>8. lépés: A védelmi csoport létrehozása
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Protection/player]
>
>

Védelmi csoportok virtuális gépek vagy fizikai kiszolgálók, amelyek azonos védelmi konfigurációval használatával védeni kívánt logikai csoportosításain alapulnak. A védelmi csoport védelmi beállításainak alkalmaz, és ezek a beállítások alkalmazása az összes gépekre (virtuális vagy fizikai) felvétele a csoportba.

1. Ugrás a **védett elemek** > **védelmi csoport** és az ikonra kattintva adja hozzá a védelmi csoport.

    ![Védelmi csoport létrehozása](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)
2. Az a **adja meg a védelmi csoport beállításainál** csoportjában adja meg a csoport nevét. Az a **a** legördülő listára, válassza ki a konfigurációs kiszolgáló, amelyen szeretné létrehozni a csoportot. **Cél** Microsoft Azure.

    ![Védelmi csoport beállításai](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)
3. Az a **adja meg a replikációs beállításokat** lapon, a replikációs beállítások konfigurálásához, amelyeket a csoport összes gépekhez használt portbesorolás.

    ![Védelmi csoport replikációs](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

   * **Több virtuális Gépre kiterjedő konzisztencia**: Ha bekapcsolja ezt a, azt közös alkalmazáskonzisztens helyreállítási pontokat hoz létre a védelmi csoport gépein. Ez a beállítás akkor a leghasznosabb, ha a védelmi csoportban lévő összes gép futnak ugyanaz az alkalmazás. Az azonos adatpont gépeire fogja visszaállítani. Akkor érhető el, hogy VMware virtuális gépek vagy fizikai kiszolgálók (Windows vagy Linux) replikál.
   * **Helyreállítási Időkorlát küszöbértéke**: a helyreállítási Időkorlát beállítása. Riasztások akkor jönnek létre, ha a folyamatos adatvédelemhez kapcsolódó replikáció túllépi a helyreállítási Időkorlát megadott küszöbértékét.
   * **Helyreállítási pontok megőrzésének ideje**: Adja meg az adatmegőrzési időtartam. Védett gépek ebben az ablakban belüli bármelyik pontra állíthatók helyre.
   * **Alkalmazáskonzisztens pillanatkép gyakorisága**: meghatározza, hogy milyen gyakran alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontok jöjjön létre.

A pipa jelre kiválasztásakor a védelmi csoport létrehozása a megadott névvel. Emellett egy második védelmi csoport létrehozása a nevű *nevű védelmi csoport*- feladat-visszavételre. A védelmi csoport akkor használatos, ha az Azure-bA a feladatátvételt követően visszaadják feladataikat a helyszíni hely. A védelmi csoportok szerint jönnek létre a figyelheti a **védett elemek** lap.

## <a name="step-9-install-the-mobility-service"></a>9. lépés: A mobilitási szolgáltatás telepítése
A virtuális gépek és fizikai kiszolgálók védelmének engedélyezése során az első lépés a mobilitási szolgáltatás telepítése. Ezt két módon teheti meg:

* Automatikusan leküldéses, és telepítse a szolgáltatást a minden gép adatok a folyamatkiszolgálótól. Ügyfélleküldéses telepítés nem fordulhat elő, amikor gépek felvétele a védelmi csoport, és azok egy megfelelő verzióját a mobilitási szolgáltatás már futtatja. A szolgáltatás a vállalati leküldéses módszerrel, például a WSUS vagy a System Center Configuration Manager automatikusan is telepítheti. Győződjön meg arról, még mielőtt ezt megtehetné állított be a felügyeleti kiszolgálón.
* Telepítse a szolgáltatást manuálisan minden védeni kívánt számítógépen. Győződjön meg arról, még mielőtt ezt megtehetné állított be a felügyeleti kiszolgálón.

### <a name="install-the-mobility-service-with-push-installation"></a>Telepítse a mobilitási szolgáltatás leküldéses telepítéshez
Gépek felvétele a védelmi csoport, amikor a mobilitási szolgáltatás automatikusan leküldött és a folyamatkiszolgáló minden számítógépen telepítve.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Az automatikus leküldés előkészületei Windows-gépeken
Windows-alapú gépek készíti elő az, hogy a mobilitási szolgáltatást a folyamatkiszolgáló automatikusan telepíthető:

1. Hozzon létre egy fiókot, a folyamatkiszolgáló segítségével fér hozzá a géphez. A fióknak (helyi vagy tartományi) rendszergazdai jogosultságokkal kell rendelkeznie. Ezek a hitelesítő adatok csak a mobilitási szolgáltatás leküldéses telepítéséhez használhatók.

   > [!NOTE]
   > Ha nem használ egy olyan tartományi fiók, meg kell tiltsa le a távoli felhasználói hozzáférés-vezérlés a helyi számítógépen. Ehhez a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, a beállításjegyzék hozzáadásához DWORD LocalAccountTokenFilterPolicy, 1 értékű alatt. A parancssori felület nyitva parancs vagy a PowerShell használatával a beállításjegyzék-bejegyzés hozzáadásához írja be a következőt  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .
   >
   >
2. A Windows tűzfal a védeni kívánt számítógépen, válassza ki a **lehetővé teszi egy alkalmazás vagy szolgáltatás átengedése a tűzfalon**, és engedélyezze **fájl- és nyomtatómegosztás** és **Windows Management Instrumentation**. Gépekhez, amely egy tartományba tartozzanak konfigurálhatja a tűzfal-házirendet egy csoportházirend-objektummal.

   ![Tűzfalbeállítások](./media/site-recovery-vmware-to-azure-classic/mobility1.png)
3. Vegye fel a létrehozott fiókot:

   * Nyissa meg a következőt: **cspsconfigtool**. A [telepítés helye] \home\svsystems\bin mappában érhető el, egy parancsikont az asztalon és található.
   * A **Manage Accounts** (Fiókok kezelése) lapon kattintson az **Add Account** (Fiók hozzáadása) lehetőségre.
   * Vegye fel a létrehozott fiók. Miután hozzáadta a fiókot, a lesz szüksége, adja meg a hitelesítő adatokat, ha a gép felvétele a védelmi csoport számára.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Az automatikus leküldés előkészületei Linux-gépeken
1. Győződjön meg arról, hogy a védeni kívánt Linux-gép támogatja-e, a [helyszíni Előfeltételek](#on-premises-prerequisites). Győződjön meg arról, hogy van-e a védeni kívánt számítógép és a folyamatkiszolgáló futtató felügyeleti kiszolgáló közötti hálózati kapcsolat.
2. Hozzon létre egy fiókot, a folyamatkiszolgáló segítségével fér hozzá a géphez. A fióknak kell lennie a gyökér szintű felhasználó a forráskiszolgálón Linux. Ezek a hitelesítő adatok csak a mobilitási szolgáltatás leküldéses telepítéséhez használhatók.

   * Nyissa meg a következőt: **cspsconfigtool**. A [telepítés helye] \home\svsystems\bin mappában érhető el, egy parancsikont az asztalon és található.
   * A **Manage Accounts** (Fiókok kezelése) lapon kattintson az **Add Account** (Fiók hozzáadása) lehetőségre.
   * Vegye fel a létrehozott fiók. Miután hozzáadta a fiókot, a lesz szüksége, adja meg a hitelesítő adatokat, ha a gép felvétele a védelmi csoport számára.
3. Ellenőrizze, hogy a forrás Linux-kiszolgálón található /etc/hosts fájl tartalmaz-e olyan bejegyzéseket, amelyek a helyi gazdanevet az összes hálózati adapterhez társított IP-címekké képezik le.
4. A legújabb openssh, a protokoll openssh-kiszolgáló és a openssl csomagok telepítéséhez a védeni kívánt számítógépen.
5. Győződjön meg arról, hogy SSH engedélyezve van és fut a 22-es portot.
6. A következő módon engedélyezze az SFTP alrendszer és a jelszó hitelesítését az sshd_config fájlban:

   * Jelentkezzen be rendszergazdaként.
   * A /etc/ssh/sshd_config fájlban található PasswordAuthentication kezdődő sort.
   * Állítsa vissza a sort, és módosítsa a **no** értéket **yes** értékre.
   * Keresse meg a sort, amelynek a kezdete **Subsystem**, és állítsa vissza.

     ![Linux bírálja felül az alapértelmezett nem alrendszerek](./media/site-recovery-vmware-to-azure-classic/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Telepítse manuálisan a mobilitási szolgáltatás
A telepítőcsomagokat a C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository érhetők el.

| Forrás operációs rendszer | Mobilitási szolgáltatás telepítőfájlja |
| --- | --- |
| Windows Server (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-the-mobility-service-manually-on-a-windows-server"></a>Telepítse manuálisan a mobilitási szolgáltatást a Windows server
1. Töltse le és futtassa a megfelelő telepítőt.
2. Az **Előkészületek** területen válassza a **Mobilitási szolgáltatás** lehetőséget.

    ![Mobilitási szolgáltatás telepítése](./media/site-recovery-vmware-to-azure-classic/mobility3.png)
3. A **konfigurációs kiszolgáló adatait**, adja meg az IP-címe a felügyeleti kiszolgáló és a jelszót, amelyet a felügyeleti kiszolgáló-összetevők telepítése során jött létre. Kérheti le a hozzáférési kód futtatásával  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** a felügyeleti kiszolgálón.

    ![Mobilitási szolgáltatás](./media/site-recovery-vmware-to-azure-classic/mobility6.png)
4. A **helyre telepítse**, hagyja az alapértelmezett helyet, és kattintson a **következő** telepítésének megkezdésére.
5. A **telepítési folyamat**, ellenőrizze a telepítést, és ha a rendszer kéri, indítsa újra a gépet.

Írja be a következő szöveget a parancssorból is telepíthető:

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]

A fenti parancsban:

* / Szerepkör: kötelező. Megadja, hogy a mobilitási szolgáltatást kell-e telepíteni.
* / InstallLocation: kötelező. Megadja a szolgáltatás telepítésének helyét.
* / PassphraseFilePath: kötelező. Adja meg a konfigurációs kiszolgáló jelszava.
* / LogFilePath: kötelező. Megadja a naplófájl telepítő helyét.

#### <a name="uninstall-the-mobility-service-manually"></a>A mobilitási szolgáltatás eltávolítása manuálisan
Eltávolíthatja a mobilitási szolgáltatás használatával **eltávolítása vagy módosítása egy program** a Vezérlőpulton, illetve a parancssor használatával.

A parancs segítségével távolítsa el a mobilitási szolgáltatást a parancssorból is:

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="change-the-ip-address-of-the-management-server"></a>A felügyeleti kiszolgáló az IP-címének módosítása
A varázsló futtatása után az alábbiak szerint módosíthatja a felügyeleti kiszolgáló IP-címe:

1. Nyissa meg a fájl hostconfig.exe (az asztalon található).
2. Az a **globális** lapra, majd a felügyeleti kiszolgáló IP-címét.

   > [!NOTE]
   > Csak a felügyeleti kiszolgáló IP-címének módosítása. A port számát a felügyeleti kiszolgáló közötti kommunikációt kell lennie a 443-as és **használja HTTPS** balra engedélyezni kell. Ne változtassa meg a jelszót.
   >
   >

    ![Felügyeleti kiszolgáló IP-címe](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-the-mobility-service-manually-on-a-linux-server"></a>Telepítse manuálisan a mobilitási szolgáltatás egy Linux-kiszolgálón
1. Másolja a megfelelő bont archív a Linux-gép, amelyet védeni kíván. Lásd a táblázatot [telepítse manuálisan a mobilitási szolgáltatás](#install-the-mobility-service-manually) meghatározásához mely bont archiválására, használjon.
2. Nyissa meg a rendszerhéj programot, és bontsa ki a tömörített bont archív helyi elérési út futtatásával:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Hozzon létre egy fájlt a helyi könyvtárban, ahová kicsomagolta az bont archívum tartalmának passphrase.txt. Ehhez a hozzáférési kód másolását C:\ProgramData\Microsoft Azure hely Recovery\private\connection.passphrase a felügyeleti kiszolgálón, és mentse a munkafüzetet passphrase.txt futtatásával  *`echo <passphrase> >passphrase.txt`*  a rendszerhéj.
4. Telepítse a mobilitási szolgáltatást a következő parancs beírásával:*`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*
5. Adja meg a management server belső IP-címét, és győződjön meg arról, hogy a 443-as porton van kiválasztva.

#### <a name="install-the-mobility-service-from-the-command-line"></a>A mobilitási szolgáltatás telepítése a parancssorból

A jelszó másolása a C:\Program Files (x86) \InMage Systems\private\connection a felügyeleti kiszolgálón, és mentse a felügyeleti kiszolgálón "passphrase.txt". Ezután futtassa a következő parancsokat. A példánkban a felügyeleti kiszolgáló IP-cím 104.40.75.37, és a HTTPS-port pedig a 443-as:

Telepítés éles kiszolgálón:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Telepítés a fő célkiszolgálón:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>10. lépés: A gép védelmének engedélyezése
Védelem engedélyezéséhez adja hozzá a virtuális gépek és fizikai kiszolgálók egy védelmi csoportba. Mielőtt elkezdené, vegye figyelembe a következőket Ha VMware virtuális gépeket lát el védelemmel:

* VMware virtuális gépek felderített 15 percenként, és a Site Recovery portálon felderítése után jelenik meg, hogy több mint 15 percig is eltarthat.
* A virtuális gépen (például a VMware-eszközök telepítése) környezetben módosítások frissítenie kell a Site Recovery szolgáltatásban több mint 15 percig is tarthat.
* A legutóbb felfedezett ellenőrizheti a VMware virtuális gépek esetén a **utolsó lépjen kapcsolatba a** mezőt a vCenter server vagy ESXi-állomáson lévő a **konfigurációs kiszolgálók** lapon.
* Ha hozzáad egy vCenter-kiszolgáló vagy ESXi-állomáson, a védelmi csoport létrehozása után, az Azure Site Recovery portálon, frissítéséhez és a virtuális gépek számára jelenik meg több mint 15 percig is eltarthat a **gépek felvétele a védelmi csoport** a párbeszédpanel.
* Ha azt szeretné, azonnal elindíthatja, és az ütemezett felderítési való várakozás nélkül a védelmi csoporthoz hozzáadni, jelölje ki a konfigurációs kiszolgálót (ne kattintson rá), majd **frissítése**.

Továbbá:

* Azt javasoljuk, hogy a védelmi csoportok tervezővel, hogy látni lehessen a munkaterhelések. Például hozzáadhat ugyanahhoz a csoporthoz egy adott alkalmazást futtató gépek.
* Gépek felvétele a védelmi csoport, ha a folyamatkiszolgáló automatikusan leküldéses értesítések, és telepíti a mobilitási szolgáltatást, ha még nincs telepítve. A leküldéses mechanizmus az előző lépésben ismertetett módon előkészített rendelkezik lesz szüksége.

### <a name="add-machines-to-a-protection-group"></a>Gépek felvétele a védelmi csoport

1. Ugrás a **védett elemek** > **védelmi csoport** > **gépek** > **gépeket**.
2. Ha a VMware virtuális gépek védi **virtuális gépek kijelölése**, egy vCenter-kiszolgálót, amely kezeli a virtuális gépek (vagy a EXSi gazdagép, amelyen futtatja), majd válassza ki és a gép.

    ![Virtuális gépek védelmének engedélyezése](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)
3. Ha a fizikai kiszolgálók védi **virtuális gépek kijelölése**, nyissa meg a **fizikai gépek felvétele** varázsló adjon meg az IP-cím és egy rövid nevet. Ezután válassza ki az operációsrendszer-családot.

   ![Fizikai kiszolgálók védelmének engedélyezése](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)
4. A **adja meg tároló erőforrásait**, válassza ki a tárfiókot, amely a replikáció használata, és válassza ki, hogy a használhatók a beállítások az összes munkaterhelés kiszolgálásához. Prémium szintű storage-fiókok jelenleg nem támogatottak.

   > [!NOTE]
   > Nem támogatjuk áthelyezése storage-fiókok használatával létrehozott a [Azure-portálon](../storage/storage-create-storage-account.md) erőforráscsoportok között.                           
   > A Site Recovery üzembe helyezéséhez használt tárfiókok esetében a [Migration of storage accounts](../azure-resource-manager/resource-group-move-resources.md) (Tárfiókok áttelepítése) művelet az egy előfizetésen belüli erőforráscsoportok között vagy előfizetések között nem támogatott.
   >
   >

    ![Cél beállításainak konfigurálása](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)
5. A **meg fiókok**, válassza ki azt a fiókot, hogy [beállítása](#install-the-mobility-service-with-push-installation) használandó automatikus telepíteni a mobilitási szolgáltatást.

    ![Adjon meg olyan fiókokat](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)
6. Kattintson a pipa jelre a befejezéshez gépek felvétele a védelmi csoport, és minden gép kezdeti replikációjának elindítása.

   > [!NOTE]
   > Ha ügyfélleküldéses telepítést készült, a mobilitási szolgáltatás automatikusan telepítve van a gépeken, amelyek nem rendelkeznek, mint azok van-e adva a védelmi csoportba. A szolgáltatás telepítése után egy védelmi feladatot elindul, és sikertelen lesz. A meghibásodás után meg kell újraindítani az egyes gépek, amelyek volt-e a mobilitási szolgáltatás telepítve van. Az újraindítás után újra megkezdi a védelmi feladat, és a kezdeti replikálást.
   >
   >

A figyelheti állapotát a **feladatok** lap.

![A feladatok lapon állapotának figyelése](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Védelmi állapot is figyelhetők a **védett elemek** > *védelmi csoport neve* > **virtuális gépek**. Miután a kezdeti replikáció befejezését követően, és szinkronizálja az adatokat, gép állapota **védett**.

![A figyelő állapota a védett elemek](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)

## <a name="step-11-set-protected-machine-properties"></a>11. lépés: Védett gép tulajdonságai
1. Után a számítógép rendelkezik egy **védett** állapotát, a feladatátvételi tulajdonságait konfigurálhatja. A védelmi csoport részleteit, jelölje ki a gépet, majd nyissa meg a **konfigurálása** fülre.
2. A Site Recovery automatikusan az Azure virtuális gép tulajdonságok alapján, és észleli a helyszíni hálózati beállításait.

    ![Virtuális gép beállításainak megadása](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)
3. Ezeket a beállításokat módosíthatja:

   * **Az Azure virtuális gép neve**: azt a nevet, amely a feladatátvétel után a gép az Azure-ban közül. A nevet meg kell felelnie, Azure-követelményeknek.
   * **Az Azure Virtuálisgép-méretet**: A hálózati adapterek számát határozza meg a méretét adja meg a cél virtuális gép. -Méretek és adapterek kapcsolatos további információkért tekintse meg a [táblák méretezés](../virtual-machines/linux/sizes.md). Vegye figyelembe:

     * Módosíthatja a virtuális gép méretét, és a beállítások mentéséhez, hálózati adapterek száma változik, amikor megnyitja a **konfigurálása** lapon a következő indításakor. A cél virtuális gépek hálózati adapterek minimális száma megegyezik a forrás virtuális gép hálózati adapterei minimális száma. A maximum hány hálózati adaptert a virtuális gép méretét határozza meg.
       * Ha a forrásgépen hálózati adapterek száma kisebb vagy egyenlő a célgép mérete engedélyezett adapterek számával, a cél lesz a ugyanannyi adapter forrásaként.
       * A hálózati adapterek esetében a forrás virtuális gép száma meghaladja a cél méretéhez engedélyezett, ha a cél maximális használható.
        Ha például a forrásgépen két hálózati adapter működik, és a célgép mérete négy adapter használatát teszi lehetővé, a célgépen két adapter fog működni. Ha a forrásgépen két adapter, de a támogatott célméretet támogatja a csak egy, a célgépen csak egy adapter fog működni.
     * Ha a virtuális gépnek több hálózati adaptert, az összes adapter ugyanarra az Azure-hálózatra kell csatlakoztatni.
   * **Azure-hálózat**: meg kell adnia az Azure-hálózatot, amely az Azure virtuális gépek csatlakoznak-e a feladatátvételt követően. Ha nem adja meg egy, az Azure virtuális gépek nem fog csatlakozni egyetlen hálózathoz sem. Emellett meg kell adnia az Azure-hálózatot, ha szeretne feladat-visszavételi az Azure-ból a helyszíni hely. Feladat-visszavétel szükséges egy VPN-kapcsolat az Azure-hálózatot és egy a helyszíni hálózat között.
   * **Az Azure IP-cím vagy alhálózat**: az egyes hálózati adaptereken, válassza ki az alhálózatot, amelyhez az Azure virtuális gép csatlakozni kell. Vegye figyelembe, hogy ha a forrásgép a hálózati adapter statikus IP-cím van konfigurálva, megadhatja a statikus IP-cím az Azure virtuális gép számára. Ha nem ad meg egy statikus IP-címet, majd bármilyen elérhető IP-címet oszt ki. Ha a cél IP-cím van megadva, de már használja egy másik Azure-ban, a feladatátvétel sikertelen lesz. Ha a hálózati adapter a forrásgép DHCP használatára van konfigurálva, akkor kell DHCP az Azure-beállításként.      

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>12. lépés: A helyreállítási terv létrehozása, és a feladatátvétel futtatása
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failover/player]
>
>

Lefuttathat egy feladatátvételi egyetlen gép, vagy több olyan virtuális gépeket, amelyek végrehajthatja ugyanezt a feladatot, vagy ugyanaz az alkalmazás futtatása sikertelen lehet. Az egyszerre több gép feladatátadását, akkor adja hozzá a helyreállítási terv.

A helyreállítási terv létrehozása:

1. Az a **helyreállítási tervek** kattintson **adja hozzá a helyreállítási terv** , és adja hozzá a helyreállítási terv. Adja meg a részleteket a terv és válassza ki a **Azure** céljaként.

 ![Konfigurálja a helyreállítási terv](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)
2. A **válassza ki a virtuális gép**, válasszon ki egy védelmi csoportot, és válassza ki a csoport hozzáadása a helyreállítási terv gépek.

 ![Virtuális gépek hozzáadása](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Testre szabhatja a terv létrehozásához csoportok és a sorrend gépeket a helyreállítási tervben feladatátvételt sorrendje. Azt is megteheti, parancsprogramok és manuális műveletek kér. Parancsfájlok manuálisan vagy segítségével hozhatók létre [Azure Automation-forgatókönyveket](site-recovery-runbook-automation.md). A helyreállítási terv testreszabásával kapcsolatos további információkért lásd: [helyreállítási tervek létrehozása](site-recovery-create-recovery-plans.md).

## <a name="run-a-failover"></a>Feladatátvétel futtatása
Mielőtt feladatátvételt:

* Győződjön meg arról, hogy a felügyeleti kiszolgálón fut, és elérhető. Ha nem, a feladatátvétel sikertelen lesz.
* Ha a nem tervezett feladatátvétel futtatása:

  * Ha lehetséges, a nem tervezett feladatátvétel előtt állítsa le az elsődleges gépeket. Ezzel gondoskodik róla, hogy a forrás és a replika gépek nem fognak egyszerre futni. Ha VMware virtuális gépeket replikál egy nem tervezett feladatátvételt futtatásakor, megadhatja, hogy a Site Recovery kísérelje meg a forrás gépek leállítása. Attól függően, hogy az elsődleges hely állapotát ez vagy nem működik. Ha fizikai kiszolgálókat replikál, a Site Recovery nem ezt a lehetőséget kínál.
  * Nem tervezett feladatátvétel leállítja az elsődleges gépek replikálása, úgy, hogy változásadatokat nem továbbítja a nem tervezett feladatátvétel megkezdése után.
  * Ha feladatátvételt követően a replika virtuális gép az Azure-ban való csatlakozáshoz, engedélyezze a távoli asztali kapcsolat a forrásgépen, a feladatátvétel végrehajtása előtt. Majd engedélyezze a távoli ASZTAL kapcsolaton keresztül a tűzfalon keresztül. Szüksége RDP engedélyezése a nyilvános végpontot az Azure virtuális gép a feladatátvételt követően is. Hajtsa végre a [ajánlott eljárások](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) annak érdekében, hogy működik-e az RDP egy feladatátvétel után.

> [!NOTE]
> Ahhoz, hogy a lehető legjobb teljesítményt érhesse el egy feladatátvételt az Azure-ba, győződjön meg arról, hogy telepítve van az Azure Agent ügynököt a védett számítógépen. Ez a gép indítása gyorsabban segít, és segít a problémák diagnosztizálásához. Az Azure-ügynök nem érhető el [Linux](https://github.com/Azure/WALinuxAgent) és [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
A feladatátvétel szimulálására irányuló feladatátvételi tesztet futtatni és helyreállítási folyamatok, elszigetelt hálózatot nincs hatással az éles környezetben, és lehetővé teszi, hogy a szokásos replikáció továbbra is a szokásos módon. A feladatátvételi teszt initiatd a forrás, és a több módon is futtathatja:

* **Ne adjon meg egy Azure-hálózatra**: Ha a hálózat nélkül feladatátvételi tesztet futtatja, a vizsgálat ellenőrzi, hogy a virtuális gépek indítása, és megfelelően jelennek meg az Azure-ban. Virtuális gépek nem csatlakozik az Azure-hálózatot a feladatátvételt követően.
* **Adja meg az Azure-hálózatot**: Ez a típus a feladatátvétel ellenőrzi, hogy az egész replikációs környezet megfelelően feláll-e, és hogy az Azure virtuális gépek csatlakoznak-e a megadott hálózati.

Feladatátvételi teszt futtatása:

1. Az a **helyreállítási tervek** lapon válassza ki a tervet, majd kattintson **feladatátvételi teszt**.

 ![Válassza ki a tervet.](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)
2. A **erősítse meg a feladatátvételi teszt**, jelölje be **nincs** annak jelzésére, hogy nem kívánja használni az Azure-hálózatot a feladatátvételi teszthez, vagy válassza ki a hálózatot, amely a teszt virtuális gépek a feladatátvételt követően is csatlakoznak. Kattintson a pipa jelre elindítani a feladatátvételt.

 ![Válasszon ki egy](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)
3. A feladatátvételi folyamat előrehaladásának figyeléséhez a **feladatok** fülre.

 ![A figyelő folyamatban](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)
4. A feladatátvétel befejezése után meg kell tudni a replika Azure machine megjelennek **virtuális gépek** az Azure portálon. Ha el szeretné indítani egy RDP-kapcsolat az Azure virtuális gépen, szüksége lesz a virtuális gép végpont 3389-es port megnyitásához.
5. Miután megismerte kész, amikor feladatátvételi művelet elér a **Complete** tesztelési fázisban, kattintson a **a teljes teszt** befejezéséhez. A **megjegyzések**, és a feladatátvételi teszttel kapcsolatos megfigyelések feljegyzéséhez mentéséhez.
6. Kattintson a **feladatátvételi teszt befejeződött** való automatikusan számolja fel a tesztkörnyezetet. Után ebben az esetben a feladatátvételi tesztet megjelenik egy **Complete** állapotát. Összes elemet és virtuális gépek feladatátvételi tesztje során automatikusan létrehozza a rendszer törli. Ha a két hétnél tovább tartó feladatátvételi tesztek, emiatt rendelkezik befejezéséhez.

### <a name="run-an-unplanned-failover"></a>A nem tervezett feladatátvétel
Nem tervezett feladatátvétel Azure kezdeményezi, és akkor is, ha nem érhető el az elsődleges hely hajtható végre.

1. Az a **helyreállítási tervek** lapon válassza ki a tervet, majd kattintson **feladatátvételi** > **nem tervezett feladatátvétel**.

 ![Válassza ki a tervet.](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)
2. Ha VMware virtuális gépeket replikál, próbálja meg a helyszíni virtuális gépek leállítása. Ez a legjobb művelet, és feladatátvételi továbbra is fennáll, hogy részéről az erőfeszítés sikeres-e vagy sem. Ha nem jár sikerrel, hiba részletei jelennek meg a **feladatok** lap **nem tervezett feladatátvétel feladatok**.

 ![A helyszíni virtuális gépek leállítása beállítása](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

 > [!NOTE]
 > Ez a lehetőség nem érhető el, ha fizikai kiszolgálókat replikál. Leállításakor azokat manuálisan lehetőség lesz szüksége.
 >
 >

3. A **megerősítéséhez feladatátvétel**, ellenőrizze a feladatátvételi irányát (az Azure-bA), és válassza ki a feladatátvételre használni kívánt helyreállítási pontot. Ha több virtuális Gépre kiterjedő replikáció tulajdonságainak konfigurálásakor engedélyezve van, helyreállíthatja a legutóbbi alkalmazás vagy összeomlás-konzisztens helyreállítási pontot. Igény szerint kiválaszthatja **egyéni helyreállítási pont** helyreállítása egy korábbi időpontbeli állapotra. Kattintson a pipa jelre elindítani a feladatátvételt.

 ![Erősítse meg a feladatátvételi iránya](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)
4. Várjon, amíg a nem tervezett feladatátvétel feladat befejeződésére. A feladatátvételi folyamat előrehaladásának figyelheti a **feladatok** fülre. Akkor is, ha hiba történik, nem tervezett feladatátvétel során, a helyreállítási terv fut, amíg nem fejeződik be. Meg kell tudni a replika Azure machine megjelennek **virtuális gépek** az Azure portálon.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Az Azure virtuális gépek replikálása a feladatátvételt követően kapcsolódni
Csatlakozás az Azure virtuális gépek a feladatátvételt követően replikálni, az alábbiak szükségesek:

- Távoli asztali kapcsolatot az elsődleges gépen engedélyezve van.
- A Windows tűzfal engedélyezi az RDP az elsődleges gépen.
- A virtuális gépet az Azure nyilvános végpontot az RDP hozzá.

Ennek beállítása kapcsolatban bővebben lásd: [ASR használatával feladatátvételt követően távoli asztali kapcsolat hibaelhárítási](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="deploy-additional-process-servers"></a>További folyamat-kiszolgálók központi telepítése
Ha a központi telepítés meghaladja a 200 forrásgépek kell horizontális, vagy a teljes napi változási sebessége meghaladja a 2 TB, a forgalom mennyisége kezeléséhez további folyamat kiszolgálók lesz szüksége. Ha kíván beállítani egy további folyamatkiszolgáló, ellenőrizze a vonatkozó követelményeket [további folyamat kiszolgálók](#additional-process-servers), majd állítsa be megfelelően az alábbi utasításokat a folyamatkiszolgáló. Miután beállította a kiszolgáló, konfigurálhatja forrásgépek rá.

### <a name="set-up-an-additional-process-server"></a>További folyamat kiszolgáló beállítása
Az alábbiak szerint állíthatja be egy további folyamatkiszolgáló:

* A egyesített-kiszolgálóként folyamat csak a felügyeleti kiszolgáló konfigurálása varázsló futtatásával.
* Ha adatreplikáció csak az új folyamatkiszolgáló használatával kezelni kívánt, akkor a védett gépek áttelepítésének.

### <a name="install-the-process-server"></a>A folyamatkiszolgáló telepítése
1. Az a **gyors üzembe helyezés** lapján töltse le az egyesített telepítési fájlját a Site Recovery-összetevő telepítéséhez. Futtassa a telepítőt.
2. Az **Előkészületek** területen válassza a **További folyamatkiszolgálók hozzáadása az üzembe helyezés horizontális felskálázásához** lehetőséget.

 ![Folyamatkiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)
3. Fejezze be a varázslót, mint amikor Ön [beállítása](#step-5-install-the-management-server) az első felügyeleti kiszolgáló.
4. A **konfigurációs kiszolgáló adatait**, adja meg az eredeti felügyeleti kiszolgáló, amelyeken telepítve van a konfigurációs kiszolgáló IP-címét, és írja be a jelszót. Ha még nem rendelkezik a jelszót, futtassa  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** vissza az eredeti felügyeleti kiszolgálón.

 ![Konfigurációs kiszolgáló adatait](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Az új folyamatkiszolgáló használandó gépek áttelepítése
1. Nyissa meg **konfigurációs kiszolgálók** > **Server** > *az eredeti felügyeleti kiszolgáló neve* > **kiszolgáló Részletek**.

 ![Kiszolgáló részletei](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)
2. Az a **folyamat kiszolgálók** listáról válassza ki **Folyamatkiszolgáló módosítása** mellett a kiszolgáló, amelyet módosítani szeretne.

 ![A folyamatkiszolgálón](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)
3. Válassza ki **Folyamatkiszolgáló módosítása**, jelölje be **folyamat-célkiszolgálót**, majd válassza ki az új felügyeleti kiszolgálót. Válassza ki a virtuális gépeket az új folyamatkiszolgáló kezelésére. Kattintson az ikonra az a kiszolgáló adatainak beolvasása. Szükség van az összes kiválasztott virtuális gépet replikálni az új folyamatkiszolgáló átlagos terület annak érdekében, hogy a döntések betöltése jelenik meg. Kattintson a pipa jelre az új folyamatkiszolgáló a replikáló elindításához.

 ![A folyamatkiszolgáló módosításának](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)

## <a name="vmware-permissions-for-vcenter-access"></a>VMware vCenter hozzáférési engedélyeinek
A folyamatkiszolgáló automatikusan fel tud deríteni a virtuális gépek a vCenter-kiszolgáló. Automatikus észlelés végrehajtására, ját definiálja a szerepkör (Azure_Site_Recovery) a vCenter-kiszolgáló eléréséhez a hely helyreállítását lehetővé tevő vCenter szinten. Ha csak a VMware rendszerű gépek áttelepítése az Azure-ba, és nem kell az Azure-ból történő feladat-visszavétel kell, megadhat egy csak olvasható szerepkör, amely elegendő-e. Az engedélyek beállítása, a [6. lépés: állítsa be a hitelesítő adatait a vCenter-kiszolgáló](#step-6-set-up-credentials-for-the-vcenter-server). A szerepkörengedélyek a következő táblázat tartalmazza:

| **Szerepkör** | **Részletek** | **Engedélyek** |
| --- | --- | --- |
| Azure_Site_Recovery szerepkör |VMware virtuális gép felderítése |Ezek a jogosultságok a v-központ kiszolgáló hozzárendelése:<br/><br/>Datastore: Szóköz, Tallózás datastore, alacsony szintű fájlműveletek foglal le, akkor távolítsa el a fájlt, a frissítés virtuálisgép-fájlok<br/><br/>Hálózati: Hálózati hozzárendelése<br/><br/>Erőforrás: Rendelje hozzá a virtuális gép erőforráskészlet, virtuális gép áttelepítése kikapcsolt, az alkalmazás bekapcsolja a virtuális gép áttelepítése<br/><br/>Feladatok: A feladat, a frissítési feladat létrehozása<br/><br/>Virtuális gép > konfiguráció<br/><br/>Virtuális gép > kommunikáció > válaszoljon a kérdést, eszköz kapcsolat, CD konfigurálása media, konfigurálás hajlékonylemez media, kapcsolja ki a bekapcsolás, VMware-eszközök telepítése<br/><br/>Virtuális gép > Készlet > létrehozása, regisztrálása, Unregister<br/><br/>Virtuális gép > kiépítési > engedélyezése virtuális gép letölthető, a virtuális gép fájlok feltöltése<br/><br/>Virtuális gép > pillanatképek > pillanatképek eltávolítása |
| vCenter felhasználói szerepkör |Felderítési VMware virtuális gép feladatátvételi a virtuális gép leállása nélkül |Ezek a jogosultságok a v-központ kiszolgáló hozzárendelése:<br/><br/>Adatközpont-objektum > propagálása gyermekobjektum, a szerepkör = csak olvasható <br/><br/>A felhasználó hozzá van rendelve a datacenter szinten, és így rendelkezik hozzáféréssel az objektumokhoz az adatközpontban. Ha szeretné korlátozni a hozzáférést, rendelje hozzá a **nem lehet hozzáférni** szerepkör-a **gyermekre propagálása** a gyermekobjektumokra (ESX-gazdagépeken, datastores, virtuális gépek és hálózatok) objektum. |
| vCenter felhasználói szerepkör |Feladatátvétel és feladat-visszavétel |Ezek a jogosultságok a v-központ kiszolgáló hozzárendelése:<br/><br/>Datacenter objektum – propagálása gyermekobjektum, a szerepkör = Azure_Site_Recovery<br/><br/>A felhasználó datacenter szinten van hozzárendelve, és így rendelkezik hozzáféréssel az objektumokhoz az adatközpontban.  Ha szeretné korlátozni a hozzáférést, rendelje hozzá a ** nem lehet hozzáférni ** szerepkört a **propagálása a gyermekobjektum** és a gyermek-objektum (ESX-gazdagépeken, datastores, virtuális gépek és hálózatok). |

## <a name="third-party-software-notices-and-information"></a>Harmadik felek szoftvereivel, megjegyzések és információk
<!--Do Not Translate or Localize-->

A szoftver- és a Microsoft termék vagy szolgáltatás futó belső vezérlőprogram alapján, vagy a szerződés magában foglalja a lenti projektek anyagok (együttesen "harmadik féltől származó kód").  A Microsoft a harmadik féltől származó kód nem eredeti szerzője.  Az eredeti szerzői és licencet, amely alatt a Microsoft az ilyen harmadik féltől származó kód kapott alábbi van beállítva.

A szakaszban található információk kapcsolatos alábbi projektek harmadik féltől származó kód-összetevőt. Ilyen licencek és információkat csak tájékoztatási célokat szolgálnak.  A harmadik féltől származó kód van folyamatban relicensed Önnek a Microsoft által a Microsoft szoftverfrissítések licencelési feltételeit a Microsoft termék vagy szolgáltatás alatt.  

B. szakaszban található információk kapcsolatos érhetőek Önnek a Microsoft által az eredeti licencfeltételek a harmadik felektől származó összetevőket.

A teljes fájl található a [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft kifejezetten át nem adott, az összes jogot fenntart által Ennek következménye, hogy belül vagy más módon.

## <a name="next-steps"></a>Következő lépések
[További információ a feladat-visszavétel](site-recovery-failback-azure-to-vmware-classic.md) az Azure-beli átvevő gép állapotú a helyszíni környezetben.
