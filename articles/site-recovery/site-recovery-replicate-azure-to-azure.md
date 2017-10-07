---
title: "aaaReplicate alkalmazások (az Azure tooAzure) |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset a replikáció a virtuális gépen fut az Azure-régió, egy Azure-ban túl egy másik régióban."
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
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: fb190dac14419f892a1c6b45a3d991d8005e4bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-virtual-machines-tooanother-azure-region"></a>Az Azure virtuális gépek tooanother az Azure-régió replikálása



>[!NOTE]
>
> Az Azure virtuális gépek helyreállítási helyreplikálásának jelenleg előzetes verzió.

Ez a cikk ismerteti, hogyan tooset a replikáció a virtuális gépek futnak a egy Azure-régiót tooanother Azure-régiót.

## <a name="prerequisites"></a>Előfeltételek

* hello a cikk feltételezi, hogy már ismeri a Site Recovery és a Recovery Services-tároló. Egy "Recovery services-tároló" előre létrehozott toohave van szüksége.

    >[!NOTE]
    >
    > Ajánlott hello hello helyre, ahol a virtuális gépek tooreplicate "Recovery services-tároló" létrehozása. Ha a célhely "USA középső RÉGIÓJA", hozzon létre például az "USA középső RÉGIÓJA" tárolóban.

* Ha hálózati biztonsági csoportok (NSG) szabályok vagy tűzfal proxy toocontrol hozzáférés toooutbound internetkapcsolattal az Azure virtuális gépek hello használ, győződjön meg arról, adott meg az engedélyezett hello szükséges URL-címek vagy IP-címek. Tekintse meg a túl[hálózati útmutató](./site-recovery-azure-to-azure-networking-guidance.md) további részleteket.

