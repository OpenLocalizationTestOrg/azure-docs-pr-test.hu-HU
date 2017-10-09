---
title: "hello forrás és cél Hyper-V replikáció tooAzure (a System Center VMM nélkül) az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket tooset forrás és cél Hyper-V virtuális gépek tooAzure tárolás az Azure Site Recovery replikálás beállításainak mentése"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 105b90e6ac053d5b842c54a36c460a26d0f5c2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-replication-tooazure"></a><span data-ttu-id="c2550-103">8. lépés: Hello forrása és célja a Hyper-V replikáció tooAzure beállítása</span><span class="sxs-lookup"><span data-stu-id="c2550-103">Step 8: Set up hello source and target for Hyper-V replication tooAzure</span></span>

<span data-ttu-id="c2550-104">Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni Hyper-V virtuális gépek (a System Center VMM nélkül) tooAzure, használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c2550-104">This article describes how tooconfigure source and target settings when replicating on-premises Hyper-V virtual machines (without System Center VMM) tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="c2550-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c2550-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="c2550-106">Hello forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="c2550-106">Set up hello source environment</span></span>

<span data-ttu-id="c2550-107">Hello Hyper-V hely beállítása hello Azure Site Recovery Provider és hello Azure Recovery Services Agent ügynök telepítése a Hyper-V gazdagépek és hello hely regisztrálja hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c2550-107">Set up hello Hyper-V site, install hello Azure Site Recovery Provider and hello Azure Recovery Services agent on Hyper-V hosts, and register hello site in hello vault.</span></span>

