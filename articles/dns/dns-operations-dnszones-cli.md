---
title: "aaaManage DNS-zónák az Azure DNS - Azure CLI 2.0 |} Microsoft Docs"
description: "Kezelheti a DNS-zónákat az Azure CLI 2.0 verziót használja. Ez a cikk bemutatja, hogyan tooupdate, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra."
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
ms.openlocfilehash: 3945a558b2db3490e50678d8395a47e55a85c8fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-20"></a><span data-ttu-id="f52f8-104">Hogyan toomanage DNS-zónák az Azure DNS használatával hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f52f8-104">How toomanage DNS Zones in Azure DNS using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f52f8-105">Portál</span><span class="sxs-lookup"><span data-stu-id="f52f8-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="f52f8-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f52f8-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="f52f8-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f52f8-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="f52f8-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f52f8-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)


<span data-ttu-id="f52f8-109">Ez az útmutató bemutatja, hogyan toomanage a DNS-zónák használatával hello platformfüggetlen Azure CLI Windows, Mac és Linux elérhető.</span><span class="sxs-lookup"><span data-stu-id="f52f8-109">This guide shows how toomanage your DNS zones by using hello cross-platform Azure CLI, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="f52f8-110">Emellett kezelhetők a DNS-zónák használatával [Azure PowerShell](dns-operations-dnszones.md) vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f52f8-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or hello Azure portal.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="f52f8-111">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="f52f8-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="f52f8-112">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="f52f8-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="f52f8-113">[Az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.</span><span class="sxs-lookup"><span data-stu-id="f52f8-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="f52f8-114">[Az Azure CLI 2.0](dns-operations-dnszones-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="f52f8-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="f52f8-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f52f8-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a><span data-ttu-id="f52f8-116">Az Azure CLI 2.0 beállítása az Azure DNS-hez</span><span class="sxs-lookup"><span data-stu-id="f52f8-116">Set up Azure CLI 2.0 for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="f52f8-117">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f52f8-117">Before you begin</span></span>

<span data-ttu-id="f52f8-118">Győződjön meg arról, hogy rendelkezik-e elemek a konfigurációs megkezdése előtt a következő hello.</span><span class="sxs-lookup"><span data-stu-id="f52f8-118">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="f52f8-119">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f52f8-119">An Azure subscription.</span></span> <span data-ttu-id="f52f8-120">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f52f8-120">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="f52f8-121">Hello hello Azure CLI 2.0-t, Windows, Linux és Mac legújabb verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="f52f8-121">Install hello latest version of hello Azure CLI 2.0, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="f52f8-122">További információt [telepítés hello Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f52f8-122">More information is available at [Install hello Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="f52f8-123">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="f52f8-123">Sign in tooyour Azure account</span></span>

<span data-ttu-id="f52f8-124">Nyisson meg egy konzolablakot, adja meg a saját hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="f52f8-124">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="f52f8-125">További információkért tekintse meg a naplót a tooAzure a hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="f52f8-125">For more information, see Log in tooAzure from hello Azure CLI</span></span>

```
az login
```

### <a name="select-hello-subscription"></a><span data-ttu-id="f52f8-126">Válassza ki a hello előfizetés</span><span class="sxs-lookup"><span data-stu-id="f52f8-126">Select hello subscription</span></span>

<span data-ttu-id="f52f8-127">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="f52f8-127">Check hello subscriptions for hello account.</span></span>

```
az account list
```

<span data-ttu-id="f52f8-128">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="f52f8-128">Choose which of your Azure subscriptions toouse.</span></span>

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="f52f8-129">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="f52f8-129">Create a resource group</span></span>

<span data-ttu-id="f52f8-130">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="f52f8-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="f52f8-131">Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál.</span><span class="sxs-lookup"><span data-stu-id="f52f8-131">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="f52f8-132">Azonban mivel minden DNS-erőforrás globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure DNS szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="f52f8-132">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="f52f8-133">Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.</span><span class="sxs-lookup"><span data-stu-id="f52f8-133">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a><span data-ttu-id="f52f8-134">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="f52f8-134">Getting help</span></span>

<span data-ttu-id="f52f8-135">TooAzure DNS vonatkozó összes CLI 2.0 parancsok kezdődnie `az network dns`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-135">All CLI 2.0 commands relating tooAzure DNS start with `az network dns`.</span></span> <span data-ttu-id="f52f8-136">Minden egyes parancsnál hello segítségével érhető el súgó `--help` beállítás (rövid alak `-h`).</span><span class="sxs-lookup"><span data-stu-id="f52f8-136">Help is available for each command using hello `--help` option (short form `-h`).</span></span>  <span data-ttu-id="f52f8-137">Példa:</span><span class="sxs-lookup"><span data-stu-id="f52f8-137">For example:</span></span>

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="f52f8-138">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="f52f8-138">Create a DNS zone</span></span>

<span data-ttu-id="f52f8-139">A DNS-zóna létrehozása hello használatával `az network dns zone create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="f52f8-139">A DNS zone is created using hello `az network dns zone create` command.</span></span> <span data-ttu-id="f52f8-140">További segítségért lásd: `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-140">For help, see `az network dns zone create -h`.</span></span>

<span data-ttu-id="f52f8-141">hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f52f8-141">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a><span data-ttu-id="f52f8-142">egy DNS-zónát címkékkel toocreate</span><span class="sxs-lookup"><span data-stu-id="f52f8-142">toocreate a DNS zone with tags</span></span>

<span data-ttu-id="f52f8-143">hello következő példa bemutatja, hogyan toocreate DNS zónát, és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*, hello segítségével `--tags` a paraméter (rövid alak `-t`):</span><span class="sxs-lookup"><span data-stu-id="f52f8-143">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using hello `--tags` parameter (short form `-t`):</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="f52f8-144">A DNS-zóna beolvasása</span><span class="sxs-lookup"><span data-stu-id="f52f8-144">Get a DNS zone</span></span>

<span data-ttu-id="f52f8-145">használja a DNS-zónák tooretrieve `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-145">tooretrieve a DNS zone, use `az network dns zone show`.</span></span> <span data-ttu-id="f52f8-146">További segítségért lásd: `az network dns zone show --help`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-146">For help, see `az network dns zone show --help`.</span></span>

<span data-ttu-id="f52f8-147">hello következő példa eredménye hello DNS-zóna *contoso.com* és kapcsolódó adataik erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="f52f8-147">hello following example returns hello DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

<span data-ttu-id="f52f8-148">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="f52f8-148">hello following example is hello response.</span></span>

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

<span data-ttu-id="f52f8-149">Megjegyzés: a DNS-rekordokat nem által visszaadott `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-149">Note that DNS records are not returned by `az network dns zone show`.</span></span> <span data-ttu-id="f52f8-150">toolist DNS-rekordokat, használjon `az network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-150">toolist DNS records, use `az network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="f52f8-151">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="f52f8-151">List DNS zones</span></span>

<span data-ttu-id="f52f8-152">DNS-zónák tooenumerate, használjon `az network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-152">tooenumerate DNS zones, use `az network dns zone list`.</span></span> <span data-ttu-id="f52f8-153">További segítségért lásd: `az network dns zone list --help`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-153">For help, see `az network dns zone list --help`.</span></span>

<span data-ttu-id="f52f8-154">Megadását hello erőforráscsoport hello erőforráscsoporton belül zónák sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="f52f8-154">Specifying hello resource group lists only those zones within hello resource group:</span></span>

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

<span data-ttu-id="f52f8-155">Hello erőforráscsoport kihagyásával hello előfizetés minden zóna tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f52f8-155">Omitting hello resource group lists all zones in hello subscription:</span></span>

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="f52f8-156">A DNS-zóna frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="f52f8-156">Update a DNS zone</span></span>

<span data-ttu-id="f52f8-157">DNS-zóna erőforrásrekordok végezhetők változások tooa `az network dns zone update`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-157">Changes tooa DNS zone resource can be made using `az network dns zone update`.</span></span> <span data-ttu-id="f52f8-158">További segítségért lásd: `az network dns zone update --help`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-158">For help, see `az network dns zone update --help`.</span></span>

<span data-ttu-id="f52f8-159">Ez a parancs frissíti hello DNS-rekordhalmazok hello zónán belül (lásd: [hogyan tooManage DNS-rekordok](dns-operations-recordsets-cli.md)).</span><span class="sxs-lookup"><span data-stu-id="f52f8-159">This command does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets-cli.md)).</span></span> <span data-ttu-id="f52f8-160">Csak használt tooupdate tulajdonságok hello zóna erőforrás maga is.</span><span class="sxs-lookup"><span data-stu-id="f52f8-160">It is only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="f52f8-161">Ezek a tulajdonságok jelenleg korlátozott toohello [Azure Resource Manager "címke"](dns-zones-records.md#tags) hello zóna erőforrás.</span><span class="sxs-lookup"><span data-stu-id="f52f8-161">These properties are currently limited toohello [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for hello zone resource.</span></span>

<span data-ttu-id="f52f8-162">hello következő példa bemutatja, hogyan tooupdate hello címkéket a DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="f52f8-162">hello following example shows how tooupdate hello tags on a DNS zone.</span></span> <span data-ttu-id="f52f8-163">hello meglévő címkék helyébe hello érték van megadva.</span><span class="sxs-lookup"><span data-stu-id="f52f8-163">hello existing tags are replaced by hello value specified.</span></span>

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="f52f8-164">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="f52f8-164">Delete a DNS zone</span></span>

<span data-ttu-id="f52f8-165">DNS-zónák törölhetők segítségével `az network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-165">DNS zones can be deleted using `az network dns zone delete`.</span></span> <span data-ttu-id="f52f8-166">További segítségért lásd: `az network dns zone delete --help`.</span><span class="sxs-lookup"><span data-stu-id="f52f8-166">For help, see `az network dns zone delete --help`.</span></span>

> [!NOTE]
> <span data-ttu-id="f52f8-167">A DNS-zónák törlésekor a hello zónán belül minden DNS-rekordokat is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="f52f8-167">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="f52f8-168">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="f52f8-168">This operation cannot be undone.</span></span> <span data-ttu-id="f52f8-169">Hello DNS-zóna van használatban, ha hello zóna használó szolgáltatások nem indulnak hello zóna törlésekor.</span><span class="sxs-lookup"><span data-stu-id="f52f8-169">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="f52f8-170">a zóna véletlen törlése ellen tooprotect lásd: [hogyan tooprotect DNS zónák, valamint megjegyzi](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="f52f8-170">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="f52f8-171">Ez a parancs felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="f52f8-171">This command prompts for confirmation.</span></span> <span data-ttu-id="f52f8-172">nem kötelező hello `--yes` letiltja a parancssorhoz.</span><span class="sxs-lookup"><span data-stu-id="f52f8-172">hello optional `--yes` switch suppresses this prompt.</span></span>

<span data-ttu-id="f52f8-173">hello következő példa bemutatja, hogyan toodelete hello zóna *contoso.com* erőforráscsoportból *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="f52f8-173">hello following example shows how toodelete hello zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="f52f8-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f52f8-174">Next steps</span></span>

<span data-ttu-id="f52f8-175">Ismerje meg, hogyan túl[rekordhalmazokat és rekordokat kezelése](dns-getstarted-create-recordset-cli.md) a DNS-zónában.</span><span class="sxs-lookup"><span data-stu-id="f52f8-175">Learn how too[manage record sets and records](dns-getstarted-create-recordset-cli.md) in your DNS zone.</span></span>

<span data-ttu-id="f52f8-176">Ismerje meg, hogyan túl[delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="f52f8-176">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

