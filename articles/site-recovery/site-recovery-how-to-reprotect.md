---
title: "a helyszíni hely Azure tooan aaaReprotect |} Microsoft Docs"
description: "A virtuális gépek tooAzure feladatátvétel után a virtuális gépek hátsó tooon helyszíni feladat-visszavétel toobring is kezdeményezhető. Megtudhatja, hogyan tooreprotect egy feladat-visszavétel előtt."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 94c86e79cba4cd3f0c5821fdd5509cca1d0e7820
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reprotect-from-azure-tooan-on-premises-site"></a>Lássa el újból védelemmel az Azure tooan a helyszíni helyről



## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooreprotect Azure virtuális gépek Azure tooan a helyszíni helyről. Kövesse az ebben a cikkben, ha már készen áll a toofail hello utasításokat a VMware virtuális gépeket vagy windowsos/Linuxos fizikai kiszolgálók biztonsági után hello helyszíni hely tooAzure a korábban sikertelenül keresztül (lásd: [replikálása VMware virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery](site-recovery-failover.md)).

> [!WARNING]
> Nem utasíthat el után vissza [áttelepítése](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), helyezze át a virtuális gép tooanother erőforráscsoport vagy egy Azure virtuális gép törlése.


Ismételt védelem befejeződése és hello védett virtuális gépeket replikál, után is kezdeményezhető a feladat-visszavételt a virtuális gépek toobring hello őket toohello helyszíni hely.

Megjegyzéseit vagy kérdéseit a cikk vagy hello hello végén utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

A gyors áttekintését tekintse meg a következő videó bemutatja, hogyan toofail keresztül Azure tooan a helyszíni hely hello.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="prerequisites"></a>Előfeltételek
Virtuális gépek tooreprotect előkészítésekor végrehajtása, vagy vegye figyelembe a következő előfeltételek műveletek hello:

* Ha a vCenter-kiszolgáló kezeli, amelyet az toofail hello virtuális gépek biztonsági, győződjön meg arról, hogy rendelkezik-e hello [szükséges engedélyek](site-recovery-vmware-to-azure-classic.md) virtuális gépeket a vCenter-kiszolgálók felderítéséhez.

  > [!WARNING]
  > Ha pillanatképeket hello megtalálható a helyszíni fő célkiszolgáló vagy hello virtuális gépet, ismételt védelem sikertelen lesz. Törölheti a fő célkiszolgálón hello hello pillanatképek tooreprotect folytatás előtt. a védelem-újrabeállítási feladat során automatikusan egyesítésekor hello pillanatképek hello virtuális gépen.

* Ahhoz, hogy feladat-visszavételt, hozzon létre két további összetevőket:

  * **Folyamatkiszolgáló**: hello [folyamatkiszolgáló](site-recovery-vmware-setup-azure-ps-resource-manager.md) adatokat fogad az hello védett virtuális gépet az Azure-ban, és elküldi az adatokat toohello helyszíni hely. A kis késleltetésű hálózatra azért szükség, hello folyamatkiszolgáló és a védett hello virtuális gép között. Így akkor is egy helyszíni folyamatkiszolgáló használata Azure ExpressRoute kapcsolat vagy egy Azure-alapú folyamatkiszolgáló és a VPN-en.
  
  * **Fő célkiszolgáló**: hello fő célkiszolgáló kap feladat-visszavétel adatokat. hello helyszíni felügyeleti kiszolgálót, amelyet hozott létre a fő célkiszolgálón, alapértelmezés szerint telepítve van. Azonban attól függően, hogy nem sikerült visszaírt forgalom mennyiségét hello, szükség lehet egy külön fő célkiszolgáló toocreate feladat-visszavételre.
    * [A Linux virtuális gép a beavatkozását Linux fő célkiszolgáló](site-recovery-how-to-install-linux-master-target.md).
    * Windows virtuális gépként kell egy Windows fő célkiszolgáló. Később újra felhasználhatja hello helyszíni folyamat kiszolgáló és a fő célszámítógépekre.

    hello fő célkiszolgáló rendelkezik egyéb szereplő Előfeltételek [közös dolgot toocheck védelem-újrabeállítási előtt a fő célkiszolgáló](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server).

