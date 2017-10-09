---
title: "Cél (VMware tooAzure) előkészítése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooprepare a VMware virtuális gépek tooAzure replikálása Azure-alapú környezetben toostart."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: 5975d3c122032f92f8df370ee74fa0c7012ebe2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-target-vmware-tooazure"></a>Cél (VMware tooAzure) előkészítése
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Fizikai tooAzure](./site-recovery-prepare-target-physical-to-azure.md)

Ez a cikk ismerteti, hogyan tooprepare a VMware virtuális gépek tooAzure replikálása Azure-alapú környezetben toostart.

## <a name="prerequisites"></a>Előfeltételek

hello cikk feltételezi hello következő:
- A Recovery Services-tároló tooprotect a VMware virtuális gépek hozott létre. A Recovery Services-tároló hello létrehozhat [Azure-portálon](http://portal.azure.com "Azure-portálon").
- Rendelkezik [a helyszíni környezet beállítása](./site-recovery-set-up-vmware-to-azure.md) tooreplicate VMware virtuális gépek tooAzure.

## <a name="prepare-target"></a>Készítse elő a cél

Hello befejezése után **lépés 1:Select védelmi cél** és **2. lépés: Felkészülés forrás**, a következő lépés az túl**3. lépés: cél**

![Készítse elő a cél](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. **Előfizetés:** hello a legördülő menüben válassza hello tooreplicate kívánt előfizetést a virtuális gépek.
2. **Telepítési modell:** válassza hello telepítési modell (klasszikus és Resource Manager)

Központi telepítési modellt hello alapján, egy ellenőrzése, hogy rendelkezik legalább egy kompatibilis tárfiók és a virtuális hálózat hello célként megadott előfizetés tooreplicate és a feladatátvételt a virtuális gép tooensure van meg.

Ha hello érvényesítések sikeres, kattintson az OK toogo toohello következő lépésre.

Ha nem rendelkezik a kompatibilis erőforrás-kezelő storage-fiók vagy a virtuális hálózaton, vagy további tooadd szeretné, akkor megteheti hello kattintva **+ Tárfiók** vagy **+ hálózat** hello hello tetején levő panelen.

## <a name="next-steps"></a>Következő lépések
[Replikáció beállításainak konfigurálása](./site-recovery-setup-replication-settings-vmware.md).
