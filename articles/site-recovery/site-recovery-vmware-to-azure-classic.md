---
title: "aaaReplicate VMware virtuális gépek és fizikai kiszolgálók tooAzure a klasszikus portálon hello |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan toodeploy Azure Site Recovery tooorchestrate replikációs, feladatátvételének és helyreállításának helyszíni VMware virtuális gépek és a windowsos/Linuxos fizikai kiszolgálók tooAzure."
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
ms.openlocfilehash: f85e4139ad45552ce963072e14d71d279bb7dac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-tooazure-with-azure-site-recovery"></a>VMware virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery replikálása
> [!div class="op_single_selector"]
> * [hello Azure-portálon](site-recovery-vmware-to-azure.md)
> * [hello klasszikus portál](site-recovery-vmware-to-azure-classic.md)
> * [klasszikus portál hello (örökölt)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

hello Azure Site Recovery szolgáltatás hozzájárul a replikáció, feladatátvétel és helyreállítási virtuális gépek és fizikai kiszolgálók replikálásával tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégiát. Gépek replikált tooAzure vagy tooa másodlagos helyszíni adatközpontba lehet. A gyors áttekintését lásd: [Mi az Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan:

* **VMware virtuális gépek tooAzure replikálása**: a Site Recovery telepítése toocoordinate replikációs, feladatátvételével és helyreállításával helyszíni VMware virtuális gépek tooAzure tárhelyet.
* **Fizikai kiszolgálók tooAzure replikálása**: Azure Site Recovery telepítése toocoordinate replikációs, feladatátvételének és a helyszíni fizikai Windows és Linux kiszolgálók tooAzure helyreállítását.

> [!NOTE]
> Ez a cikk ismerteti, hogyan tooreplicate tooAzure. Ha azt szeretné, hogy tooreplicate VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók tooa másodlagos adatközpontba, [hely helyreállítási VMware tooVMware](site-recovery-vmware-to-vmware.md).
>
>

Ez a cikk vagy hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Továbbfejlesztett üzemelő
Ez a cikk a klasszikus Azure portálon hello speciális telepítési utasításokat tartalmazza. Azt javasoljuk, hogy az új telepítésére vonatkozó jelenlegi verzióját használja. Ha által már telepített használatával hello korábbi régebbi verzióját, akkor azt javasoljuk, hogy az áttelepített toohello új verziója. Áttelepítési kapcsolatban bővebben lásd: [hely helyreállítási VMware tooAzure klasszikus örökölt](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

fokozott hello telepítési a fő frissítés. Íme hello fejlesztéseket hajtottunk összefoglalása:

* **Az Azure virtuális gépek infrastruktúra nélkül**: adatok replikálja közvetlenül tooan Azure storage-fiók. Emellett nincs nincs szükség tooset mentése bármely infrastruktúrát, virtuális gépek (például a konfigurációs kiszolgáló vagy a fő célkiszolgáló) replikációs és feladatátvételi hello örökölt környezetben volt szükség szerint.  
* **Telepítési egyesített**: egyetlen telepítés egyszerű beállítás és méretezhetőséget biztosít a helyszíni összetevőket.
* **Üzembe**: az összes forgalom titkosítva van, és a replikációs felügyeleti üzeneteket küld HTTPS 443-as porton keresztül.
* **Helyreállítási pontok**: összeomlási és alkalmazáskonzisztens helyreállítási pontokat Windows és Linux környezetek támogatása, és mindkét egyetlen virtuális gép és a virtuális Gépre kiterjedő konzisztens konfigurációk támogatása.
* **Feladatátvételi teszt**: nem zavaró teszt feladatátvételi tooAzure befolyásolja a munkakörnyezetet vagy replikáció felfüggesztése nélkül támogatása.
* **Nem tervezett feladatátvétel**: feladatátvétel előtt virtuális gépek leállítása egy bővített beállítás tooautomatically a nem tervezett feladatátvétel tooAzure támogatása.
* **Feladat-visszavétel**: integrált feladat-visszavétel, amelyek replikációja csak a változási különbözeteket biztonsági toohello helyszíni hely.
* **a vSphere 6.0**: korlátozott mértékben támogatja a VMware vSphere 6.0 központi telepítéseket.

## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Hogyan segíti a Site Recovery védelmét a virtuális gépek és fizikai kiszolgálók?
* VMware-rendszergazdák konfigurálhatják a telephelytől távoli óvintézkedéseket tooprotect Azure üzleti számítási feladatok és a VMware virtuális gépeken futó alkalmazások. Kiszolgáló kezelők fizikai, helyszíni Windows és Linux kiszolgálók tooAzure replikálhatja.
* hello Azure Site Recovery konzolján egyszerű üzembe helyezési és replikálásának, feladatátvételének és helyreállítási folyamatok kezelését egy központi helyen biztosítja.
* A vCenter-kiszolgáló által felügyelt VMware virtuális gépeket replikálja, ha a Site Recovery virtuális gépek automatikusan képes felderíteni. Ha gép ESXi-állomáson, a Site Recovery felderíti a virtuális gépek hello gazdagépen.
* Egyszerű feladatátvétel a helyszíni infrastruktúra tooAzure futtatja, ha sikertelen biztonsági (visszaállítás) hello helyszíni hely Azure tooVMware VM kiszolgálóján.
* Alkalmazások és szolgáltatások, amelyek több számítógépen is rétegzett csoport helyreállítási tervek konfigurálhatja. E terveket feladatátvételt, ha a Site Recovery a úgy, hogy ugyanazon munkaterhelések telepíthetők hello futtató gépek helyre együtt tooa konzisztens adatpont biztosít a virtuális Gépre kiterjedő konzisztencia.

## <a name="supported-operating-systems"></a>Támogatott operációs rendszerek
### <a name="windows-64-bit-only"></a>Windows (csak a 64 bites)
* Windows Server 2008 R2 SP1 +
* Windows Server 2012
* Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (csak a 64 bites)
* Red Hat Enterprise Linux 7.2, 6.7 és 7.1
* CentOS 6.5, 6.6, 6.7, 7.0, 7.1-es és 7.2
* 6.4 és 6.5 fut vagy hello Red Hat Enterprise Linux kompatibilis kernel Oracle vagy szoros vállalati Kernel Release 3 (UEK3)
* SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Forgatókönyv-architektúra
A forgatókönyv összetevőket:

* **Egy helyszíni felügyeleti kiszolgáló**: hello felügyeleti kiszolgáló futtatja a Site Recovery-összetevőkhöz:
  * **Konfigurációs kiszolgáló**: kommunikáció koordinálását, és kezeli az adatokat replikálás és helyreállítási folyamatok.
  * **Folyamatkiszolgáló**: replikációs átjáróként. Adatokat fogad védett forrásgépek; optimalizálja a gyorsítótárazás, tömörítés és titkosítás; és küld replikációs tooAzure tárolására. Kezeli a mobilitási szolgáltatás tooprotected gépek ügyfélleküldéses telepítés és a VMware virtuális gépek automatikus felderítést hajt végre.
  * **Fő célkiszolgáló**: az Azure-ból történő feladat-visszavétel során kezeli a replikációs adatokat.
    A felügyeleti kiszolgáló, amely csak a folyamat server tooscale a központi telepítés is telepíthet.
* **hello a mobilitási szolgáltatás**: Ez az összetevő minden gépet (VMware virtuális gép vagy fizikai kiszolgálón), amelyet az tooreplicate tooAzure van telepítve. Hello gépen végbemenő adatírásokat, és továbbítja őket toohello folyamatkiszolgáló.
* **Azure**: nem kell toocreate bármely Azure virtuális gépek toohandle replikációt és feladatátvételt. hello Site Recovery szolgáltatás adatkezelés kezeli, és közvetlenül tooAzure tárolási replikált adatok. A replikált Azure virtuális gépek vannak hoz létre automatikusan csak akkor, ha feladatátvételi tooAzure következik be. Azonban ha azt szeretné, hogy újból az Azure toohello helyszíni hely toofail, szüksége lesz egy Azure virtuális gép tooact mentése tooset folyamat-kiszolgálóként.

a következő ábra (Henry Robalino által létrehozott) hello bemutatja, hogyan működnek együtt ezeket az összetevőket:

![Összetevő-architektúra](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

## <a name="capacity-planning"></a>Kapacitástervezés
Arra készül, kapacitás, itt esetén teendők toothink kapcsolatban:

* **hello forráskörnyezettel**: kapacitásának tervezési vagy hello VMware infrastruktúra és a forrás gép követelményeinek.
* **hello felügyeleti kiszolgáló**: hello helyszíni felügyeleti kiszolgálókat, amelyeket a Site Recovery összetevők futtatása tervezési.
* **Hálózati sávszélesség a forrás tootarget**: hello forrás és az Azure közötti replikáció szükséges hálózati sávszélesség tervezése.

### <a name="source-environment-considerations"></a>Forrás környezettel kapcsolatos kérdések
* **Legfeljebb napi adatváltozási sebesség**: A védett gép csak egy folyamat-kiszolgálót is használhat. A folyamat egyetlen kiszolgáló mentése too2 naponta módosítani TB adatot képes kezelni. Ebből kifolyólag 2 TB hello maximális napi módosulása arány, amely a védett gépek esetén támogatott.
* **Maximális átviteli sebesség**: A replikált gép tooone Azure storage-fiókot is tartozhatnak. Standard szintű tárfiók kezelni tud a legfeljebb 20 000 kérelmek / másodperc, és azt javasoljuk, hogy ne hello száma IOPS forrás gép too20, 000 között. Például ha a forrásgép 5 lemezzel rendelkezik, és egyes lemezek hello forrás 120 IOPS (8 KB-os méret) hoz létre, lesz hello Azure lemez IOPS-korlát 500 belül. storage-fiókok szükséges száma hello teljes forrás IOPs/20 000 =.

### <a name="management-server-considerations"></a>Felügyeleti kiszolgáló kapcsolatos szempontok
a Site Recovery-összetevők, amelyek kezelik az adatok optimalizálása, replikációs és felügyeleti hello felügyeleti kiszolgáló fut. Képes toohandle hello napi módosítási gyakorisága kapacitást kell az összes védett gépeken futó munkaterhelések között, és van elegendő sávszélesség toocontinuously tooAzure adattárolás replikálni. Konkrétan:

* hello folyamatkiszolgáló védett gépek kap replikációs adatokat, és optimalizálja a gyorsítótárazás, tömörítés és titkosítás tooAzure elküldése előtt. hello felügyeleti kiszolgálójának rendelkeznie kell elegendő erőforrást tooperform ezeket a feladatokat.
* hello folyamatkiszolgáló lemezalapú gyorsítótárban használja. Azt javasoljuk, hogy egy külön gyorsítótár lemez 600 GB vagy több toohandle adatok változásait a hálózati szűk vagy leállás hello esemény tárolt. A telepítés során hello gyorsítótár konfigurálhatja a meghajtón, amelyen legalább 5 GB tárterület érhető el, de 600 GB hello minimális javaslat.
* Ajánlott eljárásként azt javasoljuk, hello felügyeleti kiszolgálón található hello ugyanazon a hálózaton és a LAN-szegmens, hello tooprotect kívánt gépek. Egy másik hálózati, de azt szeretné, hogy a tooprotect rendelkeznie kell a 3. hálózati látható tooit gépek is található.

Hello felügyeleti kiszolgáló mérete javaslatok hello a következő táblázat foglalja össze:

| **Felügyeleti kiszolgáló Processzora** | **Memória** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag) |16 GB |300 GB |500 GB vagy kevesebb |A felügyeleti kiszolgálóra ezen beállítások tooreplicate 100-nál kevesebb gépek telepítése. |
| 12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) |18 GB |600 GB |500 GB too1 TB |E beállítások tooreplicate 100-150-es gépekkel felügyeleti kiszolgáló központi telepítése. |
| 16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag) |32 GB |1 TB |1 TB-os too2 TB |A felügyeleti kiszolgáló telepítése az alábbi beállítások tooreplicate 150-200 gépekkel. |
| Egy másik folyamat-kiszolgáló központi telepítése | | |> 2 TB |Ha több mint 200 gépeket replikál, vagy ha hello napi módosulása aránya meghaladja a 2 TB további folyamat kiszolgálók telepítése |

Az elemek magyarázata:

* Minden forrásgép 100 GB, 3 lemezzel van konfigurálva.
* Összehasonlítási tárolása 8 SAS-meghajtókkal 10 000 fordulat/PERCES, RAID 10-es, gyorsítótár lemez mérések használtuk.

### <a name="network-bandwidth-from-source-tootarget"></a>Hálózati sávszélesség a forrás tootarget
Ellenőrizze, hogy hello segítségével lenne szükséges, a kezdeti replikáció és a különbözeti replikálás hello sávszélesség számítása [kapacitás planner eszköz](site-recovery-capacity-planner.md).

#### <a name="throttling-bandwidth-used-for-replication"></a>A replikáláshoz használt sávszélesség szabályozása
VMware replikált forgalom tooAzure végighalad egy adott folyamat kiszolgáló. Képes szabályozni a hello sávszélesség, amelyet a Site Recovery replikációs ezen a kiszolgálón az alábbiak szerint:

1. Nyissa meg a Microsoft Azure Backup szolgáltatás MMC beépülő modul hello hello fő felügyeleti kiszolgálón, vagy egy felügyeleti kiszolgálón futó további kiépített folyamat kiszolgálók. Alapértelmezés szerint a Microsoft Azure Backup parancsikonja hello asztalon jön létre. Vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin találja.
2. Kattintson a beépülő modul hello **tulajdonságainak módosítása**.

    ![Sávszélesség-szabályozási sávszélesség tulajdonságainak módosítása](./media/site-recovery-vmware-to-azure-classic/throttle1.png)
3. A hello **sávszélesség-szabályozási** lapra, adja meg, amely a Site Recovery replikáció használható, és a megfelelő ütemezés hello hello sávszélesség.

    ![Replikáció sávszélesség szabályozása](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Sávszélesség-szabályozás a PowerShell használatával is beállíthatja. Íme egy példa:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>A sávszélesség-használat maximalizálva
replikációs az Azure Site Recovery által használt tooincrease hello sávszélesség kell toochange egy beállításkulcs megadásával.

hello következő főbb vezérlők hello / lemez replikálásához replikálása esetén használt szálak száma:

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 Egy szükségesnél több erőforrással ellátott hálózatban ez a beállításkulcs kell toobe beállítás változása: az alapértelmezett értékeit. Legfeljebb 32 támogatott.  

További információk a részletes kapacitásának megtervezéséről lásd: [Site Recovery kapacitás planner](site-recovery-capacity-planner.md).

### <a name="additional-process-servers"></a>További folyamat kiszolgálók
Ha több mint 200 gépek kell tooprotect vagy a napi adatváltozási sebesség nagyobb 2 TB vehet fel további kiszolgálókat toohandle hello betöltése. kimenő tooscale, a következőket teheti:

* A felügyeleti kiszolgálók hello számának növeléséhez. Például védelmet biztosíthat a too400 gépek két felügyeleti kiszolgálóval.
* További folyamat kiszolgálók és az ezek toohandle forgalom helyett (vagy kívül) hello felügyeleti kiszolgálóval.

Ez a táblázat egy olyan forgatókönyvet, amelyben:

* Beállított hello eredeti felügyeleti kiszolgáló toouse csak egy konfigurációs kiszolgálón.
* További folyamat kiszolgáló beállítása.
* Védett virtuális gépek toouse hello további folyamat kiszolgáló konfigurálása.
* Minden védett forrásgép 100 GB három lemez van konfigurálva.

| **Eredeti felügyeleti kiszolgáló**<br/><br/>(konfigurációs kiszolgáló) | **További folyamatkiszolgáló** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB RAM |4 Vcpu (2 sockets * @ 2,5 GHz-es 2 mag), 8 GB RAM |300 GB |250 GB vagy kevesebb |85 vagy kevesebb gépek replikálhatja. |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB RAM |8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 12 GB RAM |600 GB |250 GB too1 TB |Replikálhatja a 85-150 gépek. |
| 12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag), 18 GB RAM |12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) 24 GB RAM |1 TB |1 TB-os too2 TB |150-225 gépek replikálhatja. |

