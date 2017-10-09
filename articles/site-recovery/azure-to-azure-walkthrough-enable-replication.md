---
title: "Azure virtuális gépek tooanother Azure-régió, az Azure Site Recovery aaaEnable replikálása |} Microsoft Docs"
description: "Azure virtuális gépek, hello Azure Site Recovery szolgáltatással tooenable replikációs tooanother Azure-régióban kell hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a309644f-d36b-4188-bba7-ad45a2d9bede
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 8/01/2017
ms.author: raynew
ms.openlocfilehash: 2fa03db45a18ccb8b9f31ed05589be0dd6d5f031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a>5. lépés: Azure virtuális gépek replikációjának engedélyezése


Miután beállította a [Recovery Services-tároló](azure-to-azure-walkthrough-vault.md), ez a cikk tooenable replikáció a virtuális gépek (VM), az Azure-régió, tooanother használata [Azure Site Recovery](site-recovery-overview.md). tooenable replikációs, akkor állítsa be a forrás és cél beállításai, ellenőrizze a hello replikációs házirend, és válassza ki a kívánt tooreplicate virtuális gépek.

- Amikor befejezi a hello a cikkben az Azure virtuális gépek kell kell replikálni toohello másodlagos Azure-régiót.
- Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Az Azure virtuális gép replikációs jelenleg előzetes verzió.


## <a name="select-hello-source"></a>Hello forrás kiválasztása 

