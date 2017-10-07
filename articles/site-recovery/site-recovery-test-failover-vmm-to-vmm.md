---
title: "aaaTest feladatátvevő (VMM tooVMM) az Azure Site Recovery |} Microsoft Docs"
description: "Az Azure Site Recovery koordinálja hello replikáció, feladatátvétel és helyreállítási virtuális gépek és fizikai kiszolgálók. További információk a feladatátvételi tooAzure vagy egy másodlagos adatközpontba."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: pratshar
ms.openlocfilehash: 6b4f65ab692cbb0665102c4f51ea0694151cd3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-failover-vmm-toovmm-in-site-recovery"></a>(A VMM tooVMM) feladatátvételi teszt a Site Recovery szolgáltatásban


Ez a cikk bemutatja, és utasításokat a feladatátvételi teszt vagy a vész-helyreállítási részletezési virtuális gépek (VM) és fizikai kiszolgálók, amelyek az Azure Site Recovery védett. Azt ismertetjük, hogy egy System Center Virtual Machine Manager VMM által felügyelt helyszíni hely hello helyreállítási helyként.

Futtassa a teszt feladatátvételi toovalidate a replikációs stratégiát, vagy hajtsa végre a vész-Helyreállítási részletezési adatvesztés vagy leállás nélkül. Feladatátvételi teszt nem rendelkezik gyakorolt hatás, folyamatban lévő replikáció hello, vagy az éles környezetben. Futtatható virtuális gép vagy egy [helyreállítási terv](site-recovery-create-recovery-plans.md). Feladatátvételi teszt folyamatban váltanak, amikor szüksége toospecify hello hálózatra, amelyhez hello teszt virtuális gépek csatlakozni fognak. Előrehaladásának hello hello a feladatátvételi tesztet a hello **feladatok** lap.  

