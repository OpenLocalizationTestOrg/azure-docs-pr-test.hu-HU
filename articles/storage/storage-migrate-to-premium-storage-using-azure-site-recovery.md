---
title: "prémium szintű Storage Azure Site Recovery segítségével aaaMigrating tooAzure |} Microsoft Docs"
description: "Telepítse át a meglévő virtuális gépek tooAzure prémium szintű Storage, a Site Recovery segítségével. Prémium szintű Storage nagy teljesítményű, alacsony késésű támogatása az Azure virtuális gépeken futó I/O-igényes munkaterhelések kínál."
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: luywang
ms.openlocfilehash: cb71c06e4a1a73d484e226a573d1ade48c87664d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-toopremium-storage-using-azure-site-recovery"></a>Áttelepítése tooPremium tárolást biztosít az Azure Site Recovery

[Prémium szintű Storage](storage-premium-storage.md) nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek (VM) támogatását biztosítja. hello Ez az útmutató célja toohelp felhasználók át a virtuális gépek lemezei egy Standard tárolási fiók tooa prémium szintű storage-fiók használatával [Azure Site Recovery](../site-recovery/site-recovery-overview.md).

A Site Recovery az Azure-szolgáltatások járul tooyour az üzletmenet folytonosságának és vész-helyreállítási stratégia megvalósításában a helyszíni fizikai kiszolgálóknak és virtuális gépek toohello Azure-felhőbe vagy tooa másodlagos adatközpontba replikációs hello. Az elsődleges helyen valamilyen okból kimaradás lép fel, ha feladatátvételt toohello másodlagos helyre tookeep alkalmazások és számítási feladatok nem állnak. Hátsó tooyour elsődleges hely rendszer visszaadja a toonormal művelet sikertelen lesz. A Site Recovery feladatátvételi teszteket biztosít a toosupport vészhelyreállítások részletezésének nem befolyásolja a termelési környezetben. A feladatátvételeket a váratlan vészhelyzetek esetére (replikáció gyakoriságától függően) minimális adatvesztéssel is futtathatja. A hello forgatókönyvet tooPremium tárolási az áttelepítést, használhatja hello [a Site Recovery feladatátvételi](../site-recovery/site-recovery-failover.md) az Azure Site Recovery toomigrate cél lemezek tooa prémium szintű storage-fiók.

Ajánlott az áttelepítés tooPremium tárolási Site Recovery használatával, mert ezt a lehetőséget biztosít a minimális állásidővel, és ezzel elkerülheti a hello manuális végrehajtási lemásolná a lemezeket, és új virtuális gépek létrehozása. A Site Recovery rendszeresen a lemezek másolása, és hozzon létre új virtuális gépek a feladatátvétel során. A Site Recovery támogatja a feladatátvételt a minimális típusú számos vagy állásidő nélkül. tooplan az állásidő és becsült adatvesztés, lásd: hello [feladatátvételi típusú](../site-recovery/site-recovery-failover.md) tábla a Site Recovery szolgáltatásban. Ha Ön [előkészítése tooconnect tooAzure virtuális gépek a feladatátvételt követően](../site-recovery/site-recovery-vmware-to-azure.md), meg kell tudni tooconnect toohello Azure virtuális gép feladatátvételt követően RDP segítségével.

![][1]

## <a name="azure-site-recovery-components"></a>Az Azure Site Recovery-összetevők

Ezek a hello Site Recovery összetevők, amelyek megfelelő toothis áttelepítési forgatókönyv szerint.

* **Konfigurációs kiszolgáló** van egy Azure virtuális Gépen, amely kommunikáció koordinálását, és az adatok replikációs és helyreállítási folyamatok kezeli. A virtuális gép elindul egy egységes telepítő tooinstall hello konfigurációs fájlkiszolgálót és egy új összetevőt, a folyamat kiszolgáló replikációs átjáróként. További információ a [konfigurációs kiszolgáló Előfeltételek](../site-recovery/site-recovery-vmware-to-azure.md). Konfigurációs kiszolgáló csak egyszer konfigurált toobe kell, és minden áttelepítések toohello nem használható ugyanabban a régióban.

