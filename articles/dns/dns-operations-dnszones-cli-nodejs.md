---
title: "Az Azure DNS - Azure CLI 1.0 DNS-zónák kezelése |} Microsoft Docs"
description: "DNS-zónák Azure CLI 1.0 használatával kezelheti. Ez a cikk bemutatja, hogyan frissítése, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: 588c87749f049eff5b9e0729f6769c8367ba41e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a><span data-ttu-id="5a4dc-104">Az Azure DNS az Azure CLI 1.0 használata DNS-zónák kezelése</span><span class="sxs-lookup"><span data-stu-id="5a4dc-104">How to manage DNS Zones in Azure DNS using the Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5a4dc-105">Portal</span><span class="sxs-lookup"><span data-stu-id="5a4dc-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="5a4dc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5a4dc-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="5a4dc-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="5a4dc-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="5a4dc-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5a4dc-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="5a4dc-109">Ez az útmutató bemutatja, hogyan kezelheti a DNS-zónák platformfüggetlen Azure CLI 1.0 elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-109">This guide shows how to manage your DNS zones by using the cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="5a4dc-110">Emellett kezelhetők a DNS-zónák használatával [Azure PowerShell](dns-operations-dnszones.md) vagy az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or the Azure portal.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="5a4dc-111">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="5a4dc-111">CLI versions to complete the task</span></span>

<span data-ttu-id="5a4dc-112">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="5a4dc-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="5a4dc-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="5a4dc-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="5a4dc-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5a4dc-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a><span data-ttu-id="5a4dc-116">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="5a4dc-116">Getting help</span></span>

<span data-ttu-id="5a4dc-117">Indítsa el az Azure DNS-vonatkozó összes CLI 1.0 parancsok `azure network dns`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-117">All CLI 1.0 commands relating to Azure DNS start with `azure network dns`.</span></span> <span data-ttu-id="5a4dc-118">Súgó áll rendelkezésre egyes parancs használatával a `--help` beállítás (rövid alak `-h`).</span><span class="sxs-lookup"><span data-stu-id="5a4dc-118">Help is available for each command using the `--help` option (short form `-h`).</span></span>  <span data-ttu-id="5a4dc-119">Példa:</span><span class="sxs-lookup"><span data-stu-id="5a4dc-119">For example:</span></span>

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="5a4dc-120">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a4dc-120">Create a DNS zone</span></span>

<span data-ttu-id="5a4dc-121">A DNS-zóna az `azure network dns zone create` parancs használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-121">A DNS zone is created using the `azure network dns zone create` command.</span></span> <span data-ttu-id="5a4dc-122">További segítségért lásd: `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-122">For help, see `azure network dns zone create -h`.</span></span>

<span data-ttu-id="5a4dc-123">Az alábbi példa létrehoz egy DNS-zónát *contoso.com* erőforráscsoportban nevű *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="5a4dc-123">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a><span data-ttu-id="5a4dc-124">A DNS-zónát címkékkel létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a4dc-124">To create a DNS zone with tags</span></span>

<span data-ttu-id="5a4dc-125">A következő példa bemutatja, hogyan hozzon létre egy DNS-zónát és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*, segítségével a `--tags` paraméter ( rövid alak `-t`):</span><span class="sxs-lookup"><span data-stu-id="5a4dc-125">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using the `--tags` parameter (short form `-t`):</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="5a4dc-126">A DNS-zóna beolvasása</span><span class="sxs-lookup"><span data-stu-id="5a4dc-126">Get a DNS zone</span></span>

<span data-ttu-id="5a4dc-127">A DNS-zónák lekéréséhez használja `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-127">To retrieve a DNS zone, use `azure network dns zone show`.</span></span> <span data-ttu-id="5a4dc-128">További segítségért lásd: `azure network dns zone show -h`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-128">For help, see `azure network dns zone show -h`.</span></span>

<span data-ttu-id="5a4dc-129">Az alábbi példában a DNS-zóna adja vissza *contoso.com* és kapcsolódó adataik erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-129">The following example returns the DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

<span data-ttu-id="5a4dc-130">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-130">The following example is the response.</span></span>

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

<span data-ttu-id="5a4dc-131">Megjegyzés: a DNS-rekordokat nem által visszaadott `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-131">Note that DNS records are not returned by `azure network dns zone show`.</span></span> <span data-ttu-id="5a4dc-132">A DNS-rekordok listában használja `azure network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-132">To list DNS records, use `azure network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="5a4dc-133">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="5a4dc-133">List DNS zones</span></span>

<span data-ttu-id="5a4dc-134">Operációs rendszer DNS-zónák, használjon `azure network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-134">To enumerate DNS zones, use `azure network dns zone list`.</span></span> <span data-ttu-id="5a4dc-135">További segítségért lásd: `azure network dns zone list -h`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-135">For help, see `azure network dns zone list -h`.</span></span>

