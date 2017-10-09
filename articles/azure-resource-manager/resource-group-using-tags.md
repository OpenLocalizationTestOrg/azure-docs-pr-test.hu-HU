---
title: "aaaTag Azure logikai szervezet erőforrásait |} Microsoft Docs"
description: "Bemutatja, hogyan tooapply címkéket tooorganize Azure erőforrások számlázási és kezelése."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tomfitz
ms.openlocfilehash: e07470463d160f8cefe5c80bc91e66a96af6ca45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-tags-tooorganize-your-azure-resources"></a><span data-ttu-id="c954f-103">Címkék tooorganize használja az Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="c954f-103">Use tags tooorganize your Azure resources</span></span>
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> <span data-ttu-id="c954f-104">Címkék csak tooresources, amely támogatja az Azure Resource Manager műveleteket is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="c954f-104">You can apply tags only tooresources that support Azure Resource Manager operations.</span></span> <span data-ttu-id="c954f-105">Ha egy virtuális géphez, a virtuális hálózati vagy a tárfiók hello klasszikus telepítési modell (például ennek a hello a klasszikus Azure portálon) használatával létrehozott, egy címke toothat erőforrás nem alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="c954f-105">If you created a virtual machine, virtual network, or storage account through hello classic deployment model (such as through hello Azure classic portal), you cannot apply a tag toothat resource.</span></span> <span data-ttu-id="c954f-106">toosupport címkézést, telepítse újra ezeket az erőforrásokat Resource Manageren keresztül.</span><span class="sxs-lookup"><span data-stu-id="c954f-106">toosupport tagging, redeploy these resources through Resource Manager.</span></span> <span data-ttu-id="c954f-107">Minden más erőforrásnál támogatja a címkézés.</span><span class="sxs-lookup"><span data-stu-id="c954f-107">All other resources support tagging.</span></span>
> 
> 

## <a name="policies-for-tag-consistency"></a><span data-ttu-id="c954f-108">Címke konzisztencia házirendek</span><span class="sxs-lookup"><span data-stu-id="c954f-108">Policies for tag consistency</span></span>

<span data-ttu-id="c954f-109">A szervezet erőforrás házirendek toocreate általános szabályok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c954f-109">You can use resource policies toocreate standard rules for your organization.</span></span> <span data-ttu-id="c954f-110">Amelyek biztosítják az erőforrások címkézett hello megfelelő értékekkel házirendeket is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="c954f-110">You can create policies that ensure resources are tagged with hello appropriate values.</span></span> <span data-ttu-id="c954f-111">További információkért lásd: [címkék erőforrás házirendek alkalmazását](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-111">For more information, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="c954f-112">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c954f-112">PowerShell</span></span>
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a><span data-ttu-id="c954f-113">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c954f-113">Azure CLI</span></span>

<span data-ttu-id="c954f-114">toosee hello meglévő címkék egy *erőforráscsoport*, használja:</span><span class="sxs-lookup"><span data-stu-id="c954f-114">toosee hello existing tags for a *resource group*, use:</span></span>

```azurecli
az group show -n examplegroup --query tags
```

<span data-ttu-id="c954f-115">Ez a parancsfájl hello a következő formátumban adja vissza:</span><span class="sxs-lookup"><span data-stu-id="c954f-115">That script returns hello following format:</span></span>

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

<span data-ttu-id="c954f-116">toosee hello meglévő címkék egy *erőforrása, amely egy megadott erőforrás-azonosító*, használja:</span><span class="sxs-lookup"><span data-stu-id="c954f-116">toosee hello existing tags for a *resource that has a specified resource ID*, use:</span></span>

```azurecli
az resource show --id {resource-id} --query tags
```

<span data-ttu-id="c954f-117">Vagy toosee hello meglévő címkék egy *erőforrást, amely rendelkezik a megadott nevét, típusát és erőforrás-csoport*, használja:</span><span class="sxs-lookup"><span data-stu-id="c954f-117">Or, toosee hello existing tags for a *resource that has a specified name, type, and resource group*, use:</span></span>

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

<span data-ttu-id="c954f-118">egy adott címkét tartalmazó erőforráscsoportokat tooget használja `az group list`:</span><span class="sxs-lookup"><span data-stu-id="c954f-118">tooget resource groups that have a specific tag, use `az group list`:</span></span>

