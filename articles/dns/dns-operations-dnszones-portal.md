---
title: "aaaManage DNS-zónák az Azure DNS - Azure-portál |} Microsoft Docs"
description: "DNS-zónák hello Azure-portál használatával kezelheti. Ez a cikk ismerteti, hogyan tooupdate, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra"
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
ms.openlocfilehash: 0d8ce302bb7126dfe8077a6f3e33418e16fcea64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-hello-azure-portal"></a><span data-ttu-id="06364-104">Hogyan toomanage DNS-zónák az hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="06364-104">How toomanage DNS Zones in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="06364-105">Portál</span><span class="sxs-lookup"><span data-stu-id="06364-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="06364-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="06364-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="06364-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="06364-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="06364-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="06364-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="06364-109">Ez a cikk bemutatja, hogyan toomanage a DNS-zónák hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="06364-109">This article shows you how toomanage your DNS zones by using hello Azure portal.</span></span> <span data-ttu-id="06364-110">A DNS-zónák hello platformfüggetlen segítségével is kezelheti [Azure CLI](dns-operations-dnszones-cli.md) vagy hello Azure [PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="06364-110">You can also manage your DNS zones using hello cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or hello Azure [PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="06364-111">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="06364-111">Create a DNS zone</span></span>

1. <span data-ttu-id="06364-112">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="06364-112">Sign in toohello Azure portal</span></span>
2. <span data-ttu-id="06364-113">Hello központ menüben kattintson, majd **új > Hálózat >** , majd **DNS-zóna** tooopen hello hozzon létre DNS-zóna panelen.</span><span class="sxs-lookup"><span data-stu-id="06364-113">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![DNS-zóna](./media/dns-operations-dnszones-portal/openzone650.png)

4. <span data-ttu-id="06364-115">A hello **hozzon létre DNS-zóna** panelen adja meg a következő értékek hello, majd kattintson a **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="06364-115">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>


   | <span data-ttu-id="06364-116">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="06364-116">**Setting**</span></span> | <span data-ttu-id="06364-117">**Érték**</span><span class="sxs-lookup"><span data-stu-id="06364-117">**Value**</span></span> | <span data-ttu-id="06364-118">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="06364-118">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="06364-119">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="06364-119">**Name**</span></span>|<span data-ttu-id="06364-120">contoso.com</span><span class="sxs-lookup"><span data-stu-id="06364-120">contoso.com</span></span>|<span data-ttu-id="06364-121">hello hello DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="06364-121">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="06364-122">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="06364-122">**Subscription**</span></span>|<span data-ttu-id="06364-123">[Az Ön előfizetése]</span><span class="sxs-lookup"><span data-stu-id="06364-123">[Your subscription]</span></span>|<span data-ttu-id="06364-124">Válasszon egy előfizetés toocreate hello DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="06364-124">Select a subscription toocreate hello DNS zone in.</span></span>|
   |<span data-ttu-id="06364-125">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="06364-125">**Resource group**</span></span>|<span data-ttu-id="06364-126">**Új létrehozása:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="06364-126">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="06364-127">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="06364-127">Create a resource group.</span></span> <span data-ttu-id="06364-128">az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="06364-128">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="06364-129">További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) a cikk áttekintése.</span><span class="sxs-lookup"><span data-stu-id="06364-129">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="06364-130">**Hely**</span><span class="sxs-lookup"><span data-stu-id="06364-130">**Location**</span></span>|<span data-ttu-id="06364-131">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="06364-131">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="06364-132">hello erőforráscsoport hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="06364-132">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="06364-133">mindig "globális" Hello DNS-zóna helyét, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="06364-133">hello DNS zone location is always "global", and is not shown.</span></span>

## <a name="list-dns-zones"></a><span data-ttu-id="06364-134">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="06364-134">List DNS zones</span></span>

<span data-ttu-id="06364-135">Az Azure-portálon hello, keresse meg a túl**további szolgáltatások** > **hálózati** > **DNS-zónák**.</span><span class="sxs-lookup"><span data-stu-id="06364-135">In hello Azure portal, navigate too**More services** > **Networking** > **DNS zones**.</span></span> <span data-ttu-id="06364-136">Minden DNS-zóna, saját erőforrás, például-rekordhalmazok száma és a név kiszolgálók ebben a nézetben látható.</span><span class="sxs-lookup"><span data-stu-id="06364-136">Each DNS zone is it's own resource, information such as number of record-sets and name servers are viewable from this view.</span></span> <span data-ttu-id="06364-137">hello oszlop **NÉVKISZOLGÁLÓK** nincs hello alapértelmezett nézetben tooadd azt kattintson **oszlopok**, jelölje be **névkiszolgálók** kattintson **végzett**.</span><span class="sxs-lookup"><span data-stu-id="06364-137">hello column **NAME SERVERS** is not in hello default view, tooadd it click **Columns**, select **Name servers** and click **Done**.</span></span>

![DNS-zónák listázása](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a><span data-ttu-id="06364-139">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="06364-139">Delete a DNS zone</span></span>

<span data-ttu-id="06364-140">Keresse meg a tooa DNS-zóna hello portálon.</span><span class="sxs-lookup"><span data-stu-id="06364-140">Navigate tooa DNS zone in hello portal.</span></span> <span data-ttu-id="06364-141">A hello **DNS-zóna** panelen kattintson a **zóna törlése**.</span><span class="sxs-lookup"><span data-stu-id="06364-141">On hello **DNS zone** blade, click **Delete zone**.</span></span> <span data-ttu-id="06364-142">Vannak, aki a toodelete hello DNS-zóna felszólító tooconfirm áll.</span><span class="sxs-lookup"><span data-stu-id="06364-142">You are prompted tooconfirm you are wanting toodelete hello DNS zone.</span></span> <span data-ttu-id="06364-143">A DNS-zónák törlésekor a hello hello zónában lévő összes rekordot is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="06364-143">Deleting a DNS zone also deletes all hello records that are contained in hello zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06364-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06364-144">Next steps</span></span>

<span data-ttu-id="06364-145">Megtudhatja, hogyan legyenek a DNS-zóna és a rekordok ellátogatva toowork [Ismerkedés az Azure DNS hello Azure-portál használatával](dns-getstarted-portal.md).</span><span class="sxs-lookup"><span data-stu-id="06364-145">Learn how toowork with your DNS Zone and records by visiting [Get started with Azure DNS using hello Azure portal](dns-getstarted-portal.md).</span></span>
