---
title: "aaaUse PowerShell toomanage Azure Service Bus erőforrásainak |} Microsoft Docs"
description: "PowerShell modul toocreate használja, és a Service Bus-erőforrások kezelése"
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/06/2017
ms.author: sethm
ms.openlocfilehash: 737044def913c5798e7e05fc4f1aeece76c8f4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-service-bus-resources"></a>PowerShell toomanage Service Bus erőforrásainak használata

A Microsoft Azure PowerShell egy parancsfájl-kezelési környezet, hogy toocontrol használ, és automatizálni hello telepítése és kezelése az Azure-szolgáltatásokhoz. Ez a cikk ismerteti, hogyan toouse hello [Service Bus Resource Manager PowerShell-modul](/powershell/module/azurerm.servicebus) tooprovision és kezelheti a Service Bus-entitások (névterek, üzenetsorok, témakörök és előfizetések) helyi Azure PowerShell-konzollal vagy parancsfájl.

Service Bus-entitások Azure Resource Manager-sablonok segítségével is kezelheti. További információkért lásd: hello cikk [Azure Resource Manager-sablonok létrehozása a Service Bus-erőforrások](service-bus-resource-manager-overview.md).

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, hello következőkre lesz szüksége:

* Azure-előfizetés. Előfizetés beszerzésével kapcsolatos további információkért lásd: [beszerzési lehetőségek][purchase options], [ajánlatok][member offers], vagy [ingyenes fiókot][free account].
* Az Azure PowerShell számítógép. Útmutatásért lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps).
* A PowerShell-parancsfájlok, a NuGet-csomagok és a .NET-keretrendszer hello általános ismertetése.

## <a name="get-started"></a>Bevezetés

hello első lépése pedig toouse PowerShell toolog tooyour Azure-fiókot az Azure-előfizetés. Hello utasításait követve [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps) toolog tooyour Azure-fiókra, és olvassák be és hozzáférés az Azure-előfizetéshez hello erőforrásokhoz.

## <a name="provision-a-service-bus-namespace"></a>A Service Bus-névtér kiépítése

A Szolgáltatásbusz-névterek használatakor hello használhatja [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), és [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) parancsmagok.

Ebben a példában néhány helyi változók hello parancsfájlban; hoz létre. `$Namespace` és `$Location`.

* `$Namespace`hello nevét, amelyhez toowork szeretnénk hello Service Bus-névtér van.
* `$Location`azonosítja a hello adatközpont, amelyen kiépíti a Microsoft hello névtér.
* `$CurrentNamespace`hello hivatkozás névtér, amely azt lekérése (vagy hozzon létre) tárolja.

Egy tényleges parancsfájlban `$Namespace` és `$Location` paraméterként kell.

Ez a kijelző hello parancsfájl hello a következő:

1. Kísérletek tooretrieve hello a Service Bus-névtér adta meg.
2. Ha hello névtérben található, mi található jelenti.
3. Ha hello névtér nem található, akkor hello névteret hoz létre, és majd beolvassa az újonnan létrehozott névtér hello.
   
    ``` powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>Egy névtér engedélyezési szabály létrehozása

hello következő példa bemutatja, hogyan toomanage névtér engedélyezési szabályok segítségével hello [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), és [Remove-AzureRmServiceBusNamespaceAuthorizationRule parancsmagok](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).

```powershell
# Query toosee if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if hello rule already exists or needs toobe created
if ($CurrentRule)
{
    Write-Host "hello $AuthRule rule already exists for hello namespace $Namespace."
}
else
{
    Write-Host "hello $AuthRule rule does not exist."
    Write-Host "Creating hello $AuthRule rule for hello $Namespace namespace..."
    New-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "hello $AuthRule rule for hello $Namespace namespace has been successfully created."

    Write-Host "Setting rights on hello namespace"
    $authRuleObj = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights toohello namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusNamespaceKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
}
```

## <a name="create-a-queue"></a>Üzenetsor létrehozása

egy várólista toocreate vagy témakör ellenőriznie a névtér hello parancsfájllal hello előző szakaszban. Ezután hozzon létre hello várólista:

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "hello queue $QueueName already exists in hello $Location region:"
}
else
{
    Write-Host "hello $QueueName queue does not exist."
    Write-Host "Creating hello $QueueName queue in hello $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "hello $QueueName queue in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>Várólista-tulajdonságok módosítása

Után hello-parancsprogram végrehajtása az előző szakaszban hello, használhatja a hello [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) parancsmag tooupdate hello tulajdonságok várólista, mint például a következő hello:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Kiépítés más Service Bus-entitások

Használhatja a hello [Service Bus PowerShell modul](/powershell/module/azurerm.servicebus) tooprovision egyéb entitások, például az üzenettémák és előfizetések. Ezek a parancsmagok szintaktikailag hasonló toohello várólista létrehozása parancsmagok hello előző szakaszban bemutatott.

## <a name="next-steps"></a>Következő lépések

- A dokumentációban hello teljes Service Bus Resource Manager PowerShell modul [Itt](/powershell/module/azurerm.servicebus). Ezen a lapon az összes elérhető parancsmagok sorolja fel.
- Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: hello cikk [Azure Resource Manager-sablonok létrehozása a Service Bus-erőforrások](service-bus-resource-manager-overview.md).
- Információ a [Service Bus .NET kezelési kódtárakat](service-bus-management-libraries.md).

Van néhány alternatív módszert toomanage Service Bus-entitások, ezek a blogbejegyzések leírtak szerint:

* [Hogyan toocreate Service Bus üzenetsorok, témakörök és előfizetések egy PowerShell-parancsfájl használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Hogyan toocreate a Service Bus Namespace, és az Event Hubs egy PowerShell-parancsfájl használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Service Bus PowerShell parancsfájlok](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