* **Folyamatkiszolgáló** replikációs adatokat fogadó forrás virtuális gép replikációs átjáró hello adatok gyorsítótárazás, tömörítés és titkosítás optimalizálja, és elküldi azt tooa tárfiók. Kezeli az ügyfélleküldéses telepítés a hello mobilitási szolgáltatás toosource virtuális gépek és a forrás virtuális gépek automatikus felderítést hajt végre. hello alapértelmezett folyamatkiszolgáló hello konfigurációs kiszolgálón van telepítve. További önálló folyamatot kiszolgálók tooscale a központi telepítés is telepíthet. További információ a [ajánlott eljárások a folyamatkiszolgáló telepítésének](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) és [további folyamat kiszolgálók üzembe helyezésével](../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Folyamatkiszolgáló csak egyszer konfigurált toobe kell, és minden áttelepítések toohello nem használható ugyanabban a régióban.

* **Mobilitásiszolgáltatás** telepített összetevő tooreplicate kívánt minden szabványos virtuális gépen. Azt a adatírásokat szabványos VM hello és toohello folyamatkiszolgáló továbbítja azokat. További információ a [replikált gép Előfeltételek](../site-recovery/site-recovery-vmware-to-azure.md).

A kép bemutatja, hogyan működnek együtt ezeket az összetevőket.

![][15]

> [!NOTE]
> A Site Recovery nem támogatja a tárolóhelyek lemezek hello áttelepítését.

A további összetevők az egyéb forgatókönyvek, tekintse meg az túl[kialakítandó architektúra](../site-recovery/site-recovery-vmware-to-azure.md).

## <a name="azure-essentials"></a>Az Azure alapjai

Ezek vannak hello Azure-követelmények az áttelepítési forgatókönyv szerint.

* Azure-előfizetés
* Egy prémium szintű Azure storage-fiók toostore replikált adatok
* Egy Azure virtuális hálózathoz (VNet) toowhich virtuális gépek csatlakozni fognak, ha feladatátvétel jönnek létre. hello Azure virtuális hálózatot kell lennie a hello ugyanabban a régióban, hello egy mely hello Site Recovery fut.
* Egy Azure standard szintű storage-fiókot mely toostore replikálási naplókhoz. Ez lehet hello ugyanazt a tárfiókot, hello méretű lemezek áttelepítendő

## <a name="prerequisites"></a>Előfeltételek

* Ismerje meg hello megfelelő áttelepítési forgatókönyv összetevőit az előző szakaszban hello
* Tervezze meg a leállás Learning kapcsolatos hello [feladatátvételi a Site Recovery szolgáltatásban](../site-recovery/site-recovery-failover.md)

## <a name="setup-and-migration-steps"></a>Telepítés és áttelepítés lépései

A Site Recovery toomigrate Azure IaaS virtuális gépeket is használhat, régiók közötti vagy régión belül. hello következő utasításokat kell lett igazított hello cikkből áttelepítési forgatókönyvhöz [replikálja a VMware virtuális gépek vagy fizikai kiszolgálók tooAzure](../site-recovery/site-recovery-vmware-to-azure.md). Kérjük, kövesse a részletes lépéseket a jelen cikkben lévő utasítások további toohello hello hivatkozásokat.

1. **Recovery Services-tároló létrehozása**. Létrehozásához és kezeléséhez a Site Recovery-tároló hello keresztül hello [Azure-portálon](https://portal.azure.com). Kattintson a **új** > **felügyeleti** > **biztonsági mentés** és **(OMS) helyreállítási hely**. Másik lehetőségként kattinthat **Tallózás** > **Recovery Services-tároló** > **Hozzáadás**. Virtuális gépek ebben a lépésben megadott replikált toohello terület lesz. Az áttelepítés a hello hello célra ugyanabban a régióban, amelyben a forrás virtuális gépeket és a forrás storage-fiókok is válassza hello régió. Vegye figyelembe, hogy áttelepítési tooPremium storage-fiókok csak akkor támogatott a hello [Azure-portálon](https://portal.azure.com), nem hello [klasszikus portál](https://manage.windowsazure.com).

2. hello következő lépések segítenek **védelmi célok megválasztása**.

    2a. A virtuális gép, ha azt szeretné, hogy tooinstall hello konfigurációs kiszolgáló hello, nyissa meg a hello [Azure-portálon](https://portal.azure.com). Nyissa meg túl**Recovery Services-tárolók** > **beállítások**. A **beállítások**, jelölje be **Site Recovery**. A **Site Recovery**, jelölje be **1. lépés: az infrastruktúra előkészítése**. A **infrastruktúra előkészítése**, jelölje be **védelmi cél**.

    ![][2]

    2b. A **védelmi cél**, a hello első legördülő listájából, válassza ki **tooAzure**. Hello második legördülő listában válassza ki a **nem virtualizált vagy egyéb**, és kattintson a **OK**.

    ![][3]

3. hello következő lépések segítenek **hello forrás környezetet (konfigurációs kiszolgáló)**.

    3a. Töltse le a hello **Azure Site Recovery az egységes telepítő** és hello **tárolóbeli regisztrációs kulcs** által is toohello **infrastruktúra előkészítése**  >  **Forrás előkészítése** > **kiszolgáló hozzáadása** panelen. Tároló regisztrációs kulcs toorun hello az egységes telepítő hello kell. hello kulcs a generálásától 5 napig esetén érvényes.

    ![][4]

    3b. Konfigurációs kiszolgáló hozzáadása a hello **kiszolgáló hozzáadása** panelen.

    ![][5]

    3 c.. Hello VM hello kiszolgálóként használ futtassa az egységes telepítő tooinstall hello konfigurációs kiszolgáló és a folyamatkiszolgáló hello. Hello képernyőképeket keresztül végigvezetheti [Itt](../site-recovery/site-recovery-vmware-to-azure.md) toocomplete hello telepítését. Olvassa el a képernyőképeket az áttelepítési forgatókönyv szerint megadott lépéseket követve toohello.

    A **megkezdése előtt**, jelölje be **hello konfigurációs kiszolgáló és a folyamatkiszolgáló telepítése**.

    ![][6]

    3D. A **regisztrációs**, keresse meg és jelölje ki a letöltött hello tárolóból hello regisztrációs kulcsot.

    ![][7]

    3e. A **környezet részletei**, adja meg, hogy tooreplicate VMware virtuális gépek fog. Válassza ki az áttelepítési forgatókönyv szerint **nem**.

    ![][8]

    3f. Hello telepítés befejezése után, látni fogja a hello **Microsoft Azure Site Recovery konfigurációs kiszolgáló** ablak. Hello használata **fiókok kezelése** lapon toocreate hello fiók, a Site Recovery automatikus felderítést is használhat. (Fizikai gépek védelme hello esetben hello fiók beállítása nem megfelelő, de legalább egy fiók tooenable egyet az alábbi lépésekkel hello van szüksége. Ebben az esetben az nevére hello fiókot és jelszót, mint bármelyik.) Használjon hello **tároló regisztrációs** lapon tooupload hello tárolói hitelesítő adatok fájlját.

    ![][9]

4. **Hello célkörnyezet beállítása**. Kattintson a **infrastruktúra előkészítése** > **cél**, és adja meg a kívánt toouse virtuális gépek a feladatátvételt követően hello üzembe helyezési modellben. Választhat **klasszikus** vagy **erőforrás-kezelő**, attól függően, a forgatókönyv.

    ![][10]

    A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal. Vegye figyelembe, hogy a replikált adatok használata a prémium szintű tárfiók, kell egy további standard szintű storage-fiók toostore replikáció tooset naplókat.

5. **Replikációs beállítások megadása**. Kövesse az [replikációs beállítások megadása](../site-recovery/site-recovery-vmware-to-azure.md) tooverify, amely a konfigurációs kiszolgáló sikeresen tartozik hello replikációs házirendet hoz létre.

6. **Kapacitástervezés**. Használjon hello [kapacitástervező](../site-recovery/site-recovery-capacity-planner.md) tooaccurately becsült hálózati sávszélesség, tárolási és egyéb követelmények toomeet kell-e a replikáció. Amikor elkészült, válassza ki a **Igen** a **elvégezte a kapacitástervezést?**.

    ![][11]

7. hello következő lépések segítenek **telepíteni a mobilitási szolgáltatást, és engedélyezze a replikálást**.

    7a. Túl választhat[leküldéses telepítése](../site-recovery/site-recovery-vmware-to-azure.md) tooyour forrás virtuális gépeknek vagy túl[manuálisan telepíteni a mobilitási szolgáltatás](../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) a forrás virtuális gépeken. Hello hivatkozásban megadott talál hello követelmény az előtte telepítési és hello hello manuális telepítő elérési útját. Akkor használatos, ha a manuális telepítés, szükség lehet egy belső IP toofind hello konfigurációs kiszolgálók toouse.

    ![][12]

    hello átvevő VM kell két ideiglenes lemezek: egyet az elsődleges virtuális gép és hello más hello hello helyreállítási régióban virtuális gép kiépítése során létrehozott hello. tooexclude hello ideiglenes lemez replikációs, mielőtt hello mobilitási szolgáltatás telepítése, replikáció engedélyezése előtt. toolearn hogyan tooexclude hello ideiglenes lemez kapcsolatos további információkért tekintse meg a túl[lemezek kizárása a replikációból](../site-recovery/site-recovery-vmware-to-azure.md).

    7b. Most már engedélyezheti a replikációt a következők szerint:
      * Kattintson a **alkalmazás replikálása** > **forrás**. Hello replikáció első alkalommal engedélyezését, kattintson a + hello tároló tooenable a további gépek replikációjának replikációt.
      * Az 1. lépésben forrásának beállítása a folyamat-kiszolgálóként.
      * A 2. lépésben adja meg a hello feladatátvételt követően telepítési modell, egy prémium szintű storage fiók toomigrate való, egy Standard tárolási fiók toosave naplók és a virtuális hálózati toofail való.
      * A 3. lépésben hozzá védett virtuális gépek IP-cím (szükség lehet egy belső IP-cím toofind őket).
      * A 4. lépésben beállított korábban hello folyamatkiszolgáló hello fiókok kiválasztásával hello tulajdonságainak konfigurálása.
      * 5. lépésben, hello replikációs házirend kiválasztásához, korábban létrehozott replikációs beállítások megadása.
      Kattintson a **OK** és engedélyezze a replikálást.

    > [!NOTE]
    > Ha egy Azure virtuális gép felszabadítása. lehetséges, és újra elindítani, akkor nem biztos, hogy megkapja az hello azonos IP-címet. Ha hello IP-címét hello konfigurációs kiszolgálófolyamat/server vagy hello Azure virtuális gépek módosítása védett, előfordulhat, hogy ebben a forgatókönyvben hello replikálás nem működik megfelelően.

    ![][13]

    Az Azure Storage környezet tervezésekor azt javasoljuk, hogy használja-e külön storage-fiókok az egyes virtuális gépek rendelkezésre állási csoportba. Javasoljuk, hogy kövesse hello a bevált gyakorlat hello tárolási réteg túl[több tárfiókot használja minden egyes rendelkezésre állási csoport](../virtual-machines/windows/manage-availability.md). VM lemezek toomultiple storage-fiókok segítségével tooimprove lévő tárterület rendelkezésre állását és hello i/o elosztja hello Azure tároló-infrastruktúra. Ha a virtuális gépek rendelkezésre állási csoportba, ahelyett, hogy azok a virtuális gépek lemezek egy tárolási figyelembe erősen ajánlott az áttelepítés több virtuális gép többször, úgy, hogy a virtuális gépek hello hello azonos rendelkezésre állási csoport nem ugyanazt a egyetlen storage-fiók. Használjon hello **Replikálásengedélyezési** panel tooset be az egyes virtuális gépek, egyenként, a cél tárfiókkal. Kiválaszthatja a feladatátvételt követően telepítési modell tooyour szükség szerint. Ha úgy dönt, erőforrás-kezelő (RM), a feladatátvételt követően üzembe helyezési modellel, egy erőforrás-kezelő VM tooan RM VM átveheti, vagy a klasszikus virtuális gép tooan RM VM átveheti.

8. **Feladatátvételi teszt futtatása**. toocheck-e a replikáció befejeződött, kattintson a Site Recovery majd **beállítások** > **replikált elemek**. Hello állapota és a replikálási folyamat százalékát jelenik meg. Kezdeti replikálás után a replikációs stratégiát befejeződése után futtassa a feladatátvételi teszt toovalidate is. A részletes lépéseket a feladatátvételi tesztet, tekintse meg az túl[feladatátvételi teszt futtatása a Site Recovery](../site-recovery/site-recovery-vmware-to-azure.md). Láthatja, hogy a feladatátvételi teszt hello állapotának **beállítások** > **feladatok** > **YOUR_FAILOVER_PLAN_NAME**. Hello paneljén láthatja hello lépéseket és a sikeres és sikertelen eredmények részletes információkat. Ha hello feladatátvételi teszthez bármely lépése meghiúsul, kattintson a hello lépés toocheck hello hibaüzenet. Győződjön meg arról, hogy a virtuális gépek és a replikációs stratégiájának követelményeknek hello feladatátvétel futtatása előtt. Olvasási [a Site Recovery feladatátvételi teszt tooAzure](../site-recovery/site-recovery-test-failover-to-azure.md) további információt és útmutatást a feladatátvételi tesztet.

9. **Feladatátvételt**. Hello a feladatátvételi teszt befejezése után futtassa a feladatátvételi toomigrate a lemezek tooPremium tárolási, és hello Virtuálisgép-példányok replikálni. Kövesse az hello részletes lépéseit [feladatátvételt](../site-recovery/site-recovery-failover.md#run-a-failover). Válasszon **virtuális gépek leállítása és hello legfrissebb adatok szinkronizálása** toospecify, hogy a Site Recovery próbálja tooshut le hello védett virtuális gépek és szinkronizálás hello hello legújabb verziójának hello adatok átvétele fog megtörténni. Ha nem ezt a beállítást, vagy hello kísérlet nem jár sikerrel hello feladatátvételi lesz a virtuális gép hello hello legújabb elérhető helyreállítási pontból. A Site Recovery hoz létre egy Virtuálisgép-példány, amelynek típusa hello, ugyanaz, mint vagy hasonló tooa prémium szintű Storage – képes a virtuális gép. Túl megnyitásával ellenőrizheti hello teljesítmény és a különböző Virtuálisgép-példányok ára[Windows Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) vagy [Linux virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Az áttelepítést követő lépések

1. **Konfigurálja a replikált virtuális gépek toohello rendelkezésre állási csoportot, ha van ilyen**. A Site Recovery nem támogatja a áttelepítése virtuális gépek hello rendelkezésre állási csoport együtt. Attól függően, hogy a replikált virtuális gép hello fürttelepítésben hello következő módokon:
  * A virtuális gépek hello klasszikus telepítési modellel készült: hello VM toohello rendelkezésre állási csoport hello Azure-portál hozzáadása. A részletes lépéseket lásd túl[adja hozzá a virtuális gép tooan rendelkezésre állási készlet](../virtual-machines/windows/classic/configure-availability.md#addmachine).
  * Hello erőforrás-kezelő telepítési modell: a virtuális gép hello a konfiguráció mentéséhez, és törölje a, és hozza létre az hello hello rendelkezésre állási csoport virtuális gépe. toodo tehát parancsfájllal hello: [beállítása Azure Resource Manager virtuális gép rendelkezésre állási csoport](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Ellenőrizze a parancsfájl hello korlátozás, és tervezze állásidő hello parancsfájl futtatása előtt.

2. **Törölje a régi virtuális gépek és lemezek**. Mielőtt törölné ezeket, ellenőrizze, hogy hello Premium lemezek megegyeznek a forráslemezekhez és új virtuális gépek hajtsa végre a virtuális gépek hello forrásként olyan funkciókat hello hello. Hello erőforrás-kezelő (RM) üzembe helyezési modellel törölje a virtuális gép hello és hello lemezek törlése a forrás tárfiókok hello Azure-portálon. Hello klasszikus üzembe helyezési modellel törölheti a hello virtuális gép és a lemezek hello klasszikus portálon vagy az Azure-portálon. Ha egy probléma milyen hello a lemez nem törlődik annak ellenére, hogy a virtuális gép hello törölt, olvassa el a [hibák elhárítása a virtuális merevlemezek törlésekor](storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Tiszta hello Azure Site Recovery-infrastruktúra**. Ha már nincs szükség a Site Recovery, törölheti is az infrastruktúra replikált elemek, a hello konfigurációs kiszolgáló és a helyreállítási irányelv hello törlésével, és ezután a hello Azure Site Recovery-tároló törlése.

## <a name="troubleshooting"></a>Hibaelhárítás

* [Figyelése és hibaelhárítása szempontjából a virtuális gépek és fizikai kiszolgálók védelme](../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [A Microsoft Azure Site Recovery-fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Következő lépések

Tekintse meg a következő virtuális gépek meghatározott forgatókönyvek erőforrásait hello:

* [Az Azure virtuális gépek közötti Storage-fiókok áttelepítése](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Hozzon létre, és töltse fel a Windows Server VHD tooAzure.](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Létrehozásával, majd ismét feltölteni a virtuális merevlemez a Contains hello Linux operációs rendszer](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Amazon AWS tooMicrosoft Azure virtuális gépek áttelepítése](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

A következő erőforrások toolearn további információk az Azure Storage és az Azure virtuális gépek hello lásd még:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Az Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz](storage-premium-storage.md)

[1]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png
