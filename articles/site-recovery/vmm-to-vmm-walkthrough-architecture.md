---
title: "a Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery aaaReview hello architektúra |} Microsoft Docs"
description: "Ez a cikk áttekintést hello architektúra a helyszíni Hyper-V virtuális gépek tooa System Center VMM rendelkező másodlagos helyről Azure Site Recovery replikálására."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 0de4b4e8601116c73e6fd710597ce4e561884368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="da201-103">1. lépés:, Tekintse át a Hyper-V replikáció tooa másodlagos hely hello architektúrája</span><span class="sxs-lookup"><span data-stu-id="da201-103">Step 1: Review hello architecture for Hyper-V replication tooa secondary site</span></span>

<span data-ttu-id="da201-104">Ez a cikk ismerteti a hello összetevők és a folyamatok replikálása esetén a helyszíni Hyper-V virtuális gépek (VM) a System Center Virtual Machine Manager (VMM) felhők, tooa VMM másodlagos hely hello segítségével [Azure Site Recovery](site-recovery-overview.md)szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="da201-104">This article describes hello components and processes involved when replicating on-premises Hyper-V virtual machines (VMs) in System Center Virtual Machine Manager (VMM) clouds, tooa secondary VMM site using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="da201-105">Ez a cikk vagy hello hello alsó megjegyzések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="da201-105">Post any comments at hello bottom of this article, or in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="architectural-components"></a><span data-ttu-id="da201-106">Az architektúra összetevői</span><span class="sxs-lookup"><span data-stu-id="da201-106">Architectural components</span></span>

<span data-ttu-id="da201-107">Ez a Hyper-V virtuális gépek tooa VMM másodlagos hely replikálására kell.</span><span class="sxs-lookup"><span data-stu-id="da201-107">Here's what you need for replicating Hyper-V VMs tooa secondary VMM site.</span></span>

<span data-ttu-id="da201-108">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="da201-108">**Component**</span></span> | <span data-ttu-id="da201-109">**Hely**</span><span class="sxs-lookup"><span data-stu-id="da201-109">**Location**</span></span> | <span data-ttu-id="da201-110">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="da201-110">**Details**</span></span>
--- | --- | ---
<span data-ttu-id="da201-111">**Azure**</span><span class="sxs-lookup"><span data-stu-id="da201-111">**Azure**</span></span> | <span data-ttu-id="da201-112">Előfizetés az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="da201-112">Subscription in Azure.</span></span> | <span data-ttu-id="da201-113">Hello Azure-előfizetésre, tooorchestrate a Recovery Services-tároló létrehozása és kezelése a VMM-helyek közötti replikációval.</span><span class="sxs-lookup"><span data-stu-id="da201-113">You create a Recovery Services vault in hello Azure subscription, tooorchestrate and manage replication between VMM locations.</span></span>
<span data-ttu-id="da201-114">**VMM-kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="da201-114">**VMM server**</span></span> | <span data-ttu-id="da201-115">Szüksége lesz egy elsődleges és egy másodlagos VMM-helyre.</span><span class="sxs-lookup"><span data-stu-id="da201-115">You need a VMM primary and secondary location.</span></span> | <span data-ttu-id="da201-116">Azt javasoljuk, hogy a VMM-kiszolgáló hello elsődleges hely és egy-egy hello másodlagos helyen</span><span class="sxs-lookup"><span data-stu-id="da201-116">We recommend a VMM server in hello primary site, and one in hello secondary site</span></span> 
<span data-ttu-id="da201-117">**Hyper-V kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="da201-117">**Hyper-V server**</span></span> |  <span data-ttu-id="da201-118">Egy vagy több Hyper-V gazdakiszolgálók hello elsődleges és másodlagos VMM-felhőkben.</span><span class="sxs-lookup"><span data-stu-id="da201-118">One or more Hyper-V host servers in hello primary and secondary VMM clouds.</span></span> | <span data-ttu-id="da201-119">Adatait hello elsődleges és másodlagos Hyper-V gazdakiszolgálók közötti hello LAN-vagy VPN-és a Kerberos- vagy Tanúsítványalapú hitelesítés használatával replikálja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="da201-119">Data is replicated between hello primary and secondary Hyper-V host servers over hello LAN or VPN, using Kerberos or certificate authentication.</span></span>  
<span data-ttu-id="da201-120">**Hyper-V virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="da201-120">**Hyper-V VMs**</span></span> | <span data-ttu-id="da201-121">A Hyper-V gazdakiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="da201-121">On Hyper-V host server.</span></span> | <span data-ttu-id="da201-122">hello forrás gazdagép-kiszolgálón legalább egy virtuális gép, amelyet az tooreplicate kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="da201-122">hello source host server should have at least one VM that you want tooreplicate.</span></span>

## <a name="replication-process"></a><span data-ttu-id="da201-123">Replikációs folyamat</span><span class="sxs-lookup"><span data-stu-id="da201-123">Replication process</span></span>

