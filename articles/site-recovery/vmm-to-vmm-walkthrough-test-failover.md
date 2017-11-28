---
title: "a Hyper-V virtuális gép replikációs tooa másodlagos hely az Azure Site Recovery feladatátvételi teszt aaaRun |} Microsoft Docs"
description: "Ismerteti, hogyan toorun Hyper-V virtuális gép replikációs tooa feladatátvételi tesztet másodlagos System Center VMM helyet az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: a60411541c2db001c573735bcb0d651f6618f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="083a5-103">10. lépés: Hyper-V replikáció tooa másodlagos hely feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="083a5-103">Step 10: Run a test failover for Hyper-V replication tooa secondary site</span></span>


<span data-ttu-id="083a5-104">Miután engedélyezte a Hyper-V virtuális gépek (VM) replikációs rendelkező [Azure Site Recovery](site-recovery-overview.md), ez a cikk toorun feladatátvételi teszt használja.</span><span class="sxs-lookup"><span data-stu-id="083a5-104">After you enable replication for Hyper-V virtual machines (VMs) with [Azure Site Recovery](site-recovery-overview.md), use this article toorun a test failover.</span></span> <span data-ttu-id="083a5-105">Feladatátvételi teszt ellenőrzi, hogy a replikáció működik, az éles környezetben befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="083a5-105">A test failover verifies that replication is working, without impacting your production environment.</span></span> 


<span data-ttu-id="083a5-106">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="083a5-106">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="083a5-107">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="083a5-107">Before you start</span></span>

