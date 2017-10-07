---
title: "aaaManage Azure PowerShell-megoldások |} Microsoft Docs"
description: "Azure PowerShell és az erőforrás-kezelő toomanage az erőforrások használatára."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 47a91af8d7eb59585bcfd43571ce76b90a0d7971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Az Azure PowerShell és a Resource Manager-erőforrások kezelése
> [!div class="op_single_selector"]
> * [Portál](resource-group-portal.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST API](resource-manager-rest-api.md)
>
>

Ebből a cikkből megismerheti, hogyan toomanage a megoldások Azure PowerShell és az Azure Resource Manager. Ha nem ismeri a Resource Manager, lásd: [Resource Manager áttekintése](resource-group-overview.md). Ez a témakör ismerteti a felügyeleti feladatokat. Az alábbiakat fogja elvégezni:

1. Hozzon létre egy erőforráscsoportot
2. Erőforráscsoport toohello erőforrás hozzáadása
3. Címke toohello erőforrás hozzáadása
4. A nevek vagy címke értékek alapján-erőforrások lekérdezése
5. Alkalmazza, és távolítsa el a hello erőforrás zárolását
6. Egy erőforráscsoportot

Ne jelenjen meg ez a cikk hogyan toodeploy Resource Manager sablon tooyour előfizetés. Ez az információ, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Ismerkedés az Azure PowerShell

Ha még nem telepítette az Azure PowerShell, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

Ha Azure PowerShell-lel telepítették az elmúlt hello, de nem frissítve, legutóbb, javasoljuk, hogy hello legújabb verzióját telepítse. Frissítheti az hello hello verzióból ugyanezt a módszert tooinstall használta azt. Például ha hello Webplatform-telepítővel, indítsa el újra, és keresse meg egy frissítés.

toocheck hello Azure-erőforrások modul verziójának használatára hello a következő parancsmagot:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Ez a témakör frissítve lett a 3.3.0 verziója. Ha egy korábbi, a felhasználói élmény nem egyeznek meg ebben a témakörben leírt hello lépés. Az ebben a verzióban a hello parancsmagokról dokumentációjáért lásd: [AzureRM.Resources modul](/powershell/module/azurerm.resources).

## <a name="log-in-tooyour-azure-account"></a>Jelentkezzen be tooyour Azure-fiók
A megoldás dolgozik, előtt be kell jelentkeznie tooyour fiók.

az Azure-fiókra, tooyour toolog hello használata **Login-AzureRmAccount** parancsmag.

```powershell
Login-AzureRmAccount
```

hello parancsmag kéri hello bejelentkezési hitelesítő adatait az Azure-fiókjával. A bejelentkezés után az tölti le a fiók beállításait,-e elérhető tooAzure PowerShell.

hello parancsmag a fiók és hello előfizetés toouse hello feladatok információt ad vissza.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Ha egynél több előfizetéssel rendelkezik, megváltoztathatja az tooa másik előfizetést. Első lépésként Ismerkedjen meg a fiókhoz az összes hello előfizetések.

```powershell
Get-AzureRmSubscription
```

Engedélyezett és letiltott előfizetésekhez adja vissza.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

tooswitch tooa másik előfizetést, adja meg a hello előfizetés nevét a hello **Set-AzureRmContext** parancsmag.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
Bármely erőforrások tooyour előfizetés telepítés megkezdése előtt létre kell hoznia a hello erőforrásokat tartalmazó erőforráscsoport.

egy erőforráscsoport, toocreate hello használata **New-AzureRmResourceGroup** parancsmag. hello parancs hello **neve** paraméter toospecify nevét hello erőforráscsoport és hello **hely** paraméter toospecify helyére.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

hello kimeneti hello a következő formátumban kell megadni:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Ha később kell tooretrieve hello erőforráscsoport, használja a következő parancsmag hello:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

tooget összes hello az előfizetés az erőforráscsoportokat, ne adjon meg egy nevet:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-tooa-resource-group"></a>Erőforrások tooa erőforráscsoport hozzáadása
tooadd erőforrás toohello erőforráscsoport, hello használható **New-AzureRmResource** létrehozásakor parancsmag vagy egy parancsmagot, amely adott toohello típusú erőforrás (például **New-AzureRmStorageAccount**). Bizonyára hasznosnak találja az egyszerűbb toouse parancsmag, amely adott tooa erőforrástípus, mert azokat a paramétereket a hello új erőforrás szükséges hello tulajdonságok tartalmazza. toouse **New-AzureRmResource**, minden hello tulajdonságok tooset ismernie kell őket megadása nélkül.

Azonban parancsmagokon keresztül erőforrás hozzáadása okozhat jövőbeli zavart mert hello új erőforrás nem létezik a Resource Manager-sablon. A Microsoft azt javasolja, adjon meg hello infrastruktúra az Azure-megoldás a Resource Manager-sablon. Sablonok lehetővé teszik a tooreliably, és a megoldás ismételt üzembe helyezése. Ez a témakör egy PowerShell-parancsmaggal hozzon létre egy tárfiókot, de később az erőforráscsoport a sablon létrehozása.

a következő parancsmag hello hoz létre egy tárfiókot. Hello példában látható módon hello neve helyett adjon egyedi nevet a hello tárfiók. hello neve 3 – 24 karakter hosszúságúnak kell, és csak számokat és kisbetűket tartalmazhatnak. Ha hello példában látható módon hello nevet használja, hibaüzenet, mert a név már használatban van.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Ha szükség tooretrieve ehhez az erőforráshoz később, akkor a következő parancsmag hello:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Címke hozzáadása

