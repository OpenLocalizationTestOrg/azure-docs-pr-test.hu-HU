---
title: "Az Azure házirend json minta - címke-egyeztetési minta kényszerítése |} Microsoft Docs"
description: "A json minta házirend feltétele, hogy erőforrások megfelelnek-e az egyeztetési minta címke értéke."
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 11/13/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: b8382df6d0f90264b2526392d4559c9ced9c6c70
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/13/2017
---
# <a name="enforce-tag-match-pattern-for-tag-values"></a>Címke-egyeztetési minta címke értékek kényszerítése

Szükséges, hogy a címke értéke megfelel-e egy egyeztetési minta. Adja meg a megengedett minta házirendszabály.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Minta sablon

[!code-json[main](../../../policy-templates/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.json "enforce match pattern")]

A sablon használatával telepíthető a [Azure-portálon](#deploy-with-the-portal), a [PowerShell](#deploy-with-powershell) vagy a [Azure CLI](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Üzembe helyezés a portállal

[![Üzembe helyezés az Azure-ban](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FTextPatterns%2Fenforce-tag-match-pattern%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Üzembe helyezés a PowerShell-lel

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "enforce-tag-match-pattern" -DisplayName "Ensure that a tag value matches a text pattern." -description "Ensure that a tag value matches a text pattern." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>PowerShell központi telepítés tisztítása

A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Üzembe helyezés az Azure parancssori felületén keresztül

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli
az policy definition create --name 'enforce-tag-match-pattern' --display-name 'Ensure that a tag value matches a text pattern.' --description 'Ensure that a tag value matches a text pattern.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "enforce-tag-match-pattern"
```

### <a name="clean-up-azure-cli-deployment"></a>Az Azure parancssori felület központi telepítés tisztítása

A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Következő lépések

- További Azure csoportházirend-sablon minták erővel [sablonok Azure házirend](../json-samples.md).
