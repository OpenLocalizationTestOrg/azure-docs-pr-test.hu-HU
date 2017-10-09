---
title: "aaaHigh rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server |} Microsoft Docs"
description: "Információt a különböző típusú HADR stratégiák hello Azure virtuális gépeken futó SQL Server."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: 2b62d8b30520952ba6b7da7177a2c2de95bea8ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Magas rendelkezésre állás és vészhelyreállítás az Azure-beli SQL Server-alapú virtuális gépeken

A Microsoft Azure virtuális gépek (VM) az SQL Server magas rendelkezésre állási és vészhelyreállítási (HADR) helyreállítási adatbázis-megoldás költsége alacsonyabb hello segítségével. A legtöbb SQL Server HADR megoldás az Azure virtuális gépeken, csak Azure- és hibrid megoldásként támogatottak. A csak Azure megoldás hello teljes HADR rendszer fut, az Azure-ban. Hibrid konfigurációban hello megoldás részét képező Azure fut, és más részben futtatása helyszíni a szervezet hello. hello hello Azure környezetben rugalmasságával lehetővé teszi a toomove részben vagy teljesen tooAzure toosatisfy hello és a HADR követelmények az SQL Server adatbázis-e a rendszer.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-hello-need-for-an-hadr-solution"></a>Egy HADR megoldás hello szükségességét ismertetése
Tooyou tooensure fel, hogy az adatbázis-rendszer hello HADR képességek hello szolgáltatásszint-szerződéssel (SLA) igénylő rendelkezik-e. Azure által biztosított magas rendelkezésre állású mechanizmusok, például a felhőszolgáltatásokhoz és virtuális gépek helyreállítási hibaészlelés szolgáltatásjavításnak hello tény maga garantálja is megfelel a szükséges hello SLA-t. Ezek a mechanizmusok hello hello virtuális gépek magas rendelkezésre állású, de nem hello magas rendelkezésre állású virtuális gépek hello belül futó SQL Server védelme. Lehetőség az SQL Server-példány toofail hello közben hello virtuális gép online állapotú, és kifogástalan. Továbbá, még akkor is, hello magas rendelkezésre állású Azure által biztosított mechanizmusokkal a virtuális gépek hello állásideje esedékes tooevents például helyreállítás szoftveres vagy hardveres hibákat és azokat az operációs rendszer frissítése során.

