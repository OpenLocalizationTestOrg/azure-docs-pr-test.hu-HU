---
title: "hello forrás és cél Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Leírja, hogyan tooset akár hello forrás és cél mikor toosecondary VMM hely Hyper-V virtuális gépek replikálása az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 451cb4413ca5c09777a7faf512b1c8ea43e695f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-hello-replication-source-and-target"></a><span data-ttu-id="e7ae2-103">6. lépés: Állítson be hello replikációs forrása és célja</span><span class="sxs-lookup"><span data-stu-id="e7ae2-103">Step 6: Set up hello replication source and target</span></span>


<span data-ttu-id="e7ae2-104">A Recovery Services létrehozása után a Hyper-V replikáció tooa VMM másodlagos hely, tároló [Azure Site Recovery](site-recovery-overview.md), ez a cikk tooset elhasználja hello forrás és cél replikációs helyét.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-104">After creating a Recovery Services vault for Hyper-V replication tooa secondary VMM site with [Azure Site Recovery](site-recovery-overview.md), use this article tooset up hello source and target replication locations.</span></span> 

<span data-ttu-id="e7ae2-105">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e7ae2-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="set-up-hello-source-environment"></a><span data-ttu-id="e7ae2-106">Hello forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="e7ae2-106">Set up hello source environment</span></span>

<span data-ttu-id="e7ae2-107">Hello Azure Site Recovery Provider telepítése a VMM-kiszolgálókon, és Fedezze fel, és regisztrálja a kiszolgálókat hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-107">Install hello Azure Site Recovery Provider on VMM servers, and discover and register servers in hello vault.</span></span>

