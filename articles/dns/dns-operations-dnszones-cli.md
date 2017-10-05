---
title: "Az Azure DNS - Azure CLI 2.0 DNS-zónák kezelése |} Microsoft Docs"
description: "Kezelheti a DNS-zónákat az Azure CLI 2.0 verziót használja. Ez a cikk bemutatja, hogyan frissítése, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
ms.openlocfilehash: 1414baf9e51d648cc3a46c4f8635040b4d276910
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-20"></a><span data-ttu-id="47bfc-104">Az Azure DNS az Azure CLI 2.0 használatával DNS-zónák kezelése</span><span class="sxs-lookup"><span data-stu-id="47bfc-104">How to manage DNS Zones in Azure DNS using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="47bfc-105">Portal</span><span class="sxs-lookup"><span data-stu-id="47bfc-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="47bfc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="47bfc-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="47bfc-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="47bfc-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="47bfc-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="47bfc-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)


<span data-ttu-id="47bfc-109">Ez az útmutató bemutatja a platformok közötti Azure parancssori felület használatával, amely érhető el a Windows, Mac és Linux a DNS-zónák kezelése.</span><span class="sxs-lookup"><span data-stu-id="47bfc-109">This guide shows how to manage your DNS zones by using the cross-platform Azure CLI, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="47bfc-110">Emellett kezelhetők a DNS-zónák használatával [Azure PowerShell](dns-operations-dnszones.md) vagy az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="47bfc-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or the Azure portal.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="47bfc-111">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="47bfc-111">CLI versions to complete the task</span></span>

<span data-ttu-id="47bfc-112">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="47bfc-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="47bfc-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez.</span><span class="sxs-lookup"><span data-stu-id="47bfc-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="47bfc-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="47bfc-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="47bfc-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="47bfc-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a><span data-ttu-id="47bfc-116">Az Azure CLI 2.0 beállítása az Azure DNS-hez</span><span class="sxs-lookup"><span data-stu-id="47bfc-116">Set up Azure CLI 2.0 for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="47bfc-117">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="47bfc-117">Before you begin</span></span>

<span data-ttu-id="47bfc-118">A konfigurálás megkezdése előtt győződjön meg arról, hogy rendelkezik a következőkkel.</span><span class="sxs-lookup"><span data-stu-id="47bfc-118">Verify that you have the following items before beginning your configuration.</span></span>

