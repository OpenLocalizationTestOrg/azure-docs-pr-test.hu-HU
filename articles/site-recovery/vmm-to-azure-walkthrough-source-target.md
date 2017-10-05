---
title: "Állítsa be a forrás és cél Hyper-V replikáció az Azure-ba (a System Center VMM) az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Forrás és cél Hyper-V virtuális gépek replikálás beállításainak beállítása az Azure storage az Azure Site Recovery VMM-felhőkben ismertetett lépéseket foglalja"
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
ms.openlocfilehash: c72f839d0a1288dccb7deb3e44fc2b20d64818f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-with-vmm-replication-to-azure"></a><span data-ttu-id="4dad1-103">8. lépés: A forrás- és a cél az Azure-ba (VMM) szolgáltatással a Hyper-V replikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="4dad1-103">Step 8: Set up the source and target for Hyper-V (with VMM) replication to Azure</span></span>

<span data-ttu-id="4dad1-104">Miután [létrehozni egy tárolót](vmm-to-azure-walkthrough-create-vault.md) , és adja meg, mit szeretne replikálni, ez a cikk forrású és beállítások konfigurálása a System Center Virtual Machine Manager (VMM) felhők helyszíni Hyper-V virtuális gépek replikálása esetén az Azure-bA használja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4dad1-104">After [creating a vault](vmm-to-azure-walkthrough-create-vault.md) and specifying what you want to replicate, use this article to configure source and target settings when replicating on-premises Hyper-V virtual machines in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="4dad1-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4dad1-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="4dad1-106">A forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="4dad1-106">Set up the source environment</span></span>