Ezenkívül földrajzi redundáns tárolás (GRS) az Azure, georeplikáció nevezett szolgáltatással segítségével valósul meg, amely nem lehet egy megfelelő vész-helyreállítási megoldást az adatbázisok esetében. Georeplikálási aszinkron módon elküldi az adatokat, mert hello esemény katasztrófa a legújabb frissítéseket is elvesznek. Georeplikálási korlátozások kapcsolatos további tudnivalókért lásd: hello [georeplikáció nem támogatott az adat- és naplófájlok külön lemezeken](#geo-replication-support) szakasz.

## <a name="hadr-deployment-architectures"></a>HADR telepítési architektúrák
Azure által támogatott SQL Server HADR a technológiák többek között:

* [Always On rendelkezésre állási csoportok](https://technet.microsoft.com/library/hh510230.aspx)
* [Mindig a Feladatátvevőfürt-példányok](https://technet.microsoft.com/library/ms189134.aspx)
* [Naplóküldés](https://technet.microsoft.com/library/ms187103.aspx)
* [SQL Server biztonsági mentése és visszaállítása az Azure Blob Storage szolgáltatással](https://msdn.microsoft.com/library/jj919148.aspx)
* [Az adatbázis-tükrözés](https://technet.microsoft.com/library/ms189852.aspx) – az SQL Server 2016 elavult

Már lehetséges toocombine hello technológiák együttesen tooimplement egy SQL Server megoldást, amely a magas rendelkezésre állású és vész-helyreállítási funkciók is rendelkezik. Attól függően, hogy hello technológiát használ a hibrid telepítés hello Azure-beli virtuális hálózat a VPN-alagúton lehet szükség. az alábbi hello szakaszok mutatják be a hello Példa telepítési architektúrák.

## <a name="azure-only-high-availability-solutions"></a>Csak Azure: magas rendelkezésre állású megoldások

Az SQL Server egy magas rendelkezésre állású megoldás lehet az Always On rendelkezésre állási csoportok - nevű rendelkezésre állási csoportok egy adatbázis szintjén. Is létrehozhat egy magas rendelkezésre állású megoldás egy példány szintjén az mindig a Feladatátvevőfürt-példányok - feladatátvevő fürtpéldányok. További redundancia jelentkezhet létrehozhat redundancia mindkét szinten létrehozásával rendelkezésre állási csoportok feladatátvevő fürtpéldányok. 

| Technológia | Példa architektúrák |
| --- | --- |
| **Rendelkezésre állási csoportok** |Az Azure virtuális gépeken futó rendelkezésre állási másodpéldányok hello azonos régióban magas rendelkezésre állás biztosításához. Tooconfigure egy tartományvezérlő virtuális Gépnek, mert a Windows feladatátvételi fürtszolgáltatás szükséges Active Directory-tartományhoz kell.<br/> ![Rendelkezésre állási csoportok](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>További információkért lásd: [rendelkezésre állási csoportok konfigurálása az Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Feladatátvevőfürt-példányok** |Feladatátvevő fürt példány (FCI), amely megosztott tárolóra van szükség, 3 különböző módon is létrehozható.<br/><br/>1. Egy két csomópontos feladatátvevő fürt csatlakoztatott tárolók használata az Azure virtuális gépeken futó [Windows Server 2016 közvetlen tárolóhelyek \(S2D\) ](virtual-machines-windows-portal-sql-create-failover-cluster.md) tooprovide szoftveres virtuális SAN.<br/><br/>2. Külső fürtözési megoldás által támogatott tárolóval az Azure virtuális gépeken futó kétcsomópontos feladatátvevő fürt. Például egy adott SIOS DataKeeper használó, lásd: [magas rendelkezésre állás a Feladatátvételi fürtszolgáltatás és a 3. fél szoftver SIOS DataKeeper fájlmegosztás](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>3. Az Azure virtuális gépeken futó távoli iSCSI cél megosztott blokktárolást keresztül ExpressRoute kétcsomópontos feladatátvevő fürt. NetApp titkos tárolási (NPS) például egy iSCSI célhoz, ExpressRoute keresztül Equinix tooAzure virtuális gépek mutatja.<br/><br/>Külső megosztott tárolási és replikációs megoldás feladatátvételi problémák kapcsolódó tooaccessing adattárolás hello gyártójához forduljon.<br/><br/>Vegye figyelembe, hogy használja a FCI [Azure File storage](https://azure.microsoft.com/services/storage/files/) még nem támogatott, mert ez a megoldás nem használja a prémium szintű Storage. Dolgozunk ennek toosupport a legrövidebb időn belül. |

## <a name="azure-only-disaster-recovery-solutions"></a>Csak Azure: vész-helyreállítási megoldások
A vész-helyreállítási megoldást rendelkezik az SQL Server-adatbázisokat az Azure-ban rendelkezésre állási csoportok, az adatbázis-tükrözés vagy biztonsági másolat, és állítsa vissza a tárolási BLOB.

| Technológia | Példa architektúrák |
| --- | --- |
| **Rendelkezésre állási csoportok** |Rendelkezésre állási másodpéldányok több adatközpontot az Azure virtuális gépeken vész-helyreállítási futtatásáért. A kereszt-régió megoldás a webhely teljes kimaradás véd. <br/> ![Rendelkezésre állási csoportok](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Összes replika, régión belül kell hello belül ugyanaz a felhőalapú szolgáltatás, és ugyanazt a virtuális hálózatot hello. Mivel minden egyes régió egy külön virtuális hálózat fog rendelkezni, akkor ezek a megoldások VNet tooVNet kapcsolatot igényelnek. További információkért lásd: [konfigurálása egy VNet – VNet-kapcsolat használatával hello Azure-portálon](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Részletes útmutatásért lásd: [egy SQL Server rendelkezésre állási csoport konfigurálása Azure virtuális gépeken különböző régiókban](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Az adatbázis-tükrözés** |Elsődleges és tükrözött és vész-helyreállítási különböző adatközpontokban futtató kiszolgálók. Kiszolgálói tanúsítványok használatával, mert egy active directory-tartomány nem terjedhetnek ki több adatközpontot kell telepítenie.<br/>![Az adatbázis-tükrözés](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Biztonsági mentés és visszaállítás az Azure Blob Storage szolgáltatással** |Éles környezetben használt adatbázisait biztonsági mentés közvetlenül tooblob tárolási egy vész-helyreállítási ugyanabban az adatközpontban.<br/>![Biztonsági mentés és visszaállítás](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>További információkért lásd: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hibrid informatikai: Vész-helyreállítási megoldások
Rendelkezik egy vész-helyreállítási megoldást hibrid-környezetben használatával a rendelkezésre állási csoportok, az adatbázis-tükrözés, a naplóküldési és a biztonsági mentés az SQL Server adatbázisok esetében, és állítsa vissza az Azure blog tárolóval.

| Technológia | Példa architektúrák |
| --- | --- |
| **Rendelkezésre állási csoportok** |Egyes rendelkezésre állási másodpéldányok futó Azure virtuális gépek és egyéb helyileg futó többhelyes vész-helyreállítási replika. hello éles hely lehet helyi vagy egy Azure-adatközpontban található.<br/>![Rendelkezésre állási csoportok](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Mivel az összes rendelkezésre állási másodpéldányok kell lennie a hello ugyanazt a feladatátvevő fürtöt, hello fürt kell span mindkét hálózatok (több alhálózatos feladatátvevő fürt). Ez a konfiguráció Azure és a hello a helyszíni hálózat között VPN kapcsolatot igényel.<br/><br/>Sikeres vész-helyreállítási adatbázis telepítenie kell egy replika tartományvezérlő hello vész helyreállítási helyen.<br/><br/>Már lehetséges toouse hello Hozzáadás replika varázsló az SSMS tooadd egy Azure replika tooan meglévő Always On rendelkezésre állási csoportnak. További információkért lásd az oktatóanyag: az Always On rendelkezésre állási csoportnak tooAzure kiterjesztése. |
| **Az adatbázis-tükrözés** |Egy Azure virtuális gép és az egyéb hello fut egy partneri helyileg futó többhelyes vész-helyreállítási kiszolgálói tanúsítványok használatával. Partnerek nem szükséges a toobe hello Active Directory-tartományhoz, és nem VPN-kapcsolatra szükség.<br/>![Az adatbázis-tükrözés](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Egy másik adatbázis-tükrözési forgatókönyv szerint egy Azure virtuális gép fut egy partneri és hello más fut a helyszíni hello az azonos többhelyes vész-helyreállítási Active Directory-tartományhoz. A [VPN-kapcsolat hello Azure virtuális hálózat és hello a helyszíni hálózat között](../../../vpn-gateway/vpn-gateway-site-to-site-create.md) szükséges.<br/><br/>Sikeres vész-helyreállítási adatbázis telepítenie kell egy replika tartományvezérlő hello vész helyreállítási helyen. |
| **Naplóküldés** |Egy Azure virtuális gép és egyéb hello rendszert futtató kiszolgálóról helyileg futó többhelyes vész-helyreállítási. Naplóküldésben függ Windows fájlmegosztás, így hello Azure-beli virtuális hálózat és hello közötti VPN-kapcsolat a helyszíni hálózatra azért szükség.<br/>![Naplóküldés](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Sikeres vész-helyreállítási adatbázis telepítenie kell egy replika tartományvezérlő hello vész helyreállítási helyen. |
| **Biztonsági mentés és visszaállítás az Azure Blob Storage szolgáltatással** |A helyi biztonsági mentés közvetlenül tooAzure blob-tároló vész-helyreállítási éles környezetben használt adatbázisait.<br/>![Biztonsági mentés és visszaállítás](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>További információkért lásd: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Fontos tudnivalók találhatók az SQL Server HADR az Azure-ban
Az Azure virtuális gépeken, tárolási és hálózatkezelési rendelkezik egy helyszíni informatikai infrastruktúra nem virtualizált eltérő működési jellemzői. Egy HADR SQL Server megoldást az Azure-ban egy sikeres végrehajtásához szükséges, hogy ezek a különbségek megismeréséhez, és a megoldás tooaccommodate tervezési őket.

### <a name="high-availability-nodes-in-an-availability-set"></a>Magas rendelkezésre állású csomópont a rendelkezésre állási csoportok
Az Azure-ban rendelkezésre állási készletek lehetővé teszik a tooplace hello magas rendelkezésre állású csomópontok külön tartalék tartományok (FDs) és a frissítési tartományok (UDs). Azure virtuális gépek toobe helyezett hello azonos rendelkezésre állási csoportot, ha már telepítette azokat a hello ugyanaz a felhőalapú szolgáltatás. Ugyanazt a felhőalapú szolgáltatás részt vehetnek hello egyetlen csomópontjának azonos rendelkezésre állási csoport hello. További információkért lásd: [kezelése hello virtuális gépek rendelkezésre állási](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>A feladatátvevő fürt viselkedés Azure hálózatkezelés
hello nem RFC-kompatibilis DHCP-szolgáltatás az Azure-ban okozhat egyes feladatátvételi fürt konfigurációk toofail fürthálózat nevének toohello folyik a hozzárendelése egy ismétlődő IP-cím miatt hello létrehozását, például hello azonos IP-cím, rendelkezésre álló hello fürtcsomópontok. Ez a probléma, attól függ, a Windows feladatátvételi fürtszolgáltatás hello rendelkezésre állási csoportok bevezetésekor.

Amikor egy két csomópontos fürt létrehozása és a hálózatra, vegye figyelembe hello forgatókönyv:

1. hello fürtön online állapotba kerül, majd csomópont1 hello fürthálózat nevének egy dinamikusan hozzárendelt IP-címet igényel.
2. Csomópont1 saját IP-címe eltérő IP-cím adja meg hello DHCP-szolgáltatást, mivel a DHCP szolgáltatás hello felismeri hello kérés csomópont1 származik magát.
3. Windows észleli, ismétlődő címet tooNODE1 és a toohello feladatátvevő fürt hálózati neve van hozzárendelve, és hello alapértelmezett fürtcsoport toocome online sikertelen lesz.
4. hello alapértelmezett fürtcsoport tooNODE2, amely csomópont1 tartozó IP-cím tekinti hello IP-címet, és hello alapértelmezett fürt csoport online állapotba hozása helyezi.
5. Amikor csomópont2 csomópont1 tooestablish kapcsolatot próbál, 1. csomópont célzó csomagok sosem hagyják el csomópont2, mert ez megoldja-csomópont1 tartozó IP-cím tooitself. 2. csomópont nem tud csomópont1 kapcsolatot létrehozni, majd megszűnik a kvóruma és hello fürt leáll.
6. A hello addig 1. csomópont küldhet csomagokat tooNODE2, de nem tud válaszolni, csomópont2. 1. csomópont megszűnik a kvóruma, és leállítja a hello fürt.

Ebben a forgatókönyvben egy nem használt statikus IP-címet, például egy kapcsolatszintű IP-cím például 169.254.1.1, toohello fürthálózat nevének rendelés toobring hello a fürt hálózati neve online hozzárendelésével elkerülhető. toosimplify ez feldolgozni, lásd: [konfigurálása Windows feladatátvevő fürtöt az Azure-ban a rendelkezésre állási csoportok](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

További információkért lásd: [rendelkezésre állási csoportok konfigurálása az Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Rendelkezésre állási csoport figyelőjének támogatása
Rendelkezésre állási csoport figyelői Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 és Windows Server 2016 rendszert futtató Azure virtuális gépeken támogatottak. Ez a támogatás hello használata elosztott terhelésű végpont már engedélyezve van a rendelkezésre állási csoport csomópontokat hello Azure virtuális gépeken tette lehetővé. Ön köteles betartani hello figyelői toowork mindkét ügyfél alkalmazásokhoz az Azure, valamint a helyszíni futó futó különleges konfigurációs lépések is szükségesek.

A figyelő beállításának két fő lehetőség: a külső (nyilvános) vagy belső. hello külső (nyilvános) figyelő által használt, egy internetre terheléselosztóhoz, és társítva, egy nyilvános virtuális IP-cím (VIP) keresztül elérhető internet hello. Egy belső figyelő belső terheléselosztót használ, és csak által támogatott ügyfelek hello ugyanazt a virtuális hálózatot. Vagy a betöltés típusokra, engedélyeznie kell a közvetlen kiszolgálói válasz. 

Hello rendelkezésre állási csoport több Azure alhálózattal (például olyan környezetben, Azure-régiók keverve) is, ha hello ügyfél kapcsolati karakterláncot tartalmaznia kell "**MultisubnetFailover = True**". Az eredmény párhuzamos kapcsolódási kísérletek toohello replikák hello különböző alhálózatokon. Egy figyelő beállításával kapcsolatos útmutatásért lásd:

* [Egy ILB figyelőt, a rendelkezésre állási csoportok konfigurálása az Azure-](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Egy külső figyelőt a rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-ext-listener.md).

Továbbra is csatlakoztathatja tooeach rendelkezésre állási másodpéldány külön csatlakozzon közvetlenül toohello szolgáltatáspéldány. Is, mivel a rendelkezésre állási csoportok visszamenőleg kompatibilis az adatbázis-tükrözési ügyfelek, a kapcsolódás toohello rendelkezésre állási másodpéldányok például adatbázis-tükrözési partnerei mindaddig, amíg hello replikák konfigurált hasonló toodatabase tükrözés:

* Egy elsődleges másodpéldány és egy másodlagos másodpéldány
* hello másodlagos replika van konfigurálva, nem olvasható (**olvasható másodlagos** beállítás túl**nem**)

Egy példa ügyfél kapcsolati karakterlánc, amely megfelel a toothis adatbázis-tükrözés hasonló konfigurációs ADO.NET vagy SQL Server Native Client alatt van:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Ügyfélkapcsolat további információkért lásd:

* [Kapcsolati karakterlánc kulcsszavak használatával az SQL Server natív ügyfél](https://msdn.microsoft.com/library/ms130822.aspx)
* [Csatlakozás az ügyfelek tooa adatbázis-tükrözési munkamenet (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
* [A hibrid informatikai csoport figyelőjének tooAvailability csatlakozás](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Rendelkezésre állási csoport figyelői ügyfélkapcsolat és alkalmazás feladatátvételi (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Az adatbázis-tükrözési kapcsolati karakterláncok használatával a rendelkezésre állási csoportokkal](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>A hibrid informatikai hálózati késés
Ha telepíti a HADR megoldást hello azt feltételezi, hogy a nagy hálózati késés a helyszíni hálózat és az Azure közötti idő is lehet. Replikák tooAzure telepítésekor kell használnia az aszinkron véglegesítésű helyett szinkron véglegesítésre hello szinkronizálási mód. Ha az adatbázis-tükrözés kiszolgálók telepítése mind a helyszíni és Azure hello nagy teljesítményű mód használata helyett hello magas biztonsági mód.

### <a name="geo-replication-support"></a>A georeplikáció támogatása
Az Azure-lemezeket a georeplikáció nem támogatja a hello adatfájl és -ugyanazon adatbázis toobe lemezen tárolja a külön hello naplófájlt. Georedundáns replikál az egyes lemezek külön és aszinkron módon. Ez a módszer biztosítja hello írási sorrendjét belül egyetlen lemezt a hello georeplikált másolatán, de több lemez georeplikált példánya között nem. Ha egy adatbázis toostore adja meg az adatfájl és a naplófájl külön lemezeken, a hello után egy olyan vészhelyzet esetén előfordulhat, hogy tartalmazza a hello naplófájlt, mely hello írási megjelenített automatikus bejelentkezés az SQL Server- és sav hello mint hello adatfájl legújabb másolatát helyre lemezek tranzakció tulajdonságai. Hello storage-fiók nem rendelkezik hello beállítás toodisable georeplikáció, ha minden adat legyen, és naplófájlok hello egy adott adatbázis azonos lemez. Ha hello adatbázis toohello mérete miatt több lemez kell használnia, hello vész-helyreállítási megoldások egyikét tooensure adatredundanciát fent felsorolt toodeploy kell.

## <a name="next-steps"></a>Következő lépések
Ha az SQL Server Azure virtuális gép toocreate van szüksége, tekintse meg [Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).

tooget hello legjobb teljesítmény érdekében az SQL Serverről az Azure virtuális gépen található hello útmutatót lásd: [teljesítmény ajánlott eljárások az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-performance.md).

Egyéb témakörök kapcsolódó toorunning SQL Server Azure virtuális gépeken, a következő témakörben: [SQL Server Azure virtuális gépeken](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Egyéb erőforrások
* [Egy új Active Directory-erdő telepítése az Azure-ban](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [A rendelkezésre állási csoportok Azure virtuális gépen a feladatátvevő fürt létrehozása](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

