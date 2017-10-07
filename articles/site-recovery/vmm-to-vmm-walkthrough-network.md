---
title: "a Hyper-V replikáció tooa VMM másodlagos hely az Azure Site Recovery hálózati aaaPlan |} Microsoft Docs"
description: "A cikk ismerteti az alaphálózati topológia tervezése a Hyper-V virtuális gépek tooa System Center VMM másodlagos hely az Azure Site Recovery replikálása esetén."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 095f2d73-994e-4fc0-96f2-e03a7d72e492
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 5934db4a661a2c697a1a799c3848852250ddb451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-networking-for-hyper-v-vm-replication-tooa-secondary-vmm-site"></a>3. lépés: Hyper-V virtuális gép replikációs tooa másodlagos VMM-hely hálózatkezelés tervezése

Miután megtekintette a telepítés előfeltételeit, olvassa el a Hyper-V virtuális gépek (VM) kezelése a System Center Virtual Machine Manager (VMM) felhők replikálása esetén hálózat cikk tooplan tooa használja a másodlagos hely [Azure Site Recovery](site-recovery-overview.md) a hello Azure-portálon. 

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-overview"></a>A hálózati hozzárendelés áttekintése

A hálózatleképezés akkor használatos, ha a Hyper-V virtuális gépek (a VMM felügyelje) tooa másodlagos adatközpontba replikálja. Hálózatleképezés kapcsolatot hoz létre a forrás VMM-kiszolgálón a Virtuálisgép-hálózatok és a célként megadott VMM-kiszolgálón a Virtuálisgép-hálózatok között. Leképezési hello a következő:

- **Hálózati kapcsolat**– csatlakoztatja a virtuális gépek tooappropriate hálózatok feladatátvételt követően. hello replika virtuális gép csatlakoztatott toohello cél hálózathoz csatlakoztatott toohello Forráshálózat lesz.
- **Optimális elhelyezési**– optimális helyek hello replika virtuális gépek a Hyper-V gazdakiszolgálókra. A replika virtuális gépeket, hogy hozzáférési hello rendelhetők a Virtuálisgép-hálózatok gazdagépeken kerülnek.
- **Nincs hálózati leképezés**– Ha nem adja meg a hálózatleképezés, a replika virtuális gépek a feladatátvételt követően nem lesz csatlakoztatott tooany Virtuálisgép-hálózatok.


### <a name="example"></a>Példa

Íme egy példa tooillustrate a mechanizmus. A következőkben két helyen található Győr és Chicagói rendelkező szervezeteknél.

**Hely** | **VMM-kiszolgáló** | **A Virtuálisgép-hálózatok** | **Leképezve**
---|---|---|---
New York | A VMM-NewYork| VMNetwork1-NewYork | Csatlakoztatott tooVMNetwork1-Chicagói
 |  | VMNetwork2-NewYork | Nincsenek leképezve:
Chicago | A VMM-Chicagói| VMNetwork1-Chicagói | Csatlakoztatott tooVMNetwork1-NewYork
 | | VMNetwork1-Chicagói | Nincsenek leképezve:

Ebben a példában:

- A replika virtuális gép létrehozásakor bármely virtuális géphez, amely csatlakoztatott tooVMNetwork1-NewYork csatlakoztatott tooVMNetwork1-Chicagói lesz.
- Amikor a replika virtuális gép VMNetwork2-NewYork vagy VMNetwork2-Chicagói hoz létre, akkor nem fog csatlakoztatott tooany hálózati.

Ez hogyan VMM-felhő beállítása a szervezet példája és hello hello-felhőkhöz társított logikai hálózat.

#### <a name="cloud-protection-settings"></a>Felhő védelmi beállításait

**Védett felhő** | **Felhő védelme** | **Logikai hálózat (New Yorkban)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicagói</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicagói</p>

#### <a name="logical-and-vm-network-settings"></a>Logikai és a virtuális gép hálózati beállításai

**Hely** | **Logikai hálózat** | **Társított Virtuálisgép-hálózat**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicagói | VMNetwork1-Chicagói
 | LogicalNetwork2Chicago | VMNetwork2-Chicagói

