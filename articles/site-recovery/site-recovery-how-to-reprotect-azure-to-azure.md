---
title: "az Azure virtuális gépek hátsó tooprimary Azure-régió, átadja a feladatokat a aaaHow tooReprotect |} Microsoft Docs"
description: "Feladatátvétel után a virtuális gépek egy Azure-régiót tooanother az Azure Site Recovery tooprotect hello gépek visszafelé is használhatja. Hello lépéseket megtudhatja, hogyan toodo a védelem-újrabeállítási újra a feladatátvétel előtt."
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
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 991c7ee8f489e84c250230bf73f3e99015c5f051
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reprotect-from-failed-over-azure-region-back-tooprimary-region"></a>A védelem-újrabeállítási átadja a feladatokat az Azure-régió hátsó tooprimary régió



>[!NOTE]
>
> Az Azure virtuális gépek helyreállítási helyreplikálásának jelenleg előzetes verzió.


## <a name="overview"></a>Áttekintés
Ha Ön [feladatátvételi](site-recovery-failover.md) hello virtuális gépeket egy Azure-régiót tooanother, hello virtuális gépek egy nem védett állapotban van. Ha azt szeretné, hogy toobring újra toohello elsődleges régióban, toofirst kell hello virtuális gépek és feladatátvételi be újra a védelmét. Hogyan között nincs különbség egy irányt vagy egyéb feladatátvételi. Ehhez hasonlóan a védelem engedélyezése a virtuális gépek hello utáni, hello védelem-újrabeállítási post feladatátvétel, vagy a feladás egy vagy több feladat-visszavétel között nincs különbség.
tooexplain hello munkafolyamatok védelem-újrabeállítási és tooavoid zavart, használjuk hello elsődleges hely hello védett gépek Kelet-Ázsia régiót, és hello helyreállítási helyen hello gépek Délkelet-Ázsia régióban. A feladatátvételi feladatátvételi hello virtuális gépek toohello Délkelet-Ázsia terület lesz. Hogy a feladat-visszavétel előtt meg kell tooreprotect hello virtuális gépek Délkelet-Ázsia hátsó tooEast Ázsia. Ez a cikk hello lépéseket ismerteti, hogyan tooreprotect.

> [!WARNING]
> Ha rendelkezik [áttelepítésével kész](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), hello áthelyezett virtuális gép tooanother erőforrás csoport vagy törölt hello Azure virtuális gép, ezt követően a feladat-visszavétel nem lehet.

Miután a védelem-újrabeállítási befejezése és hello védett virtuális gépeket replikál, a feladatátvételt a virtuális gépek toobring hello is kezdeményezhető őket biztonsági tooEast Ázsia régió.

Megjegyzéseit vagy kérdéseit a cikk vagy hello hello végén utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Előfeltételek
1. virtuális gépek hello kell véglegesítve lettek.
2. hello célhelyre – ebben az esetben hello Kelet-Ázsia Azure azon régióját elérhetőnek kell lennie, és képes tooaccess/hozzon létre új erőforrásokat, az adott régióban kell lennie.

## <a name="steps-tooreprotect"></a>Lépéseket tooreprotect

Következő lépések tooreprotect vannak hello egy virtuális gép hello alapértelmezett beállításaival.

1. A **tároló** > **replikált elemek**, kattintson a jobb gombbal a hello virtuális gépet, amely a feladatátvétel megtörtént, és válassza **védelmének újbóli beállításához**. Kattintson a hello gépi, és válassza **védelmének újbóli beállításához** a hello parancsgombok.

