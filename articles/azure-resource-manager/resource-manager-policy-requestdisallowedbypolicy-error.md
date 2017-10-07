---
title: "az Azure erőforrás-házirenddel aaaRequestDisallowedByPolicy hiba |} Microsoft Docs"
description: "Ismerteti a hello hello RequestDisallowedByPolicy hiba okát."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 7870e40205cf433ccb4ba02376b5fe809f20d0df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Az Azure erőforrás-házirenddel RequestDisallowedByPolicy hiba

Ez a cikk azt ismerteti, hello hello RequestDisallowedByPolicy hiba okát, is megoldást nyújt az ezt a hibát.

## <a name="symptom"></a>Jelenség

Amikor toodo központi telepítése során egy műveletet, akkor fordulhat elő, egy **RequestDisallowedByPolicy** hiba, amely megakadályozza a hello művelet végezhető el. hello az alábbiakban látható egy minta hello hiba:

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Hibaelhárítás

hello-szabályzatot, amely blokkolja az üzembe helyezés tooretrieve adatait hello módszer a következő hello használata:

### <a name="method-1"></a>1. módszer

A PowerShellben adja meg, hogy a házirend az azonosítója, mint hello **azonosító** hello-szabályzatot, amely blokkolja a telepítési paraméter tooretrieve részleteit.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>2. módszer 

Az Azure CLI 2.0 adja meg a házirend-definíció hello hello nevét: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Megoldás

A biztonsági és megfelelőségi esetében az informatikai részleg egy erőforrás-házirend tiltja a nyilvános IP-cím címek, hálózati biztonsági csoportokat, felhasználói útvonalak vagy útvonaltáblák létrehozása előfordulhat, hogy kényszerítése. Hello minta hello hibaüzenet hello "Jelenség" szakaszban ismertetett hello házirend nevű **regionPolicyDefinition**, de eltérő lehet.
tooresolve a probléma, az informatikai részleg tooreview hello erőforrás-házirendekkel használhatók, és határozza meg, hogyan tooperform hello kért felelnek meg ezek a házirendek művelet.


További információkért tekintse meg a következő cikkek hello:

- [Erőforrás-házirendek – áttekintés](resource-manager-policy.md)
- [Gyakori telepítési hibák RequestDisallowedByPolicy](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


