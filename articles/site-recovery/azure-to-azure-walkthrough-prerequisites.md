---
title: "Azure virtuális gépek tooanother régió replikáció megkezdése aaaBefore |} Microsoft Docs"
description: "Azure virtuális gépek replikálása Azure-régiók hello Azure Site Recovery szolgáltatással közötti előtt kell tootake hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 7fa633075eeb52d0c184a1dd8d53ccc15644f518
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-before-you-start"></a>2. lépés: Mielőtt megkezdése

Után is tekintse át a hello [architektúra](azure-to-azure-walkthrough-architecture.md) az Azure virtuális gépek (VM) replikálása Azure-régiók közötti [Azure Site Recovery](site-recovery-overview.md), ez a cikk tooverify Előfeltételek használja. 

- Hello cikk befejezése után kell egyértelműen mire van szükség a toomake hello telepítési megismerni működnek, és elvégezte hello előfeltételként szükséges lépéseket.
- Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>
> Az Azure virtuális gép replikációs jelenleg előzetes verzió.



## <a name="support-recommendations"></a>Támogatja a javaslatok

Tekintse át az alábbi hello táblázat.

**Összetevő** | **Követelmény**
--- | ---
**Recovery Services-tároló** | Azt javasoljuk, hogy hozzon létre egy Recovery Services-tároló hello cél Azure-régiót, amelyet az toouse vész-helyreállítási. Például ha azt szeretné, hogy tooreplicate forrás virtuális gépeknek az USA keleti régiója tooCentral USA, hozzon létre hello tárolót Államok középső.
**Azure-előfizetés** | Az Azure-előfizetéssel kell engedélyezett toocreate virtuális gépeket, amelyet az hello vész-helyreállítási régió szerint toouse hello célhelyet. Forduljon a támogatási szolgálathoz tooenable hello szükséges kvótát.
**Cél régió kapacitás** | Hello cél Azure-régió hello előfizetés elegendő kapacitása a virtuális gépek, a storage-fiókok és a hálózati összetevők nem rendelkezhet.
**Tárolás** | Használjon hello [tárolási útmutatásokkal](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) a forrás Azure virtuális gépeken, tooavoid teljesítményproblémákat.<br/><br/> Storage-fiókok hello kell lennie és hello-tárolónak ugyanabban a régióban.<br/><br/> Közép-és Dél-Indiában toopremium fiókok nem replikálja.<br/><br/> Ha replikációs hello alapértelmezett beállításokkal, a Site Recovery hello forrás konfigurációtól függően szükséges hello tárfiókok hoz létre. Ha beállítások testreszabásához kövesse hello [méretezhetőségi célok méretű lemezek](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Hálózat** | Kimenő kapcsolatok tooallow Azure virtuális gépek, egyedi URL-címek vagy IP-címtartományok van szükség.<br/><br/> Hálózati fiókoknak kell lenniük a hello és hello-tárolónak ugyanabban a régióban. 
**Azure virtuális gép** | Ellenőrizze, hogy hello legújabb legfelső szintű tanúsítványok mind a hello windowsos/Linuxos Azure virtuális Gépen. Ha még nem, biztonsági korlátozások miatt nem fog tudni tooregister hello a Site Recovery szolgáltatásban, a virtuális gép.
**Az Azure felhasználói fiók** | Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable Azure virtuális gép replikációját.

Támogatási követelmények teljes listájának lekérése a hello [támogatási mátrix](site-recovery-support-matrix-azure-to-azure.md)


## <a name="set-permissions-on-hello-account"></a>Hello fiók engedélyeinek beállítása

1. További információ a hello [engedélyek](site-recovery-role-based-linked-access-control.md) replikációhoz van szüksége.
2. Kövesse az alábbi [utasításokat](../active-directory/role-based-access-control-configure.md#add-access) tooadd engedélyek.


## <a name="verify-target-resources"></a>Ellenőrizze a tároló erőforrásait

1. Engedélyezze az Azure-előfizetés tooallow toocreate virtuális gépeinek hello célozhat meg, amelyet az toouse hello vész-helyreállítási régió szerint vész-helyreállítási toouse kívánt régiót. Forduljon a támogatási szolgálathoz tooenable hello szükséges kvótát.
2. Ellenőrizze, hogy elegendő erőforrások tooenable virtuális gépek méretét, amelyek megfelelnek a forrás virtuális gépeknek az előfizetéséhez. Alapértelmezés szerint amikor a replikáció, a Site Recovery beállítása kivételezések hello hello azonos mérete célként a virtuális gép vagy hello legközelebbi lehetséges méretét. További információ [hibaelhárítási](site-recovery-azure-to-azure-troubleshoot-errors.md#azure-resource-quota-issues-error-code-150097) céloz erőforrásokat.

## <a name="verify-azure-vm-certificates"></a>Azure virtuális gép tanúsítványok ellenőrzése

1. Ellenőrizze, hogy minden hello legújabb legfelső szintű tanúsítványok hello Windows vagy Linux virtuális gépek tooreplicate szeretné. Hello legújabb legfelső szintű tanúsítványok nem szerepelnek, ha a hello VM regisztrált tooSite helyreállítási toosecurity megkötések miatt nem lehet.
2. Az hello Windows virtuális gépek, a következő:

    - Hello legújabb Windows frissítések telepítése a virtuális gép hello, amelyek minden hello megbízható legfelső szintű tanúsítványok hello gépen.
    - A leválasztott környezetben hello szabványos Windows Update folyamat-tanúsítványhoz frissítési eljárást követve a szervezet, a tooget hello legújabb legfelső szintű tanúsítványok és a frissített tanúsítvány-visszavonási listát, hello virtuális gépeken.
3. Linux virtuális gépekhez követve hello a Linux forgalmazó tooget hello legújabb megbízható legfelső szintű tanúsítványok és a hello legújabb tanúsítvány-visszavonási listát a virtuális gép hello. További információ [hibaelhárítási](site-recovery-azure-to-azure-troubleshoot-errors.md#trusted-root-certificates-error-code-151066) megbízható legfelső szintű problémákat.


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[3. lépés: hálózat tervezése](azure-to-azure-walkthrough-network.md) tooset kimenő kapcsolat mentése.
