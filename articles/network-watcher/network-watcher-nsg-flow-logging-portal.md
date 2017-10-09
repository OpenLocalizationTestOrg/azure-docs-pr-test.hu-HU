---
title: "aaaManage hálózati biztonsági csoport folyamata naplók Azure hálózati figyelőt |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan naplózza az toomanage hálózati biztonsági csoport folyamata az Azure hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fb250337ab9d1a0c0d0d3569c00bc221dd102a3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-group-flow-logs-in-hello-azure-portal"></a><span data-ttu-id="179d9-103">Hálózati biztonsági csoport folyamata naplók az Azure-portálon hello kezelése</span><span class="sxs-lookup"><span data-stu-id="179d9-103">Manage network security group flow logs in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="179d9-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="179d9-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="179d9-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="179d9-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="179d9-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="179d9-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="179d9-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="179d9-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="179d9-108">REST API</span><span class="sxs-lookup"><span data-stu-id="179d9-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="179d9-109">Hálózati biztonsági csoport folyamata feldolgozásra egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="179d9-109">Network security group flow logs are a feature of Network Watcher that enables you tooview information about ingress and egress IP traffic through a network security group.</span></span> <span data-ttu-id="179d9-110">A folyamat naplók JSON formátumban vannak megírva, és a fontos információkat, beleértve:</span><span class="sxs-lookup"><span data-stu-id="179d9-110">These flow logs are written in JSON format and provide important information, including:</span></span> 

- <span data-ttu-id="179d9-111">Kimenő és bejövő forgalom szabály alapon.</span><span class="sxs-lookup"><span data-stu-id="179d9-111">Outbound and inbound flows on a per-rule basis.</span></span>
- <span data-ttu-id="179d9-112">Hálózati adapterre folyamata hello hello vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="179d9-112">hello NIC that hello flow applies to.</span></span>
- <span data-ttu-id="179d9-113">hello folyamata (forrás vagy a cél IP, forrás vagy a cél port, protocol) 5 rekordos információt.</span><span class="sxs-lookup"><span data-stu-id="179d9-113">5-tuple information about hello flow (source/destination IP, source/destination port, protocol).</span></span>
- <span data-ttu-id="179d9-114">Információ arról, hogy forgalom engedélyezett vagy megtagadott.</span><span class="sxs-lookup"><span data-stu-id="179d9-114">Information about whether traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="179d9-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="179d9-115">Before you begin</span></span>

