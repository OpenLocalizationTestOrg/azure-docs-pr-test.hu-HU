---
title: "az Azure CLI 1.0 - az Azure DNS-zónák aaaManage DNS |} Microsoft Docs"
description: "DNS-zónák Azure CLI 1.0 használatával kezelheti. Ez a cikk bemutatja, hogyan tooupdate, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra."
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
ms.openlocfilehash: cb9790cc46626ef7f38a43edb57511104fe6057e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-10"></a><span data-ttu-id="c0ccf-104">Hogyan toomanage DNS-zónák az Azure DNS használatával hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c0ccf-104">How toomanage DNS Zones in Azure DNS using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c0ccf-105">Portál</span><span class="sxs-lookup"><span data-stu-id="c0ccf-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="c0ccf-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0ccf-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="c0ccf-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c0ccf-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="c0ccf-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c0ccf-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="c0ccf-109">Ez az útmutató bemutatja, hogyan toomanage a DNS-zóna számára elérhető a Windows, Mac és Linux hello platformfüggetlen 1.0, az Azure CLI segítségével.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-109">This guide shows how toomanage your DNS zones by using hello cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="c0ccf-110">Emellett kezelhetők a DNS-zónák használatával [Azure PowerShell](dns-operations-dnszones.md) vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or hello Azure portal.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="c0ccf-111">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="c0ccf-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="c0ccf-112">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="c0ccf-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="c0ccf-113">[Az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="c0ccf-114">[Az Azure CLI 2.0](dns-operations-dnszones-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="c0ccf-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c0ccf-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a><span data-ttu-id="c0ccf-116">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="c0ccf-116">Getting help</span></span>

<span data-ttu-id="c0ccf-117">TooAzure DNS vonatkozó összes CLI 1.0 parancsok kezdődnie `azure network dns`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-117">All CLI 1.0 commands relating tooAzure DNS start with `azure network dns`.</span></span> <span data-ttu-id="c0ccf-118">Minden egyes parancsnál hello segítségével érhető el súgó `--help` beállítás (rövid alak `-h`).</span><span class="sxs-lookup"><span data-stu-id="c0ccf-118">Help is available for each command using hello `--help` option (short form `-h`).</span></span>  <span data-ttu-id="c0ccf-119">Példa:</span><span class="sxs-lookup"><span data-stu-id="c0ccf-119">For example:</span></span>

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="c0ccf-120">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0ccf-120">Create a DNS zone</span></span>

<span data-ttu-id="c0ccf-121">A DNS-zóna létrehozása hello használatával `azure network dns zone create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-121">A DNS zone is created using hello `azure network dns zone create` command.</span></span> <span data-ttu-id="c0ccf-122">További segítségért lásd: `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-122">For help, see `azure network dns zone create -h`.</span></span>

<span data-ttu-id="c0ccf-123">hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="c0ccf-123">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a><span data-ttu-id="c0ccf-124">egy DNS-zónát címkékkel toocreate</span><span class="sxs-lookup"><span data-stu-id="c0ccf-124">toocreate a DNS zone with tags</span></span>

<span data-ttu-id="c0ccf-125">hello következő példa bemutatja, hogyan toocreate DNS zónát, és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*, hello segítségével `--tags` a paraméter (rövid alak `-t`):</span><span class="sxs-lookup"><span data-stu-id="c0ccf-125">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using hello `--tags` parameter (short form `-t`):</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="c0ccf-126">A DNS-zóna beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0ccf-126">Get a DNS zone</span></span>

<span data-ttu-id="c0ccf-127">használja a DNS-zónák tooretrieve `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-127">tooretrieve a DNS zone, use `azure network dns zone show`.</span></span> <span data-ttu-id="c0ccf-128">További segítségért lásd: `azure network dns zone show -h`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-128">For help, see `azure network dns zone show -h`.</span></span>

<span data-ttu-id="c0ccf-129">hello következő példa eredménye hello DNS-zóna *contoso.com* és kapcsolódó adataik erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-129">hello following example returns hello DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

<span data-ttu-id="c0ccf-130">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-130">hello following example is hello response.</span></span>

```
info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
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

<span data-ttu-id="c0ccf-131">Megjegyzés: a DNS-rekordokat nem által visszaadott `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-131">Note that DNS records are not returned by `azure network dns zone show`.</span></span> <span data-ttu-id="c0ccf-132">toolist DNS-rekordokat, használjon `azure network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-132">toolist DNS records, use `azure network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="c0ccf-133">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="c0ccf-133">List DNS zones</span></span>

<span data-ttu-id="c0ccf-134">DNS-zónák tooenumerate, használjon `azure network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-134">tooenumerate DNS zones, use `azure network dns zone list`.</span></span> <span data-ttu-id="c0ccf-135">További segítségért lásd: `azure network dns zone list -h`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-135">For help, see `azure network dns zone list -h`.</span></span>

<span data-ttu-id="c0ccf-136">Megadását hello erőforráscsoport hello erőforráscsoporton belül zónák sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="c0ccf-136">Specifying hello resource group lists only those zones within hello resource group:</span></span>

```azurecli
azure network dns zone list MyResourceGroup
```

<span data-ttu-id="c0ccf-137">Hello erőforráscsoport kihagyásával hello előfizetés minden zóna tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="c0ccf-137">Omitting hello resource group lists all zones in hello subscription:</span></span>

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="c0ccf-138">A DNS-zóna frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="c0ccf-138">Update a DNS zone</span></span>

<span data-ttu-id="c0ccf-139">DNS-zóna erőforrásrekordok végezhetők változások tooa `azure network dns zone set`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-139">Changes tooa DNS zone resource can be made using `azure network dns zone set`.</span></span> <span data-ttu-id="c0ccf-140">További segítségért lásd: `azure network dns zone set -h`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-140">For help, see `azure network dns zone set -h`.</span></span>

<span data-ttu-id="c0ccf-141">Ez a parancs frissíti hello DNS-rekordhalmazok hello zónán belül (lásd: [hogyan tooManage DNS-rekordok](dns-operations-recordsets-cli-nodejs.md)).</span><span class="sxs-lookup"><span data-stu-id="c0ccf-141">This command does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets-cli-nodejs.md)).</span></span> <span data-ttu-id="c0ccf-142">Csak használt tooupdate tulajdonságok hello zóna erőforrás maga is.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-142">It is only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="c0ccf-143">Ezek a tulajdonságok jelenleg korlátozott toohello [Azure Resource Manager "címke"](dns-zones-records.md#tags) hello zóna erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-143">These properties are currently limited toohello [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for hello zone resource.</span></span>

<span data-ttu-id="c0ccf-144">hello következő példa bemutatja, hogyan tooupdate hello címkéket a DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-144">hello following example shows how tooupdate hello tags on a DNS zone.</span></span> <span data-ttu-id="c0ccf-145">hello meglévő címkék helyébe hello érték van megadva.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-145">hello existing tags are replaced by hello value specified.</span></span>

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="c0ccf-146">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="c0ccf-146">Delete a DNS Zone</span></span>

<span data-ttu-id="c0ccf-147">DNS-zónák törölhetők segítségével `azure network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-147">DNS zones can be deleted using `azure network dns zone delete`.</span></span> <span data-ttu-id="c0ccf-148">További segítségért lásd: `azure network dns zone delete -h`.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-148">For help, see `azure network dns zone delete -h`.</span></span>

> [!NOTE]
> <span data-ttu-id="c0ccf-149">A DNS-zónák törlésekor a hello zónán belül minden DNS-rekordokat is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-149">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="c0ccf-150">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-150">This operation cannot be undone.</span></span> <span data-ttu-id="c0ccf-151">Hello DNS-zóna van használatban, ha hello zóna használó szolgáltatások nem indulnak hello zóna törlésekor.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-151">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="c0ccf-152">a zóna véletlen törlése ellen tooprotect lásd: [hogyan tooprotect DNS zónák, valamint megjegyzi](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="c0ccf-152">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="c0ccf-153">Ez a parancs felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-153">This command prompts for confirmation.</span></span> <span data-ttu-id="c0ccf-154">nem kötelező hello `--quiet` váltás (rövid alak `-q`) mellőzi a parancssorhoz.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-154">hello optional `--quiet` switch (short form `-q`) suppresses this prompt.</span></span>

<span data-ttu-id="c0ccf-155">hello következő példa bemutatja, hogyan toodelete hello zóna *contoso.com* erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-155">hello following example shows how toodelete hello zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="c0ccf-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0ccf-156">Next steps</span></span>

<span data-ttu-id="c0ccf-157">Ismerje meg, hogyan túl[rekordhalmazokat és rekordokat kezelése](dns-getstarted-create-recordset-cli-nodejs.md) a DNS-zónában.</span><span class="sxs-lookup"><span data-stu-id="c0ccf-157">Learn how too[manage record sets and records](dns-getstarted-create-recordset-cli-nodejs.md) in your DNS zone.</span></span>

<span data-ttu-id="c0ccf-158">Ismerje meg, hogyan túl[delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="c0ccf-158">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

