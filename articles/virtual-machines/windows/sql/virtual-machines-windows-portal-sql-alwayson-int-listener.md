---
title: "Hozzon létre egy SQL Server rendelkezésre állási csoport figyelőjének az Azure virtuális gépeken |} Microsoft Docs"
description: "Részletes útmutatást ad a figyelő egy Always On rendelkezésre állási csoport létrehozása az SQL Server Azure virtuális gépeken"
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/01/2017
ms.author: mikeray
ms.openlocfilehash: 09fed7e785708d4afe64905de973becc188181d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a><span data-ttu-id="4a20b-103">A terheléselosztó egy Always On rendelkezésre állási csoport konfigurálása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4a20b-103">Configure a load balancer for an Always On availability group in Azure</span></span>
<span data-ttu-id="4a20b-104">Ez a cikk azt ismerteti, hogyan egy terheléselosztót egy SQL Server Always On rendelkezésre állási csoport létrehozása az Azure Resource Manager rendszert futtató Azure virtuális gépekben.</span><span class="sxs-lookup"><span data-stu-id="4a20b-104">This article explains how to create a load balancer for a SQL Server Always On availability group in Azure virtual machines that are running with Azure Resource Manager.</span></span> <span data-ttu-id="4a20b-105">Rendelkezésre állási csoport szükséges egy adott terheléselosztóhoz, ha az SQL Server-példányok áll az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="4a20b-105">An availability group requires a load balancer when the SQL Server instances are on Azure virtual machines.</span></span> <span data-ttu-id="4a20b-106">A terheléselosztó a rendelkezésre állási csoport figyelője az IP-cím tárolja.</span><span class="sxs-lookup"><span data-stu-id="4a20b-106">The load balancer stores the IP address for the availability group listener.</span></span> <span data-ttu-id="4a20b-107">Rendelkezésre állási csoport több régióba is, ha minden egyes régió egy terhelés-kiegyenlítő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4a20b-107">If an availability group spans multiple regions, each region needs a load balancer.</span></span>

<span data-ttu-id="4a20b-108">Ez a feladat befejezéséhez szükség lehet a Resource Manager rendszert futtató Azure virtuális gépeken telepített SQL Server rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="4a20b-108">To complete this task, you need to have a SQL Server availability group deployed on Azure virtual machines that are running with Resource Manager.</span></span> <span data-ttu-id="4a20b-109">Mindkét SQL Server virtuális gépek ugyanabban a rendelkezésre állási csoportba kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="4a20b-109">Both SQL Server virtual machines must belong to the same availability set.</span></span> <span data-ttu-id="4a20b-110">Használhatja a [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) automatikus létrehozásához a rendelkezésre állási csoportot az erőforrás-kezelőben.</span><span class="sxs-lookup"><span data-stu-id="4a20b-110">You can use the [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) to automatically create the availability group in Resource Manager.</span></span> <span data-ttu-id="4a20b-111">Ez a sablon automatikusan létrehoz egy belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="4a20b-111">This template automatically creates an internal load balancer for you.</span></span> 