1. A Recovery Services-tárolók, kattintson a hello tároló neve > **+ replikálás**.
2. A **forrás**, jelölje be **Azure - előzetes**.
2. A **adatforrásról**, jelölje be hello forrás Azure-régió, ahol a virtuális gépek futnak.
3. Jelölje be hello **Azure virtuális gép üzembe helyezési modellel** virtuális gépek: **erőforrás-kezelő** vagy **klasszikus**.
4. Jelölje be hello **forrás-erőforráscsoporton** erőforrás-kezelő virtuális gépekhez, vagy **felhőalapú szolgáltatás** klasszikus virtuális gépekhez.
5. Kattintson a **OK** toosave hello beállításait.

    ![Állítson be hello forrás](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-hello-vms"></a>Hello virtuális gépek kiválasztása

A Site Recovery lekéri azon virtuális gépek társított hello előfizetés és az erőforrás/felhőszolgáltatás hello listáját.

1. A **virtuális gépek**, válassza ki a kívánt tooreplicate hello virtuális gépeket.
2. Kattintson az **OK** gombra.

    ![Válassza ki a virtuális gépek](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a>Beállítások konfigurálása

A Site Recovery kiépítését hello cél régió (hello forrásbeállítások régió alapján), az alapértelmezett beállításait és hello replikációs házirendhez:

   - **Célhelye**: hello cél régió vész-helyreállítási toouse használni szeretne. Azt javasoljuk, hogy hello célhelye megegyezik-e a Site Recovery-tároló hello hello helyét.
   - **Célként megadott erőforráscsoportja**: erőforrás csoport toowhich hello cél régióban Azure virtuális gépek a feladatátvételt követően fog tartozni. Alapértelmezés szerint a Site Recovery hello cél régióban az "automatikus" utótaggal rendelkező egy új erőforráscsoportot hoz létre. 
   - **Cél virtuális hálózati**: hello hálózati az Azure virtuális gépek mely hello a cél régióban található feladatátvételt követően. Alapértelmezés szerint a Site Recovery hoz létre egy új virtuális hálózat (és alhálózatok) az "automatikus" utótaggal rendelkező hello cél régióban. Ehhez a hálózathoz csatlakoztatott tooyour Forráshálózat. Vegye figyelembe, hogy a virtuális gépek a feladatátvételt követően rendelhet egy adott IP-cím, ha szüksége tooretain hello azonos IP-cím hello forrás- és a célként megadott helyen. 
   - **Storage-fiókok gyorsítótár**: a Site Recovery storage-fiókot használ hello forrás régióban. A forrás virtuális gépeken módosítások előtt replikációs toohello célhelyet küldött toothis fiók. 
   - **Storage-fiókok cél**: alapértelmezés szerint a Site Recovery hoz létre egy új tárfiókot hello cél régióban, toomirror hello forrás virtuális gép tárfiók.
   -  **Rendelkezésre állási készletek cél**: alapértelmezés szerint a Site Recovery hoz létre egy új rendelkezésre állási csoportban, hello cél régióban, hello "automatikus" utótaggal rendelkező. 
   - **Replikációs házirend neve**: a házirend nevét.
   - **Helyreállítási pontok megőrzésének ideje**: alapértelmezés szerint a Site Recovery helyreállítási pontok tartja 24 órán át. Beállíthatja, hogy egy 1 és 72 óra közötti értéket.
   - **Alkalmazáskonzisztens pillanatkép gyakorisága**: alapértelmezés szerint a Site Recovery pillanatfelvételt egy alkalmazáskonzisztens 4 óránként. Beállíthatja, hogy bármely érték 1 és 12 óra között. Folyamatosan replikált adatokat:
    - Összeomlás-konzisztens helyreállítási pontot karbantartása azonos adatokat írási magasrendű létrehozásakor. Ez a helyreállítási pont típus általában elegendő, ha az alkalmazás egy összeomlási adatok inkonzisztenciát nélkül a tervezett toorecover
    - Összeomlás-konzisztens helyreállítási pontjai akkor jönnek létre, néhány percenként. A helyreállítási pontok toofail protokollt használó, és állítsa a virtuális gépek biztosít a helyreállítási pont célkitűzés (RPO) hello sorrendben perc alatt.
    - Alkalmazáskonzisztens helyreállítási pontokat (a hozzáadása toowrite magasrendű konzisztencia) Győződjön meg arról, hogy futó alkalmazások végrehajtani minden műveletnél ürítése pufferek toodisk (alkalmazás leépítése). Azt javasoljuk, hogy a helyreállítási pontok adatbázis-alkalmazások, például Exchange, SQL Server és Oracle használja.
        
    ![Beállítások konfigurálása](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a>Beállítások módosítása

Ha azt szeretné, hogy toomodify cél- és a replikációs házirend-beállítások, a hello, a következő:

1. tooview vagy a célként megadott beállítások módosításához kattintson a **beállítások**.
2. Kattintson a toooverride hello alapértelmezett tárolóbeállítások **Testreszabás**. A célként megadott erőforráscsoportja, a virtuális hálózat, a rendelkezésre állási csoport és a cél tárfiók is megadhat. Rendelkezésre állási csoportok csak virtuális gépek részei egy készlet hello forrás régióban adhat hozzá.

    ![Beállítások konfigurálása](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. Kattintson a helyreállítási pontokhoz, illetve az alkalmazáskonzisztens pillanatképek toooverride replikációs beállításainak **Testreszabás** következő túl**replikációs házirend**.
 
    ![Beállítások konfigurálása](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. Kattintson a toostart létesítési hello tároló erőforrásait, **célerőforrások létrehozása**. Kiépítés egy percet vesz igénybe. Ne zárja be a panelt hello kiépítése során, vagy konfigurálnia kell toostart keresztül.




## <a name="enable-replication"></a>A replikáció engedélyezése

1. A **beállítások**, kattintson a **engedélyezze a replikálást**. Ez lehetővé teszi a kiválasztott virtuális gépek hello kezdeti replikálása. Kezdeti replikáció állapotát is igénybe vehet néhány alkalommal toorefresh. Kattintson a **frissítése** tooget hello legutóbbi állapota.

2. Hello állapotának nyomon követheti **engedélyezni a védelmet** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**.

3. A **beállítások** > **replikált elemek**, virtuális gépek hello állapotát tekintheti meg és hello a kezdeti replikáció folyamatban van. Kattintson a hello VM toodrill le azokat a beállításokat.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[6. lépés: feladatátvételi teszt futtatása](azure-to-azure-walkthrough-test-failover.md)
