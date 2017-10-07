---
title: "hello forrás és cél Hyper-V replikáció tooAzure (a System Center VMM) az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket tooset forrás és cél Hyper-V virtuális gépek replikálását a VMM felhők tooAzure tárolás az Azure Site Recovery beállításokat"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: 3f8c5386cb64527c775aef636980bac098ee9905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-with-vmm-replication-tooazure"></a><span data-ttu-id="96829-103">8. lépés: Hello forrása és célja a Hyper-V (a VMM-mel) replikációs tooAzure beállítása</span><span class="sxs-lookup"><span data-stu-id="96829-103">Step 8: Set up hello source and target for Hyper-V (with VMM) replication tooAzure</span></span>

<span data-ttu-id="96829-104">Miután [létrehozni egy tárolót](vmm-to-azure-walkthrough-create-vault.md) , és adja meg, milyen meg szeretné, hogy tooreplicate, ez a cikk tooconfigure adatforrás és cél beállításait, a helyszíni Hyper-V virtuális gépek a System Center Virtual Machine Manager (VMM) replikálásához felhők tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="96829-104">After [creating a vault](vmm-to-azure-walkthrough-create-vault.md) and specifying what you want tooreplicate, use this article tooconfigure source and target settings when replicating on-premises Hyper-V virtual machines in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="96829-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="96829-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="96829-106">Hello forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="96829-106">Set up hello source environment</span></span>

