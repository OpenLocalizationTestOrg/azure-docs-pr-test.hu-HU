---
title: "Hálózati biztonsági csoport folyamata naplók Azure hálózati figyelőt kezelése |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan Azure hálózati figyelőt hálózati biztonsági csoport folyamata kezelése"
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
ms.openlocfilehash: 41cb5ffab9bd3a3bed75ffdb6a7383ca1690f810
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a><span data-ttu-id="653a4-103">Hálózati biztonsági csoport folyamata naplók az Azure portálon kezelése</span><span class="sxs-lookup"><span data-stu-id="653a4-103">Manage network security group flow logs in the Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="653a4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="653a4-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="653a4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="653a4-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="653a4-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="653a4-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="653a4-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="653a4-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="653a4-108">REST API</span><span class="sxs-lookup"><span data-stu-id="653a4-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="653a4-109">Hálózati biztonsági csoport folyamata feldolgozásra egyik funkciója, amely lehetővé teszi, hogy tekintse meg a hálózati biztonsági csoporton keresztül bemenő és kimenő IP-forgalom hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="653a4-109">Network security group flow logs are a feature of Network Watcher that enables you to view information about ingress and egress IP traffic through a network security group.</span></span> <span data-ttu-id="653a4-110">A folyamat naplók JSON formátumban vannak megírva, és a fontos információkat, beleértve:</span><span class="sxs-lookup"><span data-stu-id="653a4-110">These flow logs are written in JSON format and provide important information, including:</span></span> 

- <span data-ttu-id="653a4-111">Kimenő és bejövő forgalom szabály alapon.</span><span class="sxs-lookup"><span data-stu-id="653a4-111">Outbound and inbound flows on a per-rule basis.</span></span>
- <span data-ttu-id="653a4-112">A hálózati adapterre alkalmazza a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="653a4-112">The NIC that the flow applies to.</span></span>
- <span data-ttu-id="653a4-113">a folyamat (forrás vagy a cél IP, forrás vagy a cél port, protocol) 5 rekordos információt.</span><span class="sxs-lookup"><span data-stu-id="653a4-113">5-tuple information about the flow (source/destination IP, source/destination port, protocol).</span></span>
- <span data-ttu-id="653a4-114">Információ arról, hogy forgalom engedélyezett vagy megtagadott.</span><span class="sxs-lookup"><span data-stu-id="653a4-114">Information about whether traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="653a4-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="653a4-115">Before you begin</span></span>

<span data-ttu-id="653a4-116">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt példányt](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="653a4-116">This scenario assumes you have already followed the steps in [Create a Network Watcher instance](network-watcher-create.md).</span></span> <span data-ttu-id="653a4-117">A forgatókönyv emellett feltételezi, hogy egy rendelkezik egy erőforráscsoportot, egy érvényes virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="653a4-117">The scenario also assumes that a you have a resource group with a valid virtual machine.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="653a4-118">Elemzések szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="653a4-118">Register Insights provider</span></span>

<span data-ttu-id="653a4-119">A folyamat sikeres, működéséhez naplózás a **Microsoft.Insights** szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="653a4-119">For flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="653a4-120">Regisztrálja a szolgáltatót, hogy a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="653a4-120">To register the provider, take the following steps:</span></span> 

1. <span data-ttu-id="653a4-121">Ugrás a **előfizetések**, majd válassza ki az előfizetést, amelyekhez folyamata naplók engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="653a4-121">Go to **Subscriptions**, and then select the subscription for which you want to enable flow logs.</span></span> 
2. <span data-ttu-id="653a4-122">Az a **előfizetés** panelen válassza **erőforrás-szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="653a4-122">On the **Subscription** blade, select **Resource Providers**.</span></span> 
3. <span data-ttu-id="653a4-123">Nézze meg a szolgáltatók listáját, és ellenőrizze, hogy a **microsoft.insights** szolgáltató regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="653a4-123">Look at the list of providers, and verify that the **microsoft.insights** provider is registered.</span></span> <span data-ttu-id="653a4-124">Ha nem, akkor válasszon **regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="653a4-124">If not, then select **Register**.</span></span>

![Nézet szolgáltatók][providers]

## <a name="enable-flow-logs"></a><span data-ttu-id="653a4-126">Attribútumfolyam naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="653a4-126">Enable flow logs</span></span>

<span data-ttu-id="653a4-127">Ezeket a lépéseket tenni a folyamatot, amely a hálózati biztonsági csoport folyamata naplók engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="653a4-127">These steps take you through the process of enabling flow logs on a network security group.</span></span>

### <a name="step-1"></a><span data-ttu-id="653a4-128">1. lépés</span><span class="sxs-lookup"><span data-stu-id="653a4-128">Step 1</span></span>

<span data-ttu-id="653a4-129">Nyissa meg a hálózati figyelőt példányhoz, és válassza ki **NSG Flow naplózza**.</span><span class="sxs-lookup"><span data-stu-id="653a4-129">Go to a Network Watcher instance, and then select **NSG Flow logs**.</span></span>

![Attribútumfolyam naplók – áttekintés][1]

### <a name="step-2"></a><span data-ttu-id="653a4-131">2. lépés</span><span class="sxs-lookup"><span data-stu-id="653a4-131">Step 2</span></span>

<span data-ttu-id="653a4-132">Válassza ki a hálózati biztonsági csoportot a listából.</span><span class="sxs-lookup"><span data-stu-id="653a4-132">Select a network security group from the list.</span></span>

![Attribútumfolyam naplók – áttekintés][2]

### <a name="step-3"></a><span data-ttu-id="653a4-134">3. lépés</span><span class="sxs-lookup"><span data-stu-id="653a4-134">Step 3</span></span> 

<span data-ttu-id="653a4-135">A a **adatfolyam-naplói beállításainak** panelen állítsa a állapotát **a**, és majd a tárfiók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="653a4-135">On the **Flow logs settings** blade, set the status to **On**, and then configure a storage account.</span></span>  <span data-ttu-id="653a4-136">Amikor elkészült, válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="653a4-136">When you're done, select **OK**.</span></span> <span data-ttu-id="653a4-137">Válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="653a4-137">Then select **Save**.</span></span>

![Attribútumfolyam naplók – áttekintés][3]

## <a name="download-flow-logs"></a><span data-ttu-id="653a4-139">Attribútumfolyam naplók letöltése</span><span class="sxs-lookup"><span data-stu-id="653a4-139">Download flow logs</span></span>

<span data-ttu-id="653a4-140">Attribútumfolyam naplók a storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="653a4-140">Flow logs are saved in a storage account.</span></span> <span data-ttu-id="653a4-141">Töltse le a folyamat naplók megtekintheti őket.</span><span class="sxs-lookup"><span data-stu-id="653a4-141">Download your flow logs to view them.</span></span>

### <a name="step-1"></a><span data-ttu-id="653a4-142">1. lépés</span><span class="sxs-lookup"><span data-stu-id="653a4-142">Step 1</span></span>

<span data-ttu-id="653a4-143">Folyamat naplók letöltéséhez kiválasztása **folyamata naplók letölthető konfigurált tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="653a4-143">To download flow logs, select **You can download flow logs from configured storage accounts**.</span></span> <span data-ttu-id="653a4-144">Ebben a lépésben megnyitná a tárolási fiók nézetet adja meg a mely naplók letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="653a4-144">This step takes you to a storage account view where you can choose which logs to download.</span></span>

![Adatfolyam-naplói beállításainak][4]

### <a name="step-2"></a><span data-ttu-id="653a4-146">2. lépés</span><span class="sxs-lookup"><span data-stu-id="653a4-146">Step 2</span></span>

<span data-ttu-id="653a4-147">Nyissa meg a megfelelő tárolási fiók.</span><span class="sxs-lookup"><span data-stu-id="653a4-147">Go to the correct storage account.</span></span> <span data-ttu-id="653a4-148">Válassza ki **tárolók** > **insights-napló-networksecuritygroupflowevent**.</span><span class="sxs-lookup"><span data-stu-id="653a4-148">Then select **Containers** > **insights-log-networksecuritygroupflowevent**.</span></span>

![Adatfolyam-naplói beállításainak][5]

### <a name="step-3"></a><span data-ttu-id="653a4-150">3. lépés</span><span class="sxs-lookup"><span data-stu-id="653a4-150">Step 3</span></span>

<span data-ttu-id="653a4-151">Nyissa meg a folyamat napló helye, válassza ki, majd válassza ki **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="653a4-151">Go to the location of the flow log, select it, and then select **Download**.</span></span>

![Adatfolyam-naplói beállításainak][6]

<span data-ttu-id="653a4-153">A napló szerkezete kapcsolatos információkért látogasson el a [hálózati biztonsági csoport folyamata napló áttekintése](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="653a4-153">For information about the structure of the log, visit [Network security group flow log overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="653a4-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="653a4-154">Next steps</span></span>

<span data-ttu-id="653a4-155">Megtudhatja, hogyan [jelenítheti meg az NSG folyamata naplók a powerbi-jal](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="653a4-155">Learn how to [visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
