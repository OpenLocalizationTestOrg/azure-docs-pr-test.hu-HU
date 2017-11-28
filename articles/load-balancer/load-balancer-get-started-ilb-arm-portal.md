---
title: "egy belső terheléselosztó - Azure-portálon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó az erőforrás-kezelőben hello Azure-portál használatával"
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
ms.openlocfilehash: 80124217a84857b542eb41cb814ec97234176dd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-in-hello-azure-portal"></a><span data-ttu-id="c28c3-103">Hozzon létre egy belső elosztott terhelésű hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c28c3-103">Create an Internal load balancer in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c28c3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c28c3-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="c28c3-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c28c3-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="c28c3-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c28c3-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="c28c3-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="c28c3-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="c28c3-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c28c3-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="c28c3-109">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c28c3-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a><span data-ttu-id="c28c3-110">Bevezetés belső terheléselosztó Azure Portalon történő létrehozásába</span><span class="sxs-lookup"><span data-stu-id="c28c3-110">Get started creating an Internal load balancer using Azure portal</span></span>

<span data-ttu-id="c28c3-111">Hello lépéseket toocreate belső terheléselosztót követően – hello Azure portál használata.</span><span class="sxs-lookup"><span data-stu-id="c28c3-111">Use hello following steps toocreate an internal load balancer from hello Azure Portal.</span></span>