* A konfigurációs kiszolgáló szükségesek helyszíni esetén, a feladat-visszavételt. A feladat-visszavétel során hello virtuális gép konfigurációs adatbázishoz hello léteznie kell. Ellenkező esetben a feladat-visszavétel nem sikerül. 

> [!IMPORTANT]
> Győződjön meg arról, hogy az egy rendszeresen ütemezett biztonsági mentések a konfigurációs kiszolgáló. Ha egy olyan vészhelyzet esetén, hello kiszolgáló hello azonos IP-címet, hogy a feladat-visszavétel működik a helyreállítása.

* Set hello `disk.EnableUUID=true` hello konfigurációs paraméterek hello fő célkiszolgáló virtuális gép VMware-ban történő beállítását. Ha a sor nem létezik, adja hozzá. A beállítás nem szükséges tooprovide egy egységes UUID toohello virtuális gép lemezét (VMDK), hogy azt a megfelelő csatlakoztatja.

* *Hello fő célkiszolgáló nem használhatja a Storage vmotion szolgáltatások használata*. Ez a feladat-visszavétel toofail hello eredményezheti. hello virtuális gép nem indítható el, mert hello lemezek nem érhető el tooit. tooprevent probléma kizárási hello fő célkiszolgálóra a vMotion listájáról.

* Adja hozzá egy új meghajtó toohello fő célkiszolgáló: egy adatmegőrzési meghajtó. Adjon hozzá egy új lemezt és formátumának hello meghajtót.


### <a name="frequently-asked-questions"></a>Gyakori kérdések

#### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-connection-tooreplicate-data-back-toohello-on-premises-site"></a>Miért kell S2S VPN-en vagy ExpressRoute kapcsolat tooreplicate adatok hátsó toohello helyszíni hely?
Mivel a helyszíni tooAzure replikáció akkor fordulhat elő, hello interneten keresztül, vagy egy ExpressRoute kapcsolattal, amely rendelkezik a nyilvános társviszony, ismételt védelem és a feladat-visszavétel igényel egy-webhelyek (közötti S2S) VPN-tooreplicate adatot. Hello hálózatot adjon meg, így hello átvevő virtuális gépek Azure-ban képes elérni (ping) hello helyszíni konfigurációs kiszolgáló. A folyamatkiszolgáló hello Azure hello átvevő virtuálisgép-hálózat is telepíthet. A folyamatkiszolgáló is kell tudni toocommunicate hello a helyi konfigurációs kiszolgálóval.

#### <a name="when-should-i-install-a-process-server-in-azure"></a>Mikor kell a folyamatkiszolgáló telepítése az Azure-ban?


hello a virtuális gépek Azure tooreprotect kívánt replikációs adatok tooa folyamatkiszolgáló küldenek. A hálózat beállítása, hogy az Azure virtuális gépek hello hello folyamatkiszolgáló érhető el.

Az Azure-ban folyamat-kiszolgáló központi telepítése, vagy használjon hello meglévő folyamatkiszolgálót, hogy a feladatátvétel során. hello fontos pont tooconsider hello késés toosend hello adatokat hello virtuális gépek Azure toohello folyamatkiszolgáló jelzi.

