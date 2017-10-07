---
title: "Alkalmazások (VMware tooAzure) replikálni |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset a replikáció a virtuális gépek futnak a VMware az Azure rendszerben."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: asgang
ms.openlocfilehash: b07aabdacec521c7bd89e50f6a1427a774ff0287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-applications-running-on-vmware-vms-tooazure"></a>VMware virtuális gépek tooAzure futó alkalmazás replikálása



Ez a cikk ismerteti, hogyan tooset a replikáció a virtuális gépek futnak a VMware az Azure rendszerben.
## <a name="prerequisites"></a>Előfeltételek

hello cikk feltételezi, hogy már rendelkezik

1.  [A telepítő a helyszíni forráskörnyezetben](site-recovery-set-up-vmware-to-azure.md)
2.  [Az Azure-ban a célkörnyezet beállítása](site-recovery-prepare-target-vmware-to-azure.md)


## <a name="enable-replication"></a>A replikáció engedélyezése
#### <a name="before-you-start"></a>Előkészületek
Ha VMware virtuális gépeket replikál, vegye figyelembe, hogy:

* Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.
* VMware virtuális gépek felderített 15 percenként. 15 percig is eltarthat, vagy hosszabb ideig őket tooappear hello portálon felderítése után. Hasonlóképpen tarthat a felderítés 15 perc vagy több új vCenter-kiszolgáló vagy vSphere gazdagép hozzáadásakor.
* Környezet módosításokat (például a VMware-eszközök telepítése) hello virtuális gépen is igénybe vehet, 15 perc vagy több toobe hello portálon frissítve.
* Ellenőrizheti a hello a legutóbb felfedezett idő VMware virtuális gépek esetén a hello **utolsó lépjen kapcsolatba a** hello vCenter kiszolgáló vagy vSphere-gazdagép, a hello mezőt **konfigurációs kiszolgálók** panelen.
* tooadd gépek replikációjának hello ütemezett felderítési, való várakozás nélkül jelölje ki a konfigurációs kiszolgáló hello (ne kattintson azt), és kattintson a hello **frissítése** gomb.
* Replikációs, ha készen áll a testreszabásra hello gép engedélyezésekor hello folyamatkiszolgáló automatikusan telepíti hello mobilitásiszolgáltatás azt.


**Most már engedélyezheti a replikációt az alábbiak szerint**:

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre. Hello replikáció első alkalommal engedélyezését, kattintson a **+ replikálás** a hello tároló tooenable a további gépek replikációjának.
2. A hello **forrás** panel > **forrás**, jelölje be hello konfigurációs kiszolgáló.
3. A **számítógép típusú**, jelölje be **virtuális gépek** vagy **fizikai gépek**.
4. A **vCenter vagy vSphere Hipervizorra**, hello vCenter-kiszolgálót, amely hello vSphere-gazdagép felügyeli, vagy válasszon hello állomás. Ez a beállítás nem megfelelő, ha a fizikai gépeket replikál.
5. Hello folyamatkiszolgáló kijelölése. Ha még nem hozott létre minden olyan további folyamat kiszolgálóhoz ez hello konfigurációs kiszolgáló hello neve lesz. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. A **cél** hello előfizetés és a kívánt virtuális gépek a feladatátvételt toocreate hello hello erőforráscsoport kiválasztása. Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés) Azure virtuális gépek a feladatátvételt hello hello telepítési modell.
7. Válassza ki a kívánt toouse adatok replikálásához hello Azure storage-fiók. Vegye figyelembe:

   * Kiválaszthatja a premium vagy standard szintű tárfiókot. Ha a prémium szintű fiók, egy további standard szintű tárfiók toospecify lesz szüksége folyamatos replikálási naplókhoz. Fiókoknak kell lenniük a hello hello és ugyanabban a régióban Recovery Services-tároló.
   * Ha azt szeretné, hogy egy másik tárolóhelyre fiókhoz, mint a toouse, létrehozhat egy*helyőrző hivatkozás létrehozásához erőforrás használó tárfiókot kezelő, amelyek szerepelnek az első lépések*. Kattintson egy tárfiókot a Resource Manager használatával toocreate **hozzon létre új**. Ha toocreate hello klasszikus modellt használó tárfiókot, így tesz, amely [hello Azure-portálon a](../storage/common/storage-create-storage-account.md).

8. Jelölje be hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha azok még hoz létre a feladatátvételt követően. hello hálózat kell hello hello és ugyanabban a régióban Recovery Services-tároló. Válassza ki **beállítás most a kijelölt gépekhez**, tooapply hello beállítás tooall számítógépek választja a védelem. Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot. Ha a hálózat nem rendelkezik, akkor túl[hozzon létre egyet](#set-up-an-azure-network). a hálózati erőforrás-kezelő használatával toocreate kattintson **hozzon létre új**. Ha azt szeretné, toocreate hello klasszikus modellt használó hálózatot, ehhez [hello Azure-portálon a](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Ha szükséges, válassza ki az alhálózatot. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. A **tulajdonságok** > **tulajdonságainak konfigurálása**, jelölje be hello folyamat server tooautomatically által használt fiók hello hello mobilitási szolgáltatás telepítése hello gépen.  
11. Alapértelmezés szerint a rendszer replikálja az összes lemez. replikációs, tooexclude lemezei kattintson **összes lemez** , és törölje a nem kívánt tooreplicate lemezzel.  Ezután kattintson az **OK** gombra. A további tulajdonságokat később is beállíthatja. [További](site-recovery-exclude-disk.md) kapcsolatos lemezek kivételével.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication6.png)

12. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy helyes-e replikációs házirend be van jelölve hello. Módosíthatja a replikációs házirend beállításait a **beállítások** > **replikációs házirendek** > szabályzat neve > **beállításainak szerkesztése**. Végzett módosítások tooa házirend alkalmazott tooreplicating és új gépek lesz.
13. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha toogather gépeihez egy replikációs csoporthoz, és adjon meg egy nevet hello csoportnak. Ezután kattintson az **OK** gombra. Vegye figyelembe:

    * Gépek replikációs csoportba a replikálás, és ha azok feladatátvételt megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat.
    * Azt javasoljuk, hogy gyűjtse össze a virtuális gépek és fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések. Több virtuális Gépre kiterjedő konzisztencia engedélyezése befolyásolhatja a teljesítményt a munkaterhelés, és csak akkor használja, ha a gépek is működnek hello azonos munkaterhelés, és konzisztenciára van szükség.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication7.png)
14. Kattintson a **engedélyezze a replikálást**. Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.

> [!NOTE]
> Ha hello számítógép kész az ügyfélleküldéses telepítés hello a mobilitási szolgáltatás-összetevő települ, ha engedélyezve van a védelem. Hello után telepítve van a védelmi feladatok elindításának hello gép, és meghiúsul. Hello meghibásodás után meg kell toomanually minden gép újraindításához. Után hello újraindítás hello védelmet feladat elölről kezdődik, és a kezdeti replikáció.
>
>

## <a name="view-and-manage-vm-properties"></a>A virtuális gépek tulajdonságainak megtekintése és kezelése

Azt javasoljuk, hogy ellenőrizze a hello forrásgép hello tulajdonságait. Ne feledje, hogy hello Azure virtuális gép nevének meg kell felelnie az [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. Kattintson a **beállítások** > **replikált elemek** >, és jelölje be hello gép. Hello **Essentials** panel gépek beállítások és állapotával kapcsolatos adatokat jeleníti meg.
2. A **tulajdonságok**, megtekintheti a replikációs és feladatátvételi adatait hello virtuális gép.
3. A **számítás és hálózat** > **számítási tulajdonságok**, megadhatja a hello Azure virtuális gép nevét és a cél méretét. Ha szeretné, módosítsa a hello neve toocomply az Azure-követelményeknek.
    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/VMProperties_AVSET.png)
 
4.  Kiválaszthatja a [erőforráscsoport](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) mely gépre post részei lesznek a feladatátvételt. Ez a beállítás módosítható előtt sikertelen keresztül. POST feladatátvételt, ha megszakad a gép védelmi beállításait, majd telepítse át hello gép tooa másik erőforráscsoportban található.
5. Választhat egy [rendelkezésre állási csoport](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) egy post része lesz szükség a gép toobe feladatátadáshoz használhat. Amíg kiválasztásával rendelkezésre állási csoportban, adjon a következőket kell figyelembe venni, hogy:

    * Csak a rendelkezésre állási készletek tartozó toohello megadott erőforráscsoport jelennek-e  
    * Gépek, a másik virtuális hálózatot nem lehet azonos rendelkezésre állási csoport tagja
    * Csak az azonos méretű virtuális gépek azonos rendelkezésre állási csoport egy része lehet.
5. Is megtekintheti, és adja hozzá a hello célhálózat, alhálózati és toohello Azure virtuális Géphez rendelendő IP-címet.
6. A **lemezek**, megtekintheti a hello operációs rendszer és az adatlemezek hello virtuális Gépet, amely a rendszer replikálja.

### <a name="network-adapters-and-ip-addressing"></a>Hálózati adapterek és IP-címzés 

- Beállíthat hello cél IP-címet. Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni. Ha egy címet, amely nem használható feladatátadásra, hello feladatátvétel nem fog működni. cél IP-cím a feladatátvételi teszthez használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.
- hálózati adapterek száma hello hello mérete határozza meg hello cél virtuális gépre, az alábbiak szerint határozza meg:
    - Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.
    - Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet majd hello cél maximális fogja használni.
    - Ha például a forrásgépen két hálózati adapterrel és hello célgép mérete négy támogatja, hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.
    - Ha a virtuális gépnek több hálózati adapter hello lesz az összes csatlakoznak toohello ugyanazon a hálózaton.
    - Ha hello a virtuális gépnek a több hálózati adapter, majd hello először egy hello listán látható lesz hello *alapértelmezett* hello Azure virtuális gép hálózati adapteréhez.
   



## <a name="common-issues"></a>Gyakori problémák

* Egyes lemezek kisebb, mint 1TB méretű kell lennie.
* hello operációsrendszer-lemez nem dinamikus lemez és egy alaplemeznek kell lennie.
* 2/UEFI felülettel. generációs virtuális gépek engedélyezett, hello operációsrendszer-családot kell lennie a Windows és a rendszerindító lemez 300GB-nál kisebbnek kell lennie.

## <a name="next-steps"></a>Következő lépések

Ha hello védelmi befejeződött, próbálja meg [feladatátvételt](site-recovery-failover.md) toocheck e az alkalmazás megjelenik az Azure-ban vagy sem.

Abban az esetben, ha azt szeretné, hogy toodisable védelmi, ellenőrizze, hogy túl[regisztrációs és védelmi beállításainak törlése](site-recovery-manage-registration-and-protection.md)
