---
title: "Az Azure DNS - Azure-portálon DNS-zónák kezelése |} Microsoft Docs"
description: "DNS-zónákat az Azure portál használatával kezelheti. Ez a cikk ismerteti, hogyan frissítés, törlés és DNS-zóna létrehozása az Azure DNS szolgáltatásra"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 69a509612e6204fc93dd42bf2fe69cb165b5777c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a><span data-ttu-id="6ddf2-104">DNS-zónák kezelése az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6ddf2-104">How to manage DNS Zones in the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6ddf2-105">Portal</span><span class="sxs-lookup"><span data-stu-id="6ddf2-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="6ddf2-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6ddf2-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="6ddf2-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="6ddf2-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="6ddf2-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6ddf2-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="6ddf2-109">Ez a cikk bemutatja, hogyan a DNS-zónák kezelése az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-109">This article shows you how to manage your DNS zones by using the Azure portal.</span></span> <span data-ttu-id="6ddf2-110">Emellett kezelhetők a DNS-zónák használata a platformok közötti [Azure CLI](dns-operations-dnszones-cli.md) vagy az Azure [PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf2-110">You can also manage your DNS zones using the cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or the Azure [PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="6ddf2-111">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ddf2-111">Create a DNS zone</span></span>

1. <span data-ttu-id="6ddf2-112">Jelentkezzen be az Azure Portalra</span><span class="sxs-lookup"><span data-stu-id="6ddf2-112">Sign in to the Azure portal</span></span>
2. <span data-ttu-id="6ddf2-113">A központi menüben kattintson az **Új > Hálózatkezelés >** elemre, majd kattintson a **DNS-zóna** elemre a DNS-zóna létrehozása panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-113">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![DNS-zóna](./media/dns-operations-dnszones-portal/openzone650.png)

4. <span data-ttu-id="6ddf2-115">A **DNS-zóna létrehozása** panelen adja meg a következő értékeket, majd kattintson a **Létrehozás** elemre:</span><span class="sxs-lookup"><span data-stu-id="6ddf2-115">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>


   | <span data-ttu-id="6ddf2-116">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="6ddf2-116">**Setting**</span></span> | <span data-ttu-id="6ddf2-117">**Érték**</span><span class="sxs-lookup"><span data-stu-id="6ddf2-117">**Value**</span></span> | <span data-ttu-id="6ddf2-118">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="6ddf2-118">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="6ddf2-119">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="6ddf2-119">**Name**</span></span>|<span data-ttu-id="6ddf2-120">contoso.com</span><span class="sxs-lookup"><span data-stu-id="6ddf2-120">contoso.com</span></span>|<span data-ttu-id="6ddf2-121">A DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="6ddf2-121">The name of the DNS zone</span></span>|
   |<span data-ttu-id="6ddf2-122">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="6ddf2-122">**Subscription**</span></span>|<span data-ttu-id="6ddf2-123">[Az Ön előfizetése]</span><span class="sxs-lookup"><span data-stu-id="6ddf2-123">[Your subscription]</span></span>|<span data-ttu-id="6ddf2-124">Válassza ki azt az előfizetést, amelyben létre fogja hozni a DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-124">Select a subscription to create the DNS zone in.</span></span>|
   |<span data-ttu-id="6ddf2-125">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="6ddf2-125">**Resource group**</span></span>|<span data-ttu-id="6ddf2-126">**Új létrehozása:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="6ddf2-126">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="6ddf2-127">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-127">Create a resource group.</span></span> <span data-ttu-id="6ddf2-128">Az erőforráscsoport nevének egyedinek kell lennie a kiválasztott előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-128">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="6ddf2-129">Az erőforráscsoportokkal kapcsolatos további információkért olvassa el [A Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) áttekintése című cikket.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-129">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="6ddf2-130">**Hely**</span><span class="sxs-lookup"><span data-stu-id="6ddf2-130">**Location**</span></span>|<span data-ttu-id="6ddf2-131">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="6ddf2-131">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="6ddf2-132">Az erőforráscsoport az erőforráscsoport helyére vonatkozik, és nincs hatással a DNS-zónára.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-132">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="6ddf2-133">A DNS-zóna helye mindig „globális”, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-133">The DNS zone location is always "global", and is not shown.</span></span>

## <a name="list-dns-zones"></a><span data-ttu-id="6ddf2-134">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="6ddf2-134">List DNS zones</span></span>

<span data-ttu-id="6ddf2-135">Az Azure-portálon lépjen a **további szolgáltatások** > **hálózati** > **DNS-zónák**.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-135">In the Azure portal, navigate to **More services** > **Networking** > **DNS zones**.</span></span> <span data-ttu-id="6ddf2-136">Minden DNS-zóna, saját erőforrás, például-rekordhalmazok száma és a név kiszolgálók ebben a nézetben látható.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-136">Each DNS zone is it's own resource, information such as number of record-sets and name servers are viewable from this view.</span></span> <span data-ttu-id="6ddf2-137">Az oszlop **NÉVKISZOLGÁLÓK** nincs az alapértelmezett nézet felvételéhez kattintson a **oszlopok**, jelölje be **névkiszolgálók** kattintson **végzett**.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-137">The column **NAME SERVERS** is not in the default view, to add it click **Columns**, select **Name servers** and click **Done**.</span></span>

![DNS-zónák listázása](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a><span data-ttu-id="6ddf2-139">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="6ddf2-139">Delete a DNS zone</span></span>

<span data-ttu-id="6ddf2-140">Nyissa meg a DNS-zónát a portálon.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-140">Navigate to a DNS zone in the portal.</span></span> <span data-ttu-id="6ddf2-141">Az a **DNS-zóna** panelen kattintson a **zóna törlése**.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-141">On the **DNS zone** blade, click **Delete zone**.</span></span> <span data-ttu-id="6ddf2-142">Győződjön meg arról, hogy törölni kívánja a DNS-zónát kéri.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-142">You are prompted to confirm you are wanting to delete the DNS zone.</span></span> <span data-ttu-id="6ddf2-143">A zónában lévő összes rekordot is egy DNS-zóna törlése törli.</span><span class="sxs-lookup"><span data-stu-id="6ddf2-143">Deleting a DNS zone also deletes all the records that are contained in the zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ddf2-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ddf2-144">Next steps</span></span>

<span data-ttu-id="6ddf2-145">Ismerje meg, hogyan működnek együtt a DNS-zóna és a rekordok látogasson el [Ismerkedés az Azure DNS az Azure portál használatával](dns-getstarted-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf2-145">Learn how to work with your DNS Zone and records by visiting [Get started with Azure DNS using the Azure portal](dns-getstarted-portal.md).</span></span>