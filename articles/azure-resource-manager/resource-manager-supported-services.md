---
title: "aaaAzure erőforrás-szolgáltatók és erőforrástípusok |} Microsoft Docs"
description: "Erőforrás-kezelő, a sémák és elérhető API-verzió támogató hello erőforrás-szolgáltatók és hello régiók hello erőforrásokat is üzemeltető ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 23db1d3808a20166f3b44ec801e1bcc46fbb9bd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-providers-and-types"></a>Erőforrás-szolgáltatók és típusát

Erőforrások telepítésekor gyakran hello erőforrás-szolgáltatók és típusok tooretrieve információra van szüksége. Ebből a cikkből megismerheti, hogy:

* Megtekintheti az összes erőforrás-szolgáltató az Azure-ban
* Egy erőforrás-szolgáltató regisztrációs állapotának ellenőrzése
* Egy erőforrás-szolgáltató regisztrálása
* Egy erőforrás-szolgáltató típusú erőforrások megtekintése
* Egy erőforrástípus érvényes helyek megtekintése
* Az erőforrástípus érvénytelen API-verziók megtekintése

Ezeket a lépéseket hello portal, a PowerShell vagy az Azure parancssori felület használatával végezheti el.

## <a name="powershell"></a>PowerShell

toosee Azure-ban, és hello regisztrációs állapotát az előfizetéshez tartozó összes erőforrás-szolgáltató használata:

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Amely hasonló eredményeket ad vissza:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Egy erőforrás-szolgáltató regisztrálása konfigurálja az előfizetés toowork hello erőforrás-szolgáltató. hello regisztrálási hatóköre mindig hello előfizetés. Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van. Azonban szükség lehet toomanually néhány erőforrás-szolgáltató regisztrálása. egy erőforrás-szolgáltató tooregister, rendelkeznie kell engedéllyel tooperform hello `/register/action` hello erőforrás-szolgáltató a műveletet. Ez a művelet hello közreműködői és tulajdonos szerepkörök tartalmazza.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Amely hasonló eredményeket ad vissza:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.

egy adott erőforrás-szolgáltató, használjon toosee információi:

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Amely hasonló eredményeket ad vissza:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

toosee hello erőforrástípusai egy erőforrás-szolgáltató használata:

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Amely adja vissza:

```powershell
batchAccounts
operations
locations
locations/quotas
```

hello API-verzió felel meg a közzétett hello erőforrás-szolgáltató REST API-műveleteket tooa verzióját. Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel felszabadít hello REST API-t egy új verziója. 

használja a tooget hello elérhető API-verziók egy erőforrás típusa:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Amely adja vissza:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Erőforrás-kezelő támogatott minden régióban, de előfordulhat, hogy minden régióban nem támogatott hello erőforrások telepítése. Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy egyes régiókban hello erőforrás-t támogató használatával korlátozásait. 

tooget hello támogatott helyek az erőforrás-típust használja.

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Amely adja vissza:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI
toosee Azure-ban, és hello regisztrációs állapotát az előfizetéshez tartozó összes erőforrás-szolgáltató használata:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Amely hasonló eredményeket ad vissza:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Egy erőforrás-szolgáltató regisztrálása konfigurálja az előfizetés toowork hello erőforrás-szolgáltató. hello regisztrálási hatóköre mindig hello előfizetés. Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van. Azonban szükség lehet toomanually néhány erőforrás-szolgáltató regisztrálása. egy erőforrás-szolgáltató tooregister, rendelkeznie kell engedéllyel tooperform hello `/register/action` hello erőforrás-szolgáltató a műveletet. Ez a művelet hello közreműködői és tulajdonos szerepkörök tartalmazza.

```azurecli
az provider register --namespace Microsoft.Batch
```

Egy üzenet, hogy a regisztrációs visszaadó folyamatban.

Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.

egy adott erőforrás-szolgáltató, használjon toosee információi:

```azurecli
az provider show --namespace Microsoft.Batch
```

