---
title: "Alkalmazások (az Azure-bA VMware) replikálni |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan állíthat be az Azure-bA a VMware rendszerben futó virtuális gépek replikációját."
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
ms.date: 09/29/2017
ms.author: asgang
ms.openlocfilehash: 028aa0f23c3a7c98c4801d9e306c5dcfa35aab80
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/20/2017
---
# <a name="replicate-applications-running-on-vmware-virtual-machines-to-azure"></a>Az Azure-bA VMware virtuális gépeken futó alkalmazások replikálása



Ez a cikk ismerteti, hogyan replikációjának VMware futó Azure virtuális gépek (VM).
## <a name="prerequisites"></a>Előfeltételek

Ez a cikk feltételezi, hogy rendelkezik:

1.  [Állítsa be a helyszíni forráskörnyezetben](site-recovery-set-up-vmware-to-azure.md).
2.  [Az Azure-ban a célkörnyezet beállítása](site-recovery-prepare-target-vmware-to-azure.md).


## <a name="enable-replication"></a>A replikáció engedélyezése
### <a name="before-you-start"></a>Előkészületek
VMware virtuális gépek replikálásához:

* Egyes rendelkeznie kell Azure felhasználói fiókja [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) engedélyezni szeretné egy új virtuális gép az Azure-bA replikálását.
* VMware virtuális gépek felderített 15 percenként. 15 percig is eltarthat, vagy az Azure portálon felderítése után jelenik meg, hogy hosszabb. Hasonlóképpen tarthat a felderítés 15 perc vagy több új vCenter-kiszolgáló vagy vSphere gazdagép hozzáadásakor.
* A virtuális gépen (például a VMware-eszközök telepítése) környezetben módosítások is igénybe vehet, akár 15 percnél frissítenie kell a portálon.
* A legutóbb felfedezett ellenőrizheti a VMware virtuális gépek esetén a **utolsó lépjen kapcsolatba a** a vCenter kiszolgáló vagy vSphere-gazdagép, a mezőben a **konfigurációs kiszolgálók** lap.
* Gépek replikációjának az ütemezett felderítési való várakozás nélkül hozzáadásához jelölje ki a konfigurációs kiszolgálót (ne kattintson azt), és kattintson a **frissítése** gombra.
* Replikációs, ha a számítógép készen áll a testreszabásra engedélyezésekor a folyamatkiszolgáló automatikusan telepíti a mobilitási szolgáltatás azt.