1. <span data-ttu-id="c28c3-112">Nyisson meg egy böngészőt, keresse meg a toohello [Azure-portálon](http://portal.azure.com), és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c28c3-112">Open a browser, navigate toohello [Azure portal](http://portal.azure.com), and sign in with your Azure account.</span></span>
2. <span data-ttu-id="c28c3-113">Hello felső bal oldalán üdvözlő képernyőt, kattintson **új** > **hálózati** > **terheléselosztó**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-113">In hello upper left hand side of hello screen, click **New** > **Networking** > **Load balancer**.</span></span>
3. <span data-ttu-id="c28c3-114">A hello **létrehozás terheléselosztó** panelen adjon meg egy **neve** a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="c28c3-114">In hello **Create load balancer** blade, enter a **Name** for your load balancer.</span></span>
4. <span data-ttu-id="c28c3-115">A **Séma** alatt kattintson a **Belső** elemre.</span><span class="sxs-lookup"><span data-stu-id="c28c3-115">Under **Scheme**, click **Internal**.</span></span>
5. <span data-ttu-id="c28c3-116">Kattintson a **virtuális hálózati**, majd válassza ki hello virtuális hálózaton toocreate hello terheléselosztó és.</span><span class="sxs-lookup"><span data-stu-id="c28c3-116">Click **Virtual network**, and then select hello virtual network where you want toocreate hello load balancer.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c28c3-117">Ha azt szeretné, hogy toouse hello virtuális hálózati nem látható, ellenőrizze a hello **hely** hello terheléselosztót használja, és ennek megfelelően módosítsa azt.</span><span class="sxs-lookup"><span data-stu-id="c28c3-117">If you do not see hello virtual network you want toouse, check hello **Location** you are using for hello load balancer, and change it accordingly.</span></span>

6. <span data-ttu-id="c28c3-118">Kattintson a **alhálózati**, majd válassza ki a kívánt toocreate hello terheléselosztó hello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="c28c3-118">Click **Subnet**, and then select hello subnet where you want toocreate hello load balancer.</span></span>
7. <span data-ttu-id="c28c3-119">A **IP-cím hozzárendelése**, jelölje be az **dinamikus** vagy **statikus**, attól függően, hogy használni szeretne hello IP-cím hello load balancer toobe rögzített (statikus) vagy sem.</span><span class="sxs-lookup"><span data-stu-id="c28c3-119">Under **IP address assignment**, click either **Dynamic** or **Static**, depending on whether you want hello IP address for hello load balancer toobe fixed (static) or not.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c28c3-120">Ha toouse egy statikus IP-címet, hogy tooprovide hello terheléselosztó címét.</span><span class="sxs-lookup"><span data-stu-id="c28c3-120">If you select toouse a static IP address, you will have tooprovide an address for hello load balancer.</span></span>

8. <span data-ttu-id="c28c3-121">A **erőforráscsoport** adja meg a terheléselosztóhoz hello hello új erőforráscsoport neve, vagy kattintson a **válasszon meglévő** , és válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="c28c3-121">Under **Resource group** either specify hello name of a new resource group for hello load balancer, or click **select existing** and select an existing resource group.</span></span>
9. <span data-ttu-id="c28c3-122">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c28c3-122">Click **Create**.</span></span>

## <a name="configure-load-balancing-rules"></a><span data-ttu-id="c28c3-123">Terheléselosztási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c28c3-123">Configure load balancing rules</span></span>

<span data-ttu-id="c28c3-124">Hello után betöltése a terheléselosztó létrehozása, és keresse meg a toohello load balancer erőforrás tooconfigure azt.</span><span class="sxs-lookup"><span data-stu-id="c28c3-124">After hello load balancer creation, navigate toohello load balancer resource tooconfigure it.</span></span>
<span data-ttu-id="c28c3-125">Szüksége tooconfigure először egy háttér címkészletet, és egy mintavételt tartozó terheléselosztási szabályok konfigurálása előtt.</span><span class="sxs-lookup"><span data-stu-id="c28c3-125">You need tooconfigure first a back-end address pool and a probe before configuring a load balancing rule.</span></span>

### <a name="step-1-configure-a-back-end-pool"></a><span data-ttu-id="c28c3-126">1. lépés: Háttérkészlet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c28c3-126">Step 1: Configure a back-end pool</span></span>

1. <span data-ttu-id="c28c3-127">Hello Azure-portálon, kattintson **Tallózás** > **Terheléselosztók**, majd kattintson a fenti létrehozott hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="c28c3-127">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="c28c3-128">A hello **beállítások** panelen kattintson a **háttérkészletek**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-128">In hello **Settings** blade, click **Backend pools**.</span></span>
3. <span data-ttu-id="c28c3-129">A hello **háttércímkészletek** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-129">In hello **Backend address pools** blade, click **Add**.</span></span>
4. <span data-ttu-id="c28c3-130">A hello **háttérkészlet hozzáadása** panelen adjon meg egy **neve** hello háttérkészlet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-130">In hello **Add backend pool** blade, enter a **Name** for hello backend pool, and then click **OK**.</span></span>

### <a name="step-2-configure-a-probe"></a><span data-ttu-id="c28c3-131">2. lépés: Mintavétel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c28c3-131">Step 2: Configure a probe</span></span>

1. <span data-ttu-id="c28c3-132">Hello Azure-portálon, kattintson **Tallózás** > **Terheléselosztók**, majd kattintson a fenti létrehozott hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="c28c3-132">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="c28c3-133">A hello **beállítások** panelen kattintson a **vizsgálat**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-133">In hello **Settings** blade, click **Probes**.</span></span>
3. <span data-ttu-id="c28c3-134">A hello **vizsgálat** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-134">In hello **Probes**  blade, click **Add**.</span></span>
4. <span data-ttu-id="c28c3-135">A hello **Hozzáadás mintavételi** panelen adjon meg egy **neve** hello mintavételhez.</span><span class="sxs-lookup"><span data-stu-id="c28c3-135">In hello **Add probe** blade, enter a **Name** for hello probe.</span></span>
5. <span data-ttu-id="c28c3-136">A **Protokoll** alatt válassza ki a **HTTP** (webhelyekhez) vagy a **TCP** (más TCP-alapú alkalmazásokhoz) elemet.</span><span class="sxs-lookup"><span data-stu-id="c28c3-136">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="c28c3-137">A **Port**, adja meg a hello port toouse hello mintavételi való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="c28c3-137">Under **Port**, specify hello port toouse when accessing hello probe.</span></span>
7. <span data-ttu-id="c28c3-138">A **elérési** (a HTTP mintavételt csak), egy mintavételt hello elérési toouse meg.</span><span class="sxs-lookup"><span data-stu-id="c28c3-138">Under **Path** (for HTTP probes only), specify hello path toouse as a probe.</span></span>
8. <span data-ttu-id="c28c3-139">A **időköz** adja meg, hogy milyen gyakran tooprobe hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c28c3-139">Under **Interval** specify how frequently tooprobe hello application.</span></span>
9. <span data-ttu-id="c28c3-140">A **sérült küszöbérték**, adja meg, hány kísérletet amennyiben nem, mielőtt hello háttér virtuális gép megfelelő állapotúként van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="c28c3-140">Under **Unhealthy threshold**, specify how many attempts should fail before hello backend virtual machine is marked as unhealthy.</span></span>
10. <span data-ttu-id="c28c3-141">Kattintson a **OK** toocreate mintavétel.</span><span class="sxs-lookup"><span data-stu-id="c28c3-141">Click **OK** toocreate probe.</span></span>

### <a name="step-3-configure-load-balancing-rules"></a><span data-ttu-id="c28c3-142">3. lépés: Terheléselosztási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c28c3-142">Step 3: Configure load balancing rules</span></span>

1. <span data-ttu-id="c28c3-143">Hello Azure-portálon, kattintson **Tallózás** > **Terheléselosztók**, majd kattintson a fenti létrehozott hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="c28c3-143">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="c28c3-144">A hello **beállítások** panelen kattintson a **terheléselosztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-144">In hello **Settings** blade, click **Load balancing rules**.</span></span>
3. <span data-ttu-id="c28c3-145">A hello **terheléselosztási szabályok** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c28c3-145">In hello **Load balancing rules** blade, click **Add**.</span></span>
4. <span data-ttu-id="c28c3-146">A hello **terheléselosztási szabály hozzáadása** panelen adjon meg egy **neve** hello szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="c28c3-146">In hello **Add load balancing rule** blade, enter a **Name** for hello rule.</span></span>
5. <span data-ttu-id="c28c3-147">A **Protokoll** alatt válassza ki a **HTTP** (webhelyekhez) vagy a **TCP** (más TCP-alapú alkalmazásokhoz) elemet.</span><span class="sxs-lookup"><span data-stu-id="c28c3-147">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="c28c3-148">A **Port**, adja meg a hello port ügyfelek csatlakozni tooin hello terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="c28c3-148">Under **Port**, specify hello port clients connect tooin hello load balancer.</span></span>
7. <span data-ttu-id="c28c3-149">A **háttérportot**, adja meg a hello port toobe hello háttérkészlet használt (általában hello load balancer port és a háttérportnak hello vannak hello ugyanaz).</span><span class="sxs-lookup"><span data-stu-id="c28c3-149">Under **Backend port**, specify hello port toobe used in hello backend pool (usually, hello load balancer port and hello backend port are hello same).</span></span>
8. <span data-ttu-id="c28c3-150">A **háttérkészlet**, jelölje be a fenti létrehozott hello háttér alkalmazáskészletnek.</span><span class="sxs-lookup"><span data-stu-id="c28c3-150">Under **Backend pool**, select hello backend pool you created above.</span></span>
9. <span data-ttu-id="c28c3-151">A **munkamenet megőrzését**, válassza ki, hogyan szeretné a munkamenetek toopersist.</span><span class="sxs-lookup"><span data-stu-id="c28c3-151">Under **Session persistence**, select how you want sessions toopersist.</span></span>
10. <span data-ttu-id="c28c3-152">A **üresjárati időkorlátja (perc)**, adja meg a hello üresjárati időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="c28c3-152">Under **Idle timeout (minutes)**, specify hello idle timeout.</span></span>
11. <span data-ttu-id="c28c3-153">A **Nem fix IP-cím (közvetlen kiszolgálói válasz)** alatt kattintson a **Letiltva** vagy az **Engedélyezve** elemre.</span><span class="sxs-lookup"><span data-stu-id="c28c3-153">Under **Floating IP (direct server return)**, click **Disabled** or **Enabled**.</span></span>
12. <span data-ttu-id="c28c3-154">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c28c3-154">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c28c3-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c28c3-155">Next steps</span></span>

[<span data-ttu-id="c28c3-156">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c28c3-156">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="c28c3-157">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c28c3-157">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