- <span data-ttu-id="083a5-108">Amikor egy feladatátvételi teszt folyamatban váltanak, megadhatja a hello hálózati toowhich hello tesztreplikaként működő virtuális gépek csatlakozni fognak.</span><span class="sxs-lookup"><span data-stu-id="083a5-108">When you're triggering a test failover, you can specify hello network toowhich hello test replica VMs will connect.</span></span> <span data-ttu-id="083a5-109">[További](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) hello hálózati beállításokkal kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="083a5-109">[Learn more](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) about hello network options.</span></span>
- <span data-ttu-id="083a5-110">Azt javasoljuk, hogy egy hálózati hálózatra való leképezés során kiválasztott nem választja.</span><span class="sxs-lookup"><span data-stu-id="083a5-110">We recommend that you don't choose a network that you selected during network mapping.</span></span>
- <span data-ttu-id="083a5-111">Ebben a cikkben hello útmutatások mutatják be, hogyan toofail egy virtuális keresztül.</span><span class="sxs-lookup"><span data-stu-id="083a5-111">hello instructions in this article describe how toofail over a single VM.</span></span> <span data-ttu-id="083a5-112">További információ a [a helyreállítási terv létrehozása](site-recovery-create-recovery-plans.md) Ha azt szeretné, toofail át több virtuális gép együtt.</span><span class="sxs-lookup"><span data-stu-id="083a5-112">Read about [creating recovery plans](site-recovery-create-recovery-plans.md) if you want toofail over multiple VMs together.</span></span>
- <span data-ttu-id="083a5-113">További információ a [teszteket korlátozásai](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span><span class="sxs-lookup"><span data-stu-id="083a5-113">Read about [limitations for test failovers](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span></span>
- <span data-ttu-id="083a5-114">hello tooa virtuális gép feladatátvételi tesztje során megadott IP-cím hello azonos IP-címet, amely a virtuális gép hello kapja a tervezett vagy nem tervezett feladatátvételre (feltéve, hogy hello IP-cím érhető el a feladatátvételi teszt hálózatában hello).</span><span class="sxs-lookup"><span data-stu-id="083a5-114">hello IP address given tooa VM during test failover is hello same IP address that hello VM would receive for a planned or unplanned failover (presuming that hello IP address is available in hello test failover network).</span></span> <span data-ttu-id="083a5-115">Ha hello IP-cím nem érhető el a feladatátvételi teszt hálózatában hello, hello VM kap egy másik IP-címet, amely a feladatátvételi teszt hálózatában hello érhető el.</span><span class="sxs-lookup"><span data-stu-id="083a5-115">If hello IP address isn't available in hello test failover network, hello VM receives another IP address that is available in hello test failover network.</span></span>
- <span data-ttu-id="083a5-116">Ha a végpontok közötti hálózati kapcsolat gép teljes validatation toodo feladatátvételi teszt szeretné az éles hálózattól, vegye figyelembe, hogy:</span><span class="sxs-lookup"><span data-stu-id="083a5-116">If you do want toodo a test failover in your production network, for full validatation of end-to-end network connectivity machine, note that:</span></span>
    - <span data-ttu-id="083a5-117">elsődleges virtuális gép le kell állítani akkor, ha a feladatátvételi teszt hello hello.</span><span class="sxs-lookup"><span data-stu-id="083a5-117">hello primary VM should be shut down when you're doing hello test failover.</span></span> <span data-ttu-id="083a5-118">Ellenkező esetben két virtuális gépek ugyanazzal az identitással fog futni hello hello azonos hálózati: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="083a5-118">Otherwise, two VMs with hello same identity will be running in hello same network at hello same time.</span></span> 
    - <span data-ttu-id="083a5-119">Ha módosítja tootest virtuális gépeket, ezeket a módosításokat is elvesznek, tesztelésekor karbantartása hello feladatátvételi.</span><span class="sxs-lookup"><span data-stu-id="083a5-119">If you make changes tootest VMs, those changes are lost when you clean up hello test failover.</span></span> <span data-ttu-id="083a5-120">Ezek a változások nem replikált hátsó toohello elsődleges virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="083a5-120">These changes aren't replicated back toohello primary VM.</span></span>
    - <span data-ttu-id="083a5-121">Tesztelési egy éles hálózati környezetben a termelési számítási feladatokhoz toodowntime vezet.</span><span class="sxs-lookup"><span data-stu-id="083a5-121">Testing a a production network leads toodowntime for your production workloads.</span></span> <span data-ttu-id="083a5-122">Kérje meg a felhasználók nem toouse hello app Ha hello vész-helyreállítási részletezési van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="083a5-122">Ask users not toouse hello app when hello disaster recovery drill is in progress.</span></span>  


## <a name="run-a-test-failover-for-a-vm"></a><span data-ttu-id="083a5-123">A virtuális gépek feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="083a5-123">Run a test failover for a VM</span></span>

1. <span data-ttu-id="083a5-124">egy virtuális, keresztül toofail a **replikált elemek**, kattintson a virtuális gép hello > **feladatátvételi teszt**.</span><span class="sxs-lookup"><span data-stu-id="083a5-124">toofail over a single VM, in **Replicated Items**, click hello VM > **Test Failover**.</span></span>
2. <span data-ttu-id="083a5-125">A **feladatátvételi teszt**, adja meg, hogyan teszt virtuális gépek lesznek csatlakoztatott toonetworks hello a feladatátvételi teszt után.</span><span class="sxs-lookup"><span data-stu-id="083a5-125">In **Test Failover**, specify how test VMs will be connected toonetworks after hello test failover.</span></span> 
3. <span data-ttu-id="083a5-126">Kattintson a **OK** toobegin hello feladatátvételi.</span><span class="sxs-lookup"><span data-stu-id="083a5-126">Click **OK** toobegin hello failover.</span></span> <span data-ttu-id="083a5-127">Nyomon követni a hello **feladatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="083a5-127">Track progress on hello **Jobs** tab.</span></span>
5. <span data-ttu-id="083a5-128">Miután a feladatátvétel befejeződött, győződjön meg arról, hogy hello teszt virtuális gépek elindítása sikeresen.</span><span class="sxs-lookup"><span data-stu-id="083a5-128">After failover is complete, verify that hello test VMs start successfully.</span></span>
6. <span data-ttu-id="083a5-129">Amikor elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="083a5-129">When you're done, click **Cleanup test failover** on hello recovery plan.</span></span>
7. <span data-ttu-id="083a5-130">A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="083a5-130">In **Notes**, record and save any observations associated with hello test failover.</span></span> <span data-ttu-id="083a5-131">Ez a lépés törli az hello virtuális gépek és a hálózatokban, amelyek a feladatátvételi teszt során lettek létrehozva.</span><span class="sxs-lookup"><span data-stu-id="083a5-131">This step deletes hello virtual machines and networks that were created during test failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="083a5-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="083a5-132">Next steps</span></span>

<span data-ttu-id="083a5-133">Hello telepítés tesztelését, követően további információ más típusú [feladatátvételi](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="083a5-133">After you've tested hello deployment, learn more about other types of [failover](site-recovery-failover.md).</span></span>