#### <a name="target-network-settings"></a>Cél hálózati beállításai

Ezek a beállítások alapján hello cél Virtuálisgép-hálózat kiválasztásakor, hello a következő táblázatban látható hello lehetőségeket, amelyek elérhetők lesznek.

**Kiválasztás** | **Védett felhő** | **Felhő védelme** | **A célhálózat érhető el**
---|---|---|---
VMNetwork1-Chicagói | SilverCloud1 | SilverCloud2 | Elérhető
 | GoldCloud1 | GoldCloud2 | Elérhető
VMNetwork2-Chicagói | SilverCloud1 | SilverCloud2 | Nincs
 | GoldCloud1 | GoldCloud2 | Elérhető


Ha hello célhálózatban már több alhálózat működik, és ezek egyikének ugyanazzal a névvel, mely hello a forrás virtuális gép is található, alhálózati hello majd hello hello a replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.


#### <a name="failback-behavior"></a>Feladat-visszavétel viselkedése

Mi történik, a feladat-visszavétel (visszirányú replikálás), hello esetben toosee tegyük fel, hogy VMNetwork1-NewYork-e a csatlakoztatott a tooVMNetwork1-Chicago, a beállítások a következő hello rendelkező.


**Virtuális gép** | **Csatlakoztatott tooVM hálózati**
---|---
VM1 | VMNetwork1-hálózat
Vm2 virtuális gépnek (VM1 replika) | VMNetwork1-Chicagói

Ezekkel a beállításokkal tekintsük át, mi történik, több lehetséges forgatókönyv szerint.

**A forgatókönyv** | **Eredménye**
---|---
Nem módosult a feladatátvételt követően VM-2 hello hálózati tulajdonságait. | VM-1 csatlakoztatott toohello Forráshálózat marad.
VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és le van választva. | VM-1 le van választva.
VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és a csatlakoztatott tooVMNetwork2-Chicagói. | Ha nincs leképezve VMNetwork2-Chicago, VM-1 le lesz választva.
Hálózati leképezése VMNetwork1-Chicagói módosul. | VM-1 csatlakoztatott toohello most már csatlakoztatott hálózati tooVMNetwork1-Chicagói lesz.



## <a name="prepare-for-network-mapping"></a>A hálózatleképezés előkészítése

1. Hello forrás és cél VMM-kiszolgálókon rendelkeznie kell egy hello forrás és cél-felhőkhöz társított logikai hálózatot. 
2. Hello forrás és cél-kiszolgálókon rendelkeznie kell egy virtuális gép hálózati csatolt toohello logikai hálózatot.
3. Virtuális gépek a Hyper-V gazdagépek hello forráshely csatolt toohello forrás Virtuálisgép-hálózathoz kell lennie. Ha csak egyetlen VMM-kiszolgálót használ, konfigurálhatja a közötti megfeleltetés hello Virtuálisgép-hálózatok azonos kiszolgálón.

Ez történik a Site Recovery üzembe helyezése során a hálózatleképezés beállításához:

- A hálózatleképezés beállításához, és válassza ki a cél Virtuálisgép-hálózat, hello VMM forrás felhők hello forrás Virtuálisgép-hálózatot használó jelenik meg, hello az elérhető célkiszolgálók Virtuálisgép-hálózatok hello cél felhők együtt.
- - Ha leképezési megfelelően van konfigurálva és replikáció engedélyezett-e, a virtuális gép lesz csatlakoztatott tooits forrás Virtuálisgép-hálózatot, és a replika hello célhelyen csatlakoznak tooits leképezett Virtuálisgép-hálózatot.
- Ha hello célhálózatban már több alhálózat működik, és ezek egyikének ugyanazzal a névvel, mely hello a forrás virtuális gép is található, alhálózati hello majd hello hello a replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, virtuális gép hello lesz csatlakoztatott toohello első hello hálózat alhálózatának.