A kiszolgálók méretezni hogyan attól függ, hogy egy méretezett és kibővített modell igény szerint. Vertikális felskálázás néhány nagy teljesítményű felügyeleti és folyamat-kiszolgálók üzembe helyezésével, vagy kevesebb erőforrást további kiszolgálók üzembe helyezésével kiterjesztése. Példa: Ha tooprotect 220 gépek van szüksége, hello alábbiak valamelyikét teheti:

* Konfigurálja a hello eredeti felügyeleti kiszolgálót a 12 Vcpu és 18 GB RAM. Állítson be egy további folyamat kiszolgálót a 12 Vcpu és 24 GB RAM-MAL. Védett gépek toouse csak hello további folyamat kiszolgáló konfigurálása.
* (2 darab 8 Vcpu, 16 GB RAM) két felügyeleti kiszolgáló és két további folyamat-kiszolgáló (1 x 8 Vcpu és 4vCPUs x 1 toohandle 135-ös + 85 (220) gépek) konfigurálása. Konfigurálja a védett gépek toouse csak hello további folyamat kiszolgálók.

Hello utasításait követve [további folyamat-kiszolgálót telepíthet](#deploy-additional-process-servers) tooset fel egy további folyamatkiszolgáló.

## <a name="before-you-start-deployment"></a>Mielőtt elkezdené a telepítést
hello következő táblázat összefoglalja a forgatókönyv üzembe helyezését hello előfeltételeit.

### <a name="azure-prerequisites"></a>Azure-előfeltételek
| **Előfeltétel** | **Részletek** |
| --- | --- |
| **Azure-fiók** |Szüksége van egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. További információ a Site Recovery díjszabásáról: [Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
| **Azure Storage** |Szüksége van egy Azure storage fiók toostore replikált adatok. A replikált adatok az Azure storage tárolja, és az Azure virtuális gépeket hoz létre, ha feladatátvétel történik. <br/><br/>Ehhez [standard georedundáns tárfiókot kell használnia](../storage/storage-redundancy.md#geo-redundant-storage). hello fióknak kell lennie a hello és hello Site Recovery szolgáltatásnak ugyanabban a régióban, és hello társítható ugyanahhoz az előfizetéshez. Replikációs toopremium tárfiókok jelenleg nem támogatott, és használatuk kerülendő.<br/><br/>Nem támogatjuk hello segítségével létrehozott áthelyezése tárfiókok [Azure-portálon](../storage/storage-create-storage-account.md) erőforráscsoportok között. További információkért lásd: [Azure Storage bemutatása tooMicrosoft](../storage/storage-introduction.md).<br/><br/> |
| **Azure-hálózat** |Egy, az Azure virtuális gépek csatlakozni fognak az Azure virtuális hálózat szükséges toowhen feladatátvételt hajt végre. hello Azure virtuális hálózat kell hello és hello Site Recovery-tárolónak ugyanabban a régióban.<br/><br/>feladatátvételi tooAzure után vissza toofail, kell egy VPN kapcsolatot (vagy Azure ExpressRoute) beállítása hello Azure hálózati toohello a helyszíni helyről. |

### <a name="on-premises-prerequisites"></a>Helyszíni előfeltételek
| **Előfeltétel** | **Részletek** |
| --- | --- |
| **Felügyeleti kiszolgáló** |Egy virtuális gép vagy fizikai kiszolgálón fut a helyi Windows 2012 R2 kiszolgálóra van szüksége. Összes hello helyszíni Site Recovery-összetevők telepítve van a felügyeleti kiszolgálón.<br/><br/> Azt javasoljuk, hogy telepít egy magas rendelkezésre állású VMware virtuális gépként hello kiszolgáló. Feladat-visszavétel toohello helyszíni hely Azure-ból mindig tooVMware virtuális gépeket, függetlenül attól, hogy nem sikerült virtuális gépek vagy fizikai kiszolgálók. Ha VMware virtuális gép hello felügyeleti kiszolgáló nem állítja, a VMware virtuális gép tooreceive feladat-visszavétel adatforgalom kell tooset egy külön fő célkiszolgáló fel.<br/><br/>hello kiszolgáló nem lehet tartományvezérlő.<br/><br/>hello kiszolgálónak rendelkeznie kell egy statikus IP-címet.<br/><br/>hello hello kiszolgáló állomásnevét kell 15 karakter vagy kevesebb.<br/><br/>hello operációs rendszer területi beállítása csak angol nyelvű kell.<br/><br/>hello felügyeleti kiszolgáló Internet-hozzáférésre van szüksége.<br/><br/>Szüksége van kimenő hello kiszolgálóról az alábbiak szerint: hello Site Recovery összetevők (toodownload MySQL); a telepítés során a 80-as HTTP ideiglenes hozzáférés a folyamatban lévő kimenő hozzáférést a 443-as HTTPS replikációkezelés; a folyamatban lévő kimenő hozzáférést HTTPS 9443 replikálási forgalma (Ez a port módosítható).<br/><br/> Gondoskodjon arról, hogy az URL-címek elérhető hello felügyeleti kiszolgálóról: <br/>- \*.hypervrecoverymanager.windowsazure.com<br/>- \*.accesscontrol.windows.net<br/>- \*.backup.windowsazure.com<br/>- \*.blob.core.windows.net<br/>- \*.store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/archives/MySQL-5.5/MySQL-5.5.37-Win32.msi] (https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Ha IP-címeken alapuló tűzfalszabályok szabályok hello kiszolgálón, ellenőrizze, hogy hello szabályok engedélyezik a kommunikációt tooAzure. Tooallow hello kell [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653) és hello HTTPS (443) port. Hello Azure-régió, az előfizetés, és az USA nyugati is kell toowhitelist IP-címtartományok. hello URL-cím [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi] (https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") a MySQL letöltés. |
| **VMware vCenter/ESXi-állomáson** |Meg kell egy vagy több VMware vSphere ESX/ESXi hipervizorok kezeléséhez a VMware virtuális gépeket ESX/ESXi 6.0, 5.5 vagy 5.1 futtató hello legújabb frissítéseit.<br/><br/> Azt javasoljuk, hogy telepít egy VMware vCenter server toomanage az ESXi-gazdagépek. Akkor kell futtatnia vCenter verziója 6.0 vagy 5.5 hello legújabb frissítéseit.<br/><br/>Ne feledje, hogy a Site Recovery nem támogatja az új vCenter vSphere 6.0 szolgáltatások például a kereszt-vCenter vMotion, a virtuális kötetek és a tárolási DRS. Hely helyreállítási funkció korlátozott toofeatures, melyeket is 5.5-ös verzió érhető el. |
| **Védett gépek** |**Azure**<br/><br/>Tooprotect meg kell felelnie az kívánt gépek [Azure-Előfeltételek](site-recovery-prereq.md) Azure virtuális gépek létrehozásához.<br><br/>Ha tooconnect toohello Azure virtuális gépek a feladatátvételt követően azt szeretné, akkor tooenable távoli asztali kapcsolatok hello helyi tűzfalon.<br/><br/>A védett gépek önálló lemezeinek kapacitása nem haladhatja meg az 1023 GB-t. A virtuális gépek too64 lemezeket (így mentése too64 TB) lehet. Ha 1 TB-nál nagyobb lemezek, érdemes lehet például az SQL Server Always On vagy Oracle Data Guard adatbázis-replikáció.<br/><br/>Legalább 2 GB szabad terület hello telepítési meghajtóján összetevő telepítéséhez.<br/><br/>Közös lemezes vendégfürtök nem használhatók. Ha a fürtözött központi telepítéssel rendelkezik, érdemes lehet például az SQL Server Always On vagy Oracle Data Guard adatbázis-replikáció.<br/><br/>Unified Extensible Firmware Interface (UEFI) / EFI típusú rendszerindítás nem használható.<br/><br/>Gépnevek (betűket, számokat és kötőjeleket) 1 és 63 karakter között kell tartalmaznia. hello nevét kell kezdődnie, betűvel vagy számmal, és betűvel vagy számmal végződhet. A gép védelme, után hello Azure nevét módosíthatja.<br/><br/>**VMware virtuális gépek**<br/><br>Tooinstall VMware vSphere PowerCLI 6.0 van szüksége. hello felügyeleti kiszolgálón (konfigurációs kiszolgáló).<br/><br/>VMware virtuális gépek tooprotect rendelkeznie kell a VMware-eszközök telepítve és fut. szeretné.<br/><br/>Ha hello forrás virtuális gép rendelkezik hálózati adapterek összevonása, azt konvertálja tooa egyetlen hálózati adapter feladatátvételi tooAzure után.<br/><br/>Ha védett virtuális iSCSI-lemez, a Site Recovery konvertálja hello védett, virtuális gép iSCSI-lemez VHD-fájlba amikor hello VM átadja a feladatokat tooAzure. ISCSI-tároló hello Azure virtuális gép elérhető, ha azok tooiSCSI cél csatlakozni, és két lemez tulajdonképpen lásd: hello VHD lemez hello Azure virtuális gép és hello forrás iSCSI-lemezt. Ebben az esetben szüksége toodisconnect hello iSCSI-tároló hello megjelenő átvevő Azure virtuális Gépen.<br/><br/>Hello VMware felhasználói engedélyek kapcsolatos további információk a Site Recovery van szüksége, tekintse meg [VMware vCenter hozzáférési engedélyeinek](#vmware-permissions-for-vcenter-access).<br/><br/> **Windows Server-gépek (a VMware virtuális gép vagy fizikai kiszolgálón)**<br/><br/>hello server futnia kell egy támogatott 64 bites operációs rendszer: Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2: legalább SP1.<br/><br/>hello operációs rendszer telepítéséhez a C meghajtó, és hello operációsrendszer-lemez Windows alaplemeznek kell lenniük. (a hello az operációs rendszer nem telepíthető a Windows dinamikus lemezzé.)<br/><br/>A Windows Server 2008 R2 gépek kell toohave .NET-keretrendszer 3.5.1 telepítve.<br/><br/>Tooprovide rendszergazdai fiókkal kell (a Windows hello gépen helyi rendszergazdának kell lennie) hello leküldéses telepítési hello mobilitási szolgáltatás a Windows-kiszolgálók számára. Ha hello a megadott fiók nem tartományi fiók, meg kell toodisable távelérési felhasználói vezérlő hello helyi számítógépen. További információkért lásd: [hello mobilitási szolgáltatás leküldéses telepítéshez telepítse](#install-the-mobility-service-with-push-installation).<br/><br/>A Site Recovery támogatja a virtuális gépek RDM-lemeze. A feladat-visszavétel során a Site Recovery szeretné újrafelhasználni hello RDM-lemeze Ha hello eredeti forrás virtuális gép és az RDM-lemeze elérhető. Nem érhetők el, feladat-visszavétel során, a Site Recovery hoz létre egy új VMDK-fájl az egyes lemezek.<br/><br/>**Linux-gépek**<br/><br/>Egy támogatott 64 bites operációs rendszerre van szüksége: Red Hat Enterprise Linux 6.7; Centos 6.5, 6.6 vagy 6.7; Oracle Enterprise Linux 6.4 vagy 6.5 hello Red Hat kompatibilis kernel vagy szoros vállalati Kernel Release 3 (UEK3); SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts fájlokat a védett gépek bejegyzéseit, amelyek hozzárendeléséről hello helyi állomás neve tooIP minden hálózati adapterekkel társított kell tartalmaznia. <br/><br/>Ha azt szeretné, hogy tooconnect tooan Azure virtuális gép egy Secure Shell-ügyfél (ssh), a feladatátvételt követően Linuxot futtató gondoskodjon arról, hogy hello védett számítógépen hello Secure Shell szolgáltatás rendszerindításkor automatikusan a toostart van beállítva, és, hogy a tűzfalszabályok engedélyezik-e az ssh kapcsolat tooit.<br/><br/>Védelem csak akkor engedélyezhető, a Linux-gépek a következő tárolási hello: fájlrendszer (EXT3, ETX4, ReiserFS, XFS); A többutas szoftver-eszköz leképező (többutas); Kötetkezelő (LVM2). HP CCISS vezérlő tároló fizikai kiszolgálók nem támogatottak. SUSE Linux Enterprise Server 11 SP3 csak támogatott hello ReiserFS fájlrendszer.<br/><br/>A Site Recovery támogatja a virtuális gépek RDM-lemeze. Feladat-visszavétel Linux, során a Site Recovery nem használja fel hello RDM-lemeze. Ehelyett létrehoz egy új VMDK fájlt mindegyik megfelelő RDM-lemeze számára. |

Csak a Linux virtuális gép: Győződjön meg arról, hogy beállítása hello disk.enableUUID=true beállítás hello hello a VMware virtuális gép konfigurációs paraméterei. Ha a sor nem létezik, adja hozzá. Ez akkor szükséges tooprovide konzisztens UUID toohello VMDK, hogy azt a megfelelő csatlakoztatja. Ha ez a beállítás feladat-visszavétel, akkor teljes letöltésére akkor is, ha hello VM érhető el a helyszínen. Ez a beállítás hozzáadásával biztosítja, hogy csak a változási különbözeteket átkerülnek a feladat-visszavétel során vissza.

## <a name="step-1-create-a-vault"></a>1. lépés: A tároló létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://manage.windowsazure.com/).
2. Bontsa ki a **adatszolgáltatások** > **Recovery Services** kattintson **Site Recovery-tároló**.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.
5. A **régió**, válassza ki a hello hello tároló földrajzi régióját. támogatott toocheck régiókban, lásd: [Azure Site Recovery díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kattintson a **Tároló létrehozása** elemre.
    ![Tároló létrehozása](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Ellenőrizze, hogy hello állapot sáv tooconfirm hello tároló sikeresen létrejött. hello tároló tulajdonosaként **aktív** a fő hello **Recovery Services** lap.

## <a name="step-2-set-up-an-azure-network"></a>2. lépés: Az Azure-hálózat beállítása
Beállítása az Azure-hálózatot az Azure virtuális gépekhez csatlakoztatott tooa hálózati feladatátvétel után, és, hogy a helyszíni feladat-visszavétel toohello várt módon működik-e webhely is.

1. Hello Azure-portálon, válassza ki **virtuális hálózat létrehozása** és hello hálózat nevét, IP-címtartomány és alhálózati név megadása.
2. Ha a feladat-visszavétel toodo van szüksége, adja hozzá a VPN/ExpressRoute toohello hálózatot. VPN/ExpressRoute felveheti toohello hálózati feladatátvétel után is.

Azure-hálózatok kapcsolatos további információkért lásd: [virtuális hálózatok áttekintése](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> [Áttelepítési hálózatok](../azure-resource-manager/resource-group-move-resources.md) erőforrás csoportok belül ugyanahhoz az előfizetéshez hello vagy előfizetések között nem támogatott a Site Recovery üzembe helyezéséhez használt hálózatok.
>
>

## <a name="step-3-install-hello-vmware-components"></a>3. lépés: Hello VMware összetevőinek telepítése
Ha azt szeretné, hogy tooreplicate VMware virtuális gépeket, kövesse az alábbi lépéseket a hello felügyeleti kiszolgálón:

1. [Töltse le](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) és VMware vSphere PowerCLI 6.0 telepítése.
2. Indítsa újra a hello kiszolgálót.

## <a name="step-4-download-a-vault-registration-key"></a>4. lépés: A tárolóbeli regisztrációs kulcs letöltése
1. Hello felügyeleti kiszolgálót nyissa meg az Azure-ban hello Site Recovery konzolján. A hello **Recovery Services** hello tároló tooopen hello kattintson **gyors üzembe helyezés** lap. Hello is megnyithatja **gyors üzembe helyezés** hello ikonra kattintva bármikor lap.

    ![Gyors üzembe helyezési ikonra](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)
2. A hello **gyors üzembe helyezés** kattintson **Célerőforrások előkészítése** > **a regisztrációs kulcs letöltése**. hello regisztrációs fájl automatikusan jön létre. Érvénytelen, így ezt követően öt napig.

## <a name="step-5-install-hello-management-server"></a>5. lépés: Hello felügyeleti kiszolgáló telepítése
> [!TIP]
> Gondoskodjon arról, hogy az URL-címek elérhető hello felügyeleti kiszolgálóról:
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



1. A hello **gyors üzembe helyezés** lapján töltse le a hello egyesített telepítési toohello fájlkiszolgáló.
2. Hello telepítési fájl toostart telepítő fut hello **Site Recovery az egységes telepítő** varázsló.
3. A **megkezdése előtt**, jelölje be **hello konfigurációs kiszolgáló és a folyamatkiszolgáló telepítése**.

   ![Előkészületek](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. A **külső szoftverlicenc**, kattintson a **elfogadom** toodownload és MySQL telepítése.

    ![Külső gyártótól származó szoftverek](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)
5. A **regisztrációs**, keresse meg és jelölje ki a letöltött hello tárolóból hello regisztrációs kulcsot.

    ![Regisztráció](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)
6. A **Internetbeállítások**, adja meg, hogyan hello konfigurációs kiszolgálón futó hello provider a Site Recovery tooAzure hello interneten keresztül fog csatlakozni.

   * Ha a jelenleg be van állítva a hello gépen hello proxy tooconnect, jelölje be **csatlakozás meglévő proxybeállításokkal**.
   * Ha közvetlenül hello szolgáltató tooconnect, jelölje be **kapcsolódás proxy nélkül közvetlenül**.
   * Ha a hello meglévő proxy hitelesítést igényel, vagy ha hello szolgáltatói kapcsolat toouse egyéni proxyt szeretne, válassza ki a **kapcsolódás egyéni proxybeállításokkal**.
     * Ha egyéni proxyt használ, szüksége lesz toospecify hello cím, port és hitelesítő adatait.
     * Ha a proxy használata esetén kell már engedélyezte a következő URL-címek hello:
       * *.hypervrecoverymanager.windowsazure.com    
       * *.accesscontrol.windows.net
       * *.backup.windowsazure.com
       * *.blob.core.windows.net
       * *.store.core.windows.net

    ![Tűzfal](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

1. A **szükséges előfeltételek ellenőrzése**, a telepítő elindítja a jelölőnégyzet toomake meg arról, hogy futtatható-e hello telepítés.

    ![Előfeltételek](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Ha figyelmeztetés jelenik meg a vonatkozó hello **globális időhöz sync ellenőrzése**, győződjön meg arról, hogy hello idő hello rendszerórája (**dátum és idő** beállítások) van hello ugyanaz, mint hello időzóna.

     ![Idő szinkronizálási problémája](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

1. A **MySQL konfigurációs**, hozzon létre toohello MySQL kiszolgálópéldányhoz, amely telepíti az aláíró hitelesítő adatait.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)
2. A **környezet részletei**, adja meg, hogy tooreplicate VMware virtuális gépek fog. Ha, a telepítő ellenőrzi, hogy telepítve van-e a PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)
3. A **helyre telepítse**, jelölje be, ha szeretné, hogy tooinstall hello bináris fájljait és hello gyorsítótárban tárolja. Egy legalább 5 GB szabad lemezterülettel rendelkező meghajtóra is kiválaszthatja, de azt javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad lemezterület.

   ![Telepítés helye](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)
4. A **Hálózatválasztás**, adja meg a hello figyelő (hálózati adapter és SSL-port) mely hello a konfigurációs kiszolgáló küld és fogad adatokat. Módosíthatja a hello alapértelmezett portot (9443). A hozzáadása toothis port 443-as portot a kiszolgáló koordinálja a replikálási műveletek használják. Ne használja a 443-as replikációs forgalom fogadására.

    ![Hálózat kiválasztása](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



1. A **összegzés**, tekintse át a hello információkat, majd kattintson **telepítése**. A telepítés után a rendszer létrehoz egy hozzáférési kódot. Ez szüksége engedélyezze a replikálást, ezért másolja és tartsa biztonságos helyen.

   ![Összefoglalás](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)


> [!WARNING]
> a Microsoft Azure Site Recovery szolgáltatás Agent proxy hello be kell állítania.
> Hello telepítés befejezése után indítsa el a Microsoft Azure Recovery Services rendszerhéj hello hello Windows Start menüjéből. Futtassa a következő parancsok tooset hello proxykiszolgáló-beállításainak mentése készlete hello hello parancs megjelenő ablakban.
>
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
>


### <a name="run-setup-from-hello-command-line"></a>Futtassa a telepítőt a parancssorból hello
Is hello egyesített varázsló futtatása parancssorból hello, az alábbiak szerint:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]

Az elemek magyarázata:

* /ServerMode: Kötelező. Meghatározza, hogy hello kell telepítés hello konfigurációs és folyamat-kiszolgálók vagy hello folyamatkiszolgáló csak (használt tooinstall további folyamat kiszolgálók). Bemeneti értékek: CS, PS.
* InstallDrive: kötelező. Megadja a hello hello összetevők telepítési mappája.
* / MySQLCredFilePath. Kötelező. Hello hello MySQL hitelesítő adatait tároló elérési útja tooa fájl határozza meg. Hello toocreate hello sablonfájl beolvasása.
* / VaultCredFilePath. Kötelező. Hello tárolói hitelesítő adatok fájl helyét.
* /EnvType. Kötelező. A telepítés típusát. Értékek: VMware, NonVMware.
* /PSIP és /CSIP. Kötelező. Hello folyamatkiszolgáló és a konfigurációs kiszolgáló IP-címe.
* /PassphraseFilePath. Kötelező. Hello jelszót fájl helyét.
* / ByPassProxy. Választható. Itt adhatja meg, amely a proxy nélkül tooAzure hello felügyeleti kiszolgálón.
* /ProxySettingsFilePath. Választható. Határozza meg az egyéni proxy (alapértelmezett proxy hitelesítést igénylő hello kiszolgálón), vagy egyéni proxy beállításait.

## <a name="step-6-set-up-credentials-for-hello-vcenter-server"></a>6. lépés: A hitelesítő adatait a vCenter-kiszolgáló hello beállítása
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Discovery/player]
>
>

hello folyamatkiszolgáló automatikusan felderíthetők a VMware vCenter-kiszolgáló által kezelt virtuális gépek. Automatikus felderítés a Site Recovery szüksége lesz a fiók és a hitelesítő adatokat, amelyek hello vCenter server számára érhető el. Ez a nem megfelelő, ha csak fizikai kiszolgálókat replikál.

tooset hello fiók és a hitelesítő adatokat:

1. Hello vCenter-kiszolgáló szerepkör létrehozása (**Azure_Site_Recovery**) szintű hello vCenter hello [szükséges engedélyek](#vmware-permissions-for-vcenter-access).
2. Rendelje hozzá a hello **Azure_Site_Recovery** szerepkör tooa vCenter felhasználó.

   > [!NOTE]
   > A vCenter hello csak olvasható szerepkör rendelkező felhasználói fiók futtathatja a feladatátvevő védett forrásgépek leállítása nélkül. Ha azt szeretné, hogy tooshut le ezeket a gépeket, a hello Azure_Site_Recovery szerepkör lesz szüksége. Ha most csak telepít virtuális gépeket VMware tooAzure, és nem kell ismét toofail, hello csak olvasható szerepkör is használhatók.
   >
   >
3. tooadd hello fiókot, nyissa meg **cspsconfigtool**. Rendelkezésre álló hello asztalon gyors és található hello [telepítés helye] \home\svsystems\bin mappában.
4. A hello **fiókok kezelése** lapra, majd **fiók hozzáadása**.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic/credentials1.png)
5. A **fiókadatok**, adja hozzá a hitelesítő adatokat, amelyek lehetnek használt tooaccess hello vCenter-kiszolgálót. Hello portálon hello fiók neve tooappear több mint 15 percet is igénybe vehet. tooupdate azonnal, kattintson a **frissítése** a hello **konfigurációs kiszolgálók** fülre.

    ![Részletek](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>7. lépés: A vCenter-kiszolgáló és az ESXi-gazdagépek hozzáadása
Ha VMware virtuális gépeket replikál, akkor tooadd vCenter-kiszolgáló (vagy ESXi-állomáson).

1. A hello **kiszolgálók** > **konfigurációs kiszolgálók** lapon jelölje be **vCenter-kiszolgáló hozzáadása**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)
2. Adjon hozzá hello vCenter-kiszolgálót vagy ESXi-gazdagép részletek hello, hogy a megadott fiók tooaccess hello hello előző lépésben a vCenter-kiszolgálót, és hello folyamatkiszolgáló lesz használt toodiscover VMware virtuális gépek hello vCenter-kiszolgáló által kezelt hello nevét. hello vCenter-kiszolgáló vagy ESXi-állomáson, mely hello folyamatkiszolgáló telepíthető hello kiszolgálóként azonos hálózati hello kell elhelyezni.

   > [!NOTE]
   > Hello vCenter-kiszolgáló vagy ESXi-állomáson, egy olyan fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal hello vCenter vagy a gazdagép-kiszolgáló hozzáadása, ne feledjen hello vCenter vagy ESXi-fiókok ezeket a jogokat engedélyezett: Datacenter, Datastore, mappa, Jost, hálózati, Erőforrás, a virtuális gép és a vSphere elosztott kapcsoló. hello vCenter-kiszolgáló hello tárolási nézetekkel jogosultság szükséges.
   >
   >

    ![VCenter-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)
3. Felderítési befejezése után hello vCenter-kiszolgáló megjelenik a hello **konfigurációs kiszolgálók** fülre.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)

## <a name="step-8-create-a-protection-group"></a>8. lépés: A védelmi csoport létrehozása
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Protection/player]
>
>

Védelmi csoportok virtuális gépek logikai csoportosításain alapulnak, vagy fizikai kiszolgálók, amelyet az tooprotect használatával hello azonos védelmi konfigurációval. Védelmi beállítások tooa védelmi csoport alkalmaz, és ezeket a beállításokat (virtuális vagy fizikai) alkalmazott tooall gépek toohello csoport hozzáadása.

1. Nyissa meg túl**védett elemek** > **védelmi csoport** hello ikon tooadd egy védelmi csoportot, majd.

    ![Védelmi csoport létrehozása](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)
2. A hello **adja meg a védelmi csoport beállításainál** adja meg azokat a hello csoport nevét. A hello **a** legördülő listából válassza ki, válassza hello konfigurációs kiszolgálón, amelyen toocreate hello csoport. **Cél** Microsoft Azure.

    ![Védelmi csoport beállításai](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)
3. A hello **adja meg a replikációs beállításokat** lapon, az összes hello gép hello csoport használandó hello replikáció beállításainak konfigurálása.

    ![Védelmi csoport replikációs](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

   * **Több virtuális Gépre kiterjedő konzisztencia**: Ha bekapcsolja ezt a, azt megosztott alkalmazáskonzisztens helyreállítási pontokat hoz létre hello gépek hello védelmi csoport között. A beállítás akkor a leghasznosabb, ha a hello védelmi csoportban lévő összes gépen futnak hello ugyanaz az alkalmazás. Az összes gép lesz a helyreállított toohello azonos adatpont. Akkor érhető el, hogy VMware virtuális gépek vagy fizikai kiszolgálók (Windows vagy Linux) replikál.
   * **Helyreállítási Időkorlát küszöbértéke**: készletek hello RPO. Riasztások akkor jönnek létre, ha hello folyamatos adatvédelemhez kapcsolódó replikáció meghaladja a konfigurált hello helyreállítási Időkorlát küszöbértékét.
   * **Helyreállítási pontok megőrzésének ideje**: hello adatmegőrzési időtartam határozza meg. Védett gépek lehet ebben az ablakban tooany pont helyre.
   * **Alkalmazáskonzisztens pillanatkép gyakorisága**: meghatározza, hogy milyen gyakran alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontok jöjjön létre.

Hello pipa kiválasztásakor a védelmi csoport létrehozása a megadott hello nevű. Emellett egy második védelmi csoport létrehozása hello nevű *nevű védelmi csoport*- feladat-visszavételre. A védelmi csoport akkor használatos, ha vissza toohello helyszíni hely feladatátvételi tooAzure után sikertelen lesz. Figyelheti a hello védelmi csoportok szerint jönnek létre a hello **védett elemek** lap.

## <a name="step-9-install-hello-mobility-service"></a>9. lépés: Hello mobilitási szolgáltatás telepítése
hello első lépése a virtuális gépek és fizikai kiszolgálók védelem engedélyezése egy tooinstall hello mobilitási szolgáltatás. Ezt két módon teheti meg:

* Automatikusan leküldéses, és minden számítógépen az hello folyamatkiszolgáló hello szolgáltatás telepítése. Ügyfélleküldéses telepítés nem fordulhat elő, ha hozzáadta a gépek tooa védelmi csoportot, és azok már futtatja a mobilitási szolgáltatás hello megfelelő verzióját. Hello szolgáltatást telepítheti a vállalati leküldéses módszerrel, például a WSUS vagy a System Center Configuration Manager automatikusan is. Győződjön meg arról, még mielőtt ezt megtehetné állított be hello felügyeleti kiszolgálón.
* Hello szolgáltatás manuálisan telepítse a megjeleníteni kívánt tooprotect minden egyes számítógépen. Győződjön meg arról, még mielőtt ezt megtehetné állított be hello felügyeleti kiszolgálón.

### <a name="install-hello-mobility-service-with-push-installation"></a>Hello mobilitási szolgáltatás leküldéses telepítéshez telepítése
Gépek tooa védelmi csoport hozzáadásakor hello mobilitási szolgáltatás automatikusan leküldött és hello folyamat kiszolgáló minden számítógépen telepítve.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Az automatikus leküldés előkészületei Windows-gépeken
tooprepare Windows-alapú gépek úgy, hogy a mobilitási szolgáltatás hello hello folyamatkiszolgáló automatikusan telepíthető:

1. Hozzon létre egy fiókot, hogy hello folyamatkiszolgáló használhat tooaccess hello gépet. hello fióknak (helyi vagy tartományi) rendszergazdai jogosultságokkal kell rendelkeznie. Ezek a hitelesítő adatok csak hello mobilitási szolgáltatás leküldéses telepítéséhez használhatók.

   > [!NOTE]
   > Ha nem használ egy olyan tartományi fiók, szüksége lesz a toodisable távelérési felhasználói vezérlő hello helyi számítógépen. toodo, hello rendszerleíró adatbázis HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, a bejegyzés hello DWORD LocalAccountTokenFilterPolicy, 1 értékű alatt. tooadd hello beállításjegyzékbeli bejegyzést a CLI nyissa meg a parancsot, vagy adja meg a PowerShell használatával  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .
   >
   >
2. A Windows tűzfal, amelyet az tooprotect hello számítógépen, válassza ki a **lehetővé teszi egy alkalmazás vagy szolgáltatás átengedése a tűzfalon**, és engedélyezze **fájl- és nyomtatómegosztás** és **Windows Management Instrumentation**. Tooa tartományhoz tartozó gépekhez konfigurálhatja hello tűzfal-házirendet egy csoportházirend-objektummal.

   ![Tűzfalbeállítások](./media/site-recovery-vmware-to-azure-classic/mobility1.png)
3. Vegye fel a létrehozott hello fiókot:

   * Nyissa meg a következőt: **cspsconfigtool**. Rendelkezésre álló hello asztalon gyors és található hello [telepítés helye] \home\svsystems\bin mappában.
   * A hello **fiókok kezelése** lapra, majd **fiók hozzáadása**.
   * Vegye fel a létrehozott hello fiókot. Hello fiók felvétele után a gép tooa védelmi csoport hozzáadásakor szüksége lesz tooprovide hello hitelesítő adataira.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Az automatikus leküldés előkészületei Linux-gépeken
1. Győződjön meg arról, hogy kívánt tooprotect leírtak szerint támogatott hello Linux-gép [helyszíni Előfeltételek](#on-premises-prerequisites). Győződjön meg arról, hogy van hálózati kapcsolat hello gép között szeretné tooprotect és hello hello folyamatkiszolgáló futtató felügyeleti kiszolgálón.
2. Hozzon létre egy fiókot, hogy hello folyamatkiszolgáló használhat tooaccess hello gépet. hello fióknak kell lennie a gyökér szintű felhasználó hello Linux forráskiszolgálón. Ezek a hitelesítő adatok csak hello mobilitási szolgáltatás leküldéses telepítéséhez használhatók.

   * Nyissa meg a következőt: **cspsconfigtool**. Rendelkezésre álló hello asztalon gyors és található hello [telepítés helye] \home\svsystems\bin mappában.
   * A hello **fiókok kezelése** lapra, majd **fiók hozzáadása**.
   * Vegye fel a létrehozott hello fiókot. Hello fiók felvétele után a gép tooa védelmi csoport hozzáadásakor szüksége lesz tooprovide hello hitelesítő adataira.
3. Ellenőrizze, hogy hello/Etc/Hosts fájlt hello forrás Linux server bejegyzéseit, amelyek az összes hálózati adapter társított hello helyi állomásnevével tooIP címek hozzárendelését tartalmazza.
4. Telepítse a hello legújabb openssh openssh-kiszolgáló és openssl-csomagokat, amelyet az tooprotect hello számítógépen.
5. Győződjön meg arról, hogy SSH engedélyezve van és fut a 22-es portot.
6. SFTP alrendszer és a jelszó-hitelesítés engedélyezése hello sshd_config fájlban az alábbiak szerint:

   * Jelentkezzen be rendszergazdaként.
   * Hello /etc/ssh/sshd_config fájlban található PasswordAuthentication kezdődő hello sort.
   * Állítsa vissza a hello sort, és módosítsa hello értéket **nem** túl**Igen**.
   * Keresés hello kezdődő sort **alrendszer** és hello sor törölje.

     ![Linux bírálja felül az alapértelmezett nem alrendszerek](./media/site-recovery-vmware-to-azure-classic/mobility2.png)

### <a name="install-hello-mobility-service-manually"></a>Telepítse manuálisan a mobilitási szolgáltatás hello
hello telepítők C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository érhetők el.

| Forrás operációs rendszer | Mobilitási szolgáltatás telepítőfájlja |
| --- | --- |
| Windows Server (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (csak 64 bites) |Microsoft-ASR_UA_9*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-hello-mobility-service-manually-on-a-windows-server"></a>Telepítse manuálisan a mobilitási szolgáltatás hello a Windows server
1. Töltse le és futtassa a telepítőt vonatkozó hello.
2. Az **Előkészületek** területen válassza a **Mobilitási szolgáltatás** lehetőséget.

    ![Mobilitási szolgáltatás telepítése](./media/site-recovery-vmware-to-azure-classic/mobility3.png)
3. A **konfigurációs kiszolgáló adatait**hello hello felügyeleti kiszolgáló IP-címét adja meg és hello hello felügyeleti kiszolgáló-összetevők telepítése során létrehozott jelszót. Hello hozzáférési kód futtatásával le  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** hello felügyeleti kiszolgálón.

    ![Mobilitási szolgáltatás](./media/site-recovery-vmware-to-azure-classic/mobility6.png)
4. A **hely telepítése**, tartsa hello alapértelmezett helyét, és kattintson a **következő** toobegin telepítését.
5. A **telepítési folyamat**, ellenőrizze a telepítést, és hello gép újraindítása, ha a rendszer kéri.

Írja be a következő szöveg hello parancssori hello is telepíthet:

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]

A parancs megelőző hello:

* / Szerepkör: kötelező. Megadja, hogy a mobilitási szolgáltatás hello kell-e telepítve.
* / InstallLocation: kötelező. Itt adhatja meg, ahol tooinstall hello szolgáltatást.
* / PassphraseFilePath: kötelező. Megadja a hello konfigurációs kiszolgáló jelszava.
* / LogFilePath: kötelező. Hello beállítása helyének megadása

#### <a name="uninstall-hello-mobility-service-manually"></a>Manuálisan távolítsa el a mobilitási szolgáltatás hello
Hello mobilitási szolgáltatás eltávolításához használja **eltávolítása vagy módosítása egy program** a Vezérlőpulton, illetve hello parancssor használatával.

hello parancs toouninstall hello parancssorból a mobilitási szolgáltatást a következő:

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="change-hello-ip-address-of-hello-management-server"></a>Hello hello felügyeleti kiszolgáló IP-címének módosítása
Hello varázsló futtatása után az alábbiak szerint módosíthatja az hello hello felügyeleti kiszolgáló IP-címe:

1. Nyissa meg a hello fájl hostconfig.exe (hello asztalon található).
2. A hello **globális** lapján hello hello felügyeleti kiszolgáló IP-címének módosítása.

   > [!NOTE]
   > Hello felügyeleti kiszolgáló csak hello IP-címének módosítása. felügyeleti kiszolgáló közötti kommunikációt hello portszámát kell lennie a 443-as és **használja HTTPS** balra engedélyezni kell. Ne változtassa meg hello jelszót.
   >
   >

    ![Felügyeleti kiszolgáló IP-címe](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-hello-mobility-service-manually-on-a-linux-server"></a>Telepítse manuálisan a hello mobilitási szolgáltatás egy Linux-kiszolgálón
1. Hello megfelelő bont archív toohello Linux rendszerű számítógép, amelyet az tooprotect másolja. Tekintse meg a hello táblázatot [telepítse manuálisan a mobilitási szolgáltatás hello](#install-the-mobility-service-manually) toodetermine mely bont archiválja azt kell használni.
2. Nyissa meg a rendszerhéj programot, és nyerje ki a hello zip bont archív tooa helyi elérési útja való futtatásával:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Hozzon létre egy fájlt passphrase.txt hello helyi könyvtár toowhich kicsomagolta hello bont archívum hello tartalmát. toodo, a másolási hello jelszó C:\ProgramData\Microsoft Azure hely Recovery\private\connection.passphrase a hello felügyeleti kiszolgálón, és mentse azt futtatásával passphrase.txt  *`echo <passphrase> >passphrase.txt`*  hello rendszerhéjban.
4. Hello mobilitási szolgáltatás telepítése hello a következő parancs beírásával:*`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*
5. Adja meg a felügyeleti kiszolgáló hello hello belső IP-címét, és győződjön meg arról, hogy a 443-as porton van kiválasztva.

#### <a name="install-hello-mobility-service-from-hello-command-line"></a>Hello parancssorból hello mobilitási szolgáltatás telepítése

Hello jelszó másolása a C:\Program Files (x86) \InMage Systems\private\connection hello felügyeleti kiszolgálón, és a Mentés másként "passphrase.txt" hello felügyeleti kiszolgálón. Futtassa a következő parancsok hello. A jelen példában hello felügyeleti kiszolgáló IP-cím 104.40.75.37 és hello HTTPS-portja: 443:

tooinstall egy üzemi kiszolgálón:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

fő célkiszolgáló hello tooinstall:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>10. lépés: A gép védelmének engedélyezése
tooenable védelmére, adja hozzá a virtuális gépek és fizikai kiszolgálók tooa védelmi csoportot. Mielőtt elkezdené, vegye figyelembe a hello következő, ha VMware virtuális gépeket lát el védelemmel:

* VMware virtuális gépek felderített 15 percenként, és több mint 15 percbe telhet számukra tooappear hello Site Recovery portálon felderítése után.
* Környezet módosításokat (például a VMware-eszközök telepítése) hello virtuális gépen több mint 15 percig toobe frissítve a Site Recovery is tarthat.
* Ellenőrizheti a hello a legutóbb felfedezett idő VMware virtuális gépek esetén a hello **utolsó lépjen kapcsolatba a** mezőjét a hello hello vCenter kiszolgálón vagy ESXi állomások **konfigurációs kiszolgálók** fülre.
* Ha hozzáad egy vCenter-kiszolgáló vagy ESXi-állomáson, a védelmi csoport létrehozása után, hello Azure Site Recovery portál toorefresh és a virtuális gépek toobe hello felsorolt több mint 15 percig is eltarthat **tooa gépek védelmi csoport hozzáadása**párbeszédpanel megnyitásához.
* Ha azonnal szeretné tooproceed, és vegye fel a gépek tooa védelmi csoport hello ütemezett felderítési várakozás nélkül, jelölje ki a konfigurációs kiszolgáló hello (ne kattintson azt), majd **frissítése**.

Továbbá:

* Azt javasoljuk, hogy a védelmi csoportok tervezővel, hogy látni lehessen a munkaterhelések. Például hozzáadhat egy adott alkalmazás toohello futtató gépek ugyanabban a csoportban.
* Amikor gépek tooa védelmi csoport, a hello folyamatkiszolgáló automatikusan leküldéses értesítések, és hello mobilitási szolgáltatást telepíti, ha még nincs telepítve. Toohave hello leküldéses mechanizmus hello előző lépésben ismertetett módon előkészített lesz szüksége.

### <a name="add-machines-tooa-protection-group"></a>Vegye fel a gépek tooa védelmi csoporthoz

1. Nyissa meg túl**védett elemek** > **védelmi csoport** > **gépek** > **hozzáadása gépek** .
2. Ha a VMware virtuális gépek védi **virtuális gépek kijelölése**, egy vCenter-kiszolgálót, amely kezeli a virtuális gépeken (vagy hello EXSi gazdagépet, amelyen futtatja), majd válassza ki és hello gépek.

    ![Virtuális gépek védelmének engedélyezése](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)
3. Ha a fizikai kiszolgálók védi **virtuális gépek kijelölése**, nyissa meg hello **fizikai gépek felvétele** varázsló adjon meg hello IP-címet és egy rövid nevet. Ezután válassza ki a hello operációsrendszer-családot.

   ![Fizikai kiszolgálók védelmének engedélyezése](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)
4. A **adja meg tároló erőforrásait**, válassza ki a replikáció használata hello tárfiókot, és válassza ki, hogy a munkaterhelések használhatók hello-beállítások. Prémium szintű storage-fiókok jelenleg nem támogatottak.

   > [!NOTE]
   > Nem támogatjuk hello segítségével létrehozott áthelyezése tárfiókok [Azure-portálon](../storage/storage-create-storage-account.md) erőforráscsoportok között.                           
   > [A tárfiókok áttelepítési](../azure-resource-manager/resource-group-move-resources.md) erőforrás csoportok belül hello ugyanahhoz az előfizetéshez vagy előfizetések között nem támogatott a Site Recovery telepítéséhez használt tárfiókok.
   >
   >

    ![Cél beállításainak konfigurálása](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)
5. A **meg fiókok**, jelölje be hello fiókra [beállítása](#install-the-mobility-service-with-push-installation) toouse a mobilitási szolgáltatás hello automatikus telepítéséhez.

    ![Adjon meg olyan fiókokat](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)
6. Kattintson a hello pipa toofinish gépek toohello védelmi csoport és toostart gép kezdeti replikációja minden hozzáadása.

   > [!NOTE]
   > Ha ügyfélleküldéses telepítést készült, a mobilitási szolgáltatás hello automatikusan a, amelyek nem rendelkeznek, mint azok van-e adva toohello védelmi csoport készülékre van telepítve. Hello szolgáltatás telepítése után egy védelmi feladatot elindul, és sikertelen lesz. Hello meghibásodás után minden olyan gép, amely volt-e telepíteni a mobilitási szolgáltatás hello toomanually újraindítás lesz szüksége. Hello az újraindítás után hello védelmet feladat újra kezdődik, és kezdeti replikáció.
   >
   >

Figyelheti a hello állapotát **feladatok** lap.

![A feladatok lapon hello állapotának figyelése](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Védelmi állapot is figyelhetők a **védett elemek** > *védelmi csoport neve* > **virtuális gépek**. Miután a kezdeti replikáció befejezését követően, és szinkronizálja az adatokat, a gép állapotmódosítások túl**védett**.

![A figyelő állapota a védett elemek](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)

## <a name="step-11-set-protected-machine-properties"></a>11. lépés: Védett gép tulajdonságai
1. Után a számítógép rendelkezik egy **védett** állapotát, a feladatátvételi tulajdonságait konfigurálhatja. Hello védelmi csoport részleteit, válassza a számítógép és a nyitott hello hello **konfigurálása** fülre.
2. A Site Recovery automatikusan javasol hello Azure virtuális gép tulajdonságait, és hello a helyi hálózati beállítások észleli.

    ![Virtuális gép beállításainak megadása](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)
3. Ezeket a beállításokat módosíthatja:

   * **Az Azure virtuális gép neve**: hello neve, amely kap toohello gép az Azure-ban a feladatátvételt követően. hello nevét meg kell felelnie, Azure-követelményeknek.
   * **Az Azure Virtuálisgép-méretet**: hello hálózati adapterek számát határozza meg hello méretét adja meg a hello cél virtuális gépen. -Méretek és adapterek kapcsolatos további információkért lásd: hello [táblák méretezés](../virtual-machines/linux/sizes.md). Vegye figyelembe:

     * Módosítsa a virtuális gépek hello méretét és hello beállítások mentéséhez, a hálózati adapterek száma hello változik-e, hello megnyitásakor **konfigurálása** lapon hello legközelebb. hello minimális hálózati adaptereken a cél virtuális gépek száma a forrás virtuális gép hálózati adapterei minimális száma egyenlő toohello. hálózati adapterek maximális száma hello hello hello virtuális gép mérete határozza meg.
       * Hálózati adapterek hello forrásgépen hello száma-e kisebb vagy egyenlő, mint adapterek száma toohello engedélyezett hello célgép mérete, hello cél lesz hello ugyanannyi adapter hello forrásaként.
       * Hello hello forrás virtuális gép adaptereinek száma meghaladja hello hello cél méretéhez engedélyezett, ha hello cél maximális használható.
        Például ha a forrásgépen két hálózati adaptert, és hello célgép mérete négy támogatja, akkor hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, de hello támogatott célméretet támogatja a csak egy, a hello célgépen csak egy adapter fog működni.
     * Ha a hello virtuális gépnek több hálózati adaptert, az összes adapter legyen-e a csatlakoztatott toohello azonos Azure-hálózatot.
   * **Azure-hálózat**: meg kell adnia, hogy Azure virtuális gépek lesz-e a csatlakoztatott tooafter feladatátvétel az Azure-hálózatot. Ha nem adja meg egy, a hello Azure virtuális gépek nem csatlakoztatott tooany hálózati. Ezenkívül toospecify az Azure-hálózatot kell, ha azt szeretné, hogy toofailback Azure toohello a helyszíni helyről. Feladat-visszavétel szükséges egy VPN-kapcsolat az Azure-hálózatot és egy a helyszíni hálózat között.
   * **Az Azure IP-cím vagy alhálózat**: az egyes hálózati adaptereken, válassza ki a hello alhálózati toowhich hello Azure virtuális gép csatlakozni. Vegye figyelembe, hogy ha hello hálózati adapter hello forrásgép konfigurált toouse egy statikus IP-címet, megadhatja a hello Azure virtuális gép statikus IP-címet. Ha nem ad meg egy statikus IP-címet, majd bármilyen elérhető IP-címet oszt ki. Ha hello cél IP-cím van megadva, de már használja egy másik Azure-ban, a feladatátvétel sikertelen lesz. Ha hello hálózati adapter hello forrásgép konfigurált toouse DHCP, akkor kell DHCP hello beállításként az Azure-bA.      

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>12. lépés: A helyreállítási terv létrehozása, és a feladatátvétel futtatása
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failover/player]
>
>

Lefuttathat egy feladatátvételi egyetlen gép, vagy több virtuális gép, amelyek azonos feladat, vagy futtassa hello hello hajtanak végre feladatokat átveheti ugyanaz az alkalmazás. azonos: hello gépek toofail keresztül több idő, tooa helyreállítási terv hozzáadja őket.

a helyreállítási terv toocreate:

1. A hello **helyreállítási tervek** kattintson **adja hozzá a helyreállítási terv** , és adja hozzá a helyreállítási terv. Adja meg a hello terv adatait, és válassza ki **Azure** hello célként.

 ![Konfigurálja a helyreállítási terv](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)
2. A **válassza ki a virtuális gép**, egy védelmi csoportot, majd válassza ki és gépek hello csoport tooadd toohello helyreállítási tervben.

 ![Virtuális gépek hozzáadása](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Testre szabhatja hello terv toocreate csoportok és a gépek hello helyreállítási tervben feladatátvételt hello sorrendje. Azt is megteheti, parancsprogramok és manuális műveletek kér. Parancsfájlok manuálisan vagy segítségével hozhatók létre [Azure Automation-forgatókönyveket](site-recovery-runbook-automation.md). A helyreállítási terv testreszabásával kapcsolatos további információkért lásd: [helyreállítási tervek létrehozása](site-recovery-create-recovery-plans.md).

## <a name="run-a-failover"></a>Feladatátvétel futtatása
Mielőtt feladatátvételt:

* Győződjön meg arról, hogy hello felügyeleti kiszolgálón fut, és elérhető. Ha nem, a feladatátvétel sikertelen lesz.
* Ha a nem tervezett feladatátvétel futtatása:

  * Ha lehetséges, a nem tervezett feladatátvétel előtt állítsa le az elsődleges gépeket. Ez biztosítja, hogy mindkét hello futó hello forrás és a replika gépek nem rendelkezik ugyanannyi időt vesz igénybe. Ha VMware virtuális gépeket replikál egy nem tervezett feladatátvételt futtatásakor, megadhatja, hogy a Site Recovery érdemes kipróbálni tooshut hello forrásgépek le. Attól függően, hogy az elsődleges hely hello hello állapotát ez vagy nem működik. Ha fizikai kiszolgálókat replikál, a Site Recovery nem ezt a lehetőséget kínál.
  * Nem tervezett feladatátvétel leállítja az elsődleges gépek replikálása, úgy, hogy változásadatokat nem továbbítja a nem tervezett feladatátvétel megkezdése után.
  * Ha a feladatátvételt követően tooconnect toohello replika virtuális gép az Azure-ban, engedélyezze a távoli asztali kapcsolat hello forrásgépen, hello feladatátvétel végrehajtása előtt. Majd engedélyezze a távoli ASZTAL kapcsolaton keresztül hello tűzfalon keresztül. Azt is el kell tooallow RDP hello nyilvános végpontja hello Azure virtuális gép a feladatátvételt követően. Hajtsa végre a [ajánlott eljárások](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) tooensure, amely RDP működik a feladatátvétel után.

> [!NOTE]
> tooget hello legjobb teljesítményt érhesse el egy feladatátvételt tooAzure, győződjön meg arról, hogy a védett hello gépen telepítette hello Azure ügynök. Ez segít hello gép indítása gyorsabban, és segít a problémák elemzéséhez. hello Azure ügynök nem érhető el [Linux](https://github.com/Azure/WALinuxAgent) és [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
A teszt feladatátvételi toosimulate futtassa a feladatátvételi és helyreállítási folyamatok, elszigetelt hálózatot nincs hatással az éles környezetben, és lehetővé teszi, hogy a szokásos replikáció folytatja a normál. A feladatátvételi teszt initiatd hello forrás, és a több módon is futtathatja:

* **Ne adjon meg egy Azure-hálózatra**: Ha a hálózat nélkül feladatátvételi tesztet futtatja, hello teszt ellenőrzi, hogy a virtuális gépek indítása, és megfelelően jelennek meg az Azure-ban. Virtuális gépek a feladatátvételt követően nem csatlakoztatott tooan Azure-hálózatot.
* **Adja meg az Azure-hálózatot**: az ilyen típusú feladatátvételi ellenőrzi, hogy a várt módon megjelenik hello egész replikációs környezet, és, hogy az Azure virtuális gépek csatlakoztatott toohello megadott hálózati.

feladatátvételi teszt toorun:

1. A hello **helyreállítási tervek** lapon, válassza ki a hello tervet, és kattintson **feladatátvételi teszt**.

 ![Válassza ki a hello terv](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)
2. A **erősítse meg a feladatátvételi teszt**, jelölje be **nincs** tooindicate, hogy nem szeretné, hogy az Azure-hálózatot toouse hello feladatátvételi teszthez, vagy válassza hello hálózati toowhich hello teszt virtuális gépek a feladatátvételt követően fog csatlakozni. Kattintson a hello pipa toostart hello feladatátvételi.

 ![Válasszon ki egy](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)
3. Hello a feladatátvételi folyamat előrehaladásának figyeléséhez **feladatok** fülre.

 ![A figyelő folyamatban](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)
4. Hello feladatátvétel befejezése után meg kell jelennie toosee hello replika Azure machine megjelennek **virtuális gépek** a hello Azure-portálon. Ha azt szeretné, hogy tooinitiate egy RDP-kapcsolat toohello Azure virtuális Gépen, szüksége lesz a 3389-es port tooopen hello VM végponton.
5. Miután megismerte kész, amikor feladatátvételi művelet elér hello **Complete** tesztelési fázisban, kattintson a **a teljes teszt** toofinish. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez.
6. Kattintson a **hello a feladatátvételi teszt befejeződött** tooautomatically tisztítása hello tesztkörnyezetben. Ebben az esetben után hello feladatátvételi teszthez megjelenik-e egy **Complete** állapotát. Összes elemet és virtuális gépek hello teszt feladatátvétele során automatikusan létrehozza a rendszer törli. Emiatt ha két hétnél tovább tartó feladatátvételi tesztek, toofinish.

### <a name="run-an-unplanned-failover"></a>A nem tervezett feladatátvétel
Nem tervezett feladatátvétel Azure kezdeményezi, és akkor is, ha nem érhető el elsődleges hely hello hajtható végre.

1. A hello **helyreállítási tervek** lapon, válassza ki a hello tervet, és kattintson **feladatátvételi** > **nem tervezett feladatátvétel**.

 ![Válassza ki a hello terv](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)
2. Ha VMware virtuális gépeket replikál, megpróbálhatja tooshut le a helyszíni virtuális gépek. Ez a legjobb művelet, és tovább tartó feladatátvételi e hello elérhető sikeres-e vagy sem. Ha nem jár sikerrel, hiba részletei jelennek meg hello **feladatok** lap **nem tervezett feladatátvétel feladatok**.

 ![A helyszíni virtuális gépek leállítása beállítása](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

 > [!NOTE]
 > Ez a lehetőség nem érhető el, ha fizikai kiszolgálókat replikál. Szüksége lesz tootry tooshut azokat le manuálisan Ha lehetséges.
 >
 >

3. A **megerősítéséhez feladatátvétel**, ellenőrzi a hello feladatátvételi irányát (tooAzure), és válassza ki a megjeleníteni kívánt toouse hello feladatátvételi hello helyreállítási pontot. Több virtuális Gépre kiterjedő replikáció tulajdonságainak konfigurálásakor vannak engedélyezve, akkor helyreállíthatja toohello legújabb alkalmazás vagy összeomlás-konzisztens helyreállítási pontot. Igény szerint kiválaszthatja **egyéni helyreállítási pont** toorecover tooan korábbi időpontra történő. Kattintson a hello pipa toostart hello feladatátvételi.

 ![Erősítse meg a feladatátvételi iránya](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)
4. Várjon, amíg hello nem tervezett feladatátvétel feladat toofinish. Figyelheti a feladatátvételi folyamat előrehaladásának a hello **feladatok** fülre. Akkor is, ha hiba történik, nem tervezett feladatátvétel során, hello helyreállítási terv fut, amíg nem fejeződik be. Meg kell jelennie toosee hello replika Azure machine megjelennek **virtuális gépek** a hello Azure-portálon.

### <a name="connect-tooreplicated-azure-virtual-machines-after-failover"></a>Csatlakozás tooreplicated Azure virtuális gépek a feladatátvételt követően
tooconnect tooreplicated virtuális gépek Azure-ban a feladatátvételt követően lesz szüksége:

- A távoli asztali kapcsolat hello elsődleges gépen engedélyezve van.
- A Windows tűzfal hello elsődleges gépen tooallow RDP állítva.
- RDP toohello hello Azure virtuális gép nyilvános végpontot hozzáadni.

Ennek beállítása kapcsolatban bővebben lásd: [ASR használatával feladatátvételt követően távoli asztali kapcsolat hibaelhárítási](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="deploy-additional-process-servers"></a>További folyamat-kiszolgálók központi telepítése
Ha a központi telepítés meghaladja a 200 forrásgépek kell horizontális, vagy a teljes napi változási sebessége meghaladja a 2 TB, a további folyamat kiszolgálók toohandle hello adatforgalma lesz szüksége. egy további folyamat kiszolgáló, a jelölőnégyzet hello követelményeinek tooset [további folyamat kiszolgálók](#additional-process-servers), majd állítsa be megfelelően toohello hello folyamatkiszolgáló utasításai és. Hello kiszolgáló beállítása után forrás gépek toouse konfigurálhatja azt.

### <a name="set-up-an-additional-process-server"></a>További folyamat kiszolgáló beállítása
Az alábbiak szerint állíthatja be egy további folyamatkiszolgáló:

* Egyesített hello varázsló tooconfigure egy felügyeleti kiszolgáló futtatása csak a folyamatkiszolgáló.
* Ha toomanage adatreplikáció csak hello új folyamat-kiszolgáló használatával, meg kell toomigrate a védett gépek.

### <a name="install-hello-process-server"></a>Hello folyamatkiszolgáló telepítése
1. A hello **gyors üzembe helyezés** lapján töltse le a Site Recovery-összetevő telepítése hello hello egyesített telepítési fájlját. Futtassa a telepítőt.
2. A **megkezdése előtt**, jelölje be **további folyamat kiszolgálók tooscale ki a központi telepítés hozzáadása**.

 ![Folyamatkiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)
3. Hello varázsló befejezéséhez, mint amikor Ön [beállítása](#step-5-install-the-management-server) hello első felügyeleti kiszolgálót.
4. A **konfigurációs kiszolgáló adatait**, adja meg a hello hello azt az eredeti felügyeleti kiszolgálót, amelyen telepítette hello konfigurációs kiszolgáló IP-címét, és írja be a hello jelszót. Ha még nem rendelkezik hello jelszót, futtassa  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** a hello eredeti felügyeleti kiszolgáló tooretrieve azt.

 ![Konfigurációs kiszolgáló adatait](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-toouse-hello-new-process-server"></a>Gépek toouse hello új folyamat-kiszolgáló áttelepítése
1. Nyissa meg **konfigurációs kiszolgálók** > **Server** > *hello eredeti felügyeleti kiszolgáló neve*  >   **Kiszolgálóadatok**.

 ![Kiszolgáló részletei](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)
2. A hello **folyamat kiszolgálók** listáról válassza ki **Folyamatkiszolgáló módosítása** következő toohello kiszolgálóra, amelyet az toochange.

 ![Hello folyamatkiszolgáló frissítése](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)
3. Válassza ki **Folyamatkiszolgáló módosítása**, jelölje be **folyamat-célkiszolgálót**, és ezután válasszon hello új felügyeleti kiszolgálót. Ezután válassza hello virtuális gépek, amelyek az új folyamatkiszolgáló hello fogja kezelni. Kattintson a hello információ ikonra tooget hello kiszolgáló adatainak. hello szükséges tooreplicate, hogy elvégezte a döntések betöltése megjelenített toohelp minden kijelölt virtuális gép toohello új folyamatkiszolgáló átlagos lemezterület. Kattintson a hello pipa toostart toohello új folyamatkiszolgáló replikálása.

 ![Hello folyamatkiszolgáló módosítása](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)

## <a name="vmware-permissions-for-vcenter-access"></a>VMware vCenter hozzáférési engedélyeinek
hello folyamatkiszolgáló automatikusan fel tud deríteni a virtuális gépek a vCenter-kiszolgáló. automatikus felderítés tooperform, kell toodefine (Azure_Site_Recovery) szerepkör hello vCenter szintű tooallow Site Recovery tooaccess hello vCenter kiszolgálón. Ha csak toomigrate VMware gépek tooAzure kell, és nem kell az Azure-ból toofailback, megadhat egy csak olvasható szerepkör, amely elegendő-e. A hello engedélyek beállítása [6. lépés: állítsa be a hitelesítő adatait a vCenter-kiszolgáló hello](#step-6-set-up-credentials-for-the-vcenter-server). hello szerepkörengedélyek hello a következő táblázat foglalja össze:

| **Szerepkör** | **Részletek** | **Engedélyek** |
| --- | --- | --- |
| Azure_Site_Recovery szerepkör |VMware virtuális gép felderítése |Ezek a jogosultságok hello-v-központ kiszolgáló hozzárendelése:<br/><br/>Datastore: Szóköz, Tallózás datastore, alacsony szintű fájlműveletek foglal le, akkor távolítsa el a fájlt, a frissítés virtuálisgép-fájlok<br/><br/>Hálózati: Hálózati hozzárendelése<br/><br/>Erőforrás: Rendelje hozzá a virtuális gép tooresource készlet, virtuális gép áttelepítése kikapcsolt, az alkalmazás bekapcsolja a virtuális gép áttelepítése<br/><br/>Feladatok: A feladat, a frissítési feladat létrehozása<br/><br/>Virtuális gép > konfiguráció<br/><br/>Virtuális gép > kommunikáció > válaszoljon a kérdést, eszköz kapcsolat, CD konfigurálása media, konfigurálás hajlékonylemez media, kapcsolja ki a bekapcsolás, VMware-eszközök telepítése<br/><br/>Virtuális gép > Készlet > létrehozása, regisztrálása, Unregister<br/><br/>Virtuális gép > kiépítési > engedélyezése virtuális gép letölthető, a virtuális gép fájlok feltöltése<br/><br/>Virtuális gép > pillanatképek > pillanatképek eltávolítása |
| vCenter felhasználói szerepkör |Felderítési VMware virtuális gép feladatátvételi a virtuális gép leállása nélkül |Ezek a jogosultságok hello-v-központ kiszolgáló hozzárendelése:<br/><br/>Adatközpont-objektum > propagálása tooChild objektum, a szerepkör = csak olvasható <br/><br/>hello felhasználói hello datacenter szinten van hozzárendelve, és ezáltal access tooall hello objektumok hello adatközpontban. Ha azt szeretné, hogy toorestrict hello hozzáférés, rendelje hozzá az hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** toohello gyermekobjektum (ESX-gazdagépeken, datastores, virtuális gépek és hálózatok) objektum. |
| vCenter felhasználói szerepkör |Feladatátvétel és feladat-visszavétel |Ezek a jogosultságok hello-v-központ kiszolgáló hozzárendelése:<br/><br/>Datacenter objektum – propagálása toochild objektum, a szerepkör = Azure_Site_Recovery<br/><br/>hello felhasználói datacenter szinten van hozzárendelve, és ezáltal access tooall hello objektumok hello adatközpontban.  Ha azt szeretné, hogy toorestrict hello hozzáférés, rendelje hozzá az hello ** nem lehet hozzáférni ** hello szerepkört **propagálása toochild objektum** toohello gyermekobjektum (ESX-gazdagépeken, datastores, virtuális gépek és hálózatok). |

## <a name="third-party-software-notices-and-information"></a>Harmadik felek szoftvereivel, megjegyzések és információk
<!--Do Not Translate or Localize-->

hello szoftver és a belső vezérlőprogram futó hello Microsoft termék vagy szolgáltatás alapul, vagy a szerződés magában foglalja a hello anyag projektek lenti (együttesen "harmadik féltől származó kód").  A Microsoft hello hello harmadik féltől származó kód nem eredeti szerzője.  hello eredeti szerzői és a licenc, amely alatt a Microsoft az ilyen harmadik féltől származó kód kapott alábbi van beállítva.

hello leírtak A harmadik féltől származó kód hello projektek-összetevőt az alább felsorolt kapcsolódik. Ilyen licencek és információkat csak tájékoztatási célokat szolgálnak.  A harmadik féltől származó kód folyamatban van a Microsoft szoftverlicenc-feltételek hello Microsoft termék vagy szolgáltatás a Microsoft által relicensed tooyou.  

b. hello információk kapcsolatos harmadik féltől származó kód összetevők végrehajtott elérhető tooyou Microsoft hello eredeti licencelési feltételei szerint.

hello teljes fájl is található a hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft kifejezetten át nem adott, az összes jogot fenntart által Ennek következménye, hogy belül vagy más módon.

## <a name="next-steps"></a>Következő lépések
[További információ a feladat-visszavétel](site-recovery-failback-azure-to-vmware-classic.md) toobring a átvevő gépek Azure-beli biztonsági tooyour helyszíni környezetben.
