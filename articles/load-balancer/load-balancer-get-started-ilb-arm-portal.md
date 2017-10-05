---
title: "Belső terheléselosztó létrehozása – Azure Portal | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre belső terheléselosztó a Resource Managerben az Azure Portalon"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 8fbe9d5d04d745de51e0e41516d6c12683c98637
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a><span data-ttu-id="3de62-103">Belső terheléselosztó létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="3de62-103">Create an Internal load balancer in the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3de62-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3de62-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="3de62-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3de62-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="3de62-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3de62-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="3de62-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="3de62-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="3de62-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3de62-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="3de62-109">Ez a cikk a Resource Manager-alapú üzemi modell használatát ismerteti, amelyet a Microsoft a legtöbb új telepítéshez a [klasszikus üzemi modell](load-balancer-get-started-ilb-classic-ps.md) helyett javasol.</span><span class="sxs-lookup"><span data-stu-id="3de62-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a><span data-ttu-id="3de62-110">Bevezetés belső terheléselosztó Azure Portalon történő létrehozásába</span><span class="sxs-lookup"><span data-stu-id="3de62-110">Get started creating an Internal load balancer using Azure portal</span></span>

<span data-ttu-id="3de62-111">Az alábbi lépések segítségével hozzon létre egy belső terheléselosztót az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="3de62-111">Use the following steps to create an internal load balancer from the Azure Portal.</span></span>