## <a name="connect-toovms-after-failover"></a>TooVMs a feladatátvételt követően csatlakozhatnak.

A replikációs és feladatátvételi stratégiát megtervezésekor fontos kérdések hello egyik hogyan tooconnect toohello replika feladatátvételt követően. Több lehetősége van: 

- **Különböző IP-cím**: kiválaszthatja toouse egy másik IP-címet a hello replikált virtuális gép. Ez a forgatókönyv hello a virtuális gép a feladatátvételt követően lekérdezi a egy új IP-címet, és DNS-frissítés szükséges.
- **Tartsa meg IP-cím hello**: érdemes toouse hello hello replika virtuális gép ugyanazon IP-címet. Egyszerűbbé teszi a ugyanazon IP-címeket való tartása hello hello helyreállítási csökkentésével kapcsolatos probléma hálózati feladatátvételt követően. 

## <a name="retain-ip-addresses"></a>Tartsa meg az IP-címek

Ha azt szeretné, hogy tooretain hello IP-címek hello után feladatátvételi toohello másodlagos hely elsődleges hely, a teljes alhálózat feladatátvétellel, és frissítési útvonalak tooindicate hello új helyét hello IP-címek, vagy alternatív telepítheti a felhőbe archivált alhálózati hello között elsődleges és a helyreállítási hely hello.

### <a name="stretched-subnet"></a>Felhőbe archivált alhálózati

A felhőbe archivált alhálózat hello alhálózati érhető el egyszerre mindkét hello elsődleges és másodlagos hely. Ha a kiszolgáló és az IP (3. rétegbeli) konfigurációs toohello másodlagos hely, hello hálózati automatikusan hello forgalom toohello új helyet fogja átirányítani. 

2. rétegbeli (adatrétege hivatkozás) szempontjából van szüksége, melyekkel kezelhető a felhőbe archivált VLAN hálózati berendezések. Ezenkívül VLAN hello Nyújtás által hello lehetséges tartalék tartomány bővíti tooboth helyek lényegében váljon a hibaérzékeny pontok kialakulását. Ez nem egy nem valószínű, akkor fordulhat elő, hogy egy szórási storm elindult, és nem lehet elkülönített. 


### <a name="subnet-failover"></a>Alhálózati feladatátvétel

Futtathatja egy alhálózat feladatátvételi tooobtain hello felhőbe hello alhálózati előnyei ténylegesen nyújtás nélkül. Ebben a megoldásban alhálózat lesz elérhető hello forrás vagy a célként megadott helyen, de nem mindkét helyen egyszerre. toomaintain hello IP-címterének hello esemény egy feladatátvételi, programozott módon rendezheti hello útválasztó infrastruktúra toomove hello alhálózatok az egyik hely tooanother. Feladatátvétel esetén alhálózatok rendelkező áthelyezné után hello társított virtuális gépek. hello fő hátránya, hogy a hello esetre, ha nem, akkor toomove hello teljes alhálózattal rendelkezik.

### <a name="example"></a>Példa

Íme egy példa a teljes alhálózat feladatátvételi. hello elsődleges hely rendelkezik alhálózati 192.168.1.0/24 futó alkalmazások. Feladatátvétel során az alhálózat összes hello virtuális gépek feladatátvétel toohello másodlagos hely történt, és tartsa meg az IP-címét. Útvonalak kell toobe tooreflect hello tényt, hogy minden hello virtuális gép virtuális gépek toosubnet 192.168.1.0/24 tartozó most áthelyezte toohello másodlagos hely módosítani.

hello következő grafikus megjelenítése hello alhálózatok előtt, és a feladatátvétel után:

- A feladatátvétel előtt alhálózati 192.168.0.1/24, aktív hello forráshelyen, válik active hello másodlagos helyre a feladatátvételt követően.
- hello továbbítja az elsődleges hely és hely helyreállítási, harmadik és elsődleges hely között, és a harmadik és a helyreállítási hely toobe megfelelően módosítani kell.

**Feladatátvétel előtt**

