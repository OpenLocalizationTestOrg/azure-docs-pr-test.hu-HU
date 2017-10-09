---
title: "aaaMigrate Azure IaaS virtuális gépek Azure-régiók közötti |} Microsoft Docs"
description: "Azure Site Recovery toomigrate Azure IaaS virtuális gépeket egy Azure-régiót tooanother használja."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: c84dc77716b8d19969eab60707ed1332ca39b893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Az Azure Site Recovery Azure-régiók közötti Azure IaaS virtuális gépek áttelepítése
## <a name="overview"></a>Áttekintés
Üdvözli a Site Recovery tooAzure! Ez a cikk használja, ha azt szeretné, hogy toomigrate Azure virtuális gépek Azure-régiók közötti. Mielőtt elkezdené, vegye figyelembe, hogy:

* Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: Azure Resource Manager és klasszikus. Azure a két portál – hello a klasszikus Azure portálra, hello klasszikus üzembe helyezési modellt támogató, és a két üzembe helyezési modell támogató Azure-portálon hello is rendelkezik. hello alapvető lépéseit az áttelepítés: hello azonos e Site Recovery konfigurálja az erőforrás-kezelő vagy a klasszikus. Azonban a felhasználói felület utasításokat hello és képernyőképeket ebben a cikkben az Azure-portálon hello vonatkoznak.
* **Jelenleg csak áttelepíthetők egy régió tartozik tooanother. Virtuális gépek az Azure-régió, egy tooanother átveheti, de Ön visszavétel nem újra.**

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Előfeltételek
Ez mit kell ehhez a központi telepítéshez:

* **Infrastruktúra-szolgáltatási virtuális gépeket**: hello toomigrate kívánt virtuális gépeket. Telepít át virtuális gépeken való kezelésével azokat a fizikai gépként.

## <a name="deployment-steps"></a>A központi telepítés lépései
Ez a szakasz ismerteti a hello üzembe helyezés lépései hello új Azure-portálon.

1. [Hozzon létre egy tárolót](site-recovery-vmware-to-azure.md).
2. [Engedélyezze a replikálást](site-recovery-vmware-to-azure.md). Engedélyezze a hello szeretné, hogy toomigrate, és válassza ki a forrásként Azure virtuális gépek replikációját. 
3. [A nem tervezett feladatátvétel](site-recovery-failover.md). Kezdeti replikáció befejezése után egy Azure-régiót tooanother futtathatja egy nem tervezett feladatátvételt. Szükség esetén a helyreállítási terv létrehozása, és nem tervezett feladatátvétel, toomigrate több virtuális gép régiók között. [További](site-recovery-create-recovery-plans.md) helyreállítási tervek.

## <a name="next-steps"></a>Következő lépések
További információ található más fájlreplikációs forgatókönyvek [Mi az Azure Site Recovery?](site-recovery-overview.md)
