---
title: "Kiszolgáló rendelkezésre állási csoportok – Azure virtuális gépek - vész-helyreállítási aaaSQL |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooconfigure egy SQL Server rendelkezésre állási csoport és egy másik régióban egy replika Azure virtuális gépeken."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: df6238dc61c5a56879c75c9bf7314c618f43c0ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a>Always On rendelkezésre állási csoport konfigurálása az Azure virtuális gépeken különböző régiókban

Ez a cikk azt ismerteti, hogyan tooconfigure egy SQL Server Always On rendelkezésre állási csoport replika Azure távolról Azure virtuális gépeken. A konfigurációs toosupport katasztrófa utáni helyreállítás használata.

Ez a cikk vonatkozik tooAzure virtuális gépek erőforrás-kezelő módban.

hello következő kép bemutatja egy rendelkezésre állási csoport gyakori telepítési Azure virtuális gépeken:

   ![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

A központi telepítésben lévő összes virtuális gépet egy Azure-régiót szerepelnek. hello rendelkezésre állási csoport replikái szinkron véglegesítésre SQL-1 és SQL-2 automatikus feladatátvétellel is rendelkezhetnek. toobuild ebbe az architektúrába, lásd: [rendelkezésre állási csoport sablon vagy oktatóanyag](virtual-machines-windows-portal-sql-availability-group-overview.md).

Ez az architektúra sebezhető toodowntime esetén hello Azure-régió, elérhetetlenné válik. tooovercome biztonsági rést, adja hozzá a replika egy másik Azure-régiót. a következő ábra azt mutatja be, hogyan hello új architektúra lenne hello:

   ![Rendelkezésre állási csoport vész-Helyreállítási](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

hello előző ábrán látható SQL-3 nevű új virtuális gépet. SQL-3 egy másik Azure-régióban van. SQL-3 toohello Windows Server feladatátvevő fürt kerül. SQL-3 rendelkezhet egy rendelkezésre állási csoportreplikákhoz. Végül figyelje meg, hogy hello Azure-régió, az SQL-3 tartalmaz egy új Azure terheléselosztót.

>[!NOTE]
> Egy Azure rendelkezésre állási csoportot, ha egynél több virtuális gép hello ugyanabban a régióban. Ha csak egy virtuális gép hello régióban van, majd hello rendelkezésre állási csoport nincs szükség. Csak egy virtuális gép elhelyezheti rendelkezésre állási készlet létrehozása során. Hello virtuális gép már a rendelkezésre állási csoportok, ha később további replika virtuális gép is hozzáadhat.

Ebben az architektúrában hello replika hello távoli régióban általában konfigurálják, az aszinkron véglegesítésű rendelkezésre állási mód és a feladatátvételi módot kézire.

Ha rendelkezésre állási csoport replikái az Azure virtuális gépek különböző Azure-régiókban, minden egyes régió van szükség:

* A virtuális hálózati átjáró
* A virtuális hálózati átjáró kapcsolat

hello alábbi ábrán látható hello hálózatok adatközpontok közötti kommunikációra.

   ![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Ez az architektúra terhel kimenő adatok Azure-régiók közötti replikált adatokat. Lásd: [sávszélesség árképzési](http://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Távoli replika létrehozása

a replika egy távoli adatközpontban toocreate hello lépéseket követve:

1. [Virtuális hálózat létrehozása a hello új régió](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

1. [Konfigurálja a VNet – VNet-kapcsolatot, a hello Azure-portálon](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >Bizonyos esetekben előfordulhat, hogy toouse PowerShell toocreate hello VNet – VNet-kapcsolatot. Például ha különböző Azure-fiókra, hello kapcsolat hello portál nem konfigurálható. Ebben az esetben megtekintéséhez [konfigurálása egy VNet – VNet-kapcsolat használatával hello Azure-portálon](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Hozzon létre egy tartományvezérlő hello új régióban](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Ez a tartományvezérlő biztosítja a hitelesítést, ha hello tartományvezérlő hello elsődleges helyen nem érhető el.

1. [SQL Server virtuális gép létrehozása hello új régióban](virtual-machines-windows-portal-sql-server-provision.md).

1. [Hozzon létre egy Azure terheléselosztó a hello új régió hello hálózat](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

   Ez a terheléselosztó kell:

   - A kell hello azonos hálózati és alhálózati, új virtuális gép hello.
   - Egy statikus IP-címet hello rendelkezésre állási csoport figyelőjének rendelkezik.
   - Csak a virtuális gépek hello hello ugyanabban a régióban, a terheléselosztó hello álló háttérkészlet tartalmazza.
   - A TCP port mintavételi adott toohello IP-címet használja.
   - Rendelkezik a terheléselosztási szabály adott toohello SQL Server a hello ugyanabban a régióban.  

1. [Adja hozzá a Feladatátvételi fürtszolgáltatás funkció toohello új SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [Tartományhoz hello új SQL Server toohello](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

1. [Állítsa be a hello új SQL Server szolgáltatás fiók toouse egy olyan tartományi fiók](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).

1. [Adja hozzá a hello új SQL Server toohello Windows Server feladatátvevő fürt](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).

1. Hozzon létre egy IP-cím erőforrás hello fürtön.

   Létrehozhat hello IP-cím erőforrás a Feladatátvevőfürt-kezelőben. Hello rendelkezésre állási csoport ellenőrzéshez kattintson jobb gombbal kattintson **erőforrás hozzáadása**, **több erőforrások**, és kattintson a **IP-cím**.

   ![IP-cím létrehozása](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Az IP-cím konfigurálása a következőképpen:

   - Hello távoli adatközpontban hello hálózati használja.
   - Hello új Azure terheléselosztó hello IP-címet hozzárendelni. 

1. Az új SQL Server az SQL Server Configuration Manager, hello [engedélyezze az Always On rendelkezésre állási csoportok](http://msdn.microsoft.com/library/ff878259.aspx).

1. [Nyissa meg tűzfalportjai hello új SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

   hello portszámok tooopen van szüksége a környezettől függ. Megnyitott portok hello tükrözési végpont és az Azure terheléselosztó betölteni.

1. [Adja hozzá a replika toohello rendelkezésre állási csoport hello új SQL Server](http://msdn.microsoft.com/library/hh213239.aspx).

   A replika a távoli Azure-régió állítsa kézi feladatátvételre aszinkron replikációra.  

1. Adja hozzá hello IP-cím erőforrás hello figyelő ügyfél hozzáférési pont (hálózatnév) a fürt függőségei.

   hello következő képernyőfelvétel egy megfelelően konfigurált IP-cím fürt erőforrás:

   ![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >hello fürterőforrás-csoport mindkét IP-címeket tartalmaz. Mindkét IP-címei hello figyelő ügyfél-hozzáférési pont a függőségeit. Használjon hello **vagy** operátor szerepel a következő hello függőségi fürtkonfiguráció.

1. [A PowerShell hello fürt paraméterek beállítása](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).

PowerShell-parancsprogrammal hello hello fürt hálózati név, IP-címet, és a mintavételi portot a hello konfigurált terheléselosztó hello új régióban.

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster name for hello network in hello new region (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "<IPResourceName>" # hello cluster name for hello new IP Address resource.
   $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB) in hello new region. This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <nnnnn> # hello probe port you set on hello ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a>Set-kapcsolatot a több alhálózattal

hello replika hello távoli adatközpontban hello rendelkezésre állási csoport része, de egy másik alhálózat van. Ha ez a másodpéldány hello elsődleges másodpéldány, alkalmazás időtúllépésnek fordulhat elő. Ez a viselkedés van hello ugyanaz, mint a helyi rendelkezésre állási csoport egy több alhálózatos környezetben. ügyfélalkalmazások, tooallow kapcsolódásának hello ügyfél kapcsolat frissítése, vagy konfigurálhatja a névfeloldást, gyorsítótárazás hello fürt hálózatnév-erőforrás.

Lehetőleg, frissítse a hello ügyfél kapcsolati karakterláncok tooset `MultiSubnetFailover=Yes`. Lásd: [kapcsolódás a MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).

Hello kapcsolati karakterláncok nem módosítható, ha a megoldás-gyorsítótárazás is konfigurálhatja. Lásd: [kapcsolat időtúllépése több alhálózatot a rendelkezésre állási csoportban](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).

## <a name="fail-over-tooremote-region"></a>Feladatok átadása tooremote régió

tootest figyelő kapcsolat toohello távoli régió, átveheti hello replika toohello távoli régióban. Míg a hello replika aszinkron, a feladatátvétel során sebezhető toopotential adatvesztés. adatvesztés nélküli keresztül toofail hello rendelkezésre állási mód toosynchronous és hello feladatátvételi mód tooautomatic beállítása. A lépéseket követve hello használata:

1. A **Object Explorer**, csatlakozzon toohello hello elsődleges másodpéldányt futtató SQL Server példányát.
1. A **AlwaysOn rendelkezésre állási csoportok**, **rendelkezésre állási csoportok**, kattintson a jobb gombbal a rendelkezésre állási csoportot, és kattintson a **tulajdonságok**.
1. A hello **általános** lap **rendelkezésre állási másodpéldányok**, set hello másodlagos replika hello vész-Helyreállítási hely toouse **szinkron véglegesítés** rendelkezésre állási módjának és **Automatikus** feladatátvételi mód.
1. Ha egy másodlagos másodpéldány az elsődleges replika, a magas rendelkezésre állású azonos helyen található, állítsa be az adott replika túl**aszinkron véglegesítés** és **manuális**.
1. Kattintson az OK gombra.
1. A **Object Explorer**, kattintson a jobb gombbal a hello rendelkezésre állási csoportot, és kattintson a **irányítópult megjelenítése**.
1. Az irányítópult hello, győződjön meg arról, hogy hello hello vész-Helyreállítási helyen replika szinkronizálása.
1. A **Object Explorer**, kattintson a jobb gombbal a hello rendelkezésre állási csoportot, és kattintson a **feladatátvételi...** . SQL Server Management Studios megnyílik egy varázsló toofail keresztül SQL Server.  
1. Kattintson a **következő**, és jelölje be hello SQL Server-példány hello vész-Helyreállítási helyen. Kattintson a **következő** újra.
1. Csatlakozzon az SQL Server-példány toohello hello vész-Helyreállítási helyen, és kattintson a **következő**.
1. A hello **összegzés** lapon ellenőrizze a hello beállításait, majd kattintson **Befejezés**.

Kapcsolat tesztelése után helyezze át a hello elsődleges replika hátsó tooyour elsődleges data center és hello rendelkezésre állási mód hátsó tootheir normál működési beállításait. hello következő táblázatban hello normál működési beállításait a jelen dokumentumban ismertetett hello architektúra:

| Hely | Server-példány | Szerepkör | Rendelkezésre állási mód | Feladatátvételi mód
| ----- | ----- | ----- | ----- | -----
| Elsődleges adatközpont | SQL-1 | Elsődleges | Szinkron | Automatikus
| Elsődleges adatközpont | SQL-2 | Másodlagos | Szinkron | Automatikus
| Távoli vagy másodlagos adatközpont | SQL-3 | Másodlagos | Aszinkron | Manuális


### <a name="more-information-about-planned-and-forced-manual-failover"></a>További információ a tervezett és a rendszer kényszerített kézi feladatátvétel

További információkért tekintse meg a következő témakörök hello:

- [Végezzen el egy tervezett kézi feladatátvételt egy rendelkezésre állási csoport (SQL Server)](http://msdn.microsoft.com/library/hh231018.aspx)
- [Hajtsa végre a kényszerített kézi feladatátvétel egy rendelkezésre állási csoport (SQL Server)](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>További hivatkozások

* [Always On rendelkezésre állási csoportok](http://msdn.microsoft.com/library/hh510230.aspx)
* [Az Azure virtuális gépek](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure Load Balancer Terheléselosztók](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Az Azure rendelkezésre állási csoportok](../manage-availability.md)