1. <span data-ttu-id="c2550-108">A **infrastruktúra előkészítése**, kattintson a **forrás**.</span><span class="sxs-lookup"><span data-stu-id="c2550-108">In **Prepare Infrastructure**, click **Source**.</span></span> <span data-ttu-id="c2550-109">a Hyper-V gazdagépekhez vagy fürtökhöz, tárolója új Hyper-V hely tooadd kattintson **+ Hyper-V hely**.</span><span class="sxs-lookup"><span data-stu-id="c2550-109">tooadd a new Hyper-V site as a container for your Hyper-V hosts or clusters, click **+Hyper-V Site**.</span></span>

    ![A forrás beállítása](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. <span data-ttu-id="c2550-111">A **hozzon létre Hyper-V hely**, adja meg a hello hely nevét.</span><span class="sxs-lookup"><span data-stu-id="c2550-111">In **Create Hyper-V site**, specify a name for hello site.</span></span> <span data-ttu-id="c2550-112">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c2550-112">Then click **OK**.</span></span> <span data-ttu-id="c2550-113">Jelölje ki, hello helyen létrehozott, és kattintson a **+ Hyper-V Server** tooadd egy kiszolgáló toohello helyet.</span><span class="sxs-lookup"><span data-stu-id="c2550-113">Now, select hello site you created, and click **+Hyper-V Server** tooadd a server toohello site.</span></span>

    ![A forrás beállítása](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="c2550-115">A **kiszolgáló hozzáadása** > **kiszolgálótípus**, ellenőrizze, hogy **Hyper-V server** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c2550-115">In **Add Server** > **Server type**, check that **Hyper-V server** is displayed.</span></span>

    - <span data-ttu-id="c2550-116">Győződjön meg arról, hogy hello Hyper-V-kiszolgálónak tooadd hello megfelel [Előfeltételek](#on-premises-prerequisites), és képes tooaccess hello van megadott URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="c2550-116">Make sure that hello Hyper-V server you want tooadd complies with hello [prerequisites](#on-premises-prerequisites), and is able tooaccess hello specified URLs.</span></span>
    - <span data-ttu-id="c2550-117">Töltse le a hello Azure Site Recovery Provider telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="c2550-117">Download hello Azure Site Recovery Provider installation file.</span></span> <span data-ttu-id="c2550-118">A fájl tooinstall hello szolgáltatót futtatja, és minden Hyper-V gazdagépen a Recovery Services agent hello.</span><span class="sxs-lookup"><span data-stu-id="c2550-118">You run this file tooinstall hello Provider and hello Recovery Services agent on each Hyper-V host.</span></span>

    ![A forrás beállítása](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-hello-provider-and-agent"></a><span data-ttu-id="c2550-120">Hello szolgáltató és az ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="c2550-120">Install hello Provider and agent</span></span>

1. <span data-ttu-id="c2550-121">Futtassa a szolgáltató telepítőfájl hello minden állomáson hozzáadott toohello Hyper-V helyet.</span><span class="sxs-lookup"><span data-stu-id="c2550-121">Run hello Provider setup file on each host you added toohello Hyper-V site.</span></span> <span data-ttu-id="c2550-122">Hyper-V fürt telepítése, futtassa a telepítőt a fürt minden csomópontján.</span><span class="sxs-lookup"><span data-stu-id="c2550-122">If you're installing on a Hyper-V cluster, run setup on each cluster node.</span></span> <span data-ttu-id="c2550-123">Telepítése és regisztrálása a Hyper-V fürt minden csomópontján biztosítja a virtuális gépek védelmének biztosításához, akkor is, ha az áttelepítés után csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="c2550-123">Installing and registering each Hyper-V cluster node ensures that VMs are protected, even if they migrate across nodes.</span></span>
2. <span data-ttu-id="c2550-124">A **Microsoft Update** lapon kérheti a frissítések beszerzését, így a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Providerhez kiadott frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="c2550-124">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="c2550-125">A **telepítési**, fogadja el vagy módosítsa a hello Provider alapértelmezett telepítési helyét, majd kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="c2550-125">In **Installation**, accept or modify hello default Provider installation location and click **Install**.</span></span>
4. <span data-ttu-id="c2550-126">A **tároló beállításait**, kattintson a **Tallózás** tooselect hello tároló kulcsfájlját letöltött.</span><span class="sxs-lookup"><span data-stu-id="c2550-126">In **Vault Settings**, click **Browse** tooselect hello vault key file that you downloaded.</span></span> <span data-ttu-id="c2550-127">Adja meg a hello Azure Site Recovery-előfizetést, hello tároló nevére, és hello Hyper-V hely toowhich hello Hyper-V kiszolgáló tartozik.</span><span class="sxs-lookup"><span data-stu-id="c2550-127">Specify hello Azure Site Recovery subscription, hello vault name, and hello Hyper-V site toowhich hello Hyper-V server belongs.</span></span>

    ![Kiszolgáló regisztrációja](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. <span data-ttu-id="c2550-129">A **proxybeállítások**, adja meg, hogyan Hyper-V gazdagépeken futó szolgáltató az internetre csatlakozik a Site Recovery tooAzure hello hello internet.</span><span class="sxs-lookup"><span data-stu-id="c2550-129">In **Proxy Settings**, specify how hello Provider running on Hyper-V hosts connects tooAzure Site Recovery over hello internet.</span></span>

    * <span data-ttu-id="c2550-130">Ha azt szeretné, hello szolgáltató tooconnect közvetlenül válassza **csatlakozzon közvetlenül a Site Recovery tooAzure proxy nélkül**.</span><span class="sxs-lookup"><span data-stu-id="c2550-130">If you want hello Provider tooconnect directly select **Connect directly tooAzure Site Recovery without a proxy**.</span></span>
    * <span data-ttu-id="c2550-131">Ha az aktuálisan használt proxy hitelesítést igényel, vagy hello szolgáltatói kapcsolat toouse egyéni proxyt használni szeretne, válassza ki a **csatlakozzon a Site Recovery proxykiszolgálóval tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="c2550-131">If your existing proxy requires authentication, or you want toouse a custom proxy for hello Provider connection, select **Connect tooAzure Site Recovery using a proxy server**.</span></span>
    * <span data-ttu-id="c2550-132">Ha proxyt használ:</span><span class="sxs-lookup"><span data-stu-id="c2550-132">If you use a proxy:</span></span>
        - <span data-ttu-id="c2550-133">Hello cím, port és a hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="c2550-133">Specify hello address, port, and credentials</span></span>
        - <span data-ttu-id="c2550-134">Győződjön meg arról, hogy hello URL-címeket a hello [Előfeltételek](#prerequisites) hello proxyn keresztül történő használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c2550-134">Make sure hello URLs described in hello [prerequisites](#prerequisites) are allowed through hello proxy.</span></span>

    ![internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. <span data-ttu-id="c2550-136">Kattintson a telepítés befejezése után **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="c2550-136">After installation finishes, click **Register** tooregister hello server in hello vault.</span></span>

    ![Telepítés helye](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. <span data-ttu-id="c2550-138">Regisztráció befejezése után a metaadatok hello Hyper-V kiszolgálóról Azure Site Recovery lekéri, és hello kiszolgálóhoz **Site Recovery-infrastruktúra** > **Hyper-V-gazdagépek**.</span><span class="sxs-lookup"><span data-stu-id="c2550-138">After registration finishes, metadata from hello Hyper-V server is retrieved by Azure Site Recovery, and hello server is displayed in **Site Recovery Infrastructure** > **Hyper-V Hosts**.</span></span>


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="c2550-139">Hello célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="c2550-139">Set up hello target environment</span></span>

<span data-ttu-id="c2550-140">Adja meg a hello Azure storage-fiók replikációhoz, és hello Azure hálózati toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="c2550-140">Specify hello Azure storage account for replication, and hello Azure network toowhich Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="c2550-141">Kattintson a **infrastruktúra előkészítése** > **cél**.</span><span class="sxs-lookup"><span data-stu-id="c2550-141">Click **Prepare infrastructure** > **Target**.</span></span>
2. <span data-ttu-id="c2550-142">Válassza ki a hello előfizetés és hello erőforráscsoportot használni kívánt toocreate hello Azure virtuális gépek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="c2550-142">Select hello subscription and hello resource group in which you want toocreate hello Azure VMs after failover.</span></span> <span data-ttu-id="c2550-143">Válassza ki a hello telepítési modell, amelyet az Azure (klasszikus vagy erőforrás-kezelés) toouse hello virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="c2550-143">Choose hello deployment model that you want toouse in Azure (classic or resource management) for hello VMs.</span></span>

3. <span data-ttu-id="c2550-144">A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.</span><span class="sxs-lookup"><span data-stu-id="c2550-144">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    - <span data-ttu-id="c2550-145">Ha a tárfiók nincs, kattintson a **+ tárolás** toocreate egy erőforrás-kezelő fiók beágyazott.</span><span class="sxs-lookup"><span data-stu-id="c2550-145">If you don't have a storage account, click **+Storage** toocreate a Resource Manager-based account inline.</span></span> 
    - <span data-ttu-id="c2550-146">Ha nem rendelkezik Azure-hálózatot, kattintson a **+ hálózat** toocreate a Resource Manager-alapú hálózati beágyazott.</span><span class="sxs-lookup"><span data-stu-id="c2550-146">If you don't have a Azure network, click **+Network** toocreate a Resource Manager-based network inline.</span></span>






## <a name="next-steps"></a><span data-ttu-id="c2550-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2550-147">Next steps</span></span>

<span data-ttu-id="c2550-148">Nyissa meg túl[9. lépés: a replikációs házirend beállítása](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="c2550-148">Go too[Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>