Ha megjegyzéseit vagy kérdéseit, fel őket a cikk vagy hello hello alsó [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-hello-infrastructure-for-test-failover"></a>A feladatátvételi teszthez hello infrastruktúra előkészítése
Ha egy meglévő hálózati használatával toorun feladatátvételi tesztet, akkor készítse elő az Active Directory, a DHCP és DNS erre a hálózatra.

Feladatátvételi teszt toorun hello beállítás toocreate Virtuálisgép-hálózatok automatikusan használatával, adja egy manuális lépés előtt csoport-1 hello helyreállítási tervben, hogy a hello feladatátvételi teszthez toouse fog. Adja hozzá a hello infrastruktúra erőforrások toohello automatikusan létrehozott hálózati hello a feladatátvételi teszt futtatása előtt.

### <a name="things-toonote"></a>Dolgot toonote
Ha replikál tooa másodlagos hely, hello típusú hálózati hello replika gépek által használt feladatátvételi teszt végrehajtásához használt logikai hálózatot toomatch hello típusú nem szükséges, de néhány kombináció, hogy nem működik. Hello replika használ a DHCP- és a VLAN-alapú elkülönítés, ha a hello hello replika Virtuálisgép-hálózatot nem szükséges egy statikus IP-címkészletet. Hello feladatátvételi teszthez Windows Hálózatvirtualizálás használatával nem fognak működni, mert nincs címkészletek érhetők el. 

Emellett a hello a feladatátvételi teszt nem fog működni, ha hello replika hálózati elkülönítés nélküli és hello teszthálózat használja a Windows Hálózatvirtualizálás. Ennek az az oka hello elkülönítés nélküli hálózat nem rendelkezik a hello alhálózatok szükséges toocreate egy Windows Hálózatvirtualizálás hálózathoz.

Hogyan a replika virtuális gépek csatlakoztatott toomapped Virtuálisgép-hálózatok találhatók után feladatátvételi, attól függ, hogyan hello VMM-konzol Virtuálisgép-hálózat hello lett konfigurálva.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Nincs elkülönítés vagy VLAN-elkülönítéssel Virtuálisgép-hálózatot
Ha DHCP hello Virtuálisgép-hálózathoz van definiálva, hello replika virtuális gép hello beállításokban megadott hello hálózati helyhez társított logikai hálózat hello keresztül csatlakoztatott toohello VLAN-Azonosítót. hello virtuális gép fogad hello rendelkezésre álló DHCP-kiszolgáló az IP-címét. 

Egy statikus IP-címkészletet toodefine hello cél Virtuálisgép-hálózat nem szükséges. Egy statikus IP-címkészletet hello Virtuálisgép-hálózat használata esetén a hello replika virtuális gép hello beállításokban megadott hello hálózati helyhez társított logikai hálózat hello keresztül csatlakoztatott toohello VLAN-Azonosítót.

virtuális gép hello kap IP-címének hello hello Virtuálisgép-hálózathoz definiált. Ha egy statikus IP-címkészletet hello cél Virtuálisgép-hálózat nincs megadva, az IP-címek hozzárendelését sikertelen lesz. Hello IP-címkészlet létrehozása mindkét hello forrás és cél VMM-kiszolgálókon a védelmi és helyreállítási használni.

#### <a name="vm-network-with-windows-network-virtualization"></a>Virtuálisgép-hálózat Windows Hálózatvirtualizálás
Ha egy Virtuálisgép-hálózatot a Windows Hálózatvirtualizálással van konfigurálva, meg kell határozni egy statikus címkészletet hello cél Virtuálisgép-hálózathoz, függetlenül attól, hogy hello forrás Virtuálisgép-hálózathoz beállított toouse DHCP vagy statikus IP-címkészletet. 

DHCP határozza meg, ha hello cél VMM-kiszolgáló DHCP-kiszolgálóként működik, és lehetővé teszi meghatározott hello készletből IP-címet hello cél Virtuálisgép-hálózatot. Hello forráskiszolgáló meg van adva egy statikus IP-címkészlet használatát, ha hello cél VMM-kiszolgáló IP-cím hello készletből foglal le. Mindkét esetben IP-címek hozzárendelését sikertelen lesz, ha nincs megadva egy statikus IP-címkészletet.


### <a name="prepare-dhcp"></a>DHCP előkészítése
Ha részt vesz hello virtuális gépek feladatátvételi teszt DHCP használatára, és egy teszt DHCP-kiszolgáló hello elszigetelt hálózatot a feladatátvételi tesztet célját hello belül.

### <a name="prepare-active-directory"></a>Az Active Directory előkészítése
toorun alkalmazás tesztelése a feladatátvételi tesztet, egy példánya szükséges hello éles Active Directory-környezetben a tesztkörnyezetben. További információkért tekintse át a hello [Active Directoryra vonatkozó feladatátvételi szempontokat részletező cikkben](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>DNS előkészítése
Készítse elő a DNS-kiszolgáló hello feladatátvételi teszthez a következőképpen:

* **DHCP**: Ha a virtuális gépek DHCP használatára, hello teszt DNS hello IP-címét frissíteni kell a hello teszt DHCP-kiszolgáló. Windows Hálózatvirtualizálás típusú hálózati használata, hello VMM-kiszolgáló hello DHCP-kiszolgálóként működik. Ezért DNS hello IP-címét frissíteni kell hello feladatátvételi teszt hálózatában. Ebben az esetben hello virtuális gépek regisztrálják magukat toohello megfelelő DNS-kiszolgáló.
* **Statikus cím**: Ha virtuális gépeket egy statikus IP-címet, IP-címet a DNS-kiszolgáló frissíteni kell a feladatátvételi teszt hálózatában hello vizsgálat hello. Szükség lehet a DNS tooupdate hello teszt virtuális gépek hello IP-címmel. Hello mintaparancsfájl erre a célra a következő használható:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
Ez az eljárás ismerteti, hogyan toorun feladatátvételi tesztet egy helyreállítási terv. Alternatív megoldásként futtathatja egyetlen virtuális gép feladatátvétele hello hello **virtuális gépek** fülre.

![Teszt feladatátvételi panel](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Válassza ki **helyreállítási tervek** > *recoveryplan_name*. Kattintson a **feladatátvételi** > **feladatátvételi teszt**.
1. A hello **feladatátvételi teszt** panelen adja meg, hogyan virtuális gépek legyen csatlakoztatott toonetworks hello a feladatátvételi teszt után. További információkért lásd: hello [hálózati beállítások](#network-options-in-site-recovery).
1. Feladatátvételi nyomon követni a hello **feladatok** fülre.
1. Miután a feladatátvétel befejeződött, győződjön meg arról, hogy hello virtuális gép sikeresen elindul-e.
1. Amikor elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ez a lépés törli az hello virtuális gépek és a hálózatokban, amelyek a feladatátvételi teszt során lettek létrehozva.


## <a name="network-options-in-site-recovery"></a>A Site Recovery szolgáltatásban a hálózati beállítások

Feladatátvételi teszt futtatásakor kéri a teszt replika gépek tooselect hálózati beállításait. Több lehetősége van.  

| **Teszt feladatátvételi beállítás** | **Leírás** | **Feladatátvétel ellenőrzése** | **Részletek** |
| --- | --- | --- | --- |
| **Feladatok átadása tooa VMM másodlagos hely – hálózati nélkül** |Ne válassza ki a Virtuálisgép-hálózatot. |Feladatátvételi ellenőrzi, hogy a teszt gépek jönnek létre.<br/><br/>hello teszt virtuális gép jön létre hello állomáson, ahol hello replika virtuális gép is létezik. Ahol hello replika virtuális gép is található toohello felhő nem adódik. |<p>hello átvevő gép nem csatlakoztatott tooany hálózati.<br/><br/>hello gép lehet csatlakoztatott tooa Virtuálisgép-hálózat létrehozása után. |
| **Feladatok átadása tooa VMM másodlagos hely--, hálózati** |Válasszon egy meglévő Virtuálisgép-hálózatot. |Feladatátvételi ellenőrzi, hogy a virtuális gépek jönnek létre. |hello teszt virtuális gép jön létre hello állomáson, ahol hello replika virtuális gép is létezik. Ahol hello replika virtuális gép is található toohello felhő nem adódik.<br/><br/>Az éles hálózattól elkülönített Virtuálisgép-hálózatot létrehozni.<br/><br/>A VLAN-alapú hálózati használata, azt javasoljuk, hogy hozzon létre egy külön logikai hálózat (nincs használatban az üzemi) a VMM-ben erre a célra. Ez a logikai hálózat használt toocreate Virtuálisgép-hálózatok feladatátvételi tesztet.<br/><br/>hello logikai hálózat társítva van az összes virtuális gép hello Hyper-V kiszolgáló hello hálózati adapterek közül legalább egy kell lennie.<br/><br/>A VLAN logikai hálózatok hello toohello logikai hálózathoz el kell különíteni hozzá hálózati helyek.<br/><br/>Windows Hálózatvirtualizálás-alapú logikai hálózat használata, az Azure Site Recovery automatikusan létrehozza elszigetelt Virtuálisgép-hálózatok. |
| **Sikertelen tooa VMM másodlagos hely--keresztül hálózat létrehozása** |Egy ideiglenes tesztet hálózathoz megadott hello beállítás alapján automatikusan jön létre **logikai hálózati** és a kapcsolódó hálózati helyeket. |Feladatátvételi ellenőrzi, hogy a virtuális gépek jönnek létre. |Használja ezt a beállítást, ha hello helyreállítási tervet használja egynél több Virtuálisgép-hálózatot. Windows Hálózatvirtualizálás hálózatok használata, ezt a lehetőséget is automatikusan hozzon létre Virtuálisgép-hálózat hello, ugyanazokkal a beállításokkal (alhálózatokat és IP-címkészletek) hello hálózat hello replika virtuális gép. Automatikusan kiüríti a Virtuálisgép-hálózatok hello a feladatátvételi teszt befejeződése után.</p><p>hello teszt virtuális gép jön létre hello állomáson, ahol hello replika virtuális gép is létezik. Ahol hello replika virtuális gép is található toohello felhő nem adódik. |

> [!TIP]
> hello tooa virtuális gép feladatátvételi tesztje során megadott IP-cím hello azonos IP-címet, amely a virtuális gép hello kapja a tervezett vagy nem tervezett feladatátvételre (feltéve, hogy hello IP-cím érhető el a feladatátvételi teszt hálózatában hello). Ha hello IP-cím nem érhető el a feladatátvételi teszt hálózatában hello hello virtuális gép egy másik IP-címet, amely elérhető a feladatátvételi teszt hálózatában hello kapja meg.
>
>


## <a name="test-failover-tooa-production-network-on-a-recovery-site"></a>A teszt feladatátvételi tooa éles hálózati környezetben a helyreállítási helyen
Azt javasoljuk, hogy akkor, ha feladatátvételi tesztet, akkor válasszon egy hálózatot, amely eltér a termelési helyreállítási helyen lévő hálózat a megadott [hálózatleképezés](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping). Azonban ha valóban toovalidate végpontok közötti hálózati kapcsolat átvevő virtuális gépen, vegye figyelembe a következő pontok hello:

* Győződjön meg arról, hogy hello elsődleges virtuális gép le van állítva, akkor, ha hello feladatátvételi teszthez. Ha ezt elmulasztja, a két virtuális gépek ugyanazzal az identitással fog futni, azonos hálózati hello hello hello azonos idő. Ilyen esetben is tooundesired következményekkel járhat.
* A módosításokat, hogy a virtuális gépek feladatátvételi teszt toohello tegyen elvesznek, tesztelésekor karbantartása hello feladatátvételi virtuális gépek. A módosítások nincsenek replikált hátsó toohello elsődleges virtuális gép.
* A módszerre tesztelése az éles alkalmazás toodowntime vezet. Kérje meg a felhasználókat hello alkalmazás nem toouse esetén hello vész-Helyreállítási részletezéshez hello alkalmazás van folyamatban.  


## <a name="next-steps"></a>Következő lépések
Próbálkozás után sikeresen futtatta a feladatátvételi tesztet, egy [feladatátvételi](site-recovery-failover.md).
