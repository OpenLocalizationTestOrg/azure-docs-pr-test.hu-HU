---
title: "Cél (fizikai tooAzure) előkészítése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooprepare a Windows vagy Linux tooAzure futtató fizikai kiszolgálók replikálása az Azure környezetben toostart."
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
ms.openlocfilehash: 126fb86133e1a00f5669410943565c4cd78e4369
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-target-vmware-tooazure"></a><span data-ttu-id="9d075-103">Cél (VMware tooAzure) előkészítése</span><span class="sxs-lookup"><span data-stu-id="9d075-103">Prepare target (VMware tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9d075-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="9d075-104">VMware tooAzure</span></span>](./site-recovery-prepare-target-vmware-to-azure.md)
> * [<span data-ttu-id="9d075-105">Fizikai tooAzure</span><span class="sxs-lookup"><span data-stu-id="9d075-105">Physical tooAzure</span></span>](./site-recovery-prepare-target-physical-to-azure.md)

<span data-ttu-id="9d075-106">Ez a cikk ismerteti, hogyan tooprepare a Windows vagy Linux rendszerű Azure (x 64) fizikai kiszolgálók replikálása az Azure környezetben toostart.</span><span class="sxs-lookup"><span data-stu-id="9d075-106">This article describes how tooprepare your Azure environment toostart replicating physical servers (x64) running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d075-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9d075-107">Prerequisites</span></span>

<span data-ttu-id="9d075-108">hello cikk feltételezi hello következő:</span><span class="sxs-lookup"><span data-stu-id="9d075-108">hello article assumes hello following:</span></span>
- <span data-ttu-id="9d075-109">A Recovery Services-tároló tooprotect a fizikai kiszolgálók hozott létre.</span><span class="sxs-lookup"><span data-stu-id="9d075-109">You have created a Recovery Services Vault tooprotect your physical servers.</span></span> <span data-ttu-id="9d075-110">A Recovery Services-tároló hello létrehozhat [Azure-portálon](http://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="9d075-110">You can create a Recovery Services Vault from hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="9d075-111">Rendelkezik [a helyszíni környezet beállítása](./site-recovery-set-up-physical-to-azure.md) tooreplicate fizikai kiszolgálók tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9d075-111">You have [setup your on-premises environment](./site-recovery-set-up-physical-to-azure.md) tooreplicate physical servers tooAzure.</span></span>

## <a name="prepare-target"></a><span data-ttu-id="9d075-112">Készítse elő a cél</span><span class="sxs-lookup"><span data-stu-id="9d075-112">Prepare target</span></span>

<span data-ttu-id="9d075-113">Hello befejezése után **lépés 1:Select védelmi cél** és **2. lépés: Felkészülés forrás**, a következő lépés az túl**3. lépés: cél**</span><span class="sxs-lookup"><span data-stu-id="9d075-113">After completing hello **Step 1:Select Protection goal** and **Step 2:Prepare Source**, you are taken too**Step 3: Target**</span></span>

![Készítse elő a cél](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. <span data-ttu-id="9d075-115">**Előfizetés:** hello a legördülő menüben válassza hello tooreplicate kívánt előfizetést a fizikai kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="9d075-115">**Subscription:** From hello drop down menu, select hello Subscription that you want tooreplicate your physical servers to.</span></span>
2. <span data-ttu-id="9d075-116">**Telepítési modell:** válassza hello telepítési modell (klasszikus és Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="9d075-116">**Deployment Model:** Select hello deployment model (Classic or Resource Manager)</span></span>

<span data-ttu-id="9d075-117">A központi telepítési modellt hello alapján, egy ellenőrzése, hogy rendelkezik legalább egy kompatibilis tárfiók és a virtuális hálózat hello célként megadott előfizetés tooreplicate és feladatátvételi a fizikai kiszolgálók tooensure van meg.</span><span class="sxs-lookup"><span data-stu-id="9d075-117">Based on hello chosen deployment model, a validation is run tooensure that you have at least one compatible storage account and virtual network in hello target subscription tooreplicate and failover your physical servers to.</span></span>

<span data-ttu-id="9d075-118">Ha hello érvényesítések sikeres, kattintson az OK toogo toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="9d075-118">Once hello validations complete successfully, click OK toogo toohello next step.</span></span>

<span data-ttu-id="9d075-119">Ha nem rendelkezik a kompatibilis erőforrás-kezelő storage-fiók vagy a virtuális hálózaton, vagy további tooadd szeretné, akkor megteheti hello kattintva **+ Tárfiók** vagy **+ hálózat** hello hello tetején levő panelen.</span><span class="sxs-lookup"><span data-stu-id="9d075-119">If you don't have a compatible Resource Manager storage account or virtual network, or would like tooadd more, you can do so by clicking hello **+ Storage Account** or **+ Network** buttons on hello top of hello blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d075-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d075-120">Next steps</span></span>
<span data-ttu-id="9d075-121">[Replikáció beállításainak konfigurálása](./site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="9d075-121">[Configure replication settings](./site-recovery-setup-replication-settings-vmware.md).</span></span>