<span data-ttu-id="179d9-116">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt példányt](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="179d9-116">This scenario assumes you have already followed hello steps in [Create a Network Watcher instance](network-watcher-create.md).</span></span> <span data-ttu-id="179d9-117">hello is feltételezzük, hogy egy meg van-e egy érvényes virtuális géppel erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="179d9-117">hello scenario also assumes that a you have a resource group with a valid virtual machine.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="179d9-118">Elemzések szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="179d9-118">Register Insights provider</span></span>

<span data-ttu-id="179d9-119">A folyamat sikeres legyen, hello az naplózási toowork **Microsoft.Insights** szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="179d9-119">For flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="179d9-120">tooregister hello szolgáltató, a következő lépéseket hajtsa végre a megfelelő hello:</span><span class="sxs-lookup"><span data-stu-id="179d9-120">tooregister hello provider, take hello following steps:</span></span> 

1. <span data-ttu-id="179d9-121">Nyissa meg túl**előfizetések**, majd válassza ki a kívánt tooenable folyamata naplók hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="179d9-121">Go too**Subscriptions**, and then select hello subscription for which you want tooenable flow logs.</span></span> 
2. <span data-ttu-id="179d9-122">A hello **előfizetés** panelen válassza **erőforrás-szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="179d9-122">On hello **Subscription** blade, select **Resource Providers**.</span></span> 
3. <span data-ttu-id="179d9-123">Nézze meg hello szolgáltatók listáját, és győződjön meg arról, hogy hello **microsoft.insights** szolgáltató regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="179d9-123">Look at hello list of providers, and verify that hello **microsoft.insights** provider is registered.</span></span> <span data-ttu-id="179d9-124">Ha nem, akkor válasszon **regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="179d9-124">If not, then select **Register**.</span></span>

![Nézet szolgáltatók][providers]

## <a name="enable-flow-logs"></a><span data-ttu-id="179d9-126">Attribútumfolyam naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="179d9-126">Enable flow logs</span></span>

<span data-ttu-id="179d9-127">Ezen lépések hello folyamatot, amely a hálózati biztonsági csoport folyamata naplók engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="179d9-127">These steps take you through hello process of enabling flow logs on a network security group.</span></span>

### <a name="step-1"></a><span data-ttu-id="179d9-128">1. lépés</span><span class="sxs-lookup"><span data-stu-id="179d9-128">Step 1</span></span>

<span data-ttu-id="179d9-129">Nyissa meg tooa hálózati figyelőt példányt, és válassza ki **NSG Flow naplózza**.</span><span class="sxs-lookup"><span data-stu-id="179d9-129">Go tooa Network Watcher instance, and then select **NSG Flow logs**.</span></span>

![Attribútumfolyam naplók – áttekintés][1]

### <a name="step-2"></a><span data-ttu-id="179d9-131">2. lépés</span><span class="sxs-lookup"><span data-stu-id="179d9-131">Step 2</span></span>

<span data-ttu-id="179d9-132">Válassza ki a hálózati biztonsági csoport hello listából.</span><span class="sxs-lookup"><span data-stu-id="179d9-132">Select a network security group from hello list.</span></span>

![Attribútumfolyam naplók – áttekintés][2]

### <a name="step-3"></a><span data-ttu-id="179d9-134">3. lépés</span><span class="sxs-lookup"><span data-stu-id="179d9-134">Step 3</span></span> 

<span data-ttu-id="179d9-135">A hello **adatfolyam-naplói beállításainak** panelen hello állapot beállítása túl**a**, majd konfigurálja a storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="179d9-135">On hello **Flow logs settings** blade, set hello status too**On**, and then configure a storage account.</span></span>  <span data-ttu-id="179d9-136">Amikor elkészült, válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="179d9-136">When you're done, select **OK**.</span></span> <span data-ttu-id="179d9-137">Válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="179d9-137">Then select **Save**.</span></span>

![Attribútumfolyam naplók – áttekintés][3]

## <a name="download-flow-logs"></a><span data-ttu-id="179d9-139">Attribútumfolyam naplók letöltése</span><span class="sxs-lookup"><span data-stu-id="179d9-139">Download flow logs</span></span>

<span data-ttu-id="179d9-140">Attribútumfolyam naplók a storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="179d9-140">Flow logs are saved in a storage account.</span></span> <span data-ttu-id="179d9-141">A folyamat naplók tooview letölteni azokat.</span><span class="sxs-lookup"><span data-stu-id="179d9-141">Download your flow logs tooview them.</span></span>

### <a name="step-1"></a><span data-ttu-id="179d9-142">1. lépés</span><span class="sxs-lookup"><span data-stu-id="179d9-142">Step 1</span></span>

<span data-ttu-id="179d9-143">toodownload folyamat naplókat, válassza ki **folyamata naplók letölthető konfigurált tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="179d9-143">toodownload flow logs, select **You can download flow logs from configured storage accounts**.</span></span> <span data-ttu-id="179d9-144">Ez a lépés viszi tooa tárolási fiók nézetet adja meg a mely naplók toodownload.</span><span class="sxs-lookup"><span data-stu-id="179d9-144">This step takes you tooa storage account view where you can choose which logs toodownload.</span></span>

![Adatfolyam-naplói beállításainak][4]

### <a name="step-2"></a><span data-ttu-id="179d9-146">2. lépés</span><span class="sxs-lookup"><span data-stu-id="179d9-146">Step 2</span></span>

<span data-ttu-id="179d9-147">Nyissa meg a megfelelő tárfiók toohello.</span><span class="sxs-lookup"><span data-stu-id="179d9-147">Go toohello correct storage account.</span></span> <span data-ttu-id="179d9-148">Válassza ki **tárolók** > **insights-napló-networksecuritygroupflowevent**.</span><span class="sxs-lookup"><span data-stu-id="179d9-148">Then select **Containers** > **insights-log-networksecuritygroupflowevent**.</span></span>

![Adatfolyam-naplói beállításainak][5]

### <a name="step-3"></a><span data-ttu-id="179d9-150">3. lépés</span><span class="sxs-lookup"><span data-stu-id="179d9-150">Step 3</span></span>

<span data-ttu-id="179d9-151">Nyissa meg hello folyamata napló helye toohello, válassza ki azt, és válassza ki **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="179d9-151">Go toohello location of hello flow log, select it, and then select **Download**.</span></span>

![Adatfolyam-naplói beállításainak][6]

<span data-ttu-id="179d9-153">Hello struktúra hello napló kapcsolatos információkért látogasson el a [hálózati biztonsági csoport folyamata napló áttekintése](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="179d9-153">For information about hello structure of hello log, visit [Network security group flow log overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="179d9-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="179d9-154">Next steps</span></span>

<span data-ttu-id="179d9-155">Ismerje meg, hogyan túl[jelenítheti meg az NSG folyamata naplók a powerbi-jal](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="179d9-155">Learn how too[visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
