---
title: "aaaSet be egy Azure virtuális gép repliction tárolót az Azure Site Recovery régiók közötti |} Microsoft Docs"
description: "Azure Site Recovery segítségével Azure-régiók közötti Azure replikáció tooset mentése a tárolóba kell hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9959c59c7ea57114763f13bf060404ddd267ba80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-a-vault-for-azure-tooazure-replication"></a>4. lépés: Azure tooAzure replikációra tárolókba beállítása

Után [hálózatok tervezési](azure-to-azure-walkthrough-network.md), használja a cikk tooset mentése a tárolóba, az Azure virtuális gépek (VM) replikálása az Azure-régió, tooanother hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

- Hello cikk befejezése után állítsa be a Recovery Services-tárolónak kell rendelkeznie.
- Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



>[!NOTE]
>
> Az Azure virtuális gép replikációs jelenleg előzetes verzió.




## <a name="create-a-vault"></a>Tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Azt javasoljuk, hogy a hello helyre, ahol a virtuális gépek tooreplicate hozzon létre hello Recovery Services-tároló. Például ha a célhely van hello központi VELÜNK, hozzon létre hello tárolót a **USA középső RÉGIÓJA**.


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[5. lépés: replikálás engedélyezése](azure-to-azure-walkthrough-enable-replication.md)
