---
title: "a Hyper-V virtuális gép replikációs tooa másodlagos hely az Azure Site Recovery aaaMap hálózatok |} Microsoft Docs"
description: "Ismerteti, hogyan toomap hálózatok Hyper-V virtuális gépek tooa VMM másodlagos hely az Azure Site Recovery replikálása esetén."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: d4f621df4ce08ae055bc6809daea0b71b76754ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-tooa-secondary-site"></a><span data-ttu-id="686a7-103">7. lépés: Hyper-V virtuális gép replikációs tooa másodlagos hely hálózatok leképezése</span><span class="sxs-lookup"><span data-stu-id="686a7-103">Step 7: Map networks for Hyper-V VM replication tooa secondary site</span></span>


<span data-ttu-id="686a7-104">Beállítása után [forrás és cél beállításai](vmm-to-vmm-walkthrough-source-target.md) replikálásához a Hyper-V virtuális gépek (VM) tooa másodlagos System Center Virtual Machine Manager (VMM) helyet, ez a cikk tooconfigure hálózatra való leképezést a Hyper-V virtuális használata gép (VM) replikációs tooa másodlagos hely használatával [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="686a7-104">After setting up [source and target settings](vmm-to-vmm-walkthrough-source-target.md) for replicating Hyper-V virtual machines (VMs) tooa secondary System Center Virtual Machine Manager (VMM) site, use this article tooconfigure network mapping for Hyper-V virtual machine (VM) replication tooa secondary site, using  [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="686a7-105">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="686a7-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="686a7-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="686a7-106">Before you start</span></span>

- <span data-ttu-id="686a7-107">További tudnivalók [hálózatleképezés](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="686a7-107">Learn about [network mapping](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) before you start.</span></span>
- <span data-ttu-id="686a7-108">Ellenőrizze, hogy a virtuális gépek VMM-kiszolgálókon csatlakoztatott tooa Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="686a7-108">Verify that virtual machines on VMM servers are connected tooa VM network.</span></span>

## <a name="configure-network-mapping"></a><span data-ttu-id="686a7-109">Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="686a7-109">Configure network mapping</span></span>

1. <span data-ttu-id="686a7-110">A **Hálózatleképezés** > **Hálózatleképezések**, kattintson a **+ Hálózatleképezés**.</span><span class="sxs-lookup"><span data-stu-id="686a7-110">In **Network Mapping** > **Network mappings**, click **+Network Mapping**.</span></span>
2. <span data-ttu-id="686a7-111">A **hálózatleképezés hozzáadása** lapon válassza ki a hello forrás és cél VMM-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="686a7-111">In **Add network mapping** tab, select hello source and target VMM servers.</span></span> <span data-ttu-id="686a7-112">VMM-kiszolgálókon hello társított Virtuálisgép-hálózatok hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="686a7-112">hello VM networks associated with hello VMM servers are retrieved.</span></span>
3. <span data-ttu-id="686a7-113">A **Forráshálózat**, jelölje be azt szeretné, hogy a VMM-kiszolgáló hello társított Virtuálisgép-hálózatok listája hello toouse hello hálózati.</span><span class="sxs-lookup"><span data-stu-id="686a7-113">In **Source network**, select hello network you want toouse from hello list of VM networks associated with hello primary VMM server.</span></span>
4. <span data-ttu-id="686a7-114">A **célhálózat**, jelölje be azt szeretné, hogy a VMM-kiszolgálón hello másodlagos toouse hello hálózati.</span><span class="sxs-lookup"><span data-stu-id="686a7-114">In **Target network**, select hello network you want toouse on hello secondary VMM server.</span></span> <span data-ttu-id="686a7-115">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="686a7-115">Then click **OK**.</span></span>

    ![Hálózatleképezés](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="686a7-117">Ez történik a hálózatleképezés megkezdésekor:</span><span class="sxs-lookup"><span data-stu-id="686a7-117">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="686a7-118">Minden meglévő replika virtuális gépeket, amelyek megfelelnek a toohello forrás Virtuálisgép-hálózathoz csatlakoztatott toohello cél Virtuálisgép-hálózat lesz.</span><span class="sxs-lookup"><span data-stu-id="686a7-118">Any existing replica virtual machines that correspond toohello source VM network will be connected toohello target VM network.</span></span>
* <span data-ttu-id="686a7-119">Új virtuális gépek, amelyek a forrás Virtuálisgép-hálózathoz csatlakoztatott toohello replikáció után lesz csatlakoztatott toohello cél csatlakoztatott hálózati.</span><span class="sxs-lookup"><span data-stu-id="686a7-119">New virtual machines that are connected toohello source VM network will be connected toohello target mapped network after replication.</span></span>
* <span data-ttu-id="686a7-120">Ha módosít egy meglévő hozzárendelést egy új hálózati, a replika virtuális gépek csatlakoznak hello új beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="686a7-120">If you modify an existing mapping with a new network, replica virtual machines will be connected using hello new settings.</span></span>
* <span data-ttu-id="686a7-121">Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="686a7-121">If hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after failover.</span></span> <span data-ttu-id="686a7-122">Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="686a7-122">If there’s no target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="686a7-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="686a7-123">Next steps</span></span>

<span data-ttu-id="686a7-124">Nyissa meg túl[8. lépés: a replikációs házirend konfigurálása](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="686a7-124">Go too[Step 8: Configure a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>