Amely hasonló eredményeket ad vissza:

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

toosee hello erőforrástípusai egy erőforrás-szolgáltató használata:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Amely adja vissza:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

hello API-verzió felel meg a közzétett hello erőforrás-szolgáltató REST API-műveleteket tooa verzióját. Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel felszabadít hello REST API-t egy új verziója. 

használja a tooget hello elérhető API-verziók egy erőforrás típusa:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Amely adja vissza:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Erőforrás-kezelő támogatott minden régióban, de előfordulhat, hogy minden régióban nem támogatott hello erőforrások telepítése. Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy egyes régiókban hello erőforrás-t támogató használatával korlátozásait. 

tooget hello támogatott helyek az erőforrás-típust használja.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Amely adja vissza:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a>Portál

toosee Azure-ban, és hello regisztrációs állapotát az előfizetéshez tartozó összes erőforrás-szolgáltató kiválasztása **előfizetések**.

![az előfizetések kiválasztása](./media/resource-manager-supported-services/select-subscriptions.png)

Válassza ki a hello előfizetés tooview.

![Adja meg az előfizetést](./media/resource-manager-supported-services/subscription.png)

Válassza ki **erőforrás-szolgáltató** és elérhető erőforrás-szolgáltatók hello listájának megtekintése.

![erőforrás-szolgáltatók megjelenítése](./media/resource-manager-supported-services/show-resource-providers.png)

Egy erőforrás-szolgáltató regisztrálása konfigurálja az előfizetés toowork hello erőforrás-szolgáltató. hello regisztrálási hatóköre mindig hello előfizetés. Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van. Azonban szükség lehet toomanually néhány erőforrás-szolgáltató regisztrálása. egy erőforrás-szolgáltató tooregister, rendelkeznie kell engedéllyel tooperform hello `/register/action` hello erőforrás-szolgáltató a műveletet. Ez a művelet hello közreműködői és tulajdonos szerepkörök tartalmazza. egy erőforrás-szolgáltató tooregister válasszon **regisztrálása**.

![erőforrás-szolgáltató regisztrálása](./media/resource-manager-supported-services/register-provider.png)

Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.

toosee adatai egy adott erőforrás-szolgáltató kiválasztása **további szolgáltatások**.

![Jelölje ki a további szolgáltatások](./media/resource-manager-supported-services/more-services.png)

Keresse meg **erőforrás-kezelő** , és jelölje ki a hello rendelkezésre álló lehetőségek közül.

![Válassza ki az erőforrás-kezelő](./media/resource-manager-supported-services/select-resource-explorer.png)

Válassza ki **szolgáltatók**.

![Szolgáltatók kiválasztása](./media/resource-manager-supported-services/select-providers.png)

Jelölje be hello erőforrás-szolgáltató és az erőforrás írja be a megjeleníteni kívánt tooview.

![Válassza ki az erőforrás típusa](./media/resource-manager-supported-services/select-resource-type.png)

Erőforrás-kezelő támogatott minden régióban, de előfordulhat, hogy minden régióban nem támogatott hello erőforrások telepítése. Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy egyes régiókban hello erőforrás-t támogató használatával korlátozásait. hello erőforrás-kezelő hello erőforrástípus érvényes helyét jeleníti meg.

![Hely megjelenítése](./media/resource-manager-supported-services/show-locations.png)

hello API-verzió felel meg a közzétett hello erőforrás-szolgáltató REST API-műveleteket tooa verzióját. Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel felszabadít hello REST API-t egy új verziója. az erőforrás-kezelő hello hello erőforrástípus érvénytelen API-verziók jeleníti meg.

![API-verziók megjelenítése](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Következő lépések
* toolearn Resource Manager-sablonok létrehozásával kapcsolatban lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toolearn erőforrások telepítésével kapcsolatban lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).
* tooview hello műveletek erőforrás-szolgáltató, lásd: [Azure REST API](/rest/api/).