<span data-ttu-id="4dad1-107">S1.</span><span class="sxs-lookup"><span data-stu-id="4dad1-107">S1.</span></span> <span data-ttu-id="4dad1-108">Kattintson **Az infrastruktúra előkészítése** > **Forrás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="4dad1-108">Click **Prepare Infrastructure** > **Source**.</span></span>

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. <span data-ttu-id="4dad1-109">A **Forrás előkészítése** ablakban kattintson a **+ VMM** gombra a VMM-kiszolgálók felvételéhez.</span><span class="sxs-lookup"><span data-stu-id="4dad1-109">In **Prepare source**, click **+ VMM** to add a VMM server.</span></span>

    ![A forrás beállítása](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="4dad1-111">A **Kiszolgáló hozzáadása** panelen ellenőrizze, hogy a **Kiszolgálótípus** mezőben a **System Center VMM-kiszolgáló** érték látható-e, illetve, hogy a VMM-kiszolgáló megfelel-e [az előfeltételeknek és az URL-követelményeknek](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="4dad1-111">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that the VMM server meets the [prerequisites and URL requirements](#prerequisites).</span></span>
4. <span data-ttu-id="4dad1-112">Töltse le az Azure Site Recovery Provider telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="4dad1-112">Download the Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="4dad1-113">Töltse le a regisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="4dad1-113">Download the registration key.</span></span> <span data-ttu-id="4dad1-114">Erre a telepítő futtatása során lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="4dad1-114">You need this when you run setup.</span></span> <span data-ttu-id="4dad1-115">A kulcs a generálásától számított öt napig érvényes.</span><span class="sxs-lookup"><span data-stu-id="4dad1-115">The key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-the-provider-on-the-vmm-server"></a><span data-ttu-id="4dad1-117">A szolgáltató telepítése a VMM-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="4dad1-117">Install the Provider on the VMM server</span></span>

1. <span data-ttu-id="4dad1-118">Futtassa a szolgáltató telepítőfájlját a VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="4dad1-118">Run the Provider setup file on the VMM server.</span></span>
2. <span data-ttu-id="4dad1-119">A **Microsoft Update** lapon kérheti a frissítések beszerzését, így a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Providerhez kiadott frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="4dad1-119">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="4dad1-120">A **Telepítés** lapon tetszés szerint fogadja el vagy módosítsa a Provider alapértelmezett telepítési helyét, majd kattintson a **Telepítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4dad1-120">In **Installation**, accept or modify the default Provider installation location and click **Install**.</span></span>

    ![Telepítés helye](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. <span data-ttu-id="4dad1-122">A telepítés befejezését követően a VMM-kiszolgálónak a tárolóban való regisztrálásához kattintson a **Regisztráció** gombra.</span><span class="sxs-lookup"><span data-stu-id="4dad1-122">When installation finishes, click **Register** to register the VMM server in the vault.</span></span>
5. <span data-ttu-id="4dad1-123">A **Tároló beállításai** lapon kattintson a **Tallózás** gombra, és válassza ki a tároló kulcsfájlját.</span><span class="sxs-lookup"><span data-stu-id="4dad1-123">In the **Vault Settings** page, click **Browse** to select the vault key file.</span></span> <span data-ttu-id="4dad1-124">Adja meg az Azure Site Recovery-előfizetést és a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="4dad1-124">Specify the Azure Site Recovery subscription and the vault name.</span></span>

    ![Kiszolgáló regisztrációja](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. <span data-ttu-id="4dad1-126">Az **Internetkapcsolat** beállításnál adja meg, hogy a VMM-kiszolgálón futó Provider hogyan csatlakozzon a Site Recoveryhez az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="4dad1-126">In **Internet Connection**, specify how the Provider running on the VMM server will connect to Site Recovery over the internet.</span></span>

   * <span data-ttu-id="4dad1-127">Ha azt szeretné, hogy a Provider közvetlenül kapcsolódjon, válassza a **Közvetlen csatlakozás az Azure Site Recoveryhez proxykiszolgáló nélkül** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4dad1-127">If you want the Provider to connect directly, select **Connect directly to Azure Site Recovery without a proxy**.</span></span>
   * <span data-ttu-id="4dad1-128">Ha a meglévő proxy hitelesítést igényel, illetve ha egyéni proxyt szeretne használni, válassza a **Csatlakozás az Azure Site Recoveryhez proxykiszolgálóval** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4dad1-128">If your existing proxy requires authentication, or you want to use a custom proxy, select **Connect to Azure Site Recovery using a proxy server**.</span></span>
   * <span data-ttu-id="4dad1-129">Ha az egyéni proxy használatát választja, meg kell adnia a címet, a portot és a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4dad1-129">If you use a custom proxy, specify the address, port, and credentials.</span></span>
   * <span data-ttu-id="4dad1-130">Ha proxyt használt, ellenőrizze, hogy engedélyezte-e az [előfeltételekben](#on-premises-prerequisites) leírt URL-címek elérését.</span><span class="sxs-lookup"><span data-stu-id="4dad1-130">If you're using a proxy, you should have already allowed the URLs described in [prerequisites](#on-premises-prerequisites).</span></span>
   * <span data-ttu-id="4dad1-131">Ha egyéni proxyt használ, a rendszer automatikusan létrehoz egy, a megadott hitelesítő adatokat alkalmazó VMM RunAs-fiókot (DRAProxyAccount).</span><span class="sxs-lookup"><span data-stu-id="4dad1-131">If you use a custom proxy, a VMM RunAs account (DRAProxyAccount) will be created automatically using the specified proxy credentials.</span></span> <span data-ttu-id="4dad1-132">Állítsa be úgy a proxykiszolgálót, hogy ez a fiók elvégezhesse a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="4dad1-132">Configure the proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="4dad1-133">A VMM RunAs-fiók beállításait a VMM-konzolban módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="4dad1-133">The VMM RunAs account settings can be modified in the VMM console.</span></span> <span data-ttu-id="4dad1-134">A **Beállítások** menüben bontsa ki a **Biztonság** > **Futtató fiókok** lehetőséget, majd módosítsa a DRAProxyAccount jelszavát.</span><span class="sxs-lookup"><span data-stu-id="4dad1-134">In **Settings**, expand **Security** > **Run As Accounts**, and then modify the password for DRAProxyAccount.</span></span> <span data-ttu-id="4dad1-135">A beállítás akkor lép érvénybe, ha újraindítja a VMM szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4dad1-135">You’ll need to restart the VMM service so that this setting takes effect.</span></span>

     ![internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. <span data-ttu-id="4dad1-137">Fogadja el vagy módosítsa az adatok titkosításához automatikusan létrehozott SSL-tanúsítvány helyét.</span><span class="sxs-lookup"><span data-stu-id="4dad1-137">Accept or modify the location of an SSL certificate that’s automatically generated for data encryption.</span></span> <span data-ttu-id="4dad1-138">Ez a tanúsítvány akkor használatos, ha bekapcsolja az Azure által védett felhőre vonatkozó adattitkosítást az Azure Site Recovery portálon.</span><span class="sxs-lookup"><span data-stu-id="4dad1-138">This certificate is used if you enable data encryption for a cloud protected by Azure in the Azure Site Recovery portal.</span></span> <span data-ttu-id="4dad1-139">Őrizze biztonságos helyen a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="4dad1-139">Keep this certificate safe.</span></span> <span data-ttu-id="4dad1-140">Ha feladatátvételt végez az Azure-ba, és korábban bekapcsolta az adattitkosítást, szüksége lesz rá a titkosítás feloldásához.</span><span class="sxs-lookup"><span data-stu-id="4dad1-140">When you run a failover to Azure you’ll need it to decrypt, if data encryption is enabled.</span></span>
8. <span data-ttu-id="4dad1-141">A **Kiszolgáló neve** mezőben adjon meg egy, a tárolóban regisztrált VMM-kiszolgálót azonosító rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="4dad1-141">In **Server name**, specify a friendly name to identify the VMM server in the vault.</span></span> <span data-ttu-id="4dad1-142">Fürtkonfiguráció használata esetén adja meg a VMM-fürtszerepkör nevét.</span><span class="sxs-lookup"><span data-stu-id="4dad1-142">In a cluster configuration, specify the VMM cluster role name.</span></span>
9. <span data-ttu-id="4dad1-143">Jelölje be a **Felhőmetaadatok szinkronizálása** jelölőnégyzetet, ha szeretné a VMM-kiszolgálón futó összes felhő metaadatait szinkronizálni a tárolóval.</span><span class="sxs-lookup"><span data-stu-id="4dad1-143">Enable **Sync cloud metadata**, if you want to synchronize metadata for all clouds on the VMM server with the vault.</span></span> <span data-ttu-id="4dad1-144">Ezt a műveletet kiszolgálónként csak egyszer szükséges elvégezni.</span><span class="sxs-lookup"><span data-stu-id="4dad1-144">This action only needs to happen once on each server.</span></span> <span data-ttu-id="4dad1-145">Ha nem szeretné az összes felhőt szinkronizálni, ne jelölje be a jelölőnégyzetet, és szinkronizálja egyenként a felhőket a VMM-konzolban, a felhők tulajdonságainál.</span><span class="sxs-lookup"><span data-stu-id="4dad1-145">If you don't want to synchronize all clouds, you can leave this setting unchecked and synchronize each cloud individually in the cloud properties in the VMM console.</span></span> <span data-ttu-id="4dad1-146">A folyamat befejezéséhez kattintson a **Regisztráció** elemre.</span><span class="sxs-lookup"><span data-stu-id="4dad1-146">Click **Register** to complete the process.</span></span>

    ![Kiszolgáló regisztrációja](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. <span data-ttu-id="4dad1-148">Elindul a regisztráció.</span><span class="sxs-lookup"><span data-stu-id="4dad1-148">Registration starts.</span></span> <span data-ttu-id="4dad1-149">A regisztráció befejezését követően a kiszolgáló megjelenik a **Site Recovery-infrastruktúra** > **VMM-kiszolgálók** panelen.</span><span class="sxs-lookup"><span data-stu-id="4dad1-149">After registration finishes, the server is displayed in **Site Recovery Infrastructure** > **VMM Servers**.</span></span>


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a><span data-ttu-id="4dad1-150">Az Azure Recovery Services Agent ügynök telepítése a Hyper-V gazdagépekre</span><span class="sxs-lookup"><span data-stu-id="4dad1-150">Install the Azure Recovery Services agent on Hyper-V hosts</span></span>

1. <span data-ttu-id="4dad1-151">A Provider beállítását követően töltse le az Azure Recovery Services Agent telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="4dad1-151">After you've set up the Provider, you need to download the installation file for the Azure Recovery Services agent.</span></span> <span data-ttu-id="4dad1-152">Végezze el a telepítést a CMM-felhőben futó összes Hyper-V-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="4dad1-152">Run setup on each Hyper-V server in the VMM cloud.</span></span>

    ![Hyper-V helyek](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. <span data-ttu-id="4dad1-154">Az **Előfeltételek ellenőrzése** részben kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="4dad1-154">In **Prerequisites Check**, click **Next**.</span></span> <span data-ttu-id="4dad1-155">A rendszer automatikusan telepíti a hiányzó előfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="4dad1-155">Any missing prerequisites will be automatically installed.</span></span>

    ![Recovery Services Agent, előfeltételek](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. <span data-ttu-id="4dad1-157">A **Telepítési beállítások** részben fogadja el vagy módosítsa a telepítés, illetve a gyorsítótár helyét.</span><span class="sxs-lookup"><span data-stu-id="4dad1-157">In **Installation Settings**, accept or modify the installation location, and the cache location.</span></span> <span data-ttu-id="4dad1-158">A gyorsítótár már akár 5 GB-os kapacitású meghajtókon is beállítható, de javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legyen legalább 600 GB szabad hely.</span><span class="sxs-lookup"><span data-stu-id="4dad1-158">You can configure the cache on a drive that has at least 5 GB of storage available but we recommend a cache drive with 600 GB or more of free space.</span></span> <span data-ttu-id="4dad1-159">Ezt követően kattintson a **Telepítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4dad1-159">Then click **Install**.</span></span>
4. <span data-ttu-id="4dad1-160">A telepítés befejezését követően kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4dad1-160">After installation is complete, click **Close** to finish.</span></span>

    ![A MARS Agent regisztrálása](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a><span data-ttu-id="4dad1-162">Parancssori telepítés</span><span class="sxs-lookup"><span data-stu-id="4dad1-162">Command line installation</span></span>
<span data-ttu-id="4dad1-163">A Microsoft Azure Recovery Services Agent a következő paranccsal telepíthető a parancssorból:</span><span class="sxs-lookup"><span data-stu-id="4dad1-163">You can install the Microsoft Azure Recovery Services Agent from command line using the following command:</span></span>

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a><span data-ttu-id="4dad1-164">Internetes proxyn keresztüli elérés beállítása a Site Recoveryhez Hyper-V gazdagépekről</span><span class="sxs-lookup"><span data-stu-id="4dad1-164">Set up internet proxy access to Site Recovery from Hyper-V hosts</span></span>

<span data-ttu-id="4dad1-165">A Hyper-V gazdagépeken futó Recovery Services Agent ügynöknek a virtuális gépek replikálása céljából az interneten keresztül el kell érnie az Azure-t.</span><span class="sxs-lookup"><span data-stu-id="4dad1-165">The Recovery Services agent running on Hyper-V hosts needs internet access to Azure for VM replication.</span></span> <span data-ttu-id="4dad1-166">Ha proxyn keresztül éri el az internetet, adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="4dad1-166">If you're accessing the internet through a proxy, set it up as follows:</span></span>

1. <span data-ttu-id="4dad1-167">Nyissa meg a Microsoft Azure Backup MMC beépülő modult a Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="4dad1-167">Open the Microsoft Azure Backup MMC snap-in on the Hyper-V host.</span></span> <span data-ttu-id="4dad1-168">Alapértelmezés szerint a Microsoft Azure Backup parancsikonja az asztalon vagy a következő mappában érhető el: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="4dad1-168">By default, a shortcut for Microsoft Azure Backup is available on the desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="4dad1-169">Kattintson a beépülő modul **Tulajdonságok módosítása** elemére.</span><span class="sxs-lookup"><span data-stu-id="4dad1-169">In the snap-in, click **Change Properties**.</span></span>
3. <span data-ttu-id="4dad1-170">A **Proxykonfiguráció** lapon adja meg a proxykiszolgálóra vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="4dad1-170">On the **Proxy Configuration** tab, specify proxy server information.</span></span>

    ![A MARS Agent regisztrálása](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. <span data-ttu-id="4dad1-172">Ellenőrizze, hogy az ügynök el tudja-e érni az [előfeltételek](#on-premises-prerequisites) között megadott URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="4dad1-172">Check that the agent can reach the URLs described in the [prerequisites](#on-premises-prerequisites).</span></span>

## <a name="set-up-the-target-environment"></a><span data-ttu-id="4dad1-173">A célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="4dad1-173">Set up the target environment</span></span>
<span data-ttu-id="4dad1-174">Adja meg a replikációhoz használni kívánt Azure-tárfiókot, valamint az Azure-hálózatot, amelyhez az Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="4dad1-174">Specify the Azure storage account to be used for replication, and the Azure network to which Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="4dad1-175">Kattintson **Az infrastruktúra előkészítése** > **Cél** elemre, és válassza ki az előfizetést és az erőforráscsoportot, ahol a feladatátviteli virtuális gépeket létre szeretné hozni.</span><span class="sxs-lookup"><span data-stu-id="4dad1-175">Click **Prepare infrastructure** > **Target**, select the subscription and the resource group where you want to create the failed over virtual machines.</span></span> <span data-ttu-id="4dad1-176">Kattintson a feladatátvételi virtuális gépekre használni kívánt Azure üzembe helyezési modellre (klasszikus vagy erőforrás-kezelés).</span><span class="sxs-lookup"><span data-stu-id="4dad1-176">Choose the deployment model that you want to use in Azure (classic or resource management) for the failed over virtual machines.</span></span>

    ![Tárolás](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. <span data-ttu-id="4dad1-178">A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.</span><span class="sxs-lookup"><span data-stu-id="4dad1-178">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    ![Tárolás](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. <span data-ttu-id="4dad1-180">Ha még nem hozott létre tárfiókot, és ezt szeretné most megtenni a Resource Managerben, kattintson a **+Tárfiók** elemre.</span><span class="sxs-lookup"><span data-stu-id="4dad1-180">If you haven't created a storage account, and you want to create one using Resource Manager, click **+Storage account** to do that inline.</span></span>  <span data-ttu-id="4dad1-181">A **Tárfiók létrehozása** panelen adja meg a fiók nevét és típusát, az előfizetést, valamint a helyet.</span><span class="sxs-lookup"><span data-stu-id="4dad1-181">On the **Create storage account** blade specify an account name, type, subscription, and location.</span></span> <span data-ttu-id="4dad1-182">A fióknak és a Recovery Services-tárolónak ugyanazon a helyen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4dad1-182">The account should be in the same location as the Recovery Services vault.</span></span>

   ![Tárolás](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * <span data-ttu-id="4dad1-184">Ha a klasszikus modellt használó tárfiókot szeretne létrehozni, azt az Azure-portálon teheti meg.</span><span class="sxs-lookup"><span data-stu-id="4dad1-184">If you want to create a storage account using the classic model, do that in the Azure portal.</span></span> [<span data-ttu-id="4dad1-185">További információ</span><span class="sxs-lookup"><span data-stu-id="4dad1-185">Learn more</span></span>](../storage/common/storage-create-storage-account.md)
   * <span data-ttu-id="4dad1-186">Ha Prémium szintű tárfiókot használ a replikált adatok tárolására, a helyszíni adatokban bekövetkező változásokat rögzítő replikálási naplók tárolására be kell állítania egy standard tárfiókot is.</span><span class="sxs-lookup"><span data-stu-id="4dad1-186">If you’re using a premium storage account for replicated data, set up an additional standard storage account, to store replication logs that capture ongoing changes to on-premises data.</span></span>
5. <span data-ttu-id="4dad1-187">Ha még nem hozott létre Azure-hálózatot, és ezt szeretné most megtenni a Resource Managerben, kattintson a **+Hálózat** elemre.</span><span class="sxs-lookup"><span data-stu-id="4dad1-187">If you haven't created an Azure network, and you want to create one using Resource Manager, click **+Network** to do that inline.</span></span> <span data-ttu-id="4dad1-188">A **Virtuális hálózat létrehozása** panelen adja meg a hálózat nevét, a címtartományt, az alhálózati adatokat, az előfizetést, valamint a helyet.</span><span class="sxs-lookup"><span data-stu-id="4dad1-188">On the **Create virtual network** blade specify a network name, address range, subnet details, subscription, and location.</span></span> <span data-ttu-id="4dad1-189">A hálózatnak és a Recovery Services-tárolónak ugyanazon a helyen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4dad1-189">The network should be in the same location as the Recovery Services vault.</span></span>

   ![Network (Hálózat)](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   <span data-ttu-id="4dad1-191">Ha a klasszikus modellt használó hálózatot szeretne létrehozni, azt az Azure-portálon teheti meg.</span><span class="sxs-lookup"><span data-stu-id="4dad1-191">If you want to create a network using the classic model, do that in the Azure portal.</span></span> <span data-ttu-id="4dad1-192">[További információk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="4dad1-192">[Learn more](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span></span>





## <a name="next-steps"></a><span data-ttu-id="4dad1-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4dad1-193">Next steps</span></span>

<span data-ttu-id="4dad1-194">Ugrás a [9. lépés: hálózatleképezés konfigurálása](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="4dad1-194">Go to [Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>