1. <span data-ttu-id="da201-124">Hello Azure-fiók beállítása, a Recovery Services-tároló létrehozása, és határozza meg, hogy tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="da201-124">You set up hello Azure account, create a Recovery Services vault, and specify what you want tooreplicate.</span></span>
2. <span data-ttu-id="da201-125">Beállítások hello forrás és cél replikációs, beleértve a hello Azure Site Recovery Provider telepítése a VMM-kiszolgálókon és hello Microsoft Azure Recovery Services Agent ügynököt minden Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="da201-125">You configure hello source and target replication settings, which includes installing hello Azure Site Recovery Provider on VMM servers, and hello Microsoft Azure Recovery Services agent on each Hyper-V host.</span></span>
3. <span data-ttu-id="da201-126">Hello forrás VMM-felhő replikációs házirend létrehozása.</span><span class="sxs-lookup"><span data-stu-id="da201-126">You create a replication policy for hello source VMM cloud.</span></span> <span data-ttu-id="da201-127">hello házirend alkalmazott tooall hello felhőben állomáson található virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="da201-127">hello policy is applied tooall VMs located on hosts in hello cloud.</span></span>
4. <span data-ttu-id="da201-128">Minden VMM-replikáció engedélyezi, és a virtuális gépek kezdeti replikálást, a kiválasztott hello beállításokat megfelelően.</span><span class="sxs-lookup"><span data-stu-id="da201-128">You enable replication for each VMM, and initial replication of a VM occurs in accordance with hello settings you choose.</span></span>
5. <span data-ttu-id="da201-129">A kezdeti replikáció után megkezdődik a változáskülönbözetek replikációja.</span><span class="sxs-lookup"><span data-stu-id="da201-129">After initial replication, replication of delta changes begins.</span></span> <span data-ttu-id="da201-130">Az elemek nyomon követett módosításait a rendszer .hrl fájlokban tárolja.</span><span class="sxs-lookup"><span data-stu-id="da201-130">Tracked changes for an item are held in a .hrl file.</span></span>


![A helyszíni tooon helyszíni](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a><span data-ttu-id="da201-132">Feladatátvételi és feladat-visszavételi folyamat</span><span class="sxs-lookup"><span data-stu-id="da201-132">Failover and failback process</span></span>

1. <span data-ttu-id="da201-133">Futtathat tervezett vagy nem tervezett [feladatátvételt](site-recovery-failover.md) a helyszíni helyek között.</span><span class="sxs-lookup"><span data-stu-id="da201-133">You can run a planned or unplanned [failover](site-recovery-failover.md) between on-premises sites.</span></span> <span data-ttu-id="da201-134">Ha a tervezett feladatátvétel végrehajtása, majd a forrás virtuális gépeket állítsa le az tooensure adatvesztés nélküli.</span><span class="sxs-lookup"><span data-stu-id="da201-134">If you run a planned failover, then source VMs are shut down tooensure no data loss.</span></span>
2. <span data-ttu-id="da201-135">Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md) tooorchestrate több gép feladatátvétele.</span><span class="sxs-lookup"><span data-stu-id="da201-135">You can fail over a single machine, or create [recovery plans](site-recovery-create-recovery-plans.md) tooorchestrate failover of multiple machines.</span></span>
4. <span data-ttu-id="da201-136">Ha egy nem tervezett feladatátvétel tooa másodlagos hely hello feladatátvevő gépekhez hello másodlagos helyen nincsenek engedélyezve a védelem vagy replikáció után hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="da201-136">If you perform an unplanned failover tooa secondary site, after hello failover machines in hello secondary location aren't enabled for protection or replication.</span></span> <span data-ttu-id="da201-137">Ha egy tervezett feladatátvételt hello feladatátvétel után már futott, védett gépek hello másodlagos helyen.</span><span class="sxs-lookup"><span data-stu-id="da201-137">If you ran a planned failover, after hello failover, machines in hello secondary location are protected.</span></span>
5. <span data-ttu-id="da201-138">Ezt követően véglegesítse a hello feladatátvételi toostart elérése során hello munkaterhelés VM hello replikából.</span><span class="sxs-lookup"><span data-stu-id="da201-138">Then, you commit hello failover toostart accessing hello workload from hello replica VM.</span></span>
6. <span data-ttu-id="da201-139">Az elsődleges hely újra nem érhető el, akkor hello másodlagos hely toohello elsődleges a visszirányú replikálás tooreplicate kezdeményezni.</span><span class="sxs-lookup"><span data-stu-id="da201-139">When your primary site is available again, you initiate reverse replication tooreplicate from hello secondary site toohello primary.</span></span> <span data-ttu-id="da201-140">Visszirányú replikálás során hello virtuális gépek kerülnek egy védett állapotban, de hello másodlagos adatközpontba még mindig aktív hely hello.</span><span class="sxs-lookup"><span data-stu-id="da201-140">Reverse replication brings hello virtual machines into a protected state, but hello secondary datacenter is still hello active location.</span></span>
7. <span data-ttu-id="da201-141">toomake hello elsődleges hely aktív helyre hello újra, elindít egy tervezett feladatátvételt a másodlagos tooprimary, egy másik visszirányú replikálás követ.</span><span class="sxs-lookup"><span data-stu-id="da201-141">toomake hello primary site into hello active location again, you initiate a planned failover from secondary tooprimary, followed by another reverse replication.</span></span>



## <a name="next-steps"></a><span data-ttu-id="da201-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da201-142">Next steps</span></span>

<span data-ttu-id="da201-143">Nyissa meg túl[2. lépés: tekintse át a hello Előfeltételek és korlátozások](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="da201-143">Go too[Step 2: Review hello prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>