1. <span data-ttu-id="3de62-112">Egy böngészőből keresse fel az [Azure Portalt](http://portal.azure.com), majd jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="3de62-112">Open a browser, navigate to the [Azure portal](http://portal.azure.com), and sign in with your Azure account.</span></span>
2. <span data-ttu-id="3de62-113">A képernyő bal felső részében kattintson az **Új** > **Hálózat** > **Terheléselosztó** elemre.</span><span class="sxs-lookup"><span data-stu-id="3de62-113">In the upper left hand side of the screen, click **New** > **Networking** > **Load balancer**.</span></span>
3. <span data-ttu-id="3de62-114">A **Terheléselosztó létrehozása** panelen adja meg a terheléselosztó **nevét**.</span><span class="sxs-lookup"><span data-stu-id="3de62-114">In the **Create load balancer** blade, enter a **Name** for your load balancer.</span></span>
4. <span data-ttu-id="3de62-115">A **Séma** alatt kattintson a **Belső** elemre.</span><span class="sxs-lookup"><span data-stu-id="3de62-115">Under **Scheme**, click **Internal**.</span></span>
5. <span data-ttu-id="3de62-116">Kattintson a **Virtuális hálózat** elemre, majd válassza ki azt a virtuális hálózatot, amelyben a terheléselosztót szeretné létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3de62-116">Click **Virtual network**, and then select the virtual network where you want to create the load balancer.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3de62-117">Ha nem jelenik meg a használni kívánt virtuális hálózat, ellenőrizze a terheléselosztóhoz használt **helyet**, és szükség szerint módosítsa.</span><span class="sxs-lookup"><span data-stu-id="3de62-117">If you do not see the virtual network you want to use, check the **Location** you are using for the load balancer, and change it accordingly.</span></span>

6. <span data-ttu-id="3de62-118">Kattintson az **Alhálózat** elemre, majd válassza ki azt az alhálózatot, amelyben a terheléselosztót létre szeretné hozni.</span><span class="sxs-lookup"><span data-stu-id="3de62-118">Click **Subnet**, and then select the subnet where you want to create the load balancer.</span></span>
7. <span data-ttu-id="3de62-119">Az **IP-cím hozzárendelése** panelen kattintson a **Dinamikus** vagy a **Statikus** elemre attól függően, hogy a terheléselosztó IP-címét rögzítettre (statikus) szeretné-e beállítani vagy sem.</span><span class="sxs-lookup"><span data-stu-id="3de62-119">Under **IP address assignment**, click either **Dynamic** or **Static**, depending on whether you want the IP address for the load balancer to be fixed (static) or not.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3de62-120">Ha statikus IP-cím használatát választja, akkor meg kell adnia egy címet a terheléselosztó számára.</span><span class="sxs-lookup"><span data-stu-id="3de62-120">If you select to use a static IP address, you will have to provide an address for the load balancer.</span></span>

8. <span data-ttu-id="3de62-121">Az **Erőforráscsoport** alatt adja meg a terheléselosztó új erőforráscsoportjának a nevét, vagy kattintson a **Meglévő kiválasztása** elemre, és válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3de62-121">Under **Resource group** either specify the name of a new resource group for the load balancer, or click **select existing** and select an existing resource group.</span></span>
9. <span data-ttu-id="3de62-122">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3de62-122">Click **Create**.</span></span>

## <a name="configure-load-balancing-rules"></a><span data-ttu-id="3de62-123">Terheléselosztási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3de62-123">Configure load balancing rules</span></span>

<span data-ttu-id="3de62-124">A terheléselosztó létrehozása után lépjen a terheléselosztó erőforrásához, és konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="3de62-124">After the load balancer creation, navigate to the load balancer resource to configure it.</span></span>
<span data-ttu-id="3de62-125">A terheléselosztási szabály konfigurálása előtt konfigurálnia kell egy háttér-címkészletet és egy mintavételt.</span><span class="sxs-lookup"><span data-stu-id="3de62-125">You need to configure first a back-end address pool and a probe before configuring a load balancing rule.</span></span>

### <a name="step-1-configure-a-back-end-pool"></a><span data-ttu-id="3de62-126">1. lépés: Háttérkészlet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3de62-126">Step 1: Configure a back-end pool</span></span>

1. <span data-ttu-id="3de62-127">Az Azure Portalon kattintson a **Tallózás** > **Terheléselosztók** elemre, majd kattintson a fent létrehozott terheléselosztóra.</span><span class="sxs-lookup"><span data-stu-id="3de62-127">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="3de62-128">A **Beállítások** panelen kattintson a **Háttérkészletek** elemre.</span><span class="sxs-lookup"><span data-stu-id="3de62-128">In the **Settings** blade, click **Backend pools**.</span></span>
3. <span data-ttu-id="3de62-129">A **Háttércímkészletek** panelen kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3de62-129">In the **Backend address pools** blade, click **Add**.</span></span>
4. <span data-ttu-id="3de62-130">A **Háttérkészlet hozzáadása** panelen adja meg a háttérkészlet **nevét**, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3de62-130">In the **Add backend pool** blade, enter a **Name** for the backend pool, and then click **OK**.</span></span>

### <a name="step-2-configure-a-probe"></a><span data-ttu-id="3de62-131">2. lépés: Mintavétel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3de62-131">Step 2: Configure a probe</span></span>

1. <span data-ttu-id="3de62-132">Az Azure Portalon kattintson a **Tallózás** > **Terheléselosztók** elemre, majd kattintson a fent létrehozott terheléselosztóra.</span><span class="sxs-lookup"><span data-stu-id="3de62-132">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="3de62-133">A **Beállítások** panelen kattintson a **Mintavételek** elemre.</span><span class="sxs-lookup"><span data-stu-id="3de62-133">In the **Settings** blade, click **Probes**.</span></span>
3. <span data-ttu-id="3de62-134">A **Mintavételek** panelen kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3de62-134">In the **Probes**  blade, click **Add**.</span></span>
4. <span data-ttu-id="3de62-135">A **Mintavétel hozzáadása** panelen adja meg a mintavétel **nevét**.</span><span class="sxs-lookup"><span data-stu-id="3de62-135">In the **Add probe** blade, enter a **Name** for the probe.</span></span>
5. <span data-ttu-id="3de62-136">A **Protokoll** alatt válassza ki a **HTTP** (webhelyekhez) vagy a **TCP** (más TCP-alapú alkalmazásokhoz) elemet.</span><span class="sxs-lookup"><span data-stu-id="3de62-136">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="3de62-137">A **Port** alatt adja meg a mintavétel elérésekor használni kívánt portot.</span><span class="sxs-lookup"><span data-stu-id="3de62-137">Under **Port**, specify the port to use when accessing the probe.</span></span>
7. <span data-ttu-id="3de62-138">Az **Elérési út** alatt (csak HTTP-mintavételek esetén) adja meg a mintavételként használni kívánt elérési utat.</span><span class="sxs-lookup"><span data-stu-id="3de62-138">Under **Path** (for HTTP probes only), specify the path to use as a probe.</span></span>
8. <span data-ttu-id="3de62-139">Az **Időköz** alatt adja meg, hogy milyen gyakran kell mintát venni az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="3de62-139">Under **Interval** specify how frequently to probe the application.</span></span>
9. <span data-ttu-id="3de62-140">A **Nem kifogásolatlan állapot küszöbértéke** alatt adja meg, hogy hány sikertelen kísérlet után kell a háttér virtuális gépet nem megfelelő állapotúként megjelölni.</span><span class="sxs-lookup"><span data-stu-id="3de62-140">Under **Unhealthy threshold**, specify how many attempts should fail before the backend virtual machine is marked as unhealthy.</span></span>
10. <span data-ttu-id="3de62-141">Mintavétel létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3de62-141">Click **OK** to create probe.</span></span>

### <a name="step-3-configure-load-balancing-rules"></a><span data-ttu-id="3de62-142">3. lépés: Terheléselosztási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3de62-142">Step 3: Configure load balancing rules</span></span>

1. <span data-ttu-id="3de62-143">Az Azure Portalon kattintson a **Tallózás** > **Terheléselosztók** elemre, majd kattintson a fent létrehozott terheléselosztóra.</span><span class="sxs-lookup"><span data-stu-id="3de62-143">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="3de62-144">A **Beállítások** panelen kattintson a **Terheléselosztási szabályok** elemre.</span><span class="sxs-lookup"><span data-stu-id="3de62-144">In the **Settings** blade, click **Load balancing rules**.</span></span>
3. <span data-ttu-id="3de62-145">A **Terheléselosztási szabályok** panelen kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3de62-145">In the **Load balancing rules** blade, click **Add**.</span></span>
4. <span data-ttu-id="3de62-146">A **Terheléselosztási szabály hozzáadása** panelen adja meg a szabály **nevét**.</span><span class="sxs-lookup"><span data-stu-id="3de62-146">In the **Add load balancing rule** blade, enter a **Name** for the rule.</span></span>
5. <span data-ttu-id="3de62-147">A **Protokoll** alatt válassza ki a **HTTP** (webhelyekhez) vagy a **TCP** (más TCP-alapú alkalmazásokhoz) elemet.</span><span class="sxs-lookup"><span data-stu-id="3de62-147">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="3de62-148">A **Port** alatt adja meg a port azon ügyfeleit, amelyekhez a terheléselosztóban csatlakozni kell.</span><span class="sxs-lookup"><span data-stu-id="3de62-148">Under **Port**, specify the port clients connect to in the load balancer.</span></span>
7. <span data-ttu-id="3de62-149">A **Háttérport** alatt adja meg a háttérkészletben használni kívánt portot (általában a terheléselosztó portja és a háttérport azonos).</span><span class="sxs-lookup"><span data-stu-id="3de62-149">Under **Backend port**, specify the port to be used in the backend pool (usually, the load balancer port and the backend port are the same).</span></span>
8. <span data-ttu-id="3de62-150">A **Háttérkészlet** alatt válassza ki a fent létrehozott háttérkészletet.</span><span class="sxs-lookup"><span data-stu-id="3de62-150">Under **Backend pool**, select the backend pool you created above.</span></span>
9. <span data-ttu-id="3de62-151">A **Munkamenet megőrzése** alatt válassza ki, hogyan szeretné megőrizni a munkameneteket.</span><span class="sxs-lookup"><span data-stu-id="3de62-151">Under **Session persistence**, select how you want sessions to persist.</span></span>
10. <span data-ttu-id="3de62-152">Az **Üresjárat időkorlátja (perc)** alatt adja meg az üresjárati időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="3de62-152">Under **Idle timeout (minutes)**, specify the idle timeout.</span></span>
11. <span data-ttu-id="3de62-153">A **Nem fix IP-cím (közvetlen kiszolgálói válasz)** alatt kattintson a **Letiltva** vagy az **Engedélyezve** elemre.</span><span class="sxs-lookup"><span data-stu-id="3de62-153">Under **Floating IP (direct server return)**, click **Disabled** or **Enabled**.</span></span>
12. <span data-ttu-id="3de62-154">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3de62-154">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3de62-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3de62-155">Next steps</span></span>

[<span data-ttu-id="3de62-156">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3de62-156">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="3de62-157">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3de62-157">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

