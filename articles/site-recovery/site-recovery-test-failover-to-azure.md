---
title: "a helyreállítás aaaTest feladatátvételi tooAzure |} Microsoft Docs"
description: "További tudnivalók a helyi tooAzure feladatátvételi tesztet futtatja"
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
ms.date: 07/04/2017
ms.author: pratshar
ms.openlocfilehash: fa0a93f409cc9f2c2c06c2d91c65971dc90c507f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test--failover-tooazure-in-site-recovery"></a>A Site Recovery feladatátvételi tooAzure tesztelése



Ez a cikk bemutatja, és utasításokat a feladatátvételi teszt vagy a vész-Helyreállítási részletezési a virtuális gépek és fizikai kiszolgálók, amelyek az Azure használatával hello helyreállítási helyként, a Site Recovery védett.

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

A feladatátvételi teszt futtatása toovalidate a replikációs stratégiát, vagy hajtsa végre a vész-helyreállítási részletezéshez adatvesztés vagy leállás nélkül. A teszt feladatátvétel nem rendelkezik semmilyen hatása, folyamatban lévő replikáció hello, vagy az éles környezetben. A feladatátvételi teszt el lehet végezni a virtuális gép vagy egy [helyreállítási terv](site-recovery-create-recovery-plans.md). Feladatátvételi teszt váltanak, úgy kell toospecify hello hálózati toowhich teszt virtuális gép csatlakozni fog. Miután feladatátvételi teszt akkor váltódik ki, nyomon követheti a folyamat állapotát a hello **feladatok** lap.  


## <a name="supported-scenarios"></a>Támogatott helyzetek
Feladatátvételi teszt nem támogatott az összes központi telepítési forgatókönyv [örökölt VMware-hely tooAzure](site-recovery-vmware-to-azure-classic-legacy.md). A feladatátvételi teszt nem is támogatott, ha a virtuális gép átadja tooAzure.  


## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
Ez az eljárás ismerteti, hogyan toorun feladatátvételi tesztet egy helyreállítási terv. Másik lehetőségként is futtathatja egyetlen gép teszt feladatátvétele hello megfelelő lehetőséget rajta.