### <a name="enable-replication-as-follows"></a>Az alábbiak szerint a replikáció engedélyezése

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre. A replikálás első alkalommal történő engedélyezését követően kattintson a tárolóban elérhető **+Replikálás** elemre a további gépek replikációjának engedélyezéséhez.
2. Az a **forrás** lap > **forrás**, válassza ki a konfigurációs kiszolgálót.
3. A **számítógép típusú**, jelölje be **virtuális gépek** vagy **fizikai gépek**.
4. A **vCenter vagy vSphere Hipervizorra**jelölje be a vCenter-kiszolgáló, amely kezeli a vSphere-gazdagép, vagy jelölje ki a gazdagépet. Ez a beállítás nem megfelelő, ha a fizikai gépeket replikál.
5. Válassza ki a folyamatkiszolgáló, amely a konfigurációs kiszolgáló neve lesz, ha még nem hozott létre minden olyan további folyamat kiszolgálóhoz. Ezután kattintson az **OK** gombra.

    ![Replikációs forrás engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. A **cél**, válassza ki az előfizetés és az erőforráscsoport, ahová a átvevő virtuális gépek létrehozásához. Válassza ki a átvevő virtuális gépek (klasszikus vagy az Azure Resource Manager) az Azure-ban használni kívánt telepítési modelljét.

7. Válassza ki az adatok replikálásához használni kívánt Azure Storage-fiók. 

    > [!NOTE]

    >   * Kiválaszthatja a premium vagy standard szintű tárfiókot. Ha a prémium szintű fiók lehetőséget választja, meg kell adnia egy további standard szintű tárfiók folyamatos replikálási naplókhoz. Fiókok és a Recovery Services-tárolónak ugyanabban a régióban kell lennie.
    >   * Ha szeretne egy másik tárolási fiókot használja, akkor [hozzon létre egyet](../storage/common/storage-create-storage-account.md). Erőforrás-kezelő használatával hozzon létre egy tárfiókot, kattintson a **hozzon létre új**. A storage-fiók létrehozása a klasszikus modellt használó, tegye, hogy az Azure portálon.

8. Válassza ki azt az Azure-hálózatot, valamint alhálózatot, amelyhez a feladatátvételt követően felálló Azure virtuális gépek csatlakozni fognak. A hálózatnak és a Recovery Services-tárolónak ugyanabban a régióban kell elhelyezkednie. Ha a megadott hálózati beállításokat az összes védelemre kijelölt gépre szeretné alkalmazni, válassza a **Beállítás most a kijelölt gépekhez** lehetőséget. Ha az egyes gépeknél külön-külön szeretné beállítani az Azure-hálózatot, kattintson a **Beállítás később** elemre. Ha a hálózat nem rendelkezik, akkor [hozzon létre egyet](#set-up-an-azure-network). A hálózati erőforrás-kezelő használatával létrehozásához kattintson a **hozzon létre új**. Ha a hálózat létrehozása a klasszikus modellt használó teheti meg, amely [az Azure portálon](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Válassza ki egy alhálózatot, ha van ilyen, és kattintson a **OK**.

    ![Replikációs cél beállítás engedélyezése](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. A **virtuális gépek** > **válassza ki a virtuális gépek**, válasszon ki minden replikálni kívánt gépet. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Ezután kattintson az **OK** gombra.

    ![Replikációs válassza virtuális gépek engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. A **tulajdonságok** > **tulajdonságainak konfigurálása**, válassza ki a fiókot használják a folyamatkiszolgáló automatikusan telepítse a mobilitási szolgáltatás a számítógépen.  
11. Alapértelmezés szerint a rendszer replikálja az összes lemez. Lemezek kizárása a replikációból, kattintson a **összes lemez** , és törölje a lemezek nem szeretne replikálni.  Ezután kattintson az **OK** gombra. A további tulajdonságokat később is beállíthatja. [További](site-recovery-exclude-disk.md) kapcsolatos lemezek kivételével.

    ![Engedélyezése replikációs tulajdonságainak konfigurálása](./media/site-recovery-vmware-to-azure/enable-replication6.png)

12. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy a megfelelő replikációs házirend van-e kiválasztva. Módosíthatja a replikációs házirend beállításait a **beállítások** > **replikációs házirendek** > (házirend neve) > **beállításainak szerkesztése**. Egy házirend végzett módosítások is alkalmazhat replikációs és új gépek.
13. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha gyűjtse össze a gépet egy replikációs csoporthoz. Adjon meg egy nevet a csoportnak, és kattintson **OK**. 

    > [!NOTE]

    >    * Egy replikációs csoportban gépek replikálása együtt, és ha azok feladatátvételt megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat.
    >    * Virtuális gépek és fizikai kiszolgálók gyűjtse össze, hogy látni lehessen a munkaterhelések. Több virtuális Gépre kiterjedő konzisztencia engedélyezése hatással lehet a számítási feladat teljesítményére. Csak akkor, ha a gép ugyanaz az alkalmazás fut, és konzisztencia kell használni.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication7.png)
14. Kattintson a **engedélyezze a replikálást**. Előrehaladásának nyomon követheti a **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. A **Védelem véglegesítése** feladat befejeződését követően a gép készen áll a feladatátvételre.

> [!NOTE]
> Ha a számítógép kész az ügyfélleküldéses telepítést, a mobilitási szolgáltatás telepítve van, ha engedélyezve van a védelem. Miután az összetevő telepítve van a számítógépen, a védelmi feladatot elindul, és sikertelen lesz. A meghibásodás után meg kézzel kell újraindítania az egyes. Az újraindítás után újra megkezdi a védelmi feladat, és a kezdeti replikálást.
>
>

## <a name="view-and-manage-vm-properties"></a>A virtuális gépek tulajdonságainak megtekintése és kezelése

Ezután ellenőrizheti a forrásgép tulajdonságait. Ne feledje, hogy az Azure virtuális gép nevét kell adott a specifikációknak való [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. Kattintson a **beállítások** > **replikált elemek** >, majd válassza ki a gépet. A **Essentials** lapon látható a gép beállításainak és állapotával kapcsolatos adatokat.
2. A **Tulajdonságok** résznél tekintheti meg a virtuális gép replikációs és feladatátvételi adatait.
3. A **Számítás és hálózat** > **Számítási tulajdonságok** résznél adhatja meg az Azure virtuális gép nevét és a cél méretét. A név követelményeinek való megfeleléshez Azure szükség esetén módosítható.

    ![Számítási és hálózati tulajdonságok](./media/site-recovery-vmware-to-azure/vmproperties.png)

4.  Kiválaszthatja a [erőforráscsoport](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) származó, amelyek a gépek részévé válik a feladás egy vagy több feladatátvétel. Ez a beállítás módosítható feladatátvétel előtt. A feladatátvétel utáni, ha egy másik erőforráscsoportban található, a védelmi beállításait az adott gép break telepítse át a gépet.
5. Választhat egy [rendelkezésre állási csoport](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) Ha a számítógépen kell lennie a feladás egy vagy több feladatátvétel részét. Amíg a rendelkezésre állási csoportok kiválasztásánál a következőket kell figyelembe venni:

    * Csak a megadott erőforráscsoport tartozó rendelkezésre állási csoportok találhatók.  
    * Gépek, a különböző virtuális hálózatokon az azonos rendelkezésre állási készlet része nem lehet.
    * Azonos méretűnek csak virtuális gépek rendelkezésre állási csoportok egy része lehet.
5. Is megtekintheti, és a cél hálózati alhálózat és az Azure virtuális Géphez rendelt IP-cím kapcsolatos információval.
6. A **lemezek**, az operációs rendszer és az adatlemezek láthatja a replikálandó virtuális Gépre.

### <a name="network-adapters-and-ip-addressing"></a>Hálózati adapterek és IP-címzés

- A cél IP-címe beállítható. Ha nem ad meg egy címet, a átvevő gép DHCP használja. Ha egy címet, amely nem használható feladatátadásra, a feladatátvétel nem működik. Ha a cím elérhető a feladatátvételi teszt hálózatában lévő, az azonos cél IP-címet a feladatátvételi teszthez használható.
- A hálózati adapterek számát a cél virtuális gépek mérete határozza meg, a következők szerint:
    - Ha a forrásgépen hálózati adapterek száma kisebb vagy egyenlő a célgép mérete engedélyezett adapterek számával, majd a cél rendelkezik ugyanannyi adapter forrásaként.
    - Ha a forrás virtuális gépek adaptereinek száma meghaladja a célmérethez engedélyezett maximumot, a rendszer a célmérethez engedélyezett maximális számot használja.
    Például ha a forrásgépen két hálózati adaptert, és a célgép mérete négy támogatja, a célgépen két adapter rendelkezik. Ha a forrásgépen két adapter, de a cél méretét csak támogat egy, a célgépen csak egy adapterrel rendelkezik.
    - Ha a virtuális gépnek több hálózati adapter, minden csatlakoznak az ugyanazon a hálózaton. Emellett az elsőt a listán látható lesz a *alapértelmezett* az Azure virtuális gép hálózati adapteréhez.

### <a name="azure-hybrid-use-benefit"></a>Azure hibrid használata juttatás

Microsoft Szoftverbiztosítással az ügyfelek használhatják Azure hibrid használja juttatásra, licencelési költségeit, amelyek áttelepítése az Azure Windows Server-gépek mentheti, vagy vész-helyreállítási Azure használ. Ha Ön jogosult az Azure hibrid használja kiszolgálóterhelések használják ki a használandó, megadhatja, hogy a virtuális géphez hozzárendelt az előnyök egyike az Azure Site Recovery hoz létre, ha egy feladatátvevő. Ehhez tegye a következőket:
- Nyissa meg a replikált virtuális gép számítási és hálózati tulajdonságok szakaszába.
- Válaszolja meg a kérdést, amely rákérdez, hogy van-e egy Windows Server-licenc, amely lehetővé teszi, hogy jogosult Azure hibrid használata juttatásra.
- Jelölje be a jelölőnégyzetet annak megerősítéséhez, hogy rendelkezik-e egy jogosult Windows Server-licenc és frissítési garancia, amelyek segítségével a hibrid használja juttatás alkalmazni a számítógépen, a feladatátvételi létrehozott.
- A replikált gép beállításainak mentése.

További információ [Azure hibrid használata juttatás](https://aka.ms/azure-hybrid-use-benefit-pricing).

## <a name="common-issues"></a>Gyakori problémák

* Egyes lemezek kisebb, mint 1 TB méretű kell lennie.
* Az operációs rendszer lemezének alaplemeznek és a nem dinamikus lemez kell lennie.
* Generációs 2/UEFI-kompatibilis virtuális gépek az operációsrendszer-családot Windows kell lennie, és a rendszerindító lemez 300 GB-nál kisebbnek kell lennie.

## <a name="next-steps"></a>Következő lépések

Miután a védelem, és a gép elérte-e egy védett állapotot, próbálja meg egy [feladatátvételi](site-recovery-failover.md) ellenőrizze, hogy az alkalmazás megjelenik az Azure-ban, vagy nem.

Ha azt szeretné, hogy tiltsa le a védelmet, megtudhatja, hogyan [regisztrációs és védelmi beállítások tiszta](site-recovery-manage-registration-and-protection.md).