```azurecli
az group list --tag Dept=IT
```

<span data-ttu-id="c954f-119">tooget összes hello erőforrásokat, amelyek egy adott címke és értéket, használjon `az resource list`:</span><span class="sxs-lookup"><span data-stu-id="c954f-119">tooget all hello resources that have a particular tag and value, use `az resource list`:</span></span>

```azurecli
az resource list --tag Dept=Finance
```

<span data-ttu-id="c954f-120">Minden alkalommal, amikor a címkék tooa erőforrás vagy egy erőforráscsoport alkalmazásához, felülírhatja hello meglévő címkét az adott erőforrás vagy az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="c954f-120">Every time you apply tags tooa resource or a resource group, you overwrite hello existing tags on that resource or resource group.</span></span> <span data-ttu-id="c954f-121">Ezért hello erőforrás vagy erőforráscsoport rendelkezik-e meglévő címkék alapján másik módszert kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c954f-121">Therefore, you must use a different approach based on whether hello resource or resource group has existing tags.</span></span> 

<span data-ttu-id="c954f-122">tooadd címkéket tooa *erőforráscsoport meglévő címkék nélkül*, használja:</span><span class="sxs-lookup"><span data-stu-id="c954f-122">tooadd tags tooa *resource group without existing tags*, use:</span></span>

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

<span data-ttu-id="c954f-123">tooadd címkéket tooa *meglévő címkék nélkül erőforrás*, használja:</span><span class="sxs-lookup"><span data-stu-id="c954f-123">tooadd tags tooa *resource without existing tags*, use:</span></span>

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

<span data-ttu-id="c954f-124">tooadd címkék tooa erőforrás, amely már a címkék, hello meglévő címkék beolvasása, formázza újra ezt az értéket, és alkalmazza újra a meglévő és új címkék hello:</span><span class="sxs-lookup"><span data-stu-id="c954f-124">tooadd tags tooa resource that already has tags, retrieve hello existing tags, reformat that value, and reapply hello existing and new tags:</span></span> 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

<span data-ttu-id="c954f-125">minden tooapply egy erőforrás tooits erőforrások, a címkéket és *nem őriz meg a meglévő címkék hello erőforrások*, a következő parancsfájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="c954f-125">tooapply all tags from a resource group tooits resources, and *not retain existing tags on hello resources*, use hello following script:</span></span>

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    az resource tag --tags $t --id $resid
  done 
done
```

<span data-ttu-id="c954f-126">minden tooapply egy erőforrás tooits erőforrások, a címkéket és *tartsa meg az erőforrás meglévő címkék*, a következő parancsfájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="c954f-126">tooapply all tags from a resource group tooits resources, and *retain existing tags on resources*, use hello following script:</span></span>

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done 
done
```


## <a name="templates"></a><span data-ttu-id="c954f-127">Sablonok</span><span class="sxs-lookup"><span data-stu-id="c954f-127">Templates</span></span>

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a><span data-ttu-id="c954f-128">Portál</span><span class="sxs-lookup"><span data-stu-id="c954f-128">Portal</span></span>
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a><span data-ttu-id="c954f-129">REST API</span><span class="sxs-lookup"><span data-stu-id="c954f-129">REST API</span></span>
<span data-ttu-id="c954f-130">hello Azure-portál és a PowerShell is használható hello [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) hello színfalak mögött.</span><span class="sxs-lookup"><span data-stu-id="c954f-130">hello Azure portal and PowerShell both use hello [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) behind hello scenes.</span></span> <span data-ttu-id="c954f-131">Ha egy másik környezetbe címkézés toointegrate van szüksége, címkék használatával kaphat **beolvasása** hello erőforrás azonosítója és a frissítés hello csoportján címkék használatával egy **javítás** hívható meg.</span><span class="sxs-lookup"><span data-stu-id="c954f-131">If you need toointegrate tagging into another environment, you can get tags by using **GET** on hello resource ID and update hello set of tags by using a **PATCH** call.</span></span>