* Ha egy expressroute-on vagy a helyszíni és hello közötti VPN-kapcsolat az Azure-adatforrásról hajtsa végre az [hely kapcsolatos szempontok az Azure tooon helyszíni ExpressRoute / VPN-konfiguráció](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentum.

* Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable Azure virtuális gép replikációját.

* Az Azure-előfizetéssel kell engedélyezett toocreate virtuális gépek toouse vész-Helyreállítási régió, érdemes hello célhelyet. Felveheti a kapcsolatot támogatási tooenable hello szükséges kvótát.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Engedélyezze a replikálást az Azure Site Recovery-tároló
Az ezen az ábrán azt replikálja a hello "Kelet-Ázsia" Azure-beli hely toohello "Délkelet-Ázsia" helyen futó virtuális gépeket. hello lépései a következők:

 Kattintson a **+ replikálás** hello tároló tooenable replikációs hello virtuális gépekhez.

1. **Forrás:** toohello pont eredetű hello gépek, amelyek ebben az esetben hivatkozik **Azure**.

2. **A forrás helye:** hello ahonnan szeretné tooprotect a virtuális gépek Azure-régióban. Az ezen az ábrán hello forráshely "Kelet-Ázsia" lesz

3. **Telepítési modell:** hello forrásgépek Azure-telepítés típusának toohello hivatkozik. Kiválaszthatja, hogy vagy a klasszikus vagy erőforrás-kezelő és az adott modell toohello tartozó gépek listája védelemre hello következő lépésben.

      >[!NOTE]
      >
      > Csak a klasszikus virtuális gépek replikálása, és végezze el a helyreállítást a klasszikus virtuális gépként. Az erőforrás-kezelő virtuális gépként nem lehet helyreállítani.

4. **Erőforráscsoport:** azt a hello erőforrás csoport toowhich a forrás virtuális gépek tartozik. Hello kijelölt erőforráscsoportba tartozó összes hello virtuális gépek védelemre hello következő lépésben jelennek meg.

    ![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

A **virtuális gépek > Válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Ezután kattintson az OK gombra.
    ![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


A beállítások című szakaszban konfigurálhatja a cél hely tulajdonságai

1. **Cél helye:** Ez az hello hely, ahol a forrás virtuális gép adatait a rendszer replikálja. Attól függően, a kijelölt gépekhez helyét, a Site Recovery nyújtják, akkor hello megfelelő célrégiók listája.

    > [!TIP]
    > Ajánlott tookeep célhelye megegyezik a helyreállítási frissítésétől services-tároló.

2. **Cél-erőforráscsoport:** hello erőforrás csoport toowhich a replikált virtuális gépek fog tartozni. Alapértelmezés szerint az ASR létrehoz egy új erőforráscsoportot hello cél régióban "automatikus" utótaggal rendelkező nevű. Abban az esetben, ha erőforráscsoport hozta létre az ASR már létezik, a fogja használni. Másik lehetőségként toocustomize, ahogy az alábbi hello szakasz.    
3. **Cél virtuális hálózat:** alapértelmezés szerint az ASR létrehoz egy új virtuális hálózat hello cél régióban "automatikus" utótaggal rendelkező nevű. A csatlakoztatott tooyour Forráshálózat lesz, és minden jövőbeli védelmi fog történni.

    > [!NOTE]
    > [Ellenőrizze a hálózati részleteket](site-recovery-network-mapping-azure-to-azure.md) tooknow további információk a hálózat leképezését.

4. **Cél Storage-fiókok:** alapértelmezés szerint az ASR hello új cél tárfiók a forrás virtuális gép tárolókonfiguráció mimicking hoz létre. Abban az esetben, ha már az ASR által létrehozott tárfiók létezik, a fogja használni.

5. **Storage-fiókok gyorsítótárazása:** ASR gyorsítótárazása nevű hello forrás régióban külön tárfiókot kell. Az összes hello történik hello a forrás virtuális gépek követni, és ezeket toohello célhelyet replikálása előtt toocache tárfiók küldött módosítások.

6. **A rendelkezésre állási csoporthoz:** alapértelmezés szerint az ASR létrehoz egy új rendelkezésre állási hello cél régióban "automatikus" utótaggal rendelkező név megadva. Abban az esetben, ha az ASR már létrehozta a rendelkezésre állási csoport létezik, a fogja használni.

7.  **Replikációs házirend:** azt határozza meg a helyreállítási pont megőrzési előzményeit és az alkalmazás alkalmazáskonzisztens pillanatkép gyakorisága hello beállításait. Alapértelmezés szerint az ASR beállításokkal hozza létre egy új replikációs házirendet alapértelmezett 24 órányi a helyreállítási pontok megőrzésének ideje és a "60 percig app alkalmazáskonzisztens pillanatkép gyakorisága.

    ![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Tároló erőforrásait testreszabása

Abban az esetben, ha azt szeretné, hogy toochange hello alapértelmezett értékeket használják az automatikus rendszer-Helyreállítás, igényei szerint hello-beállítások módosításával.

1. **Testreszabása:** kattintson rá a toochange hello ASR által használt alapértelmezett értékeket.

2. **Cél-erőforráscsoport:** hello erőforráscsoport választhat célhelyet hello hello előfizetésen belül a meglévő összes hello erőforráscsoportok hello listáját.

3. **Cél virtuális hálózat:** hello célhelyet hello lista összes hello virtuális hálózat található.

4. **A rendelkezésre állási csoporthoz:** rendelkezésre állási készletek beállítások toohello virtuális gépek rendelkezésre állási forrás régióban egy részét képező csak akkor adhat hozzá.

5. **Cél Storage-fiókok:**

![Engedélyezze a replikálást](./media/site-recovery-replicate-azure-to-azure/customize.PNG) kattintson a **tároló-erőforrás létrehozása** és a replikáció engedélyezése


Amennyiben a védett virtuális gépek ellenőrizheti, a virtuális gépek állapotát hello állapotának **replikált elemek**

>[!NOTE]
>Hello során idő a kezdeti replikáció nem sikerült annak a lehetősége, hogy állapot ideje toorefresh vesz igénybe, és nem jelenik meg a folyamatban van egy kis ideig. Kattinthat a frissítés gomb hello hello panel tooget hello legfrissebb állapotának hello tetején.
>

![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Következő lépések
- [További](site-recovery-test-failover-to-azure.md) feladatátvételi teszt futtatása.
- [További](site-recovery-failover.md) kapcsolatos különféle a feladatátvételeket, és hogyan toorun őket.
- További információ [helyreállítási tervek segítségével](site-recovery-create-recovery-plans.md) tooreduce RTO.
- További információ [újbóli védelméhez az Azure virtuális gépek](site-recovery-how-to-reprotect.md) feladatátvételt követően.
