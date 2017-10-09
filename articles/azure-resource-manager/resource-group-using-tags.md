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
# <a name="use-tags-tooorganize-your-azure-resources"></a>Címkék tooorganize használja az Azure-erőforrások
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> Címkék csak tooresources, amely támogatja az Azure Resource Manager műveleteket is alkalmazhat. Ha egy virtuális géphez, a virtuális hálózati vagy a tárfiók hello klasszikus telepítési modell (például ennek a hello a klasszikus Azure portálon) használatával létrehozott, egy címke toothat erőforrás nem alkalmazhat. toosupport címkézést, telepítse újra ezeket az erőforrásokat Resource Manageren keresztül. Minden más erőforrásnál támogatja a címkézés.
> 
> 

## <a name="policies-for-tag-consistency"></a>Címke konzisztencia házirendek

A szervezet erőforrás házirendek toocreate általános szabályok is használhatja. Amelyek biztosítják az erőforrások címkézett hello megfelelő értékekkel házirendeket is létrehozhat. További információkért lásd: [címkék erőforrás házirendek alkalmazását](resource-manager-policy-tags.md).

## <a name="powershell"></a>PowerShell
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

toosee hello meglévő címkék egy *erőforráscsoport*, használja:

```azurecli
az group show -n examplegroup --query tags
```

Ez a parancsfájl hello a következő formátumban adja vissza:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

toosee hello meglévő címkék egy *erőforrása, amely egy megadott erőforrás-azonosító*, használja:

```azurecli
az resource show --id {resource-id} --query tags
```

Vagy toosee hello meglévő címkék egy *erőforrást, amely rendelkezik a megadott nevét, típusát és erőforrás-csoport*, használja:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

egy adott címkét tartalmazó erőforráscsoportokat tooget használja `az group list`:

```azurecli
az group list --tag Dept=IT
```

tooget összes hello erőforrásokat, amelyek egy adott címke és értéket, használjon `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Minden alkalommal, amikor a címkék tooa erőforrás vagy egy erőforráscsoport alkalmazásához, felülírhatja hello meglévő címkét az adott erőforrás vagy az erőforráscsoportot. Ezért hello erőforrás vagy erőforráscsoport rendelkezik-e meglévő címkék alapján másik módszert kell használnia. 

tooadd címkéket tooa *erőforráscsoport meglévő címkék nélkül*, használja:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

tooadd címkéket tooa *meglévő címkék nélkül erőforrás*, használja:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

tooadd címkék tooa erőforrás, amely már a címkék, hello meglévő címkék beolvasása, formázza újra ezt az értéket, és alkalmazza újra a meglévő és új címkék hello: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

minden tooapply egy erőforrás tooits erőforrások, a címkéket és *nem őriz meg a meglévő címkék hello erőforrások*, a következő parancsfájl hello használata:

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

minden tooapply egy erőforrás tooits erőforrások, a címkéket és *tartsa meg az erőforrás meglévő címkék*, a következő parancsfájl hello használata:

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


## <a name="templates"></a>Sablonok

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portál
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a>REST API
hello Azure-portál és a PowerShell is használható hello [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) hello színfalak mögött. Ha egy másik környezetbe címkézés toointegrate van szüksége, címkék használatával kaphat **beolvasása** hello erőforrás azonosítója és a frissítés hello csoportján címkék használatával egy **javítás** hívható meg.

## <a name="tags-and-billing"></a>Címkék és a számlázás
Címkék toogroup a számlázási adatokat használhatja. Például ha a másik szervezet számára több virtuális gép fut, használja hello címkék toogroup használati költségközpont által. Címkék toocategorize költségek futtatókörnyezetben, például a számlázási használata hello hello éles környezetben futó virtuális gépek által is használható.


Visszaállíthatja az keresztül hello címkékkel kapcsolatos információkat is [Azure erőforrás-használat és RateCard API-k](../billing/billing-usage-rate-card-overview.md) vagy hello használati vesszővel tagolt (CSV) fájl. Hello használati fájl letöltését hello [Azure-fiók portálon](https://account.windowsazure.com/) vagy [EA portal](https://ea.azure.com). Programozott hozzáférés toobilling információk kapcsolatos további információkért lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](../billing/billing-usage-rate-card-overview.md). A REST API-műveleteket, lásd: [Azure számlázási REST API-referencia](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).


Hello használata CSV számlázási címkék támogató szolgáltatások letöltésekor hello címkék jelennek meg hello **címkék** oszlop. További információkért lásd: [a számlázási megérteni a Microsoft Azure](../billing/billing-understand-your-bill.md).

![Tekintse meg a számlázási címkék](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Következő lépések
* Testreszabott házirendekkel alkalmazhat korlátozások és egyezmények az előfizetéshez. Megadhat egy házirendet, minden erőforrás rendelkezik-e egy adott címke értéke lehet szükség. További információkért lásd: [házirendek toomanage erőforrásainak használatához, és hozzáférést](resource-manager-policy.md).
* Egy bevezető toousing Azure PowerShell erőforrások telepít, ha talál [az Azure PowerShell használata Azure Resource Managerrel](powershell-azure-resource-manager.md).
* Egy bevezető toousing hello Azure CLI erőforrások telepít, ha talál [Using hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](xplat-cli-azure-resource-manager.md).
* Egy bevezető toousing hello portálhoz, lásd: [Using hello Azure portál toomanage az Azure-erőforrások](resource-group-portal.md).  
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