![Feladatátvétel tesztelése](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Válassza ki **helyreállítási tervek** > *recoveryplan_name*. Kattintson a **feladatátvételi teszt**.
1. Válassza ki a **helyreállítási pont** toofailover számára. Hello alábbi beállítások egyikét használhatja:
    1.  **Legújabb feldolgozott**: Ez a beállítás átadja a feladatokat az összes virtuális gép hello helyreállítási terv toohello legújabb helyreállítási pont, amely a Site Recovery szolgáltatás feldolgozása már megtörtént. A virtuális gép feladatátvételi tesztje során, időbélyegzőjét hello legutóbbi feldolgozott helyreállítási pontot is látható. Akkor használatos, ha a helyreállítási terv feladatátvételi, nyissa meg tooindividual virtuális gépet, és nézze meg **legújabb helyreállítási pontok** csempe tooget ezt az információt. Nincs időt vesz igénybe tooprocess hello feldolgozatlan adatokat, ez a beállítás egy alacsony RTO (helyreállítási idő célkitűzése) feladatátvételi lehetőséget biztosít.
    1.  **Legutóbbi alkalmazáskonzisztens**: Ez a beállítás átadja a feladatokat a hello helyreállítási terv toohello legújabb alkalmazáskonzisztens helyreállítási pontnak, amely a Site Recovery szolgáltatás által már feldolgozott összes virtuális gépet. A virtuális gép feladatátvételi tesztje során, időbélyegzőjét hello legutóbbi alkalmazáskonzisztens helyreállítási pontnak is látható. Akkor használatos, ha a helyreállítási terv feladatátvételi, nyissa meg tooindividual virtuális gépet, és nézze meg **legújabb helyreállítási pontok** csempe tooget ezt az információt.
    1.  **Legújabb**: Ez a beállítás először dolgozza fel a minden hello adat, amelyet elküldött tooSite helyreállítási szolgáltatás toocreate minden egyes virtuális géphez a helyreállítási pont előtt tooit feladatátadás őket. Ezt a lehetőséget biztosít a hello legalacsonyabb rpo-val Rendelkeznek (Helyreállításipont-célkitűzés) hello létrehozott virtuális gépek után feladatátvételi lesz minden hello adat, amelyet replikált tooSite helyreállítási szolgáltatás hello feladatátvételi indítását.
    1.  **Legújabb virtuális Gépre kiterjedő feldolgozott**: Ez a beállítás csak érhető el helyreállítási tervek, amelyeken legalább egy virtuális gép több virtuális Gépre kiterjedő konzisztencia. Virtuális gépek, amelyek egy replikációs csoport feladatátvételi toohello legújabb közös virtuális Gépre kiterjedő konzisztens helyreállítási pontot. Más virtuális gépek feladatátvételi tootheir legutóbbi feldolgozott helyreállítási pontot.  
    1.  **Legújabb virtuális Gépre kiterjedő alkalmazáskonzisztens**: Ez a beállítás csak érhető el, amelyek rendelkeznek legalább egy virtuális gép több virtuális Gépre kiterjedő konzisztencia ON helyreállítási tervek. Virtuális gépek részei egy replikációs csoport feladatátvételi toohello legújabb közös virtuális Gépre kiterjedő alkalmazáskonzisztens helyreállítási pontnak. Más virtuális gépek feladatátvételi tootheir utolsó alkalmazáskonzisztens helyreállítási pont. 
    1.  **Egyéni**: Ha egy virtuális gép feladatátvételi tesztje során, akkor használhatja ezt a beállítást toofailover tooa adott helyreállítási pontot.
1. Válasszon egy **Azure-beli virtuális hálózat**: Adjon meg egy Azure virtuális hálózatra, ahol hello teszt virtuális gépek jönnek létre. A Site Recovery kísérletet tett toocreate teszt virtuális gépek azonos nevű alhálózat, és azonos IP mint, amelyek a használatával hello **számítás és hálózat** hello virtuális gép beállításai között. Ha azonos nevű alhálózat nem érhető el a hello feladatátvételi teszthez megadott Azure virtuális hálózat teszt virtuális gép létrehozása hello első alhálózat ábécésorrendben vannak, majd. Azonos IP hello alhálózat nem érhető el, ha virtuális gépet egy másik hello alhálózat elérhető IP-cím lekérése. Ebben a szakaszban olvasható [további részletekért](#creating-a-network-for-test-failover)
1. Ha feladat-visszavétele tooAzure keresztül, és az adattitkosítás engedélyezve van, a **titkosítási kulcs** válassza hello kibocsátott tanúsítványt, ha engedélyezte az adattitkosítást a szolgáltató telepítése során. Ha nincs engedélyezve a titkosítás hello virtuális gépen, kihagyhatja ezt a lépést.
1. Feladatátvételi nyomon követni a hello **feladatok** fülre. Meg kell tudni toosee hello teszt replika gépére hello Azure-portálon.
1. tooinitiate egy RDP-kapcsolat hello virtuális gépen, akkor túl[egy nyilvános IP-cím hozzáadása](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) hello a hálózati adapterének hello átvenni a virtuális gép. Ha a rendszer a számítógép tooa klasszikus virtuális gép feladatátvétele, akkor meg kell túl[végpont hozzáadása](../virtual-machines/windows/classic/setup-endpoints.md) 3389-es porton
1. Amikor elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. A **megjegyzések** és hello feladatátvételi teszthez kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ez a művelet törli a feladatátvételi teszt során létrehozott hello virtuális gépek.


> [!TIP]
> A Site Recovery kísérletet tett toocreate teszt virtuális gépek azonos nevű alhálózat, és azonos IP mint, amelyek a használatával hello **számítás és hálózat** hello virtuális gép beállításai között. Ha azonos nevű alhálózat nem érhető el a hello feladatátvételi teszthez megadott Azure virtuális hálózat teszt virtuális gép létrehozása hello első alhálózat ábécésorrendben vannak, majd. Ha hello cél IP-címet a kiválasztott alhálózat hello része, a site recovery megpróbálja toocreate hello teszt feladatátvételi virtuális gép hello cél IP-címet használ. Ha hello cél IP-címet nem része a kiválasztott alhálózat hello majd teszt feladatátvételi virtuális gép jön létre bármilyen elérhető IP-alhálózatot a kiválasztott hello használata.
>
>

## <a name="test-failover-job"></a>A tesztfeladat feladatátvételt

![Feladatátvétel tesztelése](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Feladatátvételi teszt aktiválása esetén magában foglalja a következő lépéseket:

1. Előfeltételek ellenőrzése: Ez a lépés biztosítja, hogy a feladatátvétel szükséges összes feltétel teljesül-e
1. Feladatátvétel: Ez a lépés hello adatokat dolgozza fel, így készen, hogy egy Azure virtuális gépek hozhatók létre belőle. Ha úgy döntött, **legújabb** helyreállítási pont ebben a lépésben létrehoz egy helyreállítási pontot, amely el lett küldve toohello szolgáltatás hello adatokból.
1. Kezdete: Ebben a lépésben egy Azure virtuális gép hello adatfeldolgozási hello előző lépésben használatával hoz létre.

## <a name="time-taken-for-failover"></a>A feladatátvételi idő

Bizonyos esetekben virtuális gépeinek feladatátvételi egy extra köztes lépés, amely általában körülbelül 8 too10 perc toocomplete van szükség. Ezekben az esetekben vannak, mint a következő:

* VMware virtuális gépeket a mobilitási szolgáltatás régebbi, mint 9.8 hello verziójával
* Fizikai kiszolgálók
* VMware Linux virtuális gépek
* Fizikai kiszolgálóként védett Hyper-V virtuális gépek
* VMware virtuális gépek nincsenek jelen, ahol következő illesztőprogramok rendszerindító illesztőprogramok
    * storvsc
    * VMBus
    * storflt
    * Intelide
    * ATAPI
* VMware virtuális gépek, amelyek nem rendelkeznek a DHCP-szolgáltatás engedélyezve van, függetlenül attól, hogy használják DHCP vagy statikus IP-címek

Az összes hello más esetekben a köztes lépésre nincs szükség, és határozottan kevesebb hello feladatátvételi hello idő.


## <a name="creating-a-network-for-test-failover"></a>A feladatátvételi teszthez-hálózat létrehozása
Javasoljuk, hogy amikor a teszt feladatátvétel választhat olyan hálózatot, amely el van különítve a termelési helyreállítási helyen lévő hálózat a megadott **számítás és hálózat** hello virtuális gép beállításait. Alapértelmezés szerint egy Azure virtuális hálózat létrehozásakor az elkülönül más hálózatokkal. Ehhez a hálózathoz az éles hálózattól kell utánoznia:

1. Teszt hálózati alhálózatok, amely a termelési hálózat és hello ugyanaz, mint amelyek a termelési hálózat alhálózatai hello neve azonos számú kell rendelkeznie.
1. Teszt hálózati használandó hello azonos IP-címtartomány legyen, mint az éles hálózattól.
1. Frissítés hello hello teszt hálózat IP-cím, hello DNS virtuális gépek IP célként megadott hello a DNS-kiszolgálón **számítás és hálózat** beállításait. Lépkedjen végig [active Directoryra vonatkozó feladatátvételi szempontokat részletező cikkben](site-recovery-active-directory.md#test-failover-considerations) című szakaszban talál további információt.


## <a name="test-failover-tooa-production-network-on-recovery-site"></a>A teszt feladatátvételi tooa éles hálózati környezetben a helyreállítási helyen
Javasoljuk, hogy amikor a teszt feladatátvétel választhat egy hálózatot, amely eltér a termelési helyreállítási helyen lévő hálózat a megadott **számítás és hálózat** hello virtuális gép beállításait. De ha toovalidate end tooend kapcsolatok egy sikertelen átadó virtuális gép, vegye figyelembe a következő pontok hello:

1. Győződjön meg arról, hogy hello elsődleges virtuális gép leállítási hello a feladatátvételi teszt során. Ha nem így tesz, nem lesznek hello két virtuális gépek ugyanazzal az identitással fut hello azonos hálózati: hello ugyanannyi időt vesz igénybe, és hogy tooundesired következmények vezethet.
1. Hello teszt feladatátvételi virtuális gépekké végzett módosításokat kellene elveszhet meg karbantartási hello teszt feladatátvételi virtuális gépek. Ezek a változások nem lesznek replikált hátsó toohello elsődleges virtuális gép.
1. A módszerre tesztelése az éles alkalmazás tooa állásidő vezet. Hello alkalmazás felhasználóinak kell kérni a hello vész-Helyreállítási részletezéshez toonot használata hello alkalmazás van folyamatban.  



## <a name="prepare-active-directory-and-dns"></a>Active Directory és a DNS előkészítése
toorun alkalmazás tesztelése a feladatátvételi tesztet, egy példánya szükséges hello éles Active Directory-környezetben a tesztkörnyezetben. Lépkedjen végig [active Directoryra vonatkozó feladatátvételi szempontokat részletező cikkben](site-recovery-active-directory.md#test-failover-considerations) című szakaszban talál további információt.

## <a name="prepare-tooconnect-tooazure-vms-after-failover"></a>Felkészülés a tooconnect tooAzure virtuális gépek a feladatátvételt követően

Ha azt szeretné, hogy a virtuális gépek tooconnect tooAzure, feladatátvételt követően RDP segítségével ellenőrizze, hogy a műveletek hello táblázatban összefoglalt hello.

**Feladatátvétel** | **Hely** | **Műveletek**
--- | --- | ---
**Windows operációs rendszert futtató Azure virtuális gép** | A feladatátvétel előtt a helyi számítógépen | tooaccess hello Azure virtuális Gépen keresztül hello internet, engedélyezze az RDP-, győződjön meg arról, hogy a TCP és UDP szabályok hozzáadása történik meg a hello **nyilvános**, és hogy engedélyezve van az RDP a **Windows tűzfal** > **engedélyezett Alkalmazások**, minden profil esetében.<br/><br/> a pont-pont keresztül tooaccess engedélyezze az RDP-hello gépen, és győződjön meg arról, hogy engedélyezi-e az RDP a hello **Windows tűzfal** -> **engedélyezett alkalmazások és szolgáltatások** a  **Tartomány és a saját** hálózatok.<br/><br/>  Ellenőrizze, hogy hello operációs rendszer TÁROLÓHÁLÓZATI szabályzatát állítsa túl**OnlineAll**. [További információk](https://support.microsoft.com/kb/3031135).<br/><br/> Győződjön meg arról, hogy nincsenek függőben lévő frissítések a Windows hello virtuális gépen, amikor feladatátvételt indít el. A Windows update előfordulhat, hogy indítható, ha a feladatátvételt, és nem fogja tudni toologin toohello virtuális gép csak hello frissítés befejeződése után. <br/><br/>
**Windows operációs rendszert futtató Azure virtuális gép** | Azure virtuális gépen a feladatátvételt követően | A klasszikus virtuális gép [vegyen fel egy nyilvános végpontot](../virtual-machines/windows/classic/setup-endpoints.md) a hello RDP protokollba (3389-es port)<br/><br/>  A Resource Manager virtuális gép [egy nyilvános IP-cím hozzáadása](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) rajta.<br/><br/> hello hálózati biztonsági csoport szabályainak hello virtuális gép a feladatátvételt, és hello van csatlakoztatva, Azure-alhálózaton toowhich kell tooallow bejövő kapcsolatok toohello RDP-portjára.<br/><br/> A Resource Manager virtuális gép, ellenőrizheti a **rendszerindítási diagnosztika** toolook hello virtuális gép egy képernyőkép:<br/><br/> Ha nem sikerül, ellenőrizze, hogy hello virtuális gép fut, és keresse meg a következő [hibaelhárítási tippek](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).<br/><br/>
**Az Azure virtuális gépet** | A feladatátvétel előtt a helyi számítógépen | Győződjön meg arról, hogy hello hello Azure virtuális gép Secure Shell szolgáltatás értéke toostart automatikusan rendszerindításkor.<br/><br/> Ellenőrizze, hogy a tűzfalszabályok engedélyezik-e egy SSH-kapcsolat tooit.
**Az Azure virtuális gépet** | Azure virtuális gép a feladatátvételt követően | hello hálózati biztonsági csoport szabályainak hello virtuális gép a feladatátvételt, és hello van csatlakoztatva, Azure-alhálózaton toowhich kell tooallow bejövő kapcsolatok toohello SSH-port.<br/><br/> A klasszikus virtuális gép [vegyen fel egy nyilvános végpontot](../virtual-machines/windows/classic/setup-endpoints.md) kell létrehozni, a bejövő kapcsolatok tooallow hello SSH-port (TCP-port 22 alapértelmezés szerint).<br/><br/> A Resource Manager virtuális gép [egy nyilvános IP-cím hozzáadása](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) rajta.<br/><br/> A Resource Manager virtuális gép, ellenőrizheti a **rendszerindítási diagnosztika** toolook hello virtuális gép egy képernyőkép:<br/><br/>



## <a name="next-steps"></a>Következő lépések
Miután sikeresen próbált meg a feladatátvételi teszt próbálkozzon ezzel egy [feladatátvételi](site-recovery-failover.md).
