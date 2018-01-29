---
title: "Azure-erőforrás-szolgáltató regisztrációs hibák |} Microsoft Docs"
description: "Ismerteti, hogyan lehet az Azure erőforrás-szolgáltató regisztrációs hibák."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: d6a99917e732a3439a31cafa5608348694014054
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Hárítsa el a hibákat, az erőforrás-szolgáltató regisztrálása

Ez a cikk ismerteti a hibák jelentkezhetnek, ha előzőleg nem használta az előfizetésében erőforrás-szolgáltató használatával.

## <a name="symptom"></a>Jelenség

Erőforrás való telepítésekor a következő hibakód és üzenet jelenhet meg:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Vagy hasonló üzenet jelenhet meg:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

## <a name="cause"></a>Ok

Ezek a hibák három okok egyike jelenhet meg:

1. Az előfizetés nincs regisztrálva az erőforrás-szolgáltató
1. Az erőforrástípus nem támogatott API-verzió
1. Az erőforrástípus nem támogatott helyre

## <a name="solution"></a>Megoldás

A hibaüzenet a következő támogatott helyek és API-verziók javaslatokat adjon meg. A sablon módosíthatja a javasolt értékek egyikére. A legtöbb szolgáltatók a következők: automatikusan által az Azure-portálon vagy a parancssori felületet használ, de nem minden. Ha még nem használta az egy adott erőforrás-szolgáltató előtt, szükség lehet regisztrálni ezt a szolgáltatót. További információ a PowerShell vagy Azure parancssori felületen keresztül erőforrás-szolgáltatók felderíthetők.

### <a name="solution-1"></a>1 megoldás

A PowerShell, használjon **Get-AzureRmResourceProvider** a regisztrációs állapotát tekintheti meg.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Regisztrálja a szolgáltatót, használja a **Register-AzureRmResourceProvider** , és adja meg a regisztrálni kívánt erőforrás-szolgáltató neve.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

A támogatott helyek egy adott típusú erőforrás, amelyet:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

A támogatott API-verziók egy adott típusú erőforrás, amelyet:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

### <a name="solution-2"></a>Megoldás 2

**Azure CLI**

Regisztrálva van-e a szolgáltató megjelenítéséhez használja a `az provider list` parancsot.

```azurecli-interactive
az provider list
```

Egy erőforrás-szolgáltató regisztrálásához használja a `az provider register` parancsot, és adja meg a *névtér* regisztrálni.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

A támogatott helyek és API-verziók erőforrástípus megjelenítéséhez használja:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

### <a name="solution-3"></a>Megoldás 3

Tekintse meg a regisztrációs állapotát, és regisztrálja egy erőforrás-szolgáltató névtere a portálon keresztül.

1. Az előfizetéshez, válasszon **erőforrás-szolgáltató**.

   ![Válassza ki az erőforrás-szolgáltató](./media/resource-manager-register-provider-errors/select-resource-provider.png)

1. Nézze meg az erőforrás-szolgáltatók listáján, és ha szükséges, jelölje be a **regisztrálása** a telepíteni kívánt típusú erőforrás-szolgáltató regisztrálása mutató hivatkozást.

   ![erőforrás-szolgáltatók felsorolása](./media/resource-manager-register-provider-errors/list-resource-providers.png)
