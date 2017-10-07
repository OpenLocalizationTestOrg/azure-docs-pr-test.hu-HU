---
title: "aaaConfigure hálózatra való leképezést a VMM-ben a Hyper-V virtuális gépek replikálása az Azure Site Recovery tooAzure felhők |} Microsoft Docs"
description: "Ismerteti, hogyan tooconfigure hálózatra való leképezést a VMM-ben a Hyper-V virtuális gépek replikálása esetén felhők tooAzure az Azure Site Recovery szolgáltatással"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 081a9fdb0ffa4114099e9bcb9c1b1e43ad26ecbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-tooazure"></a><span data-ttu-id="99fd1-103">9. lépés: A Hyper-V-replikációt (VMM) tooAzure hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="99fd1-103">Step 9: Configure network mapping for Hyper-V replication (with VMM) tooAzure</span></span>

<span data-ttu-id="99fd1-104">Hello beállítása után [forrás és cél replikációs beállítások](vmm-to-azure-walkthrough-source-target.md), használja a cikk tooconfigure hálózati leképezési toomap helyszíni VMM-Virtuálisgép-hálózatok és az Azure-hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="99fd1-104">After you set up hello [source and target replication settings](vmm-to-azure-walkthrough-source-target.md), use this article tooconfigure network mapping toomap between on-premises VMM VM networks, and Azure networks.</span></span>

<span data-ttu-id="99fd1-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="99fd1-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="99fd1-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="99fd1-106">Before you start</span></span>

- <span data-ttu-id="99fd1-107">További tudnivalók [hálózatleképezés](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="99fd1-107">Learn about [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="99fd1-108">[Hálózatleképezés előkészítése a VMM](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span><span class="sxs-lookup"><span data-stu-id="99fd1-108">[Prepare VMM for network mapping](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span></span> 
- <span data-ttu-id="99fd1-109">Győződjön meg arról, hogy virtuális gépek a VMM-kiszolgálón hello csatlakoztatott tooa Virtuálisgép-hálózatot, és, hogy létrehozta-e legalább egy Azure virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="99fd1-109">Verify that virtual machines on hello VMM server are connected tooa VM network, and that you've created at least one Azure virtual network.</span></span> <span data-ttu-id="99fd1-110">Több Virtuálisgép-hálózatot csatlakoztatott tooa egyetlen Azure-hálózat lehet.</span><span class="sxs-lookup"><span data-stu-id="99fd1-110">Multiple VM networks can be mapped tooa single Azure network.</span></span>

## <a name="configure-mapping"></a><span data-ttu-id="99fd1-111">Leképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="99fd1-111">Configure mapping</span></span>

<span data-ttu-id="99fd1-112">Konfigurálja az alábbiak szerint a leképezéseket:</span><span class="sxs-lookup"><span data-stu-id="99fd1-112">Configure mapping as follows:</span></span>

1. <span data-ttu-id="99fd1-113">A **Site Recovery-infrastruktúra** > **Hálózatleképezések** > **Hálózatleképezés**, kattintson a hello **+ Hálózatleképezés**  ikonra.</span><span class="sxs-lookup"><span data-stu-id="99fd1-113">In **Site Recovery Infrastructure** > **Network mappings** > **Network Mapping**, click hello **+Network Mapping** icon.</span></span>

    ![Hálózatleképezés](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. <span data-ttu-id="99fd1-115">A **hálózatleképezés hozzáadása**, jelölje be hello forrás VMM-kiszolgálón, és **Azure** hello célként.</span><span class="sxs-lookup"><span data-stu-id="99fd1-115">In **Add network mapping**, select hello source VMM server, and **Azure** as hello target.</span></span>
3. <span data-ttu-id="99fd1-116">Ellenőrizze a hello előfizetés és hello telepítési modell a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="99fd1-116">Verify hello subscription and hello deployment model after failover.</span></span>
4. <span data-ttu-id="99fd1-117">A **Forráshálózat**, jelölje be hello forrás helyszíni Virtuálisgép-hálózat kívánt toomap hello VMM-kiszolgálóhoz társított hello listából.</span><span class="sxs-lookup"><span data-stu-id="99fd1-117">In **Source network**, select hello source on-premises VM network you want toomap from hello list associated with hello VMM server.</span></span>
5. <span data-ttu-id="99fd1-118">A **célhálózat**, válassza ki hello Azure hálózati melyik replika Azure virtuális gépeken található során jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="99fd1-118">In **Target network**, select hello Azure network in which replica Azure VMs will be located when they're created.</span></span> <span data-ttu-id="99fd1-119">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="99fd1-119">Then click **OK**.</span></span>

    ![Hálózatleképezés](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="99fd1-121">Ez történik a hálózatleképezés megkezdésekor:</span><span class="sxs-lookup"><span data-stu-id="99fd1-121">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="99fd1-122">Hello forrás Virtuálisgép-hálózat meglévő virtuális gépekhez csatlakoztatott toohello célhálózat is, ha a leképezés kezdődik.</span><span class="sxs-lookup"><span data-stu-id="99fd1-122">Existing VMs on hello source VM network are connected toohello target network when mapping begins.</span></span> <span data-ttu-id="99fd1-123">Új virtuális gépek csatlakoztatott toohello forrás Virtuálisgép-hálózathoz kapcsolódó replikáláskor toohello leképezve az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="99fd1-123">New VMs connected toohello source VM network are connected toohello mapped Azure network when replication occurs.</span></span>
* <span data-ttu-id="99fd1-124">Meglévő hálózatleképezés módosítása esetén a replika virtuális gépek új hello-beállítások használatával csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="99fd1-124">If you modify an existing network mapping, replica virtual machines connect using hello new settings.</span></span>
* <span data-ttu-id="99fd1-125">Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép a feladatátvételt követően toothat cél alhálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="99fd1-125">If hello target network has multiple subnets, and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine connects toothat target subnet after failover.</span></span>
* <span data-ttu-id="99fd1-126">Ha nincsenek egyező nevű cél alhálózathoz, hello virtuálisgép kapcsolódik toohello hello hálózat első alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="99fd1-126">If there’s no target subnet with a matching name, hello virtual machine connects toohello first subnet in hello network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="99fd1-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99fd1-127">Next steps</span></span>

<span data-ttu-id="99fd1-128">Nyissa meg túl[10. lépés: a replikációs házirend létrehozása](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="99fd1-128">Go too[Step 10: Create a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>