Ha egy ExpressRoute-kapcsolat beállítása, egy helyszíni kiszolgáló toosend adatfeldolgozásra használhatja, mert hello késés hello virtuális gép és hello a folyamatkiszolgáló között alacsony.

 ![ExpressRoute architektúra diagramja](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



De ha csak egy S2S VPN-je, ajánlott az Azure-ban hello folyamatkiszolgáló telepítése.

 ![VPN-architektúra ábrája](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Ne feledje, hogy a replikáció csak az S2S VPN-je hello vagy titkos hálózata kevésbé ExpressRoute-társviszony létesítése – hello keresztül történik. Győződjön meg arról, hogy elegendő sávszélesség érhető el, hogy hálózati csatornán keresztül.

Azure-alapú folyamat kiszolgáló telepítésével kapcsolatos információkért lásd: [az Azure-ban folyamat kiszolgáló kezeléséhez](site-recovery-vmware-setup-azure-ps-resource-manager.md).

> [!TIP]
> Feladat-visszavétel során az Azure-alapú folyamatkiszolgáló használatát javasoljuk. hello replikációs teljesítményt értéke magasabb, ha hello folyamat kiszolgáló szorosabb toohello replikáló virtuális gép (hello átvevő gép az Azure-ban). Azonban megvalósíthatósági vagy bemutató a igazolása során használata mellett ExpressRoute hello helyszíni folyamat kiszolgáló magánhálózati társviszony-létesítési toocomplete hello gyorsabb Koncepció.



#### <a name="what-ports-should-i-open-on-different-components-so-that-reprotection-can-work"></a>Mely portokat kell nyitható meg a különböző összetevőket, hogy ismételt védelemmel ellátni azt is működjenek?

![A feladatátvétel és a feladat-visszavétel portok](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

#### <a name="which-master-target-server-should-i-use-for-reprotection"></a>Mely fő célkiszolgáló kell használni az ismételt védelem?
Egy helyszíni fő célkiszolgáló hello folyamatkiszolgáló szükséges tooreceive adatait, és jegyezze toohello a helyszíni virtuális gép vmdk-fájl. Ha Windows virtuális gépek védelmét, egy Windows fő célkiszolgáló szüksége. Újrahasználhatja hello helyszíni folyamatkiszolgáló és fő célkiszolgáló<!-- !todo component -->. A Linux virtuális gépek tooset egy további helyszíni Linuxos fő célkiszolgáló fel kell.


A fő célkiszolgáló telepítésével kapcsolatos információkért lásd:

* [Hogyan tooinstall Windows fő célkiszolgáló](site-recovery-vmware-to-azure.md)
* [Hogyan tooinstall Linux fő célkiszolgáló](site-recovery-how-to-install-linux-master-target.md)


#### <a name="what-datastore-types-are-supported-on-hello-on-premises-esxi-host-during-failback"></a>Datastore milyen hello helyszíni ESXi-állomáson feladat-visszavétel során támogatottak?

Sikertelen Azure Site Recovery támogatja biztonsági jelenleg csak a tooa virtuális gép fájlrendszer (virtuálisgép-Fájlrendszereinek) vagy a vSAN-adattároló. NFS-adattároló nem támogatott. Toothis korlátozás miatt hello datastore kijelölés bemeneti hello védelem-újrabeállítási képernyő NFS datastores hello esetében üres, vagy hello vSAN datastore jeleníti meg, de hello feladat során nem sikerül. Ha azt tervezi, hogy vissza toofail, hozzon létre egy virtuálisgép-Fájlrendszereinek datastore helyszíni, és hátsó tooit sikertelen. A feladat-visszavétel miatt hello VMDK teljes letöltésére.

### <a name="common-things-toocheck-after-completing-installation-of-hello-master-target-server"></a>Közös dolgot toocheck hello fő célkiszolgáló telepítése után

* Ha hello virtuális gép jelen a helyszíni hello vCenter-kiszolgálót, tartozik hello fő célkiszolgáló kell toohello a helyszíni virtuális gép vmdk-fájl. Akkor szükséges toowrite hello replikált adatok toohello a virtuális gépek lemezeit. Győződjön meg arról, hogy hello a helyszíni virtuális gép adattároló csatlakoztatva van olvasási/írási hozzáférést hello fő cél gazdagépre.

* Ha hello virtuális gép nem található a helyi hello vCenter Server, hello Site Recovery szolgáltatás toocreate egy új virtuális gép ismételt védelem alatt kell. A virtuális gép hello ESX gazdagépen, amelyen hoz létre a fő célkiszolgáló hello jön létre. Körültekintően hello ESX-gazdagép, így hello feladat-visszavételt a virtuális gép létrehozása, amelyet hello gazdagépen.

* *Hello fő célkiszolgáló nem használhatja a Storage vmotion szolgáltatások használata*. Ez a feladat-visszavétel toofail hello eredményezheti. hello virtuális gép nem indítható el, mert hello lemezek nem érhető el tooit.

  > [!WARNING]
  > Ha olyan fő célkiszolgálót a Storage vMotion feladatok ismételt védelem után megy keresztül, hello védett virtuális gépek lemezeit fő célkiszolgálóhoz csatolt toohello áttelepítése hello vMotion feladat toohello célja. Ha ezt követően ismét próbálja toofail, hello lemez terméknek sikertelen lesz, mivel hello lemezek nem találhatók. A storage-fiókok szigorú toofind hello lemezek majd válik. Toofind kell őket manuálisan, és csatolja azokat toohello virtuális gépet. Ezt követően hello a helyszíni virtuális gép rendszerindításra alkalmas.

* Adjon hozzá egy adatmegőrzési meghajtó tooyour meglévő Windows fő célkiszolgáló. Adjon hozzá egy új lemezt és formátumának hello meghajtót. hello adatmegőrzési meghajtó használt toostop hello pontok idő, amikor hello virtuális gép replikálja vissza toohello helyszíni hely. Az alábbiakban néhány adatmegőrzési meghajtó feltétel. Ezek a feltételek nélkül hello meghajtó nem jelenik a fő célkiszolgáló hello.

   * hello kötet nem szolgál más célra, például a replikációs cél.

   * hello kötet nem zár módban van.

   * hello kötet nincs a gyorsítótár kötetén. hello fő telepítési nem létezik a köteten található. hello egyéni telepítési hello folyamat kiszolgáló és a fő cél a lemez nem jogosult az adatmegőrzési kötet. Hello folyamatkiszolgáló és fő célkiszolgáló egy olyan köteten van telepítve, hello kötet esetén hello fő cél a gyorsítótár kötetén.

   * hello fájlrendszerének hello kötet nincs FAT vagy FAT32.

   * hello kötet kapacitása nem nulla.

   * hello alapértelmezett adatmegőrzési kötet a Windows hello R köteten.

   * hello alapértelmezett adatmegőrzési kötet Linux /mnt/retention.

  > [!IMPORTANT]
  > Egy meglévő folyamat kiszolgálóhoz vagy konfigurációs kiszolgáló vagy terjedő skálán vagy folyamat server/fő cél kiszolgáló használata szüksége tooadd új meghajtóra. hello új meghajtó követelmények megelőző hello meg kell felelnie. Hello adatmegőrzési meghajtó nincs jelen, ha nem jelenik meg a legördülő listában hello hello portálon. Miután olyan meghajtó toohello a helyszíni fő célkiszolgálót, too15 percig, amíg hello meghajtó tooappear hello kijelölés hello portálon elfoglalja. Ha hello meghajtó nem jelenik meg 15 perc után is frissítheti hello konfigurációs kiszolgáló.

* Virtuális gép Linux átvevő Linux fő kiszolgáló szükséges. Virtuális gép Windows átvevő Windows fő kiszolgáló szükséges.

* A VMware-eszközök telepítése hello fő célkiszolgálón. Nélkül hello VMware-eszközökkel a hello fő célkiszolgáló ESXi-gazdagépére hello datastores nem észlelhető.

* Hello engedélyezése `disk.EnableUUID=true` paraméter hello fő célkiszolgáló virtuális gépen hello vCenter tulajdonságok használatával. <!-- !todo Needs link. -->

* hello fő célkiszolgáló rendelkeznie kell legalább egy virtuálisgép-Fájlrendszereinek datastore csatolva. Ha nincs, hello **Datastore** hello védelem-újrabeállítási oldalon bemenet üres lesz, és nem folytathatja a műveletet.

* hello fő célkiszolgáló nem lehet pillanatképeket hello lemezek. Ha pillanatképek vannak, ismételt védelem és a feladat-visszavétel nem.

* a fő célkiszolgáló hello Paravirtual SCSI-vezérlőhöz nem tartozhat. hello vezérlő csak egy LSI Logic vezérlő lehet. Egy LSI Logic vezérlőt ismételt védelem sikertelen lesz.

<!--
### Failback policy
tooreplicate back tooon-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with hello configuration server during creation.
2. This policy is not editable.
3. hello set values of hello policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-tooreprotect"></a>Lépéseket tooreprotect

> [!NOTE]
> Miután a virtuális gép elindul az Azure-ban, csak bizonyos idő hello ügynök tooregister hátsó toohello konfigurációs kiszolgáló (felfelé too15 perc). Ebben az időszakban ismételt védelem sikertelen lesz, és a hibaüzenet azt jelzi, hogy hello ügynök nincs telepítve. Várjon néhány percet, és próbálkozzon az ismételt védelem újra.



1. A **tároló** > **replikált elemek**, kattintson a jobb gombbal a hello virtuális gépet, amely a feladatátvétel megtörtént, és válassza **védelmének újbóli beállításához**. Kattintson a hello gépi, és válassza **védelmének újbóli beállításához** a hello parancsgombok.
2. Hello panelen, figyelje meg, hogy a védelem hello irány **Azure tooOn helyszíni**, már be van jelölve.
3. A **fő célkiszolgáló** és **Folyamatkiszolgáló**hello a helyszíni fő célkiszolgáló között, a folyamatkiszolgáló hello.
4. A **Datastore**, válassza ki a kívánt toorecover hello lemezek helyi adattárolóban toowhich hello. Ez a beállítás használható hello a helyszíni virtuális gép törlődik, és toocreate új lemez szükséges. A rendszer figyelmen kívül hagyja ezt a beállítást, ha hello lemezek már létezik, de továbbra is szükséges toospecify értéket.
5. Válassza ki a hello adatmegőrzési meghajtó.
6. hello feladatátvételi házirendhez automatikusan ki van jelölve.
7. Kattintson a **OK** toobegin ismételt védelem. A feladatok megkezdése tooreplicate hello virtuális gép Azure toohello a helyszíni helyről. Előrehaladásának hello a hello **feladatok** fülre.

Ha azt szeretné, hogy toorecover tooan másik helyre (hello a helyszíni virtuális gép törlésekor), válassza ki a hello adatmegőrzési meghajtó és a fő célkiszolgáló hello beállított adattároló. Ha nem sikerül hátsó toohello helyszíni hely, hello VMware virtuális gépek hello feladat-visszavétel védelmi tervet használja, mivel a(z) hello fő ugyanazon adattár hello célkiszolgáló. Új virtuális gép vCenter létrejön.

Ha azt szeretné, hogy toorecover hello virtuális gépet az Azure tooan meglévő helyszíni virtuális gép, csatlakoztatási hello a helyszíni virtuális gép datastores olvasási/írási hozzáférést hello fő célkiszolgáló ESXi-állomáson.
    ![Ismételt védelemmel párbeszédpanel](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

A helyreállítási terv hello szinten is állítsa. Replikációs csoport is látható el újra védelemmel csak a helyreállítási tervet. Akkor állítsa a helyreállítási terv használatával, meg kell tooprovide hello értékek minden védett gép.

> [!NOTE]
> Használja a replikációs csoport azonos fő server tooreprotect hello. Ha egy másik fő célkiszolgáló server tooreprotect egy replikációs csoportot használ, hello kiszolgáló nem adhatók meg a közös pontja időben.

> [!NOTE]
> hello a helyszíni virtuális gép ismételt védelem ki van kapcsolva. Ez segít a replikáció során az adatok konzisztenciájának biztosítására. Ne kapcsolja be hello virtuális gép ismételt védelem befejeződése után.

Hello ismételt védelem sikeres, követően hello virtuális gép módba lép, a védett állapotban.

## <a name="next-steps"></a>Következő lépések

Miután hello virtuális gép védett állapotban került, [kezdeményezni a feladat-visszavétel](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-fail-back). 

hello feladat-visszavétel hello virtuális gépet az Azure-ban és a rendszerindító hello a helyszíni virtuális gép le fog állni. Bizonyos időre leállítást hello alkalmazás várt. Hello alkalmazás működését állásidő időpontot a feladat-visszavétel kiválasztása

## <a name="common-problems"></a>Gyakori problémák

* Ha egy sablon toocreate a virtuális gépeket használ, győződjön meg arról, hogy minden virtuális gép rendelkezik-e a saját hello lemezek UUID. Ha hello a helyszíni virtuális gép UUID ütközés egy, a fő célkiszolgáló hello mert mindkét alapján létrehozott hello ugyanaz sablon ismételt védelem sikertelen lesz. Nem lett létrehozva a hello megegyezik egy másik fő célkiszolgáló telepítése sablont.

* Ha egy csak olvasható felhasználói vCenter-kiszolgálók automatikus észlelését, és a virtuális gépet véd, védelmi sikeres, és feladatátvételi működik. Ismételt védelem, során feladatátvétel sikertelen lesz, mivel hello datastores nem lesz felderítve. A probléma tünete, hogy hello datastores ismételt védelem alatt nem láthatók. tooresolve probléma frissítheti az hello vCenter hitelesítőadat-fiókkal egy megfelelő engedélyekkel, és próbálja meg újra hello feladatot. További információkért lásd: [replikálása VMware virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).

* Ha a feladat-visszavételt a Linux virtuális gép, és futtassa a helyszíni, lásd: hello hálózatkezelő csomag hello machine el lett távolítva. Az Eltávolítás az oka, hogy hello hálózatkezelő csomagot el kell távolítani a virtuális gép hello helyreállításakor az Azure-ban.

* Amikor egy Linux virtuális gép statikus IP-címmel van konfigurálva, és tooAzure feladatátvételt, hello IP-cím keletkezik, a DHCP-Kiszolgálótól. Amikor a rendszer átadja a tooon helyszíni, hello virtuális gép továbbra toouse DHCP tooacquire hello IP-címet. Manuálisan toohello gép bejelentkezhet, és hello IP-cím hátsó tooa statikus cím állítja be, ha szükséges. A Windows rendszerű virtuális gép újra lekérheti a statikus IP-címet.

* Vagy ESXi 5.5 ingyenes kiadásban, vagy vSphere 6 hipervizor ingyenes kiadás használatakor a feladatátvétel sikerességét, de a feladat-visszavétel nem sikerül. tooenable feladat-visszavétel, frissítési tooeither program próbalicencre.

* Hello folyamat kiszolgálóról hello konfigurációs kiszolgáló nem érhető el, ha a 443-as portot használja a Telnet toocheck kapcsolat toohello konfigurációs kiszolgáló. Megpróbálhatja tooping hello konfigurációs kiszolgáló hello folyamat kiszolgálóról is. A folyamatkiszolgáló lehet szívverés csatlakoztatott toohello konfigurációs kiszolgáló esetén.

* Ha toofail hátsó tooan alternatív vCenter, győződjön meg arról, hogy az új vCenter és a fő célkiszolgáló hello felderítése. Egy tipikus előfordulhat, hogy hello datastores nem elérhetők és hello látható **lássa el újból védelemmel** párbeszédpanel megnyitásához.

* Egy Windows Server 2008 R2 SP1-kiszolgálót, amely védett, mint egy fizikai helyszíni kiszolgáló újból az Azure tooan helyszíni helyre nem sikerült.