Címkék lehetővé teszik a tooorganize az erőforrások toodifferent tulajdonságok alapján történik. Például előfordulhat, hogy több erőforrás toohello tartozó különböző erőforrás-csoportok a részleghez. A részleg címke és érték toothose erőforrások toomark alkalmazhatja őket tartozó toohello, ugyanilyen kategóriájú. Vagy jelölheti meg használja-e egy üzemi és tesztkörnyezetben környezetben egy erőforrást. Ebben a témakörben telepítését címkék tooonly egy erőforrást, de a környezetben valószínűleg érdemes tooapply címkéket tooall az erőforrások.

a következő parancsmag hello két címkék tooyour tárfiók vonatkozik:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Címkék egyetlen objektumként frissülnek. tooadd egy címke tooa erőforrást, amely már tartalmazza a címkék, először kérjen le a meglévő hello címkék. Hello új címke toohello tartalmazó objektum hello meglévő címkék hozzáadása, és alkalmazza újra az összes hello címkék toohello erőforrás.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Erőforrások keresése

Használjon hello **keresés-AzureRmResource** parancsmag tooretrieve erőforrások különböző keresési feltételeket.

* tooget erőforrás nevét, adja meg a hello **ResourceNameContains** paraméter:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* tooget összes hello erőforráscsoportban, forrásokban hello **ResourceGroupNameContains** paraméter:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* tooget összes hello egy olyan címke nevét és értékét, forrásokban hello **TagName** és **TagValue** paraméterek:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* tooall hello erőforrásokhoz egy adott erőforrástípusra, adja meg a hello **ResourceType** paraméter:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a>Egy erőforrás zárolása

Toomake meg arról, hogy a kritikus fontosságú erőforrás nem véletlenül törölték vagy módosították van szüksége, a zárolás toohello erőforrás vonatkoznak. Vagy megadhat egy **CanNotDelete** vagy **ReadOnly**.

toocreate vagy delete felügyeleti zárolás, hozzá kell férnie túl`Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek. Hello beépített szerepkörök csak a tulajdonos és a felhasználói hozzáférés adminisztrátora kapnak ezeket a műveleteket.

a zárolási tooapply hello parancsmag a következő használja:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

hello zárolt erőforrás a fenti példa hello nem törölhető hello zárolási eltávolításáig. a zárolási tooremove használja:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

A beállítás zárolások kapcsolatos további információkért lásd: [erőforrások az Azure Resource Manager zárolása](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Távolítsa el az erőforrás vagy erőforráscsoport
Egy erőforrás vagy egy erőforráscsoportot is eltávolíthat. Egy erőforráscsoport, akkor is távolítsa el, amelyeket belül minden hello erőforrás.

* hello erőforráscsoport, használjon hello erőforrás toodelete **Remove-AzureRmResource** parancsmag. Ez a parancsmag hello erőforrás törli, de nem törli a hello erőforráscsoportot.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* toodelete az erőforráscsoportot és a hozzá tartozó összes erőforrásnak, használja a hello **Remove-AzureRmResourceGroup** parancsmag.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

Mindkét parancsmag, a rendszer kéri tooconfirm tooremove hello erőforrás vagy erőforráscsoport kívánja. Ha hello sikeresen töröl hello erőforrás vagy erőforráscsoportban található, visszaadja **igaz**.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Az Azure Automation szolgáltatásban, erőforrás-kezelő parancsfájlok futtatása

Ez a témakör bemutatja, hogyan tooperform az erőforrások az Azure PowerShell alapvető műveleteket. Speciális felügyeleti forgatókönyvekhez akkor általában szeretné, hogy egy parancsfájl toocreate, és újra felhasználhatja, hogy a parancsfájl igény szerint vagy ütemezés szerint. [Azure Automation szolgáltatásbeli](../automation/automation-intro.md) lehetőséget biztosít az Ön gyakran használt tooautomate olyan parancsfájlok, amelyek az Azure megoldások kezelése.

hello következő témakörök bemutatják, hogyan toouse Azure Automation, az erőforrás-kezelő és a PowerShell tooeffectively felügyeleti feladatok elvégzésére:

- Egy runbook létrehozásával kapcsolatos további információkért lásd: [az első PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).
- További információ a gyűjtemények, parancsfájlok,: [az Azure Automation forgatókönyv- és gyűjtemények](../automation/automation-runbook-gallery.md).
- A runbookok, amelyek indítása és leállítása a virtuális gépek, lásd: [Azure Automation-forgatókönyv: egy ütemezést az Azure virtuális gép indítási és leállítási címkék használatával JSON-formátumú toocreate](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- A runbookok, amelyek indítása és leállítása a virtuális gépek munkaidőn kívüli, lásd: [indítása/leállítása virtuális gépek munkaidőn kívüli megoldás az automatizálás során](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Következő lépések
* toolearn Resource Manager-sablonok létrehozásával kapcsolatban lásd: [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md).
* toolearn sablonok telepítésével kapcsolatban lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).
* Áthelyezheti a meglévő erőforrásokat tooa új erőforráscsoportot. Tekintse meg a [áthelyezési tooNew erőforráscsoport vagy előfizetés](resource-group-move-resources.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