![Feladatátvétel előtt](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**A feladatátvételt követően**

![A feladatátvételt követően](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

A feladatátvétel után ez történik:

- A Site Recovery lefoglalja IP-címet a virtuális gép, hello mindegyik hálózati interfész hello statikus IP-címkészletet a hello releváns hálózati minden VMM-példány.
- Ha hello IP-címkészlet hello másodlagos hely foglal le, ugyanaz, mint amellyel hello forráshelyen, a Site Recovery hello hello azonos IP-címét (hello forrás virtuális gép) toohello replika virtuális Gépre. hello IP-cím van fenntartva a VMM-ben, de hello Hyper-V gazdagépen beállítása nem hello feladatátvételi IP-címként. hello feladatátvételi IP-címet egy Hyper-v gazdagépen hello feladatátvétel előtt van beállítva.
- Hello IP-cím nem érhető el, ha a Site Recovery hello készlet lefoglalja egy másik elérhető IP-cím.
- Ha a virtuális gépek DHCP használatára, a Site Recovery hello IP-címek nem kezelhetők. Van szüksége, amely hello DHCP toocheck server hello másodlagos helyen azonos tartomány, mint amit a forráshely hello hello címet is lefoglalni.

### <a name="validate-hello-ip-address"></a>Hello IP-cím ellenőrzése

A virtuális gépek védelmének engedélyezése után alábbi minta parancsprogram tooverify hello cím toohello VM felhasználhatja. IP-cím állítja be hello feladatátvételi IP-címet, és hozzárendelt toohello virtuális gép feladatátvételi hello időpontban hello:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>IP-címek módosítása

Ebben a forgatókönyvben hello IP-címek virtuális gépeket, a feladatátvétel módosult. hello a megoldás hátránya hello karbantartási feladat. Általában a DNS fog frissülni után a replika virtuális gép elindításához. DNS-bejegyzések lehet, hogy toobe módosítani kell, vagy fluster tartalmazza, és a gyorsítótárazott bejegyzés frissítése. Ez állásidőt eredményezhet. Állásidő mérsékelhető a következőképpen:

- Intranetes alkalmazások alacsony TTL értéket használjon.
- A Site Recovery helyreállítási tervet, tooupdate hello DNS kiszolgáló tooensure egy időben történő frissítést a parancsfájl következő hello használata. Hello parancsfájl nem szükséges, ha dinamikus DNS-regisztrációt.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Példa 

Vizsgáljuk meg egy olyan forgatókönyvet, amelyben arra készül, toouse eltérő IP-címen keresztül elsődleges hello és hello helyreállítási helyeken. Ebben a példában az elsődleges és másodlagos helyek között tudunk különböző IP-címeket, és; s harmadik helyet hello elsődleges vagy tartalék hely futó alkalmazásokat is elérhetők.

- A feladatátvétel előtt alkalmazások üzemeltetett alhálózati 192.168.1.0/24 hello elsődleges helyen, és az alhálózati 172.16.1.0/24 hello másodlagos helyen konfigurált toobe egy feladatátvétel után.
- VPN-kapcsolatok és a hálózati útvonalak konfigurálva megfelelően, hogy az összes három helyen képesek elérni egymást.
- A feladatátvétel után az alkalmazások hello helyreállítási alhálózat állítható vissza. Ebben a forgatókönyvben nincs nincs szükség toofail hello teljes alhálózattal, és nem végez módosítást szükséges tooreconfigure VPN- vagy hálózati útvonalak. hello feladatátvétel, és bizonyos DNS-frissítések, győződjön meg arról, hogy alkalmazások továbbra is elérhető.
- Ha a DNS dinamikus frissítések konfigurált tooallow nem, hello virtuális gépek fognak regisztrálni saját maguk hello új IP-címet, amikor elindítja a feladatátvételt követően.

**Feladatátvétel előtt**

![Különböző IP - feladatátvétel előtt](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**A feladatátvételt követően**

![Különböző IP - feladatátvételt követően](./media/vmm-to-vmm-walkthrough-network/network-design11.png)



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[4. lépés: készítse elő a VMM és a Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md).


