---
title: "Hyper-V tooAzure replikációs (a System Center VMM nélkül) Azure Site Recovery segítségével aaaReview hello előfeltételei |} Microsoft Docs"
description: "Ismerteti a replikálása, feladatátvétele és helyreállítása az Azure Site Recovery a helyszíni Hyper-V virtuális gépek tooAzure beállítása hello előfeltételei"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 3eefc3b7e3982ec6c413c1db7f7784863f9c701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-without-vmm-tooazure-replication"></a><span data-ttu-id="379ce-103">2. lépés: Hyper-V (VMM nélkül) tooAzure replikációs hello előfeltételeinek áttekintése</span><span class="sxs-lookup"><span data-stu-id="379ce-103">Step 2: Review hello prerequisites for Hyper-V (without VMM) tooAzure replication</span></span>

<span data-ttu-id="379ce-104">hello Előfeltételek hello táblázat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="379ce-104">hello prerequisites are summarized in hello table.</span></span>


<span data-ttu-id="379ce-105">**Előfeltétel**</span><span class="sxs-lookup"><span data-stu-id="379ce-105">**Prerequisite**</span></span> | <span data-ttu-id="379ce-106">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="379ce-106">**Details**</span></span> 
--- | --- 
<span data-ttu-id="379ce-107">**Azure**</span><span class="sxs-lookup"><span data-stu-id="379ce-107">**Azure**</span></span> | <span data-ttu-id="379ce-108">Az [Azure-követelmények](site-recovery-prereq.md#azure-requirements) ismertetése.</span><span class="sxs-lookup"><span data-stu-id="379ce-108">Learn about [Azure requirements](site-recovery-prereq.md#azure-requirements).</span></span>
<span data-ttu-id="379ce-109">**Helyszíni kiszolgálók**</span><span class="sxs-lookup"><span data-stu-id="379ce-109">**On-premises servers**</span></span> | <span data-ttu-id="379ce-110">[További](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) hello helyszíni Hyper-V-gazdagépek követelményeivel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="379ce-110">[Learn more](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) about requirements for hello on-premises Hyper-V hosts.</span></span>
<span data-ttu-id="379ce-111">**Helyszíni Hyper-V rendszerű virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="379ce-111">**On-premises Hyper-V VMs**</span></span> | <span data-ttu-id="379ce-112">Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="379ce-112">VMs you want tooreplicate should be running a [supported operating system](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), and conform with [Azure prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>
<span data-ttu-id="379ce-113">**Azure-beli URL-címek**</span><span class="sxs-lookup"><span data-stu-id="379ce-113">**Azure URLs**</span></span> | <span data-ttu-id="379ce-114">Hyper-V-gazdagépek toothese URL-címeket kell elérni:</span><span class="sxs-lookup"><span data-stu-id="379ce-114">Hyper-V hosts need access toothese URLs:</span></span><br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> <span data-ttu-id="379ce-115">Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="379ce-115">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span><br/></br> <span data-ttu-id="379ce-116">Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.</span><span class="sxs-lookup"><span data-stu-id="379ce-116">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span><br/></br> <span data-ttu-id="379ce-117">Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).</span><span class="sxs-lookup"><span data-stu-id="379ce-117">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>



## <a name="next-steps"></a><span data-ttu-id="379ce-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="379ce-118">Next steps</span></span>

- <span data-ttu-id="379ce-119">Ha teljes körű telepítésére végzett, nyissa meg túl[3. lépés: kapacitástervezés](hyper-v-site-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="379ce-119">If you're doing a full deployment, go too[Step 3: Plan capacity](hyper-v-site-walkthrough-capacity.md)</span></span>
- <span data-ttu-id="379ce-120">Ha egy egyszerű próbatelepítés végzett, nyissa meg túl[4. lépés: hálózati terv](hyper-v-site-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="379ce-120">If you're doing a simple test deployment, go too[Step 4: Plan networking](hyper-v-site-walkthrough-network.md).</span></span>