<span data-ttu-id="4a20b-112">Ha kívánja, akkor [manuálisan konfigurálnia a rendelkezésre állási csoport](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="4a20b-112">If you prefer, you can [manually configure an availability group](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

<span data-ttu-id="4a20b-113">Ez a cikk megköveteli, hogy a rendelkezésre állási csoportok már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="4a20b-113">This article requires that your availability groups are already configured.</span></span>  

<span data-ttu-id="4a20b-114">Kapcsolódó témakörök az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="4a20b-114">Related topics include:</span></span>

* [<span data-ttu-id="4a20b-115">Always On rendelkezésre állási csoportok konfigurálása az Azure virtuális gép (GUI)</span><span class="sxs-lookup"><span data-stu-id="4a20b-115">Configure Always On availability groups in Azure VM (GUI)</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [<span data-ttu-id="4a20b-116">Egy VNet – VNet-kapcsolat beállítása az Azure Resource Manager és a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="4a20b-116">Configure a VNet-to-VNet connection by using Azure Resource Manager and PowerShell</span></span>](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

<span data-ttu-id="4a20b-117">Érdekében ez a cikk keresztül, hozzon létre, és a terheléselosztó konfigurálása az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4a20b-117">By walking through this article, you create and configure a load balancer in the Azure portal.</span></span> <span data-ttu-id="4a20b-118">A folyamat befejezése után, akkor a fürt használja az IP-címet a terheléselosztóról a rendelkezésre állási csoport figyelőjének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-118">After the process is complete, you configure the cluster to use the IP address from the load balancer for the availability group listener.</span></span>

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a><span data-ttu-id="4a20b-119">Hozza létre és konfigurálja a terheléselosztó az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4a20b-119">Create and configure the load balancer in the Azure portal</span></span>
<span data-ttu-id="4a20b-120">Ez a tevékenység része tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4a20b-120">In this portion of the task, do the following:</span></span>

1. <span data-ttu-id="4a20b-121">Az Azure portálon a terheléselosztó létrehozása, és konfigurálja az IP-cím.</span><span class="sxs-lookup"><span data-stu-id="4a20b-121">In the Azure portal, create the load balancer and configure the IP address.</span></span>
2. <span data-ttu-id="4a20b-122">A háttér-készlet beállítása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-122">Configure the back-end pool.</span></span>
3. <span data-ttu-id="4a20b-123">A mintavétel létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-123">Create the probe.</span></span> 
4. <span data-ttu-id="4a20b-124">A terheléselosztási szabályok beállítása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-124">Set the load balancing rules.</span></span>

> [!NOTE]
> <span data-ttu-id="4a20b-125">Ha az SQL Server-példányok több erőforráscsoportok és régiókban, hajtsa végre az kétszer, egyszer minden erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="4a20b-125">If the SQL Server instances are in multiple resource groups and regions, perform each step twice, once in each resource group.</span></span>
> 
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a><span data-ttu-id="4a20b-126">1. lépés: A terheléselosztó létrehozása, és az IP-cím konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a20b-126">Step 1: Create the load balancer and configure the IP address</span></span>
<span data-ttu-id="4a20b-127">Először hozza létre a terheléselosztó hasonló adataival.</span><span class="sxs-lookup"><span data-stu-id="4a20b-127">First, create the load balancer.</span></span> 

1. <span data-ttu-id="4a20b-128">Nyissa meg az SQL Server virtuális gépeket tartalmazó erőforráscsoportot az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4a20b-128">In the Azure portal, open the resource group that contains the SQL Server virtual machines.</span></span> 

2. <span data-ttu-id="4a20b-129">Kattintson az erőforráscsoportot, **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-129">In the resource group, click **Add**.</span></span>

3. <span data-ttu-id="4a20b-130">Keresse meg **terheléselosztó** majd, a keresési eredmények **terheléselosztó**, által közzétett **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-130">Search for **load balancer** and then, in the search results, select **Load Balancer**, which is published by **Microsoft**.</span></span>

4. <span data-ttu-id="4a20b-131">Az a **terheléselosztó** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-131">On the **Load Balancer** blade, click **Create**.</span></span>

5. <span data-ttu-id="4a20b-132">Az a **létrehozás terheléselosztó** párbeszédpanel az alábbiak szerint adja meg a terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="4a20b-132">In the **Create load balancer** dialog box, configure the load balancer as follows:</span></span>

   | <span data-ttu-id="4a20b-133">Beállítás</span><span class="sxs-lookup"><span data-stu-id="4a20b-133">Setting</span></span> | <span data-ttu-id="4a20b-134">Érték</span><span class="sxs-lookup"><span data-stu-id="4a20b-134">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="4a20b-135">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-135">**Name**</span></span> |<span data-ttu-id="4a20b-136">A load balancer képviselő szöveges nevét.</span><span class="sxs-lookup"><span data-stu-id="4a20b-136">A text name representing the load balancer.</span></span> <span data-ttu-id="4a20b-137">Például **sqlLB**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-137">For example, **sqlLB**.</span></span> |
   | <span data-ttu-id="4a20b-138">**Típus**</span><span class="sxs-lookup"><span data-stu-id="4a20b-138">**Type**</span></span> |<span data-ttu-id="4a20b-139">**Belső**: a legtöbb megvalósítások belső terheléselosztót, lehetővé teszi a rendelkezésre állási csoporthoz való csatlakozáshoz a virtuális hálózaton belül alkalmazások használja.</span><span class="sxs-lookup"><span data-stu-id="4a20b-139">**Internal**: Most implementations use an internal load balancer, which allows applications within the same virtual network to connect to the availability group.</span></span>  </br> <span data-ttu-id="4a20b-140">**Külső**: lehetővé teszi olyan alkalmazások nyilvános internetkapcsolaton keresztül a rendelkezésre állási csoporthoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="4a20b-140">**External**: Allows applications to connect to the availability group through a public Internet connection.</span></span> |
   | <span data-ttu-id="4a20b-141">**Virtuális hálózat**</span><span class="sxs-lookup"><span data-stu-id="4a20b-141">**Virtual network**</span></span> |<span data-ttu-id="4a20b-142">Válassza ki a virtuális hálózat, amely az SQL Server intances szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="4a20b-142">Select the virtual network that the SQL Server intances are in.</span></span> |
   | <span data-ttu-id="4a20b-143">**Alhálózat**</span><span class="sxs-lookup"><span data-stu-id="4a20b-143">**Subnet**</span></span> |<span data-ttu-id="4a20b-144">Válassza ki az alhálózatot, amely az SQL Server-példányok szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="4a20b-144">Select the subnet that the SQL Server instances are in.</span></span> |
   | <span data-ttu-id="4a20b-145">**IP-cím hozzárendelése**</span><span class="sxs-lookup"><span data-stu-id="4a20b-145">**IP address assignment**</span></span> |<span data-ttu-id="4a20b-146">**Statikus**</span><span class="sxs-lookup"><span data-stu-id="4a20b-146">**Static**</span></span> |
   | <span data-ttu-id="4a20b-147">**Magánhálózati IP-cím**</span><span class="sxs-lookup"><span data-stu-id="4a20b-147">**Private IP address**</span></span> |<span data-ttu-id="4a20b-148">Adja meg az alhálózat elérhető IP-címeit.</span><span class="sxs-lookup"><span data-stu-id="4a20b-148">Specify an available IP address from the subnet.</span></span> <span data-ttu-id="4a20b-149">Az IP-címet használja, a figyelő a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="4a20b-149">Use this IP address when you create a listener on the cluster.</span></span> <span data-ttu-id="4a20b-150">Egy PowerShell-parancsfájlt, a cikk későbbi részében használni ezt a címet a `$ILBIP` változó.</span><span class="sxs-lookup"><span data-stu-id="4a20b-150">In a PowerShell script, later in this article, use this address for the `$ILBIP` variable.</span></span> |
   | <span data-ttu-id="4a20b-151">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="4a20b-151">**Subscription**</span></span> |<span data-ttu-id="4a20b-152">Ha több előfizetéssel rendelkezik, ez a mező jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="4a20b-152">If you have multiple subscriptions, this field might appear.</span></span> <span data-ttu-id="4a20b-153">Válassza ki az előfizetést, amelyet hozzá szeretne rendelni ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="4a20b-153">Select the subscription that you want to associate with this resource.</span></span> <span data-ttu-id="4a20b-154">Akkor általában a rendelkezésre állási csoporthoz tartozó összes erőforrás tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="4a20b-154">It is normally the same subscription as all the resources for the availability group.</span></span> |
   | <span data-ttu-id="4a20b-155">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="4a20b-155">**Resource group**</span></span> |<span data-ttu-id="4a20b-156">Válassza ki az erőforráscsoportot, amelyek az SQL Server-példányokat.</span><span class="sxs-lookup"><span data-stu-id="4a20b-156">Select the resource group that the SQL Server instances are in.</span></span> |
   | <span data-ttu-id="4a20b-157">**Hely**</span><span class="sxs-lookup"><span data-stu-id="4a20b-157">**Location**</span></span> |<span data-ttu-id="4a20b-158">Válassza ki az Azure-beli hely, amely az SQL Server-példányok szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="4a20b-158">Select the Azure location that the SQL Server instances are in.</span></span> |

6. <span data-ttu-id="4a20b-159">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4a20b-159">Click **Create**.</span></span> 

<span data-ttu-id="4a20b-160">A load balancer az Azure létrehoz.</span><span class="sxs-lookup"><span data-stu-id="4a20b-160">Azure creates the load balancer.</span></span> <span data-ttu-id="4a20b-161">A load balancer egy adott hálózati, alhálózati, erőforráscsoportot és helyet tartozik.</span><span class="sxs-lookup"><span data-stu-id="4a20b-161">The load balancer belongs to a specific network, subnet, resource group, and location.</span></span> <span data-ttu-id="4a20b-162">Azure a feladat befejezése után ellenőrizze a terheléselosztási beállításokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="4a20b-162">After Azure completes the task, verify the load balancer settings in Azure.</span></span> 

### <a name="step-2-configure-the-back-end-pool"></a><span data-ttu-id="4a20b-163">2. lépés: A háttér-készlet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a20b-163">Step 2: Configure the back-end pool</span></span>
<span data-ttu-id="4a20b-164">Azure meghívja a háttér-címkészlet *háttérkészlet*.</span><span class="sxs-lookup"><span data-stu-id="4a20b-164">Azure calls the back-end address pool *backend pool*.</span></span> <span data-ttu-id="4a20b-165">Ebben az esetben a háttér-készlet a rendelkezésre állási csoport két SQL Server-példánya a címeket.</span><span class="sxs-lookup"><span data-stu-id="4a20b-165">In this case, the back-end pool is the addresses of the two SQL Server instances in your availability group.</span></span> 

1. <span data-ttu-id="4a20b-166">Az erőforráscsoport kattintson a terheléselosztóhoz, amely létrehozta.</span><span class="sxs-lookup"><span data-stu-id="4a20b-166">In your resource group, click the load balancer that you created.</span></span> 

2. <span data-ttu-id="4a20b-167">A **beállítások**, kattintson a **háttérkészletek**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-167">On **Settings**, click **Backend pools**.</span></span>

3. <span data-ttu-id="4a20b-168">A **háttérkészletek**, kattintson a **Hozzáadás** egy háttér-címkészlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-168">On **Backend pools**, click **Add** to create a back-end address pool.</span></span> 

4. <span data-ttu-id="4a20b-169">A **háttérkészlet hozzáadása**a **neve**, adjon meg egy nevet a háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="4a20b-169">On **Add backend pool**, under **Name**, type a name for the back-end pool.</span></span>

5. <span data-ttu-id="4a20b-170">A **virtuális gépek**, kattintson a **adja hozzá a virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-170">Under **Virtual machines**, click **Add a virtual machine**.</span></span> 

6. <span data-ttu-id="4a20b-171">A **válassza ki a virtuális gépek**, kattintson a **rendelkezésre állási csoport kiválasztása**, és adja meg a rendelkezésre állási csoport, hogy az SQL Server virtuális gépek tartozik.</span><span class="sxs-lookup"><span data-stu-id="4a20b-171">Under **Choose virtual machines**, click **Choose an availability set**, and then specify the availability set that the SQL Server virtual machines belong to.</span></span>

7. <span data-ttu-id="4a20b-172">A rendelkezésre állási csoport kiválasztása után kattintson **válassza ki a virtuális gépek**, jelölje ki a két virtuális gépeket üzemeltető SQL Server-példányok a rendelkezésre állási csoport, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-172">After you have chosen the availability set, click **Choose the virtual machines**, select the two virtual machines that host the SQL Server instances in the availability group, and then click **Select**.</span></span> 

8. <span data-ttu-id="4a20b-173">Kattintson a **OK** bezárásához a paneleket az **válassza ki a virtuális gépek**, és **háttérkészlet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-173">Click **OK** to close the blades for **Choose virtual machines**, and **Add backend pool**.</span></span> 

<span data-ttu-id="4a20b-174">Azure frissíti a háttér-címkészlet beállításait.</span><span class="sxs-lookup"><span data-stu-id="4a20b-174">Azure updates the settings for the back-end address pool.</span></span> <span data-ttu-id="4a20b-175">A rendelkezésre állási csoport már két SQL Server-példányokat készletét.</span><span class="sxs-lookup"><span data-stu-id="4a20b-175">Now your availability set has a pool of two SQL Server instances.</span></span>

### <a name="step-3-create-a-probe"></a><span data-ttu-id="4a20b-176">3. lépés: A mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a20b-176">Step 3: Create a probe</span></span>
<span data-ttu-id="4a20b-177">A mintavétel határozza meg, hogyan Azure ellenőrzi, amelyek az SQL Server-példányok éppen birtokolja a rendelkezésre állási csoport figyelőjét.</span><span class="sxs-lookup"><span data-stu-id="4a20b-177">The probe defines how Azure verifies which of the SQL Server instances currently owns the availability group listener.</span></span> <span data-ttu-id="4a20b-178">Azure-vizsgálat a szolgáltatás az IP-cím a mintavétel létrehozásakor meghatározó port alapján.</span><span class="sxs-lookup"><span data-stu-id="4a20b-178">Azure probes the service based on the IP address on a port that you define when you create the probe.</span></span>

1. <span data-ttu-id="4a20b-179">A terheléselosztóhoz **beállítások** panelen kattintson a **állapot-mintavételi csomagjai**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-179">On the load balancer **Settings** blade, click **Health probes**.</span></span> 

2. <span data-ttu-id="4a20b-180">Az a **állapot-mintavételi csomagjai** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-180">On the **Health probes** blade, click **Add**.</span></span>

3. <span data-ttu-id="4a20b-181">A mintavétel konfigurálása a **Hozzáadás mintavételi** panelen.</span><span class="sxs-lookup"><span data-stu-id="4a20b-181">Configure the probe on the **Add probe** blade.</span></span> <span data-ttu-id="4a20b-182">A mintavétel konfigurálása a következő értékeket használja:</span><span class="sxs-lookup"><span data-stu-id="4a20b-182">Use the following values to configure the probe:</span></span>

   | <span data-ttu-id="4a20b-183">Beállítás</span><span class="sxs-lookup"><span data-stu-id="4a20b-183">Setting</span></span> | <span data-ttu-id="4a20b-184">Érték</span><span class="sxs-lookup"><span data-stu-id="4a20b-184">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="4a20b-185">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-185">**Name**</span></span> |<span data-ttu-id="4a20b-186">A mintavétel képviselő szöveges nevét.</span><span class="sxs-lookup"><span data-stu-id="4a20b-186">A text name representing the probe.</span></span> <span data-ttu-id="4a20b-187">Például **SQLAlwaysOnEndPointProbe**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-187">For example, **SQLAlwaysOnEndPointProbe**.</span></span> |
   | <span data-ttu-id="4a20b-188">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-188">**Protocol**</span></span> |<span data-ttu-id="4a20b-189">**TCP**</span><span class="sxs-lookup"><span data-stu-id="4a20b-189">**TCP**</span></span> |
   | <span data-ttu-id="4a20b-190">**Port**</span><span class="sxs-lookup"><span data-stu-id="4a20b-190">**Port**</span></span> |<span data-ttu-id="4a20b-191">A rendelkezésre álló portot is használhat.</span><span class="sxs-lookup"><span data-stu-id="4a20b-191">You can use any available port.</span></span> <span data-ttu-id="4a20b-192">Például *59999*.</span><span class="sxs-lookup"><span data-stu-id="4a20b-192">For example, *59999*.</span></span> |
   | <span data-ttu-id="4a20b-193">**Időköz**</span><span class="sxs-lookup"><span data-stu-id="4a20b-193">**Interval**</span></span> |<span data-ttu-id="4a20b-194">*5*</span><span class="sxs-lookup"><span data-stu-id="4a20b-194">*5*</span></span> |
   | <span data-ttu-id="4a20b-195">**Sérült küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="4a20b-195">**Unhealthy threshold**</span></span> |<span data-ttu-id="4a20b-196">*2*</span><span class="sxs-lookup"><span data-stu-id="4a20b-196">*2*</span></span> |

4.  <span data-ttu-id="4a20b-197">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a20b-197">Click **OK**.</span></span> 

> [!NOTE]
> <span data-ttu-id="4a20b-198">Győződjön meg arról, hogy a megadott port meg nyitva a tűzfalon, mind az SQL Server-példányok.</span><span class="sxs-lookup"><span data-stu-id="4a20b-198">Make sure that the port you specify is open on the firewall of both SQL Server instances.</span></span> <span data-ttu-id="4a20b-199">Mindkét esetben szükséges egy bejövő forgalomra vonatkozó szabály, amelyekkel a TCP-portot.</span><span class="sxs-lookup"><span data-stu-id="4a20b-199">Both instances require an inbound rule for the TCP port that you use.</span></span> <span data-ttu-id="4a20b-200">További információkért lásd: [hozzáadása vagy szerkesztése tűzfalszabály](http://technet.microsoft.com/library/cc753558.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a20b-200">For more information, see [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx).</span></span> 
> 
> 

<span data-ttu-id="4a20b-201">Azure hoz létre a mintavételi, és azt teszteléséhez melyik SQL Server-példány van a rendelkezésre állási csoport figyelőjének használja.</span><span class="sxs-lookup"><span data-stu-id="4a20b-201">Azure creates the probe and then uses it to test which SQL Server instance has the listener for the availability group.</span></span>

### <a name="step-4-set-the-load-balancing-rules"></a><span data-ttu-id="4a20b-202">4. lépés: A terheléselosztási szabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="4a20b-202">Step 4: Set the load balancing rules</span></span>
<span data-ttu-id="4a20b-203">A terheléselosztási szabályok konfigurálása, hogy a terheléselosztó hogyan irányítja a forgalmat az SQL Server-példányokat.</span><span class="sxs-lookup"><span data-stu-id="4a20b-203">The load balancing rules configure how the load balancer routes traffic to the SQL Server instances.</span></span> <span data-ttu-id="4a20b-204">A terheléselosztóhoz engedélyezi a közvetlen kiszolgálói válasz, mert a két SQL Server-példányok csak az egyik a rendelkezésre állási csoport figyelőjének erőforrás tulajdonosa egyszerre.</span><span class="sxs-lookup"><span data-stu-id="4a20b-204">For this load balancer, you enable direct server return because only one of the two SQL Server instances owns the availability group listener resource at a time.</span></span>

1. <span data-ttu-id="4a20b-205">A terheléselosztóhoz **beállítások** panelen kattintson a **terheléselosztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-205">On the load balancer **Settings** blade, click **Load balancing rules**.</span></span> 

2. <span data-ttu-id="4a20b-206">Az a **terheléselosztási szabályok** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-206">On the **Load balancing rules** blade, click **Add**.</span></span>

3. <span data-ttu-id="4a20b-207">Az a **Hozzáadás terheléselosztási szabályok** panelen a terheléselosztási szabály konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-207">On the **Add load balancing rules** blade, configure the load balancing rule.</span></span> <span data-ttu-id="4a20b-208">A következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="4a20b-208">Use the following settings:</span></span> 

   | <span data-ttu-id="4a20b-209">Beállítás</span><span class="sxs-lookup"><span data-stu-id="4a20b-209">Setting</span></span> | <span data-ttu-id="4a20b-210">Érték</span><span class="sxs-lookup"><span data-stu-id="4a20b-210">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="4a20b-211">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-211">**Name**</span></span> |<span data-ttu-id="4a20b-212">A terheléselosztási szabályok képviselő szöveges nevét.</span><span class="sxs-lookup"><span data-stu-id="4a20b-212">A text name representing the load balancing rules.</span></span> <span data-ttu-id="4a20b-213">Például **SQLAlwaysOnEndPointListener**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-213">For example, **SQLAlwaysOnEndPointListener**.</span></span> |
   | <span data-ttu-id="4a20b-214">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-214">**Protocol**</span></span> |<span data-ttu-id="4a20b-215">**TCP**</span><span class="sxs-lookup"><span data-stu-id="4a20b-215">**TCP**</span></span> |
   | <span data-ttu-id="4a20b-216">**Port**</span><span class="sxs-lookup"><span data-stu-id="4a20b-216">**Port**</span></span> |<span data-ttu-id="4a20b-217">*1433*</span><span class="sxs-lookup"><span data-stu-id="4a20b-217">*1433*</span></span> |
   | <span data-ttu-id="4a20b-218">**Háttér-Port**</span><span class="sxs-lookup"><span data-stu-id="4a20b-218">**Backend Port**</span></span> |<span data-ttu-id="4a20b-219">*1433*. Rendszer figyelmen kívül hagyja ezt az értéket, mert ez a szabály **fix IP-Címek (közvetlen kiszolgálói válasz)**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-219">*1433*. This value is ignored because this rule uses **Floating IP (direct server return)**.</span></span> |
   | <span data-ttu-id="4a20b-220">**Hálózatfigyelő**</span><span class="sxs-lookup"><span data-stu-id="4a20b-220">**Probe**</span></span> |<span data-ttu-id="4a20b-221">A mintavétel létrehozott nevét használni a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="4a20b-221">Use the name of the probe that you created for this load balancer.</span></span> |
   | <span data-ttu-id="4a20b-222">**Munkamenet megőrzését**</span><span class="sxs-lookup"><span data-stu-id="4a20b-222">**Session persistence**</span></span> |<span data-ttu-id="4a20b-223">**Egyik sem**</span><span class="sxs-lookup"><span data-stu-id="4a20b-223">**None**</span></span> |
   | <span data-ttu-id="4a20b-224">**Üresjárati időkorlátja (perc)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-224">**Idle timeout (minutes)**</span></span> |<span data-ttu-id="4a20b-225">*4*</span><span class="sxs-lookup"><span data-stu-id="4a20b-225">*4*</span></span> |
   | <span data-ttu-id="4a20b-226">**Lebegőpontos IP (közvetlen kiszolgálói válasz)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-226">**Floating IP (direct server return)**</span></span> |<span data-ttu-id="4a20b-227">**Engedélyezve**</span><span class="sxs-lookup"><span data-stu-id="4a20b-227">**Enabled**</span></span> |

   > [!NOTE]
   > <span data-ttu-id="4a20b-228">Lehetséges, hogy a megadott beállítások panel le kell görgetnie.</span><span class="sxs-lookup"><span data-stu-id="4a20b-228">You might have to scroll down the blade to view all the settings.</span></span>
   > 

4. <span data-ttu-id="4a20b-229">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a20b-229">Click **OK**.</span></span> 
5. <span data-ttu-id="4a20b-230">Azure terheléselosztási szabályt konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="4a20b-230">Azure configures the load balancing rule.</span></span> <span data-ttu-id="4a20b-231">A terheléselosztó van konfigurálva irányíthatja a forgalmat a rendelkezésre állási csoport figyelőjének az SQL Server-példány számára.</span><span class="sxs-lookup"><span data-stu-id="4a20b-231">Now the load balancer is configured to route traffic to the SQL Server instance that hosts the listener for the availability group.</span></span> 

<span data-ttu-id="4a20b-232">Ezen a ponton az erőforráscsoport rendelkezik olyan terheléselosztóhoz, amely mindkét SQL Server-gépek csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="4a20b-232">At this point, the resource group has a load balancer that connects to both SQL Server machines.</span></span> <span data-ttu-id="4a20b-233">A terheléselosztó is tartalmazza az SQL Server Always On rendelkezésre állási csoport figyelőjét, IP-címet, hogy vagy a gép válaszolhassanak a rendelkezésre állási csoportok kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="4a20b-233">The load balancer also contains an IP address for the SQL Server Always On availability group listener, so that either machine can respond to requests for the availability groups.</span></span>

> [!NOTE]
> <span data-ttu-id="4a20b-234">Ha az SQL Server-példány két külön régióban, ismételje meg a más régióban.</span><span class="sxs-lookup"><span data-stu-id="4a20b-234">If your SQL Server instances are in two separate regions, repeat the steps in the other region.</span></span> <span data-ttu-id="4a20b-235">Minden egyes régió egy terheléselosztót igényel.</span><span class="sxs-lookup"><span data-stu-id="4a20b-235">Each region requires a load balancer.</span></span> 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a><span data-ttu-id="4a20b-236">A load balancer IP-címet a fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a20b-236">Configure the cluster to use the load balancer IP address</span></span>
<span data-ttu-id="4a20b-237">A következő lépés, hogy adja meg a figyelő a fürtön, és a figyelő online állapotba.</span><span class="sxs-lookup"><span data-stu-id="4a20b-237">The next step is to configure the listener on the cluster, and bring the listener online.</span></span> <span data-ttu-id="4a20b-238">Tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4a20b-238">Do the following:</span></span> 

1. <span data-ttu-id="4a20b-239">A rendelkezésre állási csoport figyelőjének létrehozásához a feladatátvevő fürtön.</span><span class="sxs-lookup"><span data-stu-id="4a20b-239">Create the availability group listener on the failover cluster.</span></span> 

2. <span data-ttu-id="4a20b-240">A figyelő online állapotba.</span><span class="sxs-lookup"><span data-stu-id="4a20b-240">Bring the listener online.</span></span>

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a><span data-ttu-id="4a20b-241">5. lépés: A rendelkezésre állási csoport figyelőjének létrehozása a feladatátvevő fürt</span><span class="sxs-lookup"><span data-stu-id="4a20b-241">Step 5: Create the availability group listener on the failover cluster</span></span>
<span data-ttu-id="4a20b-242">Ebben a lépésben kézzel létrehozhat a rendelkezésre állási csoport figyelőjének a Feladatátvevőfürt-kezelő és az SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="4a20b-242">In this step, you manually create the availability group listener in Failover Cluster Manager and SQL Server Management Studio.</span></span>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a><span data-ttu-id="4a20b-243">A figyelő konfigurációjának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4a20b-243">Verify the configuration of the listener</span></span>

<span data-ttu-id="4a20b-244">Ha a fürt erőforrásait, és a függőségek megfelelően vannak konfigurálva, a figyelő megtekintheti az SQL Server Management Studio kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4a20b-244">If the cluster resources and dependencies are correctly configured, you should be able to view the listener in SQL Server Management Studio.</span></span> <span data-ttu-id="4a20b-245">A figyelő port megadásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4a20b-245">To set the listener port, do the following:</span></span>

1. <span data-ttu-id="4a20b-246">Indítsa el az SQL Server Management Studio eszközt, és csatlakozzon az elsődleges másodpéldány.</span><span class="sxs-lookup"><span data-stu-id="4a20b-246">Start SQL Server Management Studio, and then connect to the primary replica.</span></span>

2. <span data-ttu-id="4a20b-247">Nyissa meg a **AlwaysOn magas rendelkezésre állás** > **rendelkezésre állási csoportok** > **rendelkezésre állási csoport figyelői**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-247">Go to **AlwaysOn High Availability** > **Availability Groups** > **Availability Group Listeners**.</span></span>  
    <span data-ttu-id="4a20b-248">Meg kell jelennie a figyelő nevét, amelyet a Feladatátvevőfürt-kezelőt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="4a20b-248">You should now see the listener name that you created in Failover Cluster Manager.</span></span> 

3. <span data-ttu-id="4a20b-249">Kattintson a jobb gombbal a figyelő nevét, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-249">Right-click the listener name, and then click **Properties**.</span></span>

4. <span data-ttu-id="4a20b-250">Az a **Port** mezőben adja meg a portszámot az elérhetőségi csoport figyelője az a korábban használt $EndpointPort használatával (az alapértelmezett 1433-as volt az), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-250">In the **Port** box, specify the port number for the availability group listener by using the $EndpointPort you used earlier (1433 was the default), and then click **OK**.</span></span>

<span data-ttu-id="4a20b-251">Most már rendelkezik egy rendelkezésre állási csoport erőforrás-kezelő módban futó Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="4a20b-251">You now have an availability group in Azure virtual machines running in Resource Manager mode.</span></span> 

## <a name="test-the-connection-to-the-listener"></a><span data-ttu-id="4a20b-252">Tesztelje a kapcsolatot a figyelő</span><span class="sxs-lookup"><span data-stu-id="4a20b-252">Test the connection to the listener</span></span>
<span data-ttu-id="4a20b-253">Tesztelje a kapcsolatot a következő módon:</span><span class="sxs-lookup"><span data-stu-id="4a20b-253">Test the connection by doing the following:</span></span>

1. <span data-ttu-id="4a20b-254">Az SQL Server-példányt, amely ugyanabban a virtuális hálózatban, de nem tulajdonosa a replika RDP.</span><span class="sxs-lookup"><span data-stu-id="4a20b-254">RDP to a SQL Server instance that is in the same virtual network, but does not own the replica.</span></span> <span data-ttu-id="4a20b-255">A kiszolgáló lehet a más SQL Server-példány a fürtben.</span><span class="sxs-lookup"><span data-stu-id="4a20b-255">This server can be the other SQL Server instance in the cluster.</span></span>

2. <span data-ttu-id="4a20b-256">Használjon **sqlcmd** segédprogram létrehozott kapcsolat ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="4a20b-256">Use **sqlcmd** utility to test the connection.</span></span> <span data-ttu-id="4a20b-257">Például az alábbi parancsfájlt hoz létre egy **sqlcmd** kapcsolatot az elsődleges másodpéldány, a figyelő a Windows-hitelesítés használatával:</span><span class="sxs-lookup"><span data-stu-id="4a20b-257">For example, the following script establishes a **sqlcmd** connection to the primary replica through the listener with Windows authentication:</span></span>
   
        sqlcmd -S <listenerName> -E

<span data-ttu-id="4a20b-258">Az SQLCMD kapcsolat automatikusan csatlakozik, amelyen az elsődleges replika SQL Server-példány.</span><span class="sxs-lookup"><span data-stu-id="4a20b-258">The SQLCMD connection automatically connects to the SQL Server instance that hosts the primary replica.</span></span> 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a><span data-ttu-id="4a20b-259">Egy IP-cím, egy további rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a20b-259">Create an IP address for an additional availability group</span></span>

<span data-ttu-id="4a20b-260">Egyes rendelkezésre állási csoport egy külön figyelő használja.</span><span class="sxs-lookup"><span data-stu-id="4a20b-260">Each availability group uses a separate listener.</span></span> <span data-ttu-id="4a20b-261">Minden egyes figyelő saját IP-címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4a20b-261">Each listener has its own IP address.</span></span> <span data-ttu-id="4a20b-262">Az azonos terheléselosztóhoz segítségével az IP-cím tárolásához további figyelők.</span><span class="sxs-lookup"><span data-stu-id="4a20b-262">Use the same load balancer to hold the IP address for additional listeners.</span></span> <span data-ttu-id="4a20b-263">Miután létrehozott egy rendelkezésre állási csoporthoz, az IP-cím hozzáadása a terheléselosztóhoz, és adja meg a figyelő.</span><span class="sxs-lookup"><span data-stu-id="4a20b-263">After you create an availability group, add the IP address to the load balancer, and then configure the listener.</span></span>

<span data-ttu-id="4a20b-264">IP-cím hozzáadása a terheléselosztó és az Azure portál, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4a20b-264">To add an IP address to a load balancer with the Azure portal, do the following:</span></span>

1. <span data-ttu-id="4a20b-265">Az Azure portálon nyissa meg a terheléselosztó tartalmazó erőforráscsoportot, és kattintson a terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="4a20b-265">In the Azure portal, open the resource group that contains the load balancer, and then click the load balancer.</span></span> 

2. <span data-ttu-id="4a20b-266">A **beállítások**, kattintson a **előtér-IP-címkészlet**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-266">Under **SETTINGS**, click **Frontend IP pool**, and then click **Add**.</span></span> 

3. <span data-ttu-id="4a20b-267">A **előtérbeli IP-cím hozzáadása**, rendelje hozzá az előtér nevét.</span><span class="sxs-lookup"><span data-stu-id="4a20b-267">Under **Add frontend IP address**, assign a name for the front end.</span></span> 

4. <span data-ttu-id="4a20b-268">Ellenőrizze, hogy a **virtuális hálózati** és a **alhálózati** ugyanazok, mint az SQL Server-példányokat.</span><span class="sxs-lookup"><span data-stu-id="4a20b-268">Verify that the **Virtual network** and the **Subnet** are the same as the SQL Server instances.</span></span>

5. <span data-ttu-id="4a20b-269">Az IP-címének beállítása a figyelőhöz.</span><span class="sxs-lookup"><span data-stu-id="4a20b-269">Set the IP address for the listener.</span></span> 
   
   >[!TIP]
   ><span data-ttu-id="4a20b-270">Az IP-cím beállítása statikus, és adjon meg egy címet, amely jelenleg nincs használatban az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="4a20b-270">You can set the IP address to static and type an address that is not currently used in the subnet.</span></span> <span data-ttu-id="4a20b-271">Másik lehetőségként az IP-címének beállítása a dinamikus, és mentse az új előtér-IP-címkészlet.</span><span class="sxs-lookup"><span data-stu-id="4a20b-271">Alternatively, you can set the IP address to dynamic and save the new front-end IP pool.</span></span> <span data-ttu-id="4a20b-272">Ha így tesz, az Azure-portálon automatikusan hozzárendeli az elérhető IP-címet a készletbe.</span><span class="sxs-lookup"><span data-stu-id="4a20b-272">When you do so, the Azure portal automatically assigns an available IP address to the pool.</span></span> <span data-ttu-id="4a20b-273">Ezután nyissa meg újra az előtér-IP-címkészletet, és módosítsa a hozzárendelési statikus.</span><span class="sxs-lookup"><span data-stu-id="4a20b-273">You can then reopen the front-end IP pool and change the assignment to static.</span></span> 

6. <span data-ttu-id="4a20b-274">Mentse az IP-címet a figyelőhöz.</span><span class="sxs-lookup"><span data-stu-id="4a20b-274">Save the IP address for the listener.</span></span> 

7. <span data-ttu-id="4a20b-275">Adja hozzá a állapotmintáihoz a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="4a20b-275">Add a health probe by using the following settings:</span></span>

   |<span data-ttu-id="4a20b-276">Beállítás</span><span class="sxs-lookup"><span data-stu-id="4a20b-276">Setting</span></span> |<span data-ttu-id="4a20b-277">Érték</span><span class="sxs-lookup"><span data-stu-id="4a20b-277">Value</span></span>
   |:-----|:----
   |<span data-ttu-id="4a20b-278">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-278">**Name**</span></span> |<span data-ttu-id="4a20b-279">A mintavétel azonosító nevet.</span><span class="sxs-lookup"><span data-stu-id="4a20b-279">A name to identify the probe.</span></span>
   |<span data-ttu-id="4a20b-280">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-280">**Protocol**</span></span> |<span data-ttu-id="4a20b-281">TCP</span><span class="sxs-lookup"><span data-stu-id="4a20b-281">TCP</span></span>
   |<span data-ttu-id="4a20b-282">**Port**</span><span class="sxs-lookup"><span data-stu-id="4a20b-282">**Port**</span></span> |<span data-ttu-id="4a20b-283">Egy nem használt TCP-port, amelyen az összes virtuális gép rendelkezésre kell állnia.</span><span class="sxs-lookup"><span data-stu-id="4a20b-283">An unused TCP port, which must be available on all virtual machines.</span></span> <span data-ttu-id="4a20b-284">Semmilyen más célra nem használható.</span><span class="sxs-lookup"><span data-stu-id="4a20b-284">It cannot be used for any other purpose.</span></span> <span data-ttu-id="4a20b-285">Két figyelői nem használhatja ugyanazt a mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="4a20b-285">No two listeners can use the same probe port.</span></span> 
   |<span data-ttu-id="4a20b-286">**Időköz**</span><span class="sxs-lookup"><span data-stu-id="4a20b-286">**Interval**</span></span> |<span data-ttu-id="4a20b-287">A mintavételi kísérletek közötti időtartam.</span><span class="sxs-lookup"><span data-stu-id="4a20b-287">The amount of time between probe attempts.</span></span> <span data-ttu-id="4a20b-288">Használja az alapértelmezett (5).</span><span class="sxs-lookup"><span data-stu-id="4a20b-288">Use the default (5).</span></span>
   |<span data-ttu-id="4a20b-289">**Sérült küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="4a20b-289">**Unhealthy threshold**</span></span> |<span data-ttu-id="4a20b-290">Egymást követő küszöbértékeket, amelyek a virtuális gép nem kell száma nem megfelelő állapotúnak számít.</span><span class="sxs-lookup"><span data-stu-id="4a20b-290">The number of consecutive thresholds that should fail before a virtual machine is considered unhealthy.</span></span>

8. <span data-ttu-id="4a20b-291">Kattintson a **OK** a mintavétel mentése.</span><span class="sxs-lookup"><span data-stu-id="4a20b-291">Click **OK** to save the probe.</span></span> 

9. <span data-ttu-id="4a20b-292">Terheléselosztási szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-292">Create a load balancing rule.</span></span> <span data-ttu-id="4a20b-293">Kattintson a **terheléselosztási szabályok**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-293">Click **Load balancing rules**, and then click **Add**.</span></span>

10. <span data-ttu-id="4a20b-294">Adja meg az új terheléselosztási szabályt a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="4a20b-294">Configure the new load balancing rule by using the following settings:</span></span>

   |<span data-ttu-id="4a20b-295">Beállítás</span><span class="sxs-lookup"><span data-stu-id="4a20b-295">Setting</span></span> |<span data-ttu-id="4a20b-296">Érték</span><span class="sxs-lookup"><span data-stu-id="4a20b-296">Value</span></span>
   |:-----|:----
   |<span data-ttu-id="4a20b-297">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-297">**Name**</span></span> |<span data-ttu-id="4a20b-298">A terheléselosztási szabályt azonosító nevet.</span><span class="sxs-lookup"><span data-stu-id="4a20b-298">A name to identify the load balancing rule.</span></span> 
   |<span data-ttu-id="4a20b-299">**Előtérbeli IP-cím**</span><span class="sxs-lookup"><span data-stu-id="4a20b-299">**Frontend IP address**</span></span> |<span data-ttu-id="4a20b-300">Válassza ki a létrehozott IP-cím.</span><span class="sxs-lookup"><span data-stu-id="4a20b-300">Select the IP address you created.</span></span> 
   |<span data-ttu-id="4a20b-301">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-301">**Protocol**</span></span> |<span data-ttu-id="4a20b-302">TCP</span><span class="sxs-lookup"><span data-stu-id="4a20b-302">TCP</span></span>
   |<span data-ttu-id="4a20b-303">**Port**</span><span class="sxs-lookup"><span data-stu-id="4a20b-303">**Port**</span></span> |<span data-ttu-id="4a20b-304">Az SQL Server-példányok által használt portot használja.</span><span class="sxs-lookup"><span data-stu-id="4a20b-304">Use the port that the SQL Server instances are using.</span></span> <span data-ttu-id="4a20b-305">Egy alapértelmezett példány 1433-as portot használja, kivéve, ha módosította az.</span><span class="sxs-lookup"><span data-stu-id="4a20b-305">A default instance uses port 1433, unless you changed it.</span></span> 
   |<span data-ttu-id="4a20b-306">**Háttér-port**</span><span class="sxs-lookup"><span data-stu-id="4a20b-306">**Backend port**</span></span> |<span data-ttu-id="4a20b-307">Ugyanazt az értéket, mint a **Port**.</span><span class="sxs-lookup"><span data-stu-id="4a20b-307">Use the same value as **Port**.</span></span>
   |<span data-ttu-id="4a20b-308">**Háttérkészlet**</span><span class="sxs-lookup"><span data-stu-id="4a20b-308">**Backend pool**</span></span> |<span data-ttu-id="4a20b-309">A virtuális gépek az SQL Server-példányokat tartalmazó készlet.</span><span class="sxs-lookup"><span data-stu-id="4a20b-309">The pool that contains the virtual machines with the SQL Server instances.</span></span> 
   |<span data-ttu-id="4a20b-310">**Állapotmintáihoz**</span><span class="sxs-lookup"><span data-stu-id="4a20b-310">**Health probe**</span></span> |<span data-ttu-id="4a20b-311">Válassza ki a létrehozott mintavétel.</span><span class="sxs-lookup"><span data-stu-id="4a20b-311">Choose the probe you created.</span></span>
   |<span data-ttu-id="4a20b-312">**Munkamenet megőrzését**</span><span class="sxs-lookup"><span data-stu-id="4a20b-312">**Session persistence**</span></span> |<span data-ttu-id="4a20b-313">None</span><span class="sxs-lookup"><span data-stu-id="4a20b-313">None</span></span>
   |<span data-ttu-id="4a20b-314">**Üresjárati időkorlátja (perc)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-314">**Idle timeout (minutes)**</span></span> |<span data-ttu-id="4a20b-315">Alapértelmezett (4)</span><span class="sxs-lookup"><span data-stu-id="4a20b-315">Default (4)</span></span>
   |<span data-ttu-id="4a20b-316">**Lebegőpontos IP (közvetlen kiszolgálói válasz)**</span><span class="sxs-lookup"><span data-stu-id="4a20b-316">**Floating IP (direct server return)**</span></span> | <span data-ttu-id="4a20b-317">Engedélyezve</span><span class="sxs-lookup"><span data-stu-id="4a20b-317">Enabled</span></span>

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a><span data-ttu-id="4a20b-318">Az új IP-címet használja a rendelkezésre állási csoport konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a20b-318">Configure the availability group to use the new IP address</span></span>

<span data-ttu-id="4a20b-319">A fürt konfigurálásának befejezéséhez, ismételje meg a lépéseket, amelyek az első rendelkezésre állási csoport elküldésekor követni.</span><span class="sxs-lookup"><span data-stu-id="4a20b-319">To finish configuring the cluster, repeat the steps that you followed when you made the first availability group.</span></span> <span data-ttu-id="4a20b-320">Ez azt jelenti, hogy konfigurálja a [az új IP-cím fürt](#configure-the-cluster-to-use-the-load-balancer-ip-address).</span><span class="sxs-lookup"><span data-stu-id="4a20b-320">That is, configure the [cluster to use the new IP address](#configure-the-cluster-to-use-the-load-balancer-ip-address).</span></span> 

<span data-ttu-id="4a20b-321">Miután hozzáadta az IP-címet a figyelőhöz, további rendelkezésre állási csoport konfigurálása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="4a20b-321">After you have added an IP address for the listener, configure the additional availability group by doing the following:</span></span> 

1. <span data-ttu-id="4a20b-322">Győződjön meg arról, hogy mindkét SQL Server virtuális gépen nyissa meg a mintavételi portot az új IP-címhez.</span><span class="sxs-lookup"><span data-stu-id="4a20b-322">Verify that the probe port for the new IP address is open on both SQL Server virtual machines.</span></span> 

2. <span data-ttu-id="4a20b-323">[A kezelő hozzáadása az ügyfél-hozzáférési pont](#addcap).</span><span class="sxs-lookup"><span data-stu-id="4a20b-323">[In Cluster Manager, add the client access point](#addcap).</span></span>

3. <span data-ttu-id="4a20b-324">[Az IP-erőforrás a rendelkezésre állási csoport konfigurálása](#congroup).</span><span class="sxs-lookup"><span data-stu-id="4a20b-324">[Configure the IP resource for the availability group](#congroup).</span></span>

   >[!IMPORTANT]
   ><span data-ttu-id="4a20b-325">Az IP-cím létrehozásakor használja a terheléselosztó hozzáadott IP-címét.</span><span class="sxs-lookup"><span data-stu-id="4a20b-325">When you create the IP address, use the IP address that you added to the load balancer.</span></span>  

4. <span data-ttu-id="4a20b-326">[Az SQL Server rendelkezésre állási csoport erőforrása függővé az ügyfél-hozzáférési pontokon](#dependencyGroup).</span><span class="sxs-lookup"><span data-stu-id="4a20b-326">[Make the SQL Server availability group resource dependent on the client access point](#dependencyGroup).</span></span>

5. <span data-ttu-id="4a20b-327">[Ellenőrizze az ügyfél hozzáférési pont erőforrás az IP-címtől függő](#listname).</span><span class="sxs-lookup"><span data-stu-id="4a20b-327">[Make the client access point resource dependent on the IP address](#listname).</span></span>
 
6. <span data-ttu-id="4a20b-328">[A fürt paraméterek beállítása a PowerShell](#setparam).</span><span class="sxs-lookup"><span data-stu-id="4a20b-328">[Set the cluster parameters in PowerShell](#setparam).</span></span>

<span data-ttu-id="4a20b-329">Miután konfigurálta a rendelkezésre állási csoportot az új IP-címet használja, a figyelő a kapcsolat beállítása.</span><span class="sxs-lookup"><span data-stu-id="4a20b-329">After you configure the availability group to use the new IP address, configure the connection to the listener.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4a20b-330">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a20b-330">Next steps</span></span>

- [<span data-ttu-id="4a20b-331">Különböző régiókban Azure virtuális gépeken futó SQL Server Always On rendelkezésre állási csoport konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a20b-331">Configure a SQL Server Always On availability group on Azure virtual machines in different regions</span></span>](virtual-machines-windows-portal-sql-availability-group-dr.md)