1. <span data-ttu-id="e7ae2-108">Kattintson a **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-108">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span>

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. <span data-ttu-id="e7ae2-110">A **forrás előkészítése**, kattintson a **+ VMM** tooadd egy VMM-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-110">In **Prepare source**, click **+ VMM** tooadd a VMM server.</span></span>

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. <span data-ttu-id="e7ae2-112">A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus** és, hogy hello VMM-kiszolgáló megfelel-e hello [Előfeltételek](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="e7ae2-112">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that hello VMM server meets hello [prerequisites](#prerequisites).</span></span>
4. <span data-ttu-id="e7ae2-113">Töltse le a hello Azure Site Recovery Provider telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-113">Download hello Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="e7ae2-114">Hello regisztrációs kulcs letöltése.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-114">Download hello registration key.</span></span> <span data-ttu-id="e7ae2-115">Erre a telepítő futtatása során lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-115">You need this when you run setup.</span></span> <span data-ttu-id="e7ae2-116">hello kulcs a generálásától öt napig esetén érvényes.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-116">hello key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. <span data-ttu-id="e7ae2-118">Hello Azure Site Recovery Provider telepítése hello VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-118">Install hello Azure Site Recovery Provider on hello VMM server.</span></span> <span data-ttu-id="e7ae2-119">Nem kell tooexplicitly telepít semmit a Hyper-V gazdakiszolgálókra.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-119">You don't need tooexplicitly install anything on Hyper-V host servers.</span></span>


## <a name="install-hello-azure-site-recovery-provider"></a><span data-ttu-id="e7ae2-120">Hello Azure Site Recovery Provider telepítése</span><span class="sxs-lookup"><span data-stu-id="e7ae2-120">Install hello Azure Site Recovery Provider</span></span>

1. <span data-ttu-id="e7ae2-121">Futtassa a szolgáltató telepítőfájl hello minden VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-121">Run hello Provider setup file on each VMM server.</span></span> <span data-ttu-id="e7ae2-122">Ha a VMM fürtben lett telepítve, tegye a hello hello amikor először telepíti a következő:</span><span class="sxs-lookup"><span data-stu-id="e7ae2-122">If VMM is deployed in a cluster, do hello following hello first time you install:</span></span>
    -  <span data-ttu-id="e7ae2-123">Hello szolgáltató telepíthető az aktív csomópont, és a Befejezés hello telepítési tooregister hello VMM-kiszolgáló hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-123">Install hello provider on an active node, and finish hello installation tooregister hello VMM server in hello vault.</span></span>
    - <span data-ttu-id="e7ae2-124">Ezután telepítse hello szolgáltató hello más csomópontokra.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-124">Then, install hello Provider on hello other nodes.</span></span> <span data-ttu-id="e7ae2-125">Fürtcsomópontok fusson összes hello hello szolgáltató azonos verziója.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-125">Cluster nodes should all run hello same version of hello Provider.</span></span>
2. <span data-ttu-id="e7ae2-126">A telepítő néhány előfeltétel-ellenőrzéseket futtatja, és kéri engedély toostop hello a VMM szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-126">Setup runs a few prerequisite checks, and requests permission toostop hello VMM service.</span></span> <span data-ttu-id="e7ae2-127">a VMM szolgáltatás hello automatikusan újraindul a telepítő befejezése után.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-127">hello VMM service will be restarted automatically when setup finishes.</span></span> <span data-ttu-id="e7ae2-128">Ha telepíti a VMM-fürthöz, Ön felszólító toostop hello fürtszerepkört.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-128">If you install on a VMM cluster, you're prompted toostop hello Cluster role.</span></span>
3. <span data-ttu-id="e7ae2-129">A **Microsoft Update**, dönthet úgy is toospecify a frissítéseket a Microsoft Update-szabályzatnak megfelelően van.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-129">In **Microsoft Update**, you can opt in toospecify that provider updates are installed in accordance with your Microsoft Update policy.</span></span>
4. <span data-ttu-id="e7ae2-130">A **telepítési**, fogadja el vagy módosítsa a hello alapértelmezett telepítési hely, és kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-130">In **Installation**, accept or modify hello default installation location, and click **Install**.</span></span>

    ![Telepítés helye](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. <span data-ttu-id="e7ae2-132">Kattintson a telepítés befejezése után **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-132">After installation is complete, click **Register** tooregister hello server in hello vault.</span></span>

    ![Telepítés helye](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. <span data-ttu-id="e7ae2-134">A **tároló neve**, ellenőrizze, melyik hello a kiszolgálót regisztrálni fogja hello tároló hello nevére.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-134">In **Vault name**, verify hello name of hello vault in which hello server will be registered.</span></span> <span data-ttu-id="e7ae2-135">Kattintson a *Tovább* gombra.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-135">Click *Next*.</span></span>

    ![Kiszolgáló regisztrációja](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. <span data-ttu-id="e7ae2-137">A **internetkapcsolat**, adja meg, hogy hello VMM-kiszolgálón futó hello provider hogyan csatlakozzon a tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-137">In **Internet Connection**, specify how hello provider running on hello VMM server connects tooAzure.</span></span>

    ![Internetbeállítások](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - <span data-ttu-id="e7ae2-139">Megadhatja a hello szolgáltatót közvetlenül toohello csatlakoznia internetes, vagy egy proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-139">You can specify that hello provider should connect directly toohello internet, or via a proxy.</span></span>
   - <span data-ttu-id="e7ae2-140">Adja meg a proxybeállításokat, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-140">Specify proxy settings if needed.</span></span>
   - <span data-ttu-id="e7ae2-141">Ha proxyt használ, a VMM RunAs-fiókot (DRAProxyAccount) jön létre, megadott hello használatával automatikusan proxy hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-141">If you use a proxy, a VMM RunAs account (DRAProxyAccount) is created automatically using hello specified proxy credentials.</span></span> <span data-ttu-id="e7ae2-142">Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-142">Configure hello proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="e7ae2-143">hello RunAs-fiók beállításait módosíthatja hello VMM-konzol > **beállítások** > **biztonsági** > **futtató fiókok**.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-143">hello RunAs account settings can be modified in hello VMM console > **Settings** > **Security** > **Run As Accounts**.</span></span> <span data-ttu-id="e7ae2-144">Indítsa újra a hello a VMM szolgáltatás tooupdate módosításait.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-144">Restart hello VMM service tooupdate changes.</span></span>
8. <span data-ttu-id="e7ae2-145">A **regisztrációs kulcs**, jelölje be az Azure Site Recoveryből letöltött, és másolja a toohello VMM-kiszolgáló hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-145">In **Registration Key**, select hello key that you downloaded from Azure Site Recovery and copied toohello VMM server.</span></span>
9. <span data-ttu-id="e7ae2-146">hello titkosítási beállítást csak akkor használja, ha a Hyper-V virtuális gépek a VMM-felhők tooAzure replikál.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-146">hello encryption setting is only used when you're replicating Hyper-V VMs in VMM clouds tooAzure.</span></span> <span data-ttu-id="e7ae2-147">Ha tooa másodlagos helyre replikál nem használható.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-147">If you're replicating tooa secondary site it's not used.</span></span>
10. <span data-ttu-id="e7ae2-148">A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-148">In **Server name**, specify a friendly name tooidentify hello VMM server in hello vault.</span></span> <span data-ttu-id="e7ae2-149">Adja meg a fürt konfigurációjába hello VMM-fürtszerepkör nevét.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-149">In a cluster configuration specify hello VMM cluster role name.</span></span>
11. <span data-ttu-id="e7ae2-150">A **Synchronize cloud metaadatok**, jelölje be, hogy a VMM-kiszolgálón hello hello tárolóban futó összes felhő toosynchronize metaadatok szeretné-e.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-150">In **Synchronize cloud metadata**, select whether you want toosynchronize metadata for all clouds on hello VMM server with hello vault.</span></span> <span data-ttu-id="e7ae2-151">Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-151">This action only needs toohappen once on each server.</span></span> <span data-ttu-id="e7ae2-152">Ha az összes felhő toosynchronize nem szeretné, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-152">If you don't want toosynchronize all clouds, you can leave this setting unchecked, and synchronize each cloud individually in hello cloud properties in hello VMM console.</span></span>
12. <span data-ttu-id="e7ae2-153">Kattintson a **következő** toocomplete hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-153">Click **Next** toocomplete hello process.</span></span> <span data-ttu-id="e7ae2-154">A regisztrációt követően az Azure Site Recovery által lekéri metaadatait hello VMM-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-154">After registration, metadata from hello VMM server is retrieved by Azure Site Recovery.</span></span> <span data-ttu-id="e7ae2-155">hello kiszolgáló megjelenik a hello **VMM-kiszolgálókon** hello lapján **kiszolgálók** lap hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-155">hello server is displayed on hello **VMM Servers** tab on hello **Servers** page in hello vault.</span></span>

    ![Kiszolgáló](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. <span data-ttu-id="e7ae2-157">Miután hello kiszolgáló elérhető hello Site Recovery konzolján, a **forrás** > **forrás előkészítése** hello VMM kiszolgálót válassza ki, melyik hello Hyper-V állomás válassza hello felhő.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-157">After hello server is available in hello Site Recovery console, in **Source** > **Prepare source** select hello VMM server, and select hello cloud in which hello Hyper-V host is located.</span></span> <span data-ttu-id="e7ae2-158">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-158">Then click **OK**.</span></span>

<span data-ttu-id="e7ae2-159">Hello szolgáltató hello parancssorból is telepíthető:</span><span class="sxs-lookup"><span data-stu-id="e7ae2-159">You can also install hello provider from hello command line:</span></span>

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="e7ae2-160">Hello célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="e7ae2-160">Set up hello target environment</span></span>

<span data-ttu-id="e7ae2-161">Hello cél VMM-kiszolgáló és a felhő kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="e7ae2-161">Select hello target VMM server and cloud:</span></span>

1. <span data-ttu-id="e7ae2-162">Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza hello cél VMM-kiszolgálónak toouse.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-162">Click **Prepare infrastructure** > **Target**, and select hello target VMM server you want toouse.</span></span>
2. <span data-ttu-id="e7ae2-163">Felhők hello kiszolgálón, amely szinkronizálva legyenek a Site Recovery jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-163">Clouds on hello server that are synchronized with Site Recovery will be displayed.</span></span> <span data-ttu-id="e7ae2-164">Válassza ki a hello megcélzott felhőt.</span><span class="sxs-lookup"><span data-stu-id="e7ae2-164">Select hello target cloud.</span></span>

   ![cél](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a><span data-ttu-id="e7ae2-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7ae2-166">Next steps</span></span>

<span data-ttu-id="e7ae2-167">Nyissa meg túl[7. lépés: hálózatleképezés konfigurálása](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="e7ae2-167">Go too[Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>