![Kattintson jobb gombbal a tooreprotect](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Hello panelen, figyelje meg, hogy a védelem hello irány **Délkelet-Ázsia tooEast Ázsia**, már be van jelölve.

![Lássa el újból védelemmel panel](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Felülvizsgálati hello **erőforrás csoport, hálózati, tárolási és a rendelkezésre állási készletek** információkat, és kattintson az OK gombra. Ha bármely (új) megjelölt erőforrások, akkor jön létre, mivel hello része lássa el újból védelemmel.

Ez a feladat indítási lássa el újból védelemmel feladatot, amely először rendezendő hello célhelyre (ebben az esetben SZE) hello legújabb adatokkal, és után, hogy befejeződött, a művelet replikálja a hello eltérések, feladatátvétel előtt biztonsági tooSoutheast Ázsia.

### <a name="reprotect-customization"></a>Lássa el újból védelemmel testreszabása
Ha azt szeretné, hogy toochoose hello kivonat tárolási fiók vagy hello során hálózati lássa el újból védelemmel, a, hello segítségével testre szabhatja a hello védelem-újrabeállítási panelen megadott beállítást.

![A beállítás testreszabása](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

He cél virtuális gép tulajdonságainak védelem-újrabeállítási során a következő hello testre.

![Testre szabhatja a panelen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Tulajdonság |Megjegyzések  |
|---------|---------|
|Cél-erőforráscsoport     | Kiválaszthatja a toochange hello célként megadott erőforráscsoportja th virtuális géphez létre. A védelem-újrabeállítási hello részeként hello cél virtuális gép törlődik, ezért választhat egy új erőforráscsoportot, amelyek alapján is létrehozhat hello VM post feladatátvevő         |
|Virtuális hálózati cél     | Hálózati hello védelem-újrabeállítási közben nem módosítható. toochange hello hálózati, mégis hello hálózatra való leképezés.         |
|Célként megadott     | Hello tárfiók toowhich hello virtuális gép lesz a létrehozott post feladatátvételi módosíthatja.         |
|Gyorsítótár-tároló     | A replikáció során használt gyorsítótár tárfiók is megadhat. Ha az alapértelmezett hello, a gyorsítótár új tárfiók létrehozza, ha még nem létezik.         |
|Rendelkezésre állási csoport     |Kelet-Ázsia hello virtuális gép rendelkezésre állási csoport része, ha választhat a rendelkezésre állási készlet hello cél virtuális gépen a Délkelet-Ázsia. Alapértelmezett hello meglévő tenger rendelkezésre állási csoport megtalálja, és próbálja meg toouse azt. Testreszabás adjon meg egy teljesen új AV-készlet.         |


### <a name="what-happens-during-reprotect"></a>Mi történik a védelem-újrabeállítási során?

Csak hello után először engedélyezni a védelmet, például a következők hello vakpróbát, ha hello Alapértelmezések használata létrehozása.
1. A gyorsítótár tárfiók hello Kelet-Ázsia régióban végrehajtásakor létrejön.
2. Ha hello cél tárfiók (hello eredeti tárfiókját hello Délkelet-Ázsia méretű) nem létezik, egy új jön létre. hello értéke a "automatikus" utótaggal hello Kelet-Ázsia virtuális gép tárfiók.
3. Ha hello cél AV-készlet nem létezik, és hello alapértelmezett észleli, hogy ezt követően a rendszer automatikusan létrehozza a hello részeként kell egy új AV beállításához toocreate állítsa a feladatot. Ha testreszabott hello állítsa, majd kijelölt hello AV set fogja használni.
4.

Az alábbiakban hello hello listája lépéseket, amelyek fordulhat elő, amikor a védelem-újrabeállítási feladatot indít. Ez az a virtuális gép valóban létezik hello eset hello cél oldalon.

1. hello szükséges vakpróbát védelem-újrabeállítási részeként jönnek létre. Ha már léteznek, majd azokat újra felhasználja a rendszer.
2. hello cél ügyféloldali (Délkelet-Ázsiában) virtuális gép először ki van kapcsolva, ha fut-e.
3. hello cél ügyféloldali virtuális gép lemezén tárolnia másolja a program Azure Site Recovery által egy tárolóba kezdőérték blob.
4. majd törölni a hello cél ügyféloldali virtuális gépet.
5. hello kezdőérték blob hello aktuális forrás oldalon (Kelet-Ázsia) virtuális gép tooreplicate használják. Ez biztosítja, hogy csak eltérések replikálódnak.
6. hello jelentős változásokat hello forráslemez és hello kezdőérték blob között szinkronizálja a rendszer. A művelet bizonyos idő toocomplete is eltarthat.
7. Miután hello lássa el újból védelemmel feladat befejeződik, hello változásreplikálás kezdődik, létrehoz egy helyreállítási pontot hello házirend szerint.

> [!NOTE]
> A helyreállítási terv szinten nem védhetők. Akkor is csak lássa el újból védelemmel a virtuális gép szintenként.

Miután hello lássa el újból védelemmel sikeres, hello virtuális gép védett állapotban adja meg.

## <a name="next-steps"></a>Következő lépések

Hello virtuális gép védett állapotban került, miután a feladatátvétel is kezdeményezhető. hello feladatátvételi Kelet-Ázsia Azure régióban hello virtuális gép leállítása és majd létrehoz és hello Délkelet-Ázsia régió virtuális gép. Ezért van egy kis állásidő hello alkalmazáshoz. Hello időt a feladatátvételi Igen, akkor válassza, ha az alkalmazás működését egy állásidő. Javasoljuk, hogy először tesztelje feladatátvételi hello virtuális gép toomake meg arról, hogy az hamarosan megfelelően, a feladatátvétel kezdeményezése előtt.

-   [Lépéseket tooinitiate hello virtuális gép feladatátvételi teszt](site-recovery-test-failover-to-azure.md)

-   [Lépéseket tooinitiate hello virtuális gép feladatainak átvétele](site-recovery-failover.md)
