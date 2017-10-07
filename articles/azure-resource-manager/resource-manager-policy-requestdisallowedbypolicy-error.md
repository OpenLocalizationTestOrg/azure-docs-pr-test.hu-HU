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
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a><span data-ttu-id="5951d-103">Az Azure erőforrás-házirenddel RequestDisallowedByPolicy hiba</span><span class="sxs-lookup"><span data-stu-id="5951d-103">RequestDisallowedByPolicy error with Azure resource policy</span></span>

<span data-ttu-id="5951d-104">Ez a cikk azt ismerteti, hello hello RequestDisallowedByPolicy hiba okát, is megoldást nyújt az ezt a hibát.</span><span class="sxs-lookup"><span data-stu-id="5951d-104">This article describes hello cause of hello RequestDisallowedByPolicy error, it also provides solution for this error.</span></span>

## <a name="symptom"></a><span data-ttu-id="5951d-105">Jelenség</span><span class="sxs-lookup"><span data-stu-id="5951d-105">Symptom</span></span>

<span data-ttu-id="5951d-106">Amikor toodo központi telepítése során egy műveletet, akkor fordulhat elő, egy **RequestDisallowedByPolicy** hiba, amely megakadályozza a hello művelet végezhető el.</span><span class="sxs-lookup"><span data-stu-id="5951d-106">When you try toodo an action during deployment, you might receive a **RequestDisallowedByPolicy** error that prevents hello action be performed.</span></span> <span data-ttu-id="5951d-107">hello az alábbiakban látható egy minta hello hiba:</span><span class="sxs-lookup"><span data-stu-id="5951d-107">hello following is a sample of hello error:</span></span>

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a><span data-ttu-id="5951d-108">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="5951d-108">Troubleshooting</span></span>

<span data-ttu-id="5951d-109">hello-szabályzatot, amely blokkolja az üzembe helyezés tooretrieve adatait hello módszer a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="5951d-109">tooretrieve details about hello policy that blocked your deployment, use hello following one of hello methods:</span></span>

### <a name="method-1"></a><span data-ttu-id="5951d-110">1. módszer</span><span class="sxs-lookup"><span data-stu-id="5951d-110">Method 1</span></span>

<span data-ttu-id="5951d-111">A PowerShellben adja meg, hogy a házirend az azonosítója, mint hello **azonosító** hello-szabályzatot, amely blokkolja a telepítési paraméter tooretrieve részleteit.</span><span class="sxs-lookup"><span data-stu-id="5951d-111">In PowerShell, provide that policy identifier as hello **Id** parameter tooretrieve details about hello policy that blocked your deployment.</span></span>

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a><span data-ttu-id="5951d-112">2. módszer</span><span class="sxs-lookup"><span data-stu-id="5951d-112">Method 2</span></span> 

<span data-ttu-id="5951d-113">Az Azure CLI 2.0 adja meg a házirend-definíció hello hello nevét:</span><span class="sxs-lookup"><span data-stu-id="5951d-113">In Azure CLI 2.0, provide hello name of hello policy definition:</span></span> 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a><span data-ttu-id="5951d-114">Megoldás</span><span class="sxs-lookup"><span data-stu-id="5951d-114">Solution</span></span>

<span data-ttu-id="5951d-115">A biztonsági és megfelelőségi esetében az informatikai részleg egy erőforrás-házirend tiltja a nyilvános IP-cím címek, hálózati biztonsági csoportokat, felhasználói útvonalak vagy útvonaltáblák létrehozása előfordulhat, hogy kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="5951d-115">For security or compliance, your IT department might enforce a resource policy that prohibits creating Public IP addresses, Network Security Groups, User-Defined Routes, or route tables.</span></span> <span data-ttu-id="5951d-116">Hello minta hello hibaüzenet hello "Jelenség" szakaszban ismertetett hello házirend nevű **regionPolicyDefinition**, de eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="5951d-116">In hello sample of hello error message that is described in hello "Symptoms" section, hello policy is named **regionPolicyDefinition**, but it could be different.</span></span>
<span data-ttu-id="5951d-117">tooresolve a probléma, az informatikai részleg tooreview hello erőforrás-házirendekkel használhatók, és határozza meg, hogyan tooperform hello kért felelnek meg ezek a házirendek művelet.</span><span class="sxs-lookup"><span data-stu-id="5951d-117">tooresolve this problem, work with your IT department tooreview hello resource policies, and determine how tooperform hello requested action in compliance with those policies.</span></span>


<span data-ttu-id="5951d-118">További információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="5951d-118">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="5951d-119">Erőforrás-házirendek – áttekintés</span><span class="sxs-lookup"><span data-stu-id="5951d-119">Resource policy overview</span></span>](resource-manager-policy.md)
- [<span data-ttu-id="5951d-120">Gyakori telepítési hibák RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="5951d-120">Common deployment errors-RequestDisallowedByPolicy</span></span>](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