<span data-ttu-id="5a4dc-136">Adja meg az erőforráscsoport csak az erőforráscsoporton belül zónák sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="5a4dc-136">Specifying the resource group lists only those zones within the resource group:</span></span>

```azurecli
azure network dns zone list MyResourceGroup
```

<span data-ttu-id="5a4dc-137">Az előfizetés minden zóna az erőforráscsoport kihagyásával sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="5a4dc-137">Omitting the resource group lists all zones in the subscription:</span></span>

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="5a4dc-138">A DNS-zóna frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="5a4dc-138">Update a DNS zone</span></span>

<span data-ttu-id="5a4dc-139">DNS-zóna erőforráshoz módosítás használatával `azure network dns zone set`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-139">Changes to a DNS zone resource can be made using `azure network dns zone set`.</span></span> <span data-ttu-id="5a4dc-140">További segítségért lásd: `azure network dns zone set -h`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-140">For help, see `azure network dns zone set -h`.</span></span>

<span data-ttu-id="5a4dc-141">Ez a parancs frissíti a DNS-rekordhalmazok a zónán belül (lásd: [kezelése DNS-rekordok hogyan](dns-operations-recordsets-cli-nodejs.md)).</span><span class="sxs-lookup"><span data-stu-id="5a4dc-141">This command does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets-cli-nodejs.md)).</span></span> <span data-ttu-id="5a4dc-142">Csak a zóna erőforrás maga tulajdonságainak frissítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-142">It is only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="5a4dc-143">Ezeket a tulajdonságokat a rendszer jelenleg csak a [Azure Resource Manager "címke"](dns-zones-records.md#tags) a zóna erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-143">These properties are currently limited to the [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for the zone resource.</span></span>

<span data-ttu-id="5a4dc-144">A következő példa bemutatja, hogyan a címke van megadva a DNS-zónák frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-144">The following example shows how to update the tags on a DNS zone.</span></span> <span data-ttu-id="5a4dc-145">A meglévő címkék megadott helyett.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-145">The existing tags are replaced by the value specified.</span></span>

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="5a4dc-146">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="5a4dc-146">Delete a DNS Zone</span></span>

<span data-ttu-id="5a4dc-147">DNS-zónák törölhetők segítségével `azure network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-147">DNS zones can be deleted using `azure network dns zone delete`.</span></span> <span data-ttu-id="5a4dc-148">További segítségért lásd: `azure network dns zone delete -h`.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-148">For help, see `azure network dns zone delete -h`.</span></span>

> [!NOTE]
> <span data-ttu-id="5a4dc-149">Is egy DNS-zóna törlésével törli az összes DNS-rekordokat a zónán belül.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-149">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="5a4dc-150">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-150">This operation cannot be undone.</span></span> <span data-ttu-id="5a4dc-151">Ha a DNS-zóna használatban van, a zóna szolgáltatásokat sikertelen lesz a zóna törlődik.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-151">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="5a4dc-152">A zóna véletlen törlés elleni védelem érdekében, lásd: [hogyan védi a DNS-zónák és rekordok](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="5a4dc-152">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="5a4dc-153">Ez a parancs felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-153">This command prompts for confirmation.</span></span> <span data-ttu-id="5a4dc-154">A választható `--quiet` váltás (rövid alak `-q`) letiltja a parancssorhoz.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-154">The optional `--quiet` switch (short form `-q`) suppresses this prompt.</span></span>

<span data-ttu-id="5a4dc-155">A következő példa bemutatja, hogyan törölni a zónát *contoso.com* erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-155">The following example shows how to delete the zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="5a4dc-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a4dc-156">Next steps</span></span>

<span data-ttu-id="5a4dc-157">Megtudhatja, hogyan [rekordhalmazokat és rekordokat kezelése](dns-getstarted-create-recordset-cli-nodejs.md) a DNS-zónában.</span><span class="sxs-lookup"><span data-stu-id="5a4dc-157">Learn how to [manage record sets and records](dns-getstarted-create-recordset-cli-nodejs.md) in your DNS zone.</span></span>

<span data-ttu-id="5a4dc-158">Megtudhatja, hogyan [tartomány delegálása az Azure DNS-](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="5a4dc-158">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