* <span data-ttu-id="47bfc-119">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="47bfc-119">An Azure subscription.</span></span> <span data-ttu-id="47bfc-120">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="47bfc-120">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="47bfc-121">Az Azure CLI 2.0-t, Windows, Linux és Mac legújabb verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="47bfc-121">Install the latest version of the Azure CLI 2.0, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="47bfc-122">További információt az [Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2) című cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="47bfc-122">More information is available at [Install the Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

### <a name="sign-in-to-your-azure-account"></a><span data-ttu-id="47bfc-123">Jelentkezzen be az Azure-fiókjába</span><span class="sxs-lookup"><span data-stu-id="47bfc-123">Sign in to your Azure account</span></span>

<span data-ttu-id="47bfc-124">Nyisson meg egy konzolablakot, adja meg a saját hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="47bfc-124">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="47bfc-125">További információt az Azure parancssori felületből (CLI) Azure-ba történő bejelentkezést ismertető cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="47bfc-125">For more information, see Log in to Azure from the Azure CLI</span></span>

```
az login
```

### <a name="select-the-subscription"></a><span data-ttu-id="47bfc-126">Válassza ki az előfizetést</span><span class="sxs-lookup"><span data-stu-id="47bfc-126">Select the subscription</span></span>

<span data-ttu-id="47bfc-127">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="47bfc-127">Check the subscriptions for the account.</span></span>

```
az account list
```

<span data-ttu-id="47bfc-128">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="47bfc-128">Choose which of your Azure subscriptions to use.</span></span>

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="47bfc-129">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="47bfc-129">Create a resource group</span></span>

<span data-ttu-id="47bfc-130">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="47bfc-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="47bfc-131">Ez szolgál az erőforráscsoport erőforrásainak alapértelmezett helyeként.</span><span class="sxs-lookup"><span data-stu-id="47bfc-131">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="47bfc-132">Mivel azonban minden DNS-erőforrás globális, nem pedig regionális, az erőforráscsoport kiválasztott helye nincs hatással az Azure DNS szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="47bfc-132">However, because all DNS resources are global, not regional, the choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="47bfc-133">Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.</span><span class="sxs-lookup"><span data-stu-id="47bfc-133">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a><span data-ttu-id="47bfc-134">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="47bfc-134">Getting help</span></span>

<span data-ttu-id="47bfc-135">Indítsa el az Azure DNS-vonatkozó összes CLI 2.0 parancsok `az network dns`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-135">All CLI 2.0 commands relating to Azure DNS start with `az network dns`.</span></span> <span data-ttu-id="47bfc-136">Súgó áll rendelkezésre egyes parancs használatával a `--help` beállítás (rövid alak `-h`).</span><span class="sxs-lookup"><span data-stu-id="47bfc-136">Help is available for each command using the `--help` option (short form `-h`).</span></span>  <span data-ttu-id="47bfc-137">Példa:</span><span class="sxs-lookup"><span data-stu-id="47bfc-137">For example:</span></span>

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="47bfc-138">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="47bfc-138">Create a DNS zone</span></span>

<span data-ttu-id="47bfc-139">A DNS-zóna az `az network dns zone create` parancs használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="47bfc-139">A DNS zone is created using the `az network dns zone create` command.</span></span> <span data-ttu-id="47bfc-140">További segítségért lásd: `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-140">For help, see `az network dns zone create -h`.</span></span>

<span data-ttu-id="47bfc-141">Az alábbi példa létrehoz egy DNS-zónát *contoso.com* erőforráscsoportban nevű *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="47bfc-141">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a><span data-ttu-id="47bfc-142">A DNS-zónát címkékkel létrehozása</span><span class="sxs-lookup"><span data-stu-id="47bfc-142">To create a DNS zone with tags</span></span>

<span data-ttu-id="47bfc-143">A következő példa bemutatja, hogyan hozzon létre egy DNS-zónát és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*, segítségével a `--tags` paraméter ( rövid alak `-t`):</span><span class="sxs-lookup"><span data-stu-id="47bfc-143">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using the `--tags` parameter (short form `-t`):</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="47bfc-144">A DNS-zóna beolvasása</span><span class="sxs-lookup"><span data-stu-id="47bfc-144">Get a DNS zone</span></span>

<span data-ttu-id="47bfc-145">A DNS-zónák lekéréséhez használja `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-145">To retrieve a DNS zone, use `az network dns zone show`.</span></span> <span data-ttu-id="47bfc-146">További segítségért lásd: `az network dns zone show --help`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-146">For help, see `az network dns zone show --help`.</span></span>

<span data-ttu-id="47bfc-147">Az alábbi példában a DNS-zóna adja vissza *contoso.com* és kapcsolódó adataik erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="47bfc-147">The following example returns the DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

<span data-ttu-id="47bfc-148">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="47bfc-148">The following example is the response.</span></span>

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="47bfc-149">Megjegyzés: a DNS-rekordokat nem által visszaadott `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-149">Note that DNS records are not returned by `az network dns zone show`.</span></span> <span data-ttu-id="47bfc-150">A DNS-rekordok listában használja `az network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-150">To list DNS records, use `az network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="47bfc-151">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="47bfc-151">List DNS zones</span></span>

<span data-ttu-id="47bfc-152">Operációs rendszer DNS-zónák, használjon `az network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-152">To enumerate DNS zones, use `az network dns zone list`.</span></span> <span data-ttu-id="47bfc-153">További segítségért lásd: `az network dns zone list --help`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-153">For help, see `az network dns zone list --help`.</span></span>

<span data-ttu-id="47bfc-154">Adja meg az erőforráscsoport csak az erőforráscsoporton belül zónák sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="47bfc-154">Specifying the resource group lists only those zones within the resource group:</span></span>

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

<span data-ttu-id="47bfc-155">Az előfizetés minden zóna az erőforráscsoport kihagyásával sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="47bfc-155">Omitting the resource group lists all zones in the subscription:</span></span>

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="47bfc-156">A DNS-zóna frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="47bfc-156">Update a DNS zone</span></span>

<span data-ttu-id="47bfc-157">DNS-zóna erőforráshoz módosítás használatával `az network dns zone update`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-157">Changes to a DNS zone resource can be made using `az network dns zone update`.</span></span> <span data-ttu-id="47bfc-158">További segítségért lásd: `az network dns zone update --help`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-158">For help, see `az network dns zone update --help`.</span></span>

<span data-ttu-id="47bfc-159">Ez a parancs frissíti a DNS-rekordhalmazok a zónán belül (lásd: [kezelése DNS-rekordok hogyan](dns-operations-recordsets-cli.md)).</span><span class="sxs-lookup"><span data-stu-id="47bfc-159">This command does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets-cli.md)).</span></span> <span data-ttu-id="47bfc-160">Csak a zóna erőforrás maga tulajdonságainak frissítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="47bfc-160">It is only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="47bfc-161">Ezeket a tulajdonságokat a rendszer jelenleg csak a [Azure Resource Manager "címke"](dns-zones-records.md#tags) a zóna erőforrás.</span><span class="sxs-lookup"><span data-stu-id="47bfc-161">These properties are currently limited to the [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for the zone resource.</span></span>

<span data-ttu-id="47bfc-162">A következő példa bemutatja, hogyan a címke van megadva a DNS-zónák frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="47bfc-162">The following example shows how to update the tags on a DNS zone.</span></span> <span data-ttu-id="47bfc-163">A meglévő címkék megadott helyett.</span><span class="sxs-lookup"><span data-stu-id="47bfc-163">The existing tags are replaced by the value specified.</span></span>

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="47bfc-164">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="47bfc-164">Delete a DNS zone</span></span>

<span data-ttu-id="47bfc-165">DNS-zónák törölhetők segítségével `az network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-165">DNS zones can be deleted using `az network dns zone delete`.</span></span> <span data-ttu-id="47bfc-166">További segítségért lásd: `az network dns zone delete --help`.</span><span class="sxs-lookup"><span data-stu-id="47bfc-166">For help, see `az network dns zone delete --help`.</span></span>

> [!NOTE]
> <span data-ttu-id="47bfc-167">Is egy DNS-zóna törlésével törli az összes DNS-rekordokat a zónán belül.</span><span class="sxs-lookup"><span data-stu-id="47bfc-167">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="47bfc-168">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="47bfc-168">This operation cannot be undone.</span></span> <span data-ttu-id="47bfc-169">Ha a DNS-zóna használatban van, a zóna szolgáltatásokat sikertelen lesz a zóna törlődik.</span><span class="sxs-lookup"><span data-stu-id="47bfc-169">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="47bfc-170">A zóna véletlen törlés elleni védelem érdekében, lásd: [hogyan védi a DNS-zónák és rekordok](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="47bfc-170">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="47bfc-171">Ez a parancs felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="47bfc-171">This command prompts for confirmation.</span></span> <span data-ttu-id="47bfc-172">A választható `--yes` letiltja a parancssorhoz.</span><span class="sxs-lookup"><span data-stu-id="47bfc-172">The optional `--yes` switch suppresses this prompt.</span></span>

<span data-ttu-id="47bfc-173">A következő példa bemutatja, hogyan törölni a zónát *contoso.com* erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="47bfc-173">The following example shows how to delete the zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="47bfc-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47bfc-174">Next steps</span></span>

<span data-ttu-id="47bfc-175">Megtudhatja, hogyan [rekordhalmazokat és rekordokat kezelése](dns-getstarted-create-recordset-cli.md) a DNS-zónában.</span><span class="sxs-lookup"><span data-stu-id="47bfc-175">Learn how to [manage record sets and records](dns-getstarted-create-recordset-cli.md) in your DNS zone.</span></span>

<span data-ttu-id="47bfc-176">Megtudhatja, hogyan [tartomány delegálása az Azure DNS-](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="47bfc-176">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

