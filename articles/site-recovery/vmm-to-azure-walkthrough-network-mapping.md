---
title: "A VMM-felhőkben Hyper-V virtuális gépek replikálása Azure-bA az Azure Site Recovery hálózatleképezés konfigurálása |} Microsoft Docs"
description: "Hálózatleképezés konfigurálása az Azure-bA az Azure Site Recovery VMM-felhőkben Hyper-V virtuális gépek replikálása esetén"
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
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a><span data-ttu-id="a9a15-103">9. lépés: Az Azure-ba (VMM) szolgáltatással a Hyper-V replikáció hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a9a15-103">Step 9: Configure network mapping for Hyper-V replication (with VMM) to Azure</span></span>

<span data-ttu-id="a9a15-104">Miután beállította a [forrás és cél replikációs beállítások](vmm-to-azure-walkthrough-source-target.md), ez a cikk segítségével helyszíni VMM-Virtuálisgép-hálózatok és az Azure-hálózatok közötti hálózatleképezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a9a15-104">After you set up the [source and target replication settings](vmm-to-azure-walkthrough-source-target.md), use this article to configure network mapping to map between on-premises VMM VM networks, and Azure networks.</span></span>

<span data-ttu-id="a9a15-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="a9a15-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="a9a15-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a9a15-106">Before you start</span></span>

- <span data-ttu-id="a9a15-107">További tudnivalók [hálózatleképezés](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="a9a15-107">Learn about [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="a9a15-108">[Hálózatleképezés előkészítése a VMM](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span><span class="sxs-lookup"><span data-stu-id="a9a15-108">[Prepare VMM for network mapping](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span></span> 
- <span data-ttu-id="a9a15-109">Ellenőrizze, hogy a VMM-kiszolgálón futó virtuális gépek csatlakoznak-e a virtuálisgép-hálózathoz, illetve, hogy létrehozott-e legalább egy Azure virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="a9a15-109">Verify that virtual machines on the VMM server are connected to a VM network, and that you've created at least one Azure virtual network.</span></span> <span data-ttu-id="a9a15-110">Egyetlen Azure-hálózatra több virtuálisgép-hálózatot is le lehet képezni.</span><span class="sxs-lookup"><span data-stu-id="a9a15-110">Multiple VM networks can be mapped to a single Azure network.</span></span>

## <a name="configure-mapping"></a><span data-ttu-id="a9a15-111">Leképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a9a15-111">Configure mapping</span></span>

<span data-ttu-id="a9a15-112">Konfigurálja az alábbiak szerint a leképezéseket:</span><span class="sxs-lookup"><span data-stu-id="a9a15-112">Configure mapping as follows:</span></span>

1. <span data-ttu-id="a9a15-113">A **Site Recovery-infrastruktúra** > **Hálózatleképezések** > **Hálózatleképezés** menüpontban kattintson a **+Hálózatleképezés** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a9a15-113">In **Site Recovery Infrastructure** > **Network mappings** > **Network Mapping**, click the **+Network Mapping** icon.</span></span>

    ![Hálózatleképezés](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. <span data-ttu-id="a9a15-115">A **Hálózatleképezés hozzáadása** részben válassza ki a forrás VMM-kiszolgálót, célként pedig az **Azure-t**.</span><span class="sxs-lookup"><span data-stu-id="a9a15-115">In **Add network mapping**, select the source VMM server, and **Azure** as the target.</span></span>
3. <span data-ttu-id="a9a15-116">Ellenőrizze az előfizetést, illetve a feladatok átadását követően használatos üzembe helyezési modellt.</span><span class="sxs-lookup"><span data-stu-id="a9a15-116">Verify the subscription and the deployment model after failover.</span></span>
4. <span data-ttu-id="a9a15-117">A **Forráshálózat** panelen a VMM-kiszolgálóhoz társítottak listájából válassza ki azt a helyszíni virtuálisgép-hálózatot, amelyet le kíván képezni.</span><span class="sxs-lookup"><span data-stu-id="a9a15-117">In **Source network**, select the source on-premises VM network you want to map from the list associated with the VMM server.</span></span>
5. <span data-ttu-id="a9a15-118">A **Célhálózat** panelen válassza ki azt az Azure-hálózatot, amelyre a létrehozott replika Azure virtuális gépek kerülnek.</span><span class="sxs-lookup"><span data-stu-id="a9a15-118">In **Target network**, select the Azure network in which replica Azure VMs will be located when they're created.</span></span> <span data-ttu-id="a9a15-119">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9a15-119">Then click **OK**.</span></span>

    ![Hálózatleképezés](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="a9a15-121">Ez történik a hálózatleképezés megkezdésekor:</span><span class="sxs-lookup"><span data-stu-id="a9a15-121">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="a9a15-122">A forrás virtuálisgép-hálózatban található meglévő virtuális gépek a leképezés elkezdésekor csatlakoznak a célhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="a9a15-122">Existing VMs on the source VM network are connected to the target network when mapping begins.</span></span> <span data-ttu-id="a9a15-123">A forrás virtuálisgép-hálózathoz csatlakoztatott új virtuális gépek a replikálás végrehajtásakor csatlakoznak a leképezett Azure-hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="a9a15-123">New VMs connected to the source VM network are connected to the mapped Azure network when replication occurs.</span></span>
* <span data-ttu-id="a9a15-124">Meglévő hálózatleképezés módosítása esetén a replika virtuális gépek az új beállításokkal fognak csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="a9a15-124">If you modify an existing network mapping, replica virtual machines connect using the new settings.</span></span>
* <span data-ttu-id="a9a15-125">Ha a célhálózatban már több alhálózat működik, és ezek egyikének ugyanaz a neve, mint annak, amelyen a forrás virtuális gép is található, a replika virtuális gép a feladatátvételt követően ehhez a cél alhálózathoz fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="a9a15-125">If the target network has multiple subnets, and one of those subnets has the same name as subnet on which the source virtual machine is located, then the replica virtual machine connects to that target subnet after failover.</span></span>
* <span data-ttu-id="a9a15-126">Ha nem létezik egyező nevű alhálózat, a virtuális gép a hálózat első virtuális alhálózatához csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="a9a15-126">If there’s no target subnet with a matching name, the virtual machine connects to the first subnet in the network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="a9a15-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9a15-127">Next steps</span></span>

<span data-ttu-id="a9a15-128">Ugrás [10. lépés: a replikációs házirend létrehozása](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="a9a15-128">Go to [Step 10: Create a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>