## <a name="tags-and-billing"></a><span data-ttu-id="c954f-132">Címkék és a számlázás</span><span class="sxs-lookup"><span data-stu-id="c954f-132">Tags and billing</span></span>
<span data-ttu-id="c954f-133">Címkék toogroup a számlázási adatokat használhatja.</span><span class="sxs-lookup"><span data-stu-id="c954f-133">You can use tags toogroup your billing data.</span></span> <span data-ttu-id="c954f-134">Például ha a másik szervezet számára több virtuális gép fut, használja hello címkék toogroup használati költségközpont által.</span><span class="sxs-lookup"><span data-stu-id="c954f-134">For example, if you are running multiple VMs for different organizations, use hello tags toogroup usage by cost center.</span></span> <span data-ttu-id="c954f-135">Címkék toocategorize költségek futtatókörnyezetben, például a számlázási használata hello hello éles környezetben futó virtuális gépek által is használható.</span><span class="sxs-lookup"><span data-stu-id="c954f-135">You can also use tags toocategorize costs by runtime environment, such as hello billing usage for VMs running in hello production environment.</span></span>


<span data-ttu-id="c954f-136">Visszaállíthatja az keresztül hello címkékkel kapcsolatos információkat is [Azure erőforrás-használat és RateCard API-k](../billing/billing-usage-rate-card-overview.md) vagy hello használati vesszővel tagolt (CSV) fájl.</span><span class="sxs-lookup"><span data-stu-id="c954f-136">You can retrieve information about tags through hello [Azure Resource Usage and RateCard APIs](../billing/billing-usage-rate-card-overview.md) or hello usage comma-separated values (CSV) file.</span></span> <span data-ttu-id="c954f-137">Hello használati fájl letöltését hello [Azure-fiók portálon](https://account.windowsazure.com/) vagy [EA portal](https://ea.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c954f-137">You download hello usage file from hello [Azure account portal](https://account.windowsazure.com/) or [EA portal](https://ea.azure.com).</span></span> <span data-ttu-id="c954f-138">Programozott hozzáférés toobilling információk kapcsolatos további információkért lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](../billing/billing-usage-rate-card-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-138">For more information about programmatic access toobilling information, see [Gain insights into your Microsoft Azure resource consumption](../billing/billing-usage-rate-card-overview.md).</span></span> <span data-ttu-id="c954f-139">A REST API-műveleteket, lásd: [Azure számlázási REST API-referencia](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span><span class="sxs-lookup"><span data-stu-id="c954f-139">For REST API operations, see [Azure Billing REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span></span>


<span data-ttu-id="c954f-140">Hello használata CSV számlázási címkék támogató szolgáltatások letöltésekor hello címkék jelennek meg hello **címkék** oszlop.</span><span class="sxs-lookup"><span data-stu-id="c954f-140">When you download hello usage CSV for services that support tags with billing, hello tags appear in hello **Tags** column.</span></span> <span data-ttu-id="c954f-141">További információkért lásd: [a számlázási megérteni a Microsoft Azure](../billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-141">For more information, see [Understand your bill for Microsoft Azure](../billing/billing-understand-your-bill.md).</span></span>

![Tekintse meg a számlázási címkék](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a><span data-ttu-id="c954f-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c954f-143">Next steps</span></span>
* <span data-ttu-id="c954f-144">Testreszabott házirendekkel alkalmazhat korlátozások és egyezmények az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c954f-144">You can apply restrictions and conventions across your subscription by using customized policies.</span></span> <span data-ttu-id="c954f-145">Megadhat egy házirendet, minden erőforrás rendelkezik-e egy adott címke értéke lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="c954f-145">A policy that you define might require that all resources have a value for a particular tag.</span></span> <span data-ttu-id="c954f-146">További információkért lásd: [házirendek toomanage erőforrásainak használatához, és hozzáférést](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-146">For more information, see [Use policies toomanage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="c954f-147">Egy bevezető toousing Azure PowerShell erőforrások telepít, ha talál [az Azure PowerShell használata Azure Resource Managerrel](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-147">For an introduction toousing Azure PowerShell when you're deploying resources, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="c954f-148">Egy bevezető toousing hello Azure CLI erőforrások telepít, ha talál [Using hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-148">For an introduction toousing hello Azure CLI when you're deploying resources, see [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="c954f-149">Egy bevezető toousing hello portálhoz, lásd: [Using hello Azure portál toomanage az Azure-erőforrások](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-149">For an introduction toousing hello portal, see [Using hello Azure portal toomanage your Azure resources](resource-group-portal.md).</span></span>  
* <span data-ttu-id="c954f-150">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="c954f-150">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

