---
title: "aaaLock Azure-erőforrások tooprevent módosítások |} Microsoft Docs"
description: "Egyes felhasználók a frissítése vagy törlése a kritikus fontosságú Azure-erőforrások a zárolási összes felhasználók és szerepkörök alkalmazásával."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 1f0d8911b2b129069bd2f01a9a4ec0a3cf700118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="lock-resources-tooprevent-unexpected-changes"></a>Erőforrások tooprevent zárolása váratlan változások 
Ha Ön rendszergazda szükség lehet toolock előfizetés, erőforrás vagy az erőforrás tooprevent más felhasználók véletlen törlése vagy a kritikus erőforrásokat módosítása a szervezetében. Beállíthatja azt hello zárolási szint túl**CanNotDelete** vagy **ReadOnly**. 

* **CanNotDelete** azt jelenti, hogy a jogosult felhasználók továbbra is olvasni és módosítani az erőforrást, de azokat nem lehet törölni a hello erőforrás. 
* **Csak olvasható** azt jelenti, hogy a jogosult felhasználók olvashatják egy erőforrást, de nem lehet törölni vagy hello erőforrás frissítése. A zárolás alkalmazása az összes jogosult felhasználók toohello engedélyekre hello hasonló toorestricting **olvasó** szerepkör. 

## <a name="how-locks-are-applied"></a>Hogyan alkalmazza a zárolásokat

A szülő hatókörben zárolást alkalmazásakor hatókörön belüli összes erőforrás öröklése hello azonos zárolása. Később fel még akkor is, erőforrások örökölt hello zárolási hello. hello szigorúbb zárolást hello öröklési lép érvénybe.

Szerepköralapú hozzáférés-vezérlés, ellentétben a felügyeleti zárolás tooapply korlátozás összes felhasználók és szerepkörök használhat. toolearn vonatkozó engedélyek beállítása a felhasználók és szerepkörök, lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

Erőforrás-kezelő zárolások alkalmazása csak a fordul elő hello felügyeleti vezérlősík, amely túl elküldött műveletek toooperations`https://management.azure.com`. Hogyan erőforrások feladatokat a saját hello zárolások feloldása nem korlátozzák. Erőforrás módosítások korlátozott, de erőforrás műveletek nem korlátozottak. Például egy SQL-adatbázis csak olvasható zárolást megakadályozza, hogy törli vagy módosítása hello adatbázis, de nem akadályozza meg a létrehozása, frissítése vagy törlése hello adatbázis adatait. Adatok tranzakciója engedélyezettek, mert ezek a műveletek nem kerülnek túl`https://management.azure.com`.

Alkalmazása **ReadOnly** toounexpected eredmények vezethet, mivel az egyes műveletek, amelyek az adatok, például olvasási műveletek ténylegesen szükséges további műveleteket. Például, ha kialakít egy **ReadOnly** zárolását egy tárfiókot minden felhasználót megakadályoz hello kulcsainak listázása. hello listában kulcsok művelet segítségével egy POST kérést kezel, mert hello vissza kulcsok elérhetők az írási műveletek. Például ha helyez el egy **csak olvasható** egy App Service erőforrás zárolását megakadályozza, hogy a Visual Studio Server Explorer hello erőforrás fájl jelenik meg, mert az adott interakció írási hozzáféréssel kell rendelkeznie.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Aki létrehozhat vagy törölhet zárolásokat a szervezetében
toocreate vagy delete felügyeleti zárolás, hozzá kell férnie túl`Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek. Hello beépített szerepkörök, csak **tulajdonos** és **felhasználói hozzáférés adminisztrátora** kapnak a csatolási műveleteket.

## <a name="portal"></a>Portál
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Sablon
hello következő példa bemutatja a sablont, amely a zárolási létrehoz egy tárfiókot. a mely tooapply hello zárolási paraméterként megadott hello tárfiók. hello hello zárolás neve által létrehozott hello erőforrás nevét hozzáfűzésével **/Microsoft.Authorization/** és hello zárolás név ebben az esetben hello **myLock**.

hello megadott típus adott toohello erőforrástípus. A tároláshoz beállítása hello típus too"Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a>PowerShell
Akkor zárolási lett telepítve az Azure PowerShell erőforrások hello [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) parancsot.

toolock egy erőforrást, adja meg a hello hello erőforrás nevét, az erőforrás típusa és az erőforráscsoport neve.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

toolock egy erőforráscsoport, hello hello erőforráscsoport nevét adja meg.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

a zárolási, használjon tooget információt [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). tooget az előfizetésében szereplő összes hello zárolás használja:

```powershell
Get-AzureRmResourceLock
```

tooget összes zárolás erőforrás esetén használja:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

tooget erőforráscsoport, az összes zárolás használja:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Az Azure PowerShell biztosít a más parancsok működő zárolásokat, például a [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate zárolást, és [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete zárolást.

## <a name="azure-cli"></a>Azure CLI

Ön zárolási lett telepítve az Azure parancssori Felülettel rendelkező erőforrásokat hello [az zárolási létrehozása](/cli/azure/lock#create) parancsot.

toolock egy erőforrást, adja meg a hello hello erőforrás nevét, az erőforrás típusa és az erőforráscsoport neve.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

toolock egy erőforráscsoport, hello hello erőforráscsoport nevét adja meg.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

a zárolási, használjon tooget információt [az zárolási lista](/cli/azure/lock#list). tooget az előfizetésében szereplő összes hello zárolás használja:

```azurecli
az lock list
```

tooget összes zárolás erőforrás esetén használja:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

tooget erőforráscsoport, az összes zárolás használja:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Az Azure CLI biztosít a más parancsok működő zárolásokat, például a [az zárolási frissítés](/cli/azure/lock#update) tooupdate zárolást, és [az zárolási törlése](/cli/azure/lock#delete) toodelete zárolást.

## <a name="rest-api"></a>REST API
Zárolhatja hello telepített erőforrások [REST API-t a felügyeleti zárolás](https://docs.microsoft.com/rest/api/resources/managementlocks). hello REST API lehetővé teszi a toocreate zárolásainak törlése és meglévő zárolások vonatkozó információk lekéréséhez.

a zárolási toocreate futtatása:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás. hello zárolási-név bármilyen kívánt toocall hello zárolása. Api-version, használjon **2015-01-01**.

Hello kérelem egy JSON-objektum, amely meghatározza a hello zárolási hello tulajdonságait tartalmazza.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Következő lépések
* Működő erőforrás zárral kapcsolatos további információkért lásd: [zár le az Azure-erőforrások](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* az erőforrások logikailag szervezése toolearn lásd [Using címkéket tooorganize az erőforrások](resource-group-using-tags.md)
* toochange, melyik erőforráscsoport erőforrás fájlcsoportban helyezkedik el, lásd: [erőforrások toonew erőforrás csoport áthelyezése](resource-group-move-resources.md)
* Az előfizetés testreszabott házirendekkel korlátozások és egyezmények alkalmazhat. További információkért lásd: [vezérlési hozzáférési és használati feltételei toomanage erőforrások](resource-manager-policy.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