<span data-ttu-id="96829-107">S1.</span><span class="sxs-lookup"><span data-stu-id="96829-107">S1.</span></span> <span data-ttu-id="96829-108">Kattintson **Az infrastruktúra előkészítése** > **Forrás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="96829-108">Click **Prepare Infrastructure** > **Source**.</span></span>

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. <span data-ttu-id="96829-109">A **forrás előkészítése**, kattintson a **+ VMM** tooadd egy VMM-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="96829-109">In **Prepare source**, click **+ VMM** tooadd a VMM server.</span></span>

    ![A forrás beállítása](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="96829-111">A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus** és, hogy hello VMM-kiszolgáló megfelel-e hello [Előfeltételek és URL-címe követelmények](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="96829-111">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that hello VMM server meets hello [prerequisites and URL requirements](#prerequisites).</span></span>
4. <span data-ttu-id="96829-112">Töltse le a hello Azure Site Recovery Provider telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="96829-112">Download hello Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="96829-113">Hello regisztrációs kulcs letöltése.</span><span class="sxs-lookup"><span data-stu-id="96829-113">Download hello registration key.</span></span> <span data-ttu-id="96829-114">Erre a telepítő futtatása során lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="96829-114">You need this when you run setup.</span></span> <span data-ttu-id="96829-115">hello kulcs a generálásától öt napig esetén érvényes.</span><span class="sxs-lookup"><span data-stu-id="96829-115">hello key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-hello-provider-on-hello-vmm-server"></a><span data-ttu-id="96829-117">Telepítse a szolgáltató hello hello VMM-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="96829-117">Install hello Provider on hello VMM server</span></span>

1. <span data-ttu-id="96829-118">Futtassa a szolgáltató telepítőfájl hello hello VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="96829-118">Run hello Provider setup file on hello VMM server.</span></span>
2. <span data-ttu-id="96829-119">A **Microsoft Update** lapon kérheti a frissítések beszerzését, így a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Providerhez kiadott frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="96829-119">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="96829-120">A **telepítési**, fogadja el vagy módosítsa a hello Provider alapértelmezett telepítési helyét, majd kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="96829-120">In **Installation**, accept or modify hello default Provider installation location and click **Install**.</span></span>

    ![Telepítés helye](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. <span data-ttu-id="96829-122">Kattintson a telepítés befejezése után **regisztrálása** tooregister hello VMM-kiszolgáló hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="96829-122">When installation finishes, click **Register** tooregister hello VMM server in hello vault.</span></span>
5. <span data-ttu-id="96829-123">A hello **tároló beállításait** kattintson **Tallózás** tooselect hello tároló kulcsfájlját.</span><span class="sxs-lookup"><span data-stu-id="96829-123">In hello **Vault Settings** page, click **Browse** tooselect hello vault key file.</span></span> <span data-ttu-id="96829-124">Adja meg a hello Azure Site Recovery-előfizetést és hello tároló nevére.</span><span class="sxs-lookup"><span data-stu-id="96829-124">Specify hello Azure Site Recovery subscription and hello vault name.</span></span>

    ![Kiszolgáló regisztrációja](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. <span data-ttu-id="96829-126">A **internetkapcsolat**, adja meg, hogyan hello VMM-kiszolgálón futó Provider keresztül fog csatlakozni tooSite helyreállítási hello hello internet.</span><span class="sxs-lookup"><span data-stu-id="96829-126">In **Internet Connection**, specify how hello Provider running on hello VMM server will connect tooSite Recovery over hello internet.</span></span>

   * <span data-ttu-id="96829-127">Ha közvetlenül hello szolgáltató tooconnect, jelölje be **csatlakozzon közvetlenül a Site Recovery tooAzure proxy nélkül**.</span><span class="sxs-lookup"><span data-stu-id="96829-127">If you want hello Provider tooconnect directly, select **Connect directly tooAzure Site Recovery without a proxy**.</span></span>
   * <span data-ttu-id="96829-128">Ha az aktuálisan használt proxy hitelesítést igényel, vagy toouse egyéni proxyt szeretne, válassza ki a **csatlakozzon a Site Recovery proxykiszolgálóval tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="96829-128">If your existing proxy requires authentication, or you want toouse a custom proxy, select **Connect tooAzure Site Recovery using a proxy server**.</span></span>
   * <span data-ttu-id="96829-129">Ha egyéni proxyt használ, adja meg a hello cím, port és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="96829-129">If you use a custom proxy, specify hello address, port, and credentials.</span></span>
   * <span data-ttu-id="96829-130">Ha a proxy használata esetén kell már engedélyezte a hello URL-címeket a [Előfeltételek](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="96829-130">If you're using a proxy, you should have already allowed hello URLs described in [prerequisites](#on-premises-prerequisites).</span></span>
   * <span data-ttu-id="96829-131">Ha egyéni proxyt használ, a VMM RunAs-fiókot (DRAProxyAccount) használatával jön létre automatikusan a megadott hello proxy hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="96829-131">If you use a custom proxy, a VMM RunAs account (DRAProxyAccount) will be created automatically using hello specified proxy credentials.</span></span> <span data-ttu-id="96829-132">Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="96829-132">Configure hello proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="96829-133">hello VMM RunAs-fiók beállításait hello VMM-konzolon módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="96829-133">hello VMM RunAs account settings can be modified in hello VMM console.</span></span> <span data-ttu-id="96829-134">A **beállítások**, bontsa ki a **biztonsági** > **futtató fiókok**, majd módosítsa a draproxyaccount jelszavát hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="96829-134">In **Settings**, expand **Security** > **Run As Accounts**, and then modify hello password for DRAProxyAccount.</span></span> <span data-ttu-id="96829-135">Szüksége lesz toorestart hello VMM szolgáltatást, hogy ez a beállítás lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="96829-135">You’ll need toorestart hello VMM service so that this setting takes effect.</span></span>

     ![internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. <span data-ttu-id="96829-137">Fogadja el, vagy módosítsa az adatok titkosításához automatikusan létrehozott SSL-tanúsítvány hello helyét.</span><span class="sxs-lookup"><span data-stu-id="96829-137">Accept or modify hello location of an SSL certificate that’s automatically generated for data encryption.</span></span> <span data-ttu-id="96829-138">Ez a tanúsítvány akkor használatos, ha engedélyezi az adattitkosítást a hello Azure Site Recovery portálon az Azure által védett felhő.</span><span class="sxs-lookup"><span data-stu-id="96829-138">This certificate is used if you enable data encryption for a cloud protected by Azure in hello Azure Site Recovery portal.</span></span> <span data-ttu-id="96829-139">Őrizze biztonságos helyen a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="96829-139">Keep this certificate safe.</span></span> <span data-ttu-id="96829-140">A feladatátvételi tooAzure futtatásakor szüksége lehet rájuk toodecrypt, ha engedélyezett az adattitkosítás.</span><span class="sxs-lookup"><span data-stu-id="96829-140">When you run a failover tooAzure you’ll need it toodecrypt, if data encryption is enabled.</span></span>
8. <span data-ttu-id="96829-141">A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="96829-141">In **Server name**, specify a friendly name tooidentify hello VMM server in hello vault.</span></span> <span data-ttu-id="96829-142">Fürtkonfiguráció adja meg a hello VMM-fürtszerepkör nevét.</span><span class="sxs-lookup"><span data-stu-id="96829-142">In a cluster configuration, specify hello VMM cluster role name.</span></span>
9. <span data-ttu-id="96829-143">Engedélyezése **felhőmetaadatok szinkronizálása**, ha azt szeretné, toosynchronize metaadatokat VMM-kiszolgálón hello hello tárolóban futó összes felhő.</span><span class="sxs-lookup"><span data-stu-id="96829-143">Enable **Sync cloud metadata**, if you want toosynchronize metadata for all clouds on hello VMM server with hello vault.</span></span> <span data-ttu-id="96829-144">Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége.</span><span class="sxs-lookup"><span data-stu-id="96829-144">This action only needs toohappen once on each server.</span></span> <span data-ttu-id="96829-145">Ha nem szeretné, toosynchronize összes felhő, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon.</span><span class="sxs-lookup"><span data-stu-id="96829-145">If you don't want toosynchronize all clouds, you can leave this setting unchecked and synchronize each cloud individually in hello cloud properties in hello VMM console.</span></span> <span data-ttu-id="96829-146">Kattintson a **regisztrálása** toocomplete hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="96829-146">Click **Register** toocomplete hello process.</span></span>

    ![Kiszolgáló regisztrációja](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. <span data-ttu-id="96829-148">Elindul a regisztráció.</span><span class="sxs-lookup"><span data-stu-id="96829-148">Registration starts.</span></span> <span data-ttu-id="96829-149">Regisztráció befejezése után hello kiszolgáló megjelenik **Site Recovery-infrastruktúra** > **VMM-kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="96829-149">After registration finishes, hello server is displayed in **Site Recovery Infrastructure** > **VMM Servers**.</span></span>


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a><span data-ttu-id="96829-150">Hello Azure Recovery Services agent telepítése a Hyper-V gazdagépek</span><span class="sxs-lookup"><span data-stu-id="96829-150">Install hello Azure Recovery Services agent on Hyper-V hosts</span></span>

1. <span data-ttu-id="96829-151">Hello szolgáltató beállítása után szüksége toodownload hello telepítőfájl hello Azure Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="96829-151">After you've set up hello Provider, you need toodownload hello installation file for hello Azure Recovery Services agent.</span></span> <span data-ttu-id="96829-152">Futtassa a telepítőt a VMM-felhő hello minden Hyper-V kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="96829-152">Run setup on each Hyper-V server in hello VMM cloud.</span></span>

    ![Hyper-V helyek](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. <span data-ttu-id="96829-154">Az **Előfeltételek ellenőrzése** részben kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="96829-154">In **Prerequisites Check**, click **Next**.</span></span> <span data-ttu-id="96829-155">A rendszer automatikusan telepíti a hiányzó előfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="96829-155">Any missing prerequisites will be automatically installed.</span></span>

    ![Recovery Services Agent, előfeltételek](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. <span data-ttu-id="96829-157">A **telepítési beállítások**, fogadja el, vagy módosítsa a hello telepítési helyét, és hello gyorsítótár helyét.</span><span class="sxs-lookup"><span data-stu-id="96829-157">In **Installation Settings**, accept or modify hello installation location, and hello cache location.</span></span> <span data-ttu-id="96829-158">Hello gyorsítótár konfigurálhat egy meghajtón, amelyen legalább 5 GB tárterület érhető el, de azt javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad terület.</span><span class="sxs-lookup"><span data-stu-id="96829-158">You can configure hello cache on a drive that has at least 5 GB of storage available but we recommend a cache drive with 600 GB or more of free space.</span></span> <span data-ttu-id="96829-159">Ezt követően kattintson a **Telepítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="96829-159">Then click **Install**.</span></span>
4. <span data-ttu-id="96829-160">Kattintson a telepítés befejezése után **Bezárás** toofinish.</span><span class="sxs-lookup"><span data-stu-id="96829-160">After installation is complete, click **Close** toofinish.</span></span>

    ![A MARS Agent regisztrálása](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a><span data-ttu-id="96829-162">Parancssori telepítés</span><span class="sxs-lookup"><span data-stu-id="96829-162">Command line installation</span></span>
<span data-ttu-id="96829-163">Hello Microsoft Azure Recovery Services Agent a parancssorból a következő parancs hello segítségével telepítheti:</span><span class="sxs-lookup"><span data-stu-id="96829-163">You can install hello Microsoft Azure Recovery Services Agent from command line using hello following command:</span></span>

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a><span data-ttu-id="96829-164">Internetes proxy hozzáférés tooSite helyreállítási Hyper-V gazdagépekről beállítása</span><span class="sxs-lookup"><span data-stu-id="96829-164">Set up internet proxy access tooSite Recovery from Hyper-V hosts</span></span>

<span data-ttu-id="96829-165">hello Recovery Services Agent ügynököt a Hyper-V gazdagépeken futó virtuális gép replikációs internet access tooAzure igényeinek.</span><span class="sxs-lookup"><span data-stu-id="96829-165">hello Recovery Services agent running on Hyper-V hosts needs internet access tooAzure for VM replication.</span></span> <span data-ttu-id="96829-166">Ha elérni hello internet egy proxyn keresztül, állítsa be az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="96829-166">If you're accessing hello internet through a proxy, set it up as follows:</span></span>

1. <span data-ttu-id="96829-167">Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="96829-167">Open hello Microsoft Azure Backup MMC snap-in on hello Hyper-V host.</span></span> <span data-ttu-id="96829-168">Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="96829-168">By default, a shortcut for Microsoft Azure Backup is available on hello desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="96829-169">Kattintson a beépülő modul hello **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="96829-169">In hello snap-in, click **Change Properties**.</span></span>
3. <span data-ttu-id="96829-170">A hello **proxykonfigurációt** lapra, adja meg a proxykiszolgáló-adatok.</span><span class="sxs-lookup"><span data-stu-id="96829-170">On hello **Proxy Configuration** tab, specify proxy server information.</span></span>

    ![A MARS Agent regisztrálása](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. <span data-ttu-id="96829-172">Ellenőrizze, hogy hello ügynök képes elérni hello URL-címeket a hello [Előfeltételek](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="96829-172">Check that hello agent can reach hello URLs described in hello [prerequisites](#on-premises-prerequisites).</span></span>

## <a name="set-up-hello-target-environment"></a><span data-ttu-id="96829-173">Hello célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="96829-173">Set up hello target environment</span></span>
<span data-ttu-id="96829-174">Adja meg a replikáláshoz használt hello az Azure storage-fiók toobe, és hello Azure hálózati toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="96829-174">Specify hello Azure storage account toobe used for replication, and hello Azure network toowhich Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="96829-175">Kattintson a **infrastruktúra előkészítése** > **cél**válassza ki a hello előfizetés, ahová átadja a virtuális gépek toocreate hello erőforráscsoport hello.</span><span class="sxs-lookup"><span data-stu-id="96829-175">Click **Prepare infrastructure** > **Target**, select hello subscription and hello resource group where you want toocreate hello failed over virtual machines.</span></span> <span data-ttu-id="96829-176">Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés) Azure virtuális gépek a feladatátvételt hello hello telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="96829-176">Choose hello deployment model that you want toouse in Azure (classic or resource management) for hello failed over virtual machines.</span></span>

    ![Storage](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. <span data-ttu-id="96829-178">A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.</span><span class="sxs-lookup"><span data-stu-id="96829-178">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    ![Storage](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. <span data-ttu-id="96829-180">Ha még nem hozott létre egy tárfiókot, és azt szeretné, hogy egy toocreate erőforrás-kezelő használatával kattintson **+ tárfiók** toodo megtenni.</span><span class="sxs-lookup"><span data-stu-id="96829-180">If you haven't created a storage account, and you want toocreate one using Resource Manager, click **+Storage account** toodo that inline.</span></span>  <span data-ttu-id="96829-181">A hello **storage-fiók létrehozása** panelen adja meg egy fiók nevét, típusát, előfizetés és hely.</span><span class="sxs-lookup"><span data-stu-id="96829-181">On hello **Create storage account** blade specify an account name, type, subscription, and location.</span></span> <span data-ttu-id="96829-182">hello fióknak kell lennie a hello hello és ugyanazon a helyen Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="96829-182">hello account should be in hello same location as hello Recovery Services vault.</span></span>

   ![Storage](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * <span data-ttu-id="96829-184">Ha azt szeretné, toocreate hello klasszikus modellt használó tárfiókot, ehhez a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="96829-184">If you want toocreate a storage account using hello classic model, do that in hello Azure portal.</span></span> [<span data-ttu-id="96829-185">További információ</span><span class="sxs-lookup"><span data-stu-id="96829-185">Learn more</span></span>](../storage/common/storage-create-storage-account.md)
   * <span data-ttu-id="96829-186">A replikált adatok használata a prémium szintű storage-fiók, állítsa be egy további standard szintű tárfiók toostore replikálási naplókhoz, folyamatban lévő változtatások tooon helyszíni adatok rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="96829-186">If you’re using a premium storage account for replicated data, set up an additional standard storage account, toostore replication logs that capture ongoing changes tooon-premises data.</span></span>
5. <span data-ttu-id="96829-187">Ha még nem hozott létre az Azure-hálózatot, és azt szeretné, hogy egy toocreate erőforrás-kezelő használatával kattintson **+ hálózat** toodo megtenni.</span><span class="sxs-lookup"><span data-stu-id="96829-187">If you haven't created an Azure network, and you want toocreate one using Resource Manager, click **+Network** toodo that inline.</span></span> <span data-ttu-id="96829-188">A hello **virtuális hálózat létrehozása** panelen adja meg a hálózati név, címtartomány, alhálózati adatokat, előfizetést és helyet.</span><span class="sxs-lookup"><span data-stu-id="96829-188">On hello **Create virtual network** blade specify a network name, address range, subnet details, subscription, and location.</span></span> <span data-ttu-id="96829-189">hello hálózat legyen hello hello és ugyanazon a helyen Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="96829-189">hello network should be in hello same location as hello Recovery Services vault.</span></span>

   ![Network (Hálózat)](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   <span data-ttu-id="96829-191">Ha azt szeretné, toocreate hello klasszikus modellt használó hálózatot, ehhez a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="96829-191">If you want toocreate a network using hello classic model, do that in hello Azure portal.</span></span> <span data-ttu-id="96829-192">[További információk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="96829-192">[Learn more](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span></span>





## <a name="next-steps"></a><span data-ttu-id="96829-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96829-193">Next steps</span></span>

<span data-ttu-id="96829-194">Nyissa meg túl[9. lépés: hálózatleképezés konfigurálása](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="96829-194">Go too[Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>
