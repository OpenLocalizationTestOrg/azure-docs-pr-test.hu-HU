---
title: "A forrás és cél Hyper-V replikáció egy másodlagos helyre, az Azure Site Recovery beállítása |} Microsoft Docs"
description: "Ismerteti, hogyan kell beállítani a forrás és cél Hyper-V virtuális gépek replikálása az Azure Site Recovery másodlagos VMM-hely."
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
ms.openlocfilehash: 07135e9b5e619971a59cc22ec68a0a4e8bcaabe1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-set-up-the-replication-source-and-target"></a><span data-ttu-id="51316-103">6. lépés: A replikáció forrás- és a cél beállítása</span><span class="sxs-lookup"><span data-stu-id="51316-103">Step 6: Set up the replication source and target</span></span>


<span data-ttu-id="51316-104">Miután létrehozta a Recovery Services tároló VMM egy másodlagos helyre, amelynek a Hyper-V replikáció [Azure Site Recovery](site-recovery-overview.md), ez a cikk használatával a forrás és cél replikációs helyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="51316-104">After creating a Recovery Services vault for Hyper-V replication to a secondary VMM site with [Azure Site Recovery](site-recovery-overview.md), use this article to set up the source and target replication locations.</span></span> 

<span data-ttu-id="51316-105">A cikk elolvasása után felmerülő megjegyzéseit alul vagy az [Azure Recovery Services fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti közzé.</span><span class="sxs-lookup"><span data-stu-id="51316-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="set-up-the-source-environment"></a><span data-ttu-id="51316-106">A forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="51316-106">Set up the source environment</span></span>

<span data-ttu-id="51316-107">Az Azure Site Recovery Provider telepítése a VMM-kiszolgálókon, és Fedezze fel, és regisztrálja a kiszolgálót a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="51316-107">Install the Azure Site Recovery Provider on VMM servers, and discover and register servers in the vault.</span></span>

1. <span data-ttu-id="51316-108">Kattintson a **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="51316-108">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span>

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. <span data-ttu-id="51316-110">A **Forrás előkészítése** ablakban kattintson a **+ VMM** gombra a VMM-kiszolgálók felvételéhez.</span><span class="sxs-lookup"><span data-stu-id="51316-110">In **Prepare source**, click **+ VMM** to add a VMM server.</span></span>

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. <span data-ttu-id="51316-112">A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus** , és hogy a VMM-kiszolgáló megfelel-e a [Előfeltételek](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="51316-112">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that the VMM server meets the [prerequisites](#prerequisites).</span></span>
4. <span data-ttu-id="51316-113">Töltse le az Azure Site Recovery Provider telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="51316-113">Download the Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="51316-114">Töltse le a regisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="51316-114">Download the registration key.</span></span> <span data-ttu-id="51316-115">Erre a telepítő futtatása során lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="51316-115">You need this when you run setup.</span></span> <span data-ttu-id="51316-116">A kulcs a generálásától számított öt napig érvényes.</span><span class="sxs-lookup"><span data-stu-id="51316-116">The key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. <span data-ttu-id="51316-118">Telepítse az Azure Site Recovery Providert a VMM-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="51316-118">Install the Azure Site Recovery Provider on the VMM server.</span></span> <span data-ttu-id="51316-119">Nem kell explicit módon a Hyper-V gazdakiszolgálókra telepít semmit.</span><span class="sxs-lookup"><span data-stu-id="51316-119">You don't need to explicitly install anything on Hyper-V host servers.</span></span>


## <a name="install-the-azure-site-recovery-provider"></a><span data-ttu-id="51316-120">Az Azure Site Recovery Provider telepítése</span><span class="sxs-lookup"><span data-stu-id="51316-120">Install the Azure Site Recovery Provider</span></span>

1. <span data-ttu-id="51316-121">Futtassa a Providert telepítőfájl minden VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="51316-121">Run the Provider setup file on each VMM server.</span></span> <span data-ttu-id="51316-122">Ha a VMM fürtben lett telepítve, tegye a következőket telepíti először listájában:</span><span class="sxs-lookup"><span data-stu-id="51316-122">If VMM is deployed in a cluster, do the following the first time you install:</span></span>
    -  <span data-ttu-id="51316-123">Telepítse a szolgáltató aktív csomópontra, és fejezze be a telepítést, a VMM-kiszolgáló regisztrálása a tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="51316-123">Install the provider on an active node, and finish the installation to register the VMM server in the vault.</span></span>
    - <span data-ttu-id="51316-124">Ezután telepítse a szolgáltatót a többi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="51316-124">Then, install the Provider on the other nodes.</span></span> <span data-ttu-id="51316-125">Fürtcsomópontok összes futtassa a szolgáltató azonos verziója.</span><span class="sxs-lookup"><span data-stu-id="51316-125">Cluster nodes should all run the same version of the Provider.</span></span>
2. <span data-ttu-id="51316-126">A telepítő néhány előfeltétel-ellenőrzéseket futtatja, és a VMM szolgáltatás engedélyt kér.</span><span class="sxs-lookup"><span data-stu-id="51316-126">Setup runs a few prerequisite checks, and requests permission to stop the VMM service.</span></span> <span data-ttu-id="51316-127">A VMM szolgáltatás automatikusan újraindul a telepítő befejezése után.</span><span class="sxs-lookup"><span data-stu-id="51316-127">The VMM service will be restarted automatically when setup finishes.</span></span> <span data-ttu-id="51316-128">Ha telepíti a VMM-fürthöz, megkéri a fürtszerepkör leállítása.</span><span class="sxs-lookup"><span data-stu-id="51316-128">If you install on a VMM cluster, you're prompted to stop the Cluster role.</span></span>
3. <span data-ttu-id="51316-129">A **Microsoft Update**, lapon kérheti a adja meg, hogy szolgáltató frissítések telepítve vannak-e a Microsoft Update-szabályzatnak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="51316-129">In **Microsoft Update**, you can opt in to specify that provider updates are installed in accordance with your Microsoft Update policy.</span></span>
4. <span data-ttu-id="51316-130">A **telepítési**, fogadja el vagy módosítsa az alapértelmezett telepítési hely, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="51316-130">In **Installation**, accept or modify the default installation location, and click **Install**.</span></span>

    ![Telepítés helye](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. <span data-ttu-id="51316-132">Kattintson a telepítés befejezése után **regisztrálása** regisztrálni a kiszolgálót a tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="51316-132">After installation is complete, click **Register** to register the server in the vault.</span></span>

    ![Telepítés helye](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. <span data-ttu-id="51316-134">A **Tároló neve** résznél ellenőrizze a tároló nevét, amelyben a kiszolgálót regisztrálni fogja.</span><span class="sxs-lookup"><span data-stu-id="51316-134">In **Vault name**, verify the name of the vault in which the server will be registered.</span></span> <span data-ttu-id="51316-135">Kattintson a *Tovább* gombra.</span><span class="sxs-lookup"><span data-stu-id="51316-135">Click *Next*.</span></span>

    ![Kiszolgáló regisztrációja](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. <span data-ttu-id="51316-137">A **internetkapcsolat**, adja meg, hogy a VMM-kiszolgálón futó provider hogyan csatlakozzon az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="51316-137">In **Internet Connection**, specify how the provider running on the VMM server connects to Azure.</span></span>

    ![Internetbeállítások](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - <span data-ttu-id="51316-139">Megadhatja, hogy a szolgáltató közvetlenül az internethez, vagy egy proxyn keresztül kell-e csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="51316-139">You can specify that the provider should connect directly to the internet, or via a proxy.</span></span>
   - <span data-ttu-id="51316-140">Adja meg a proxybeállításokat, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="51316-140">Specify proxy settings if needed.</span></span>
   - <span data-ttu-id="51316-141">A proxyt használ, ha a VMM RunAs-fiókot (DRAProxyAccount) jön létre automatikusan a megadott proxy hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="51316-141">If you use a proxy, a VMM RunAs account (DRAProxyAccount) is created automatically using the specified proxy credentials.</span></span> <span data-ttu-id="51316-142">Állítsa be úgy a proxykiszolgálót, hogy ez a fiók elvégezhesse a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="51316-142">Configure the proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="51316-143">A RunAs fiók beállításait módosíthatja a VMM-konzol > **beállítások** > **biztonsági** > **futtató fiókok**.</span><span class="sxs-lookup"><span data-stu-id="51316-143">The RunAs account settings can be modified in the VMM console > **Settings** > **Security** > **Run As Accounts**.</span></span> <span data-ttu-id="51316-144">Indítsa újra a VMM szolgáltatást módosításainak frissítésére.</span><span class="sxs-lookup"><span data-stu-id="51316-144">Restart the VMM service to update changes.</span></span>
8. <span data-ttu-id="51316-145">A **Regisztrációs kulcs** résznél válassza ki az Azure Site Recoveryből letöltött, majd a VMM-kiszolgálóra másolt kulcsot.</span><span class="sxs-lookup"><span data-stu-id="51316-145">In **Registration Key**, select the key that you downloaded from Azure Site Recovery and copied to the VMM server.</span></span>
9. <span data-ttu-id="51316-146">A titkosítási beállítást csak akkor használja a rendszer, amikor a VMM-felhőkben található Hyper-V virtuális gépeket az Azure-ba replikálja.</span><span class="sxs-lookup"><span data-stu-id="51316-146">The encryption setting is only used when you're replicating Hyper-V VMs in VMM clouds to Azure.</span></span> <span data-ttu-id="51316-147">Másodlagos helyre történő replikáláskor a beállítást nem használja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="51316-147">If you're replicating to a secondary site it's not used.</span></span>
10. <span data-ttu-id="51316-148">A **Kiszolgáló neve** mezőben adjon meg egy, a tárolóban regisztrált VMM-kiszolgálót azonosító rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="51316-148">In **Server name**, specify a friendly name to identify the VMM server in the vault.</span></span> <span data-ttu-id="51316-149">Fürtkonfiguráció használata esetén adja meg a VMM-fürtszerepkör nevét.</span><span class="sxs-lookup"><span data-stu-id="51316-149">In a cluster configuration specify the VMM cluster role name.</span></span>
11. <span data-ttu-id="51316-150">A **Synchronize cloud metaadatok**, adja meg, hogy a VMM-kiszolgálón futó összes felhő metaadatait szinkronizálni a tárolóval szeretné.</span><span class="sxs-lookup"><span data-stu-id="51316-150">In **Synchronize cloud metadata**, select whether you want to synchronize metadata for all clouds on the VMM server with the vault.</span></span> <span data-ttu-id="51316-151">Ezt a műveletet kiszolgálónként csak egyszer szükséges elvégezni.</span><span class="sxs-lookup"><span data-stu-id="51316-151">This action only needs to happen once on each server.</span></span> <span data-ttu-id="51316-152">Ha nem kívánja az összes felhő szinkronizálásához, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a felhő tulajdonságai a VMM-konzolon.</span><span class="sxs-lookup"><span data-stu-id="51316-152">If you don't want to synchronize all clouds, you can leave this setting unchecked, and synchronize each cloud individually in the cloud properties in the VMM console.</span></span>
12. <span data-ttu-id="51316-153">A folyamat befejezéséhez kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="51316-153">Click **Next** to complete the process.</span></span> <span data-ttu-id="51316-154">A regisztrációt követően az Azure Site Recovery lekéri a metaadatokat VMM-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="51316-154">After registration, metadata from the VMM server is retrieved by Azure Site Recovery.</span></span> <span data-ttu-id="51316-155">A kiszolgáló ezt követően megjelenik a tároló **Kiszolgálók** lapjának **VMM-kiszolgálók** lapján.</span><span class="sxs-lookup"><span data-stu-id="51316-155">The server is displayed on the **VMM Servers** tab on the **Servers** page in the vault.</span></span>

    ![Kiszolgáló](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. <span data-ttu-id="51316-157">Miután a kiszolgáló elérhető, a Site Recovery konzolján a **forrás** > **forrás előkészítése** válassza ki a VMM-kiszolgálót, és válassza ki a felhőt, amelyben a Hyper-V gazdagépen helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="51316-157">After the server is available in the Site Recovery console, in **Source** > **Prepare source** select the VMM server, and select the cloud in which the Hyper-V host is located.</span></span> <span data-ttu-id="51316-158">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="51316-158">Then click **OK**.</span></span>

<span data-ttu-id="51316-159">A provider a parancssorból is telepíthető:</span><span class="sxs-lookup"><span data-stu-id="51316-159">You can also install the provider from the command line:</span></span>

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a><span data-ttu-id="51316-160">A célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="51316-160">Set up the target environment</span></span>

<span data-ttu-id="51316-161">Válassza ki a cél VMM-kiszolgáló és a felhő:</span><span class="sxs-lookup"><span data-stu-id="51316-161">Select the target VMM server and cloud:</span></span>

1. <span data-ttu-id="51316-162">Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a használni kívánt cél VMM-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="51316-162">Click **Prepare infrastructure** > **Target**, and select the target VMM server you want to use.</span></span>
2. <span data-ttu-id="51316-163">Felhők a kiszolgálón, amely szinkronizálva legyenek a Site Recovery jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="51316-163">Clouds on the server that are synchronized with Site Recovery will be displayed.</span></span> <span data-ttu-id="51316-164">Válassza ki a megcélzott felhőt.</span><span class="sxs-lookup"><span data-stu-id="51316-164">Select the target cloud.</span></span>

   ![cél](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a><span data-ttu-id="51316-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51316-166">Next steps</span></span>

<span data-ttu-id="51316-167">Ugrás a [7. lépés: hálózatleképezés konfigurálása](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="51316-167">Go to [Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>
