---
title: "aaaCreate Azure Service Bus-erőforrások Azure Resource Manager-sablonok |} Microsoft Docs"
description: "Azure Resource Manager sablonok tooautomate hello létrehozása a Service Bus erőforrásainak használata"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: e539902cae307b63ae7c332580e2064761331ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Service Bus erőforrásainak használata Azure Resource Manager-sablonok létrehozása

Ez a cikk ismerteti, hogyan toocreate és központi telepítése a Service Bus erőforrásainak Azure Resource Manager sablonok, a PowerShell és a Service Bus-hello erőforrás-szolgáltató használatával.

Az Azure Resource Manager-sablonok segítségével hello erőforrások toodeploy megoldás, és toospecify paramétereket és változókat, amelyek lehetővé teszik a különböző környezetek tooinput értékeinek meghatározása. hello sablon JSON és, hogy tooconstruct értékeket is használhat az üzembe helyezéshez kifejezések áll. Azure Resource Manager-sablonok és hello sablon formátum döntéseken írt kapcsolatos részletes információkért lásd: [struktúra és az Azure Resource Manager-sablonok szintaxisát](../azure-resource-manager/resource-group-authoring-templates.md).

> [!NOTE]
> Ez a cikk megjelenítése szereplő példák hogyan hello Azure Resource Manager toocreate Service Bus-névtér toouse és üzenetküldési entitás (várólista). Más sablon példákat a Microsoft hello [Azure gyors üzembe helyezés sablontárban] [ Azure Quickstart Templates gallery] , és keressen a "Service Bus."
>
>

## <a name="service-bus-resource-manager-templates"></a>Service Bus Resource Manager-sablonok

A Service Bus Azure Resource Manager sablonok letöltése és központi telepítés érhetők el. Kattintson a következő hivatkozások minden egy, a Githubon található hivatkozások toohello sablonokkal kapcsolatos részletekért hello:

* [Service Bus-névtér létrehozása](service-bus-resource-manager-namespace.md)
* [Hozzon létre egy Service Bus-névtér várólista](service-bus-resource-manager-namespace-queue.md)
* [Hozzon létre egy Service Bus-névtér témakör és előfizetés](service-bus-resource-manager-namespace-topic.md)
* [Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály](service-bus-resource-manager-namespace-auth-rule.md)
* [A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>Üzembe helyezés a PowerShell-lel

hello alábbi eljárás ismerteti, hogyan toouse PowerShell toodeploy Azure Resource Manager-sablon, amely létrehoz egy **szabványos** réteg a Service Bus-névtér, és egy sor az adott névtérben. Ez a példa alapján hello [hozzon létre egy Service Bus-névtér várólista](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) sablont. hello hozzávetőleges munkafolyamata a következőképpen történik:

1. Telepítse a PowerShell.
2. Hozzon létre hello sablon és (opcionálisan) a paraméter.
3. PowerShell, a bejelentkezés tooyour Azure-fiók.
4. Ha még nem létezik, hozzon létre egy új erőforráscsoportot.
5. Hello telepítés tesztelésére.
6. Ha szükséges, állítsa be a hello telepítési módban.
7. Hello sablon üzembe helyezése.

Azure Resource Manager-sablonok telepítésével kapcsolatos részletes információkért lásd: [telepítése Azure Resource Manager-sablonok erőforrások][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>A PowerShell telepítése

Telepítse az Azure PowerShell hello utasításait követve [Ismerkedés az Azure PowerShell](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Sablon létrehozása

Klónozott vagy másolat hello [201-szolgáltatásbusz--várólista létrehozása](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) sablont a Githubból:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by hello template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Hozzon létre egy paraméterfájl (nem kötelező)

toouse egy választható paraméterek: fájl másolása hello [201-szolgáltatásbusz--várólista létrehozása](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) fájlt. Cserélje le a hello értékének `serviceBusNamespaceName` hello Service Bus-névtér hello nevű szeretné, hogy toocreate ebben a telepítésben, és cserélje le a hello értékének `serviceBusQueueName` hello várólista hello nevű toocreate szeretné.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

További információkért lásd: hello [paraméterek](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) témakör.

### <a name="log-in-tooazure-and-set-hello-azure-subscription"></a>Jelentkezzen be tooAzure és hello Azure-előfizetés beállítása

Egy PowerShell-parancssorba futtassa a következő parancs hello:

```powershell
Login-AzureRmAccount
```

Az Azure-fiók tooyour rákérdezéses toolog áll. A bejelentkezés után futtassa a következő parancs tooview hello az elérhető előfizetések.

```powershell
Get-AzureRMSubscription
```

Ez a parancs elérhető Azure-előfizetések listáját adja vissza. Válasszon egy előfizetést az aktuális munkamenet a hello hello a következő parancs futtatásával. Cserélje le `<YourSubscriptionId>` , az Azure-előfizetés hello GUID hello toouse szeretne.

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-hello-resource-group"></a>Hello erőforrás csoport beállítása

Ha nem rendelkezik meglévő erőforrás csoport, hozzon létre egy új erőforráscsoportot hello ** New-AzureRmResourceGroup ** parancsot. Adja meg a hello erőforráscsoportot és helyet szeretne toouse hello nevét. Példa:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Ha sikeres, hello új erőforráscsoport összegzését jelenik meg.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-hello-deployment"></a>Hello központi telepítés tesztelése

A telepítés ellenőrzése hello futtatásával `Test-AzureRmResourceGroupDeployment` parancsmag. Hello központi telepítés tesztelésekor meg paramétereket pontosan ugyanúgy hello központi telepítés végrehajtása közben.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="create-hello-deployment"></a>Hello telepítésének létrehozása

toocreate hello új központi telepítés, futtatási hello `New-AzureRmResourceGroupDeployment` parancsmagot, és adja meg a hello szükséges paramétert, ha a rendszer kéri. hello paraméternek számít a nevet a telepítésnek, a erőforráscsoport, és hello elérési útja vagy URL-cím toohello sablonfájl hello nevét. Ha hello **mód** paraméter nincs megadva, alapértelmezett értéke hello **növekményes** szolgál. További információkért lásd: [növekményes és teljes telepítések](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

hello következő parancssorok meg hello három paraméterek hello PowerShell-ablakban:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

Paraméterfájl toospecify inkább a következő parancs hello.

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -TemplateParameterFile <path tooparameters file>\azuredeploy.parameters.json
```

Beágyazott paraméterek hello központi telepítési parancsmag futtatása esetén is használható. hello parancs a következőképpen történik:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -parameterName "parameterValue"
```

toorun egy [teljes](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) a központi telepítés, a set hello **mód** paraméter túl**Complete**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="verify-hello-deployment"></a>Hello telepítés ellenőrzése
Ha hello erőforrások telepítése sikeres volt, hello telepítés összegzését hello PowerShell ablakban jelenik meg:

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Következő lépések
Most láthatta hello alapvető munkafolyamattal és parancsok telepítése Azure Resource Manager-sablonok. További részletes információkért látogasson el a következő hivatkozások hello:

* [Az Azure Resource Manager áttekintése][Azure Resource Manager overview]
* [Erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése][Deploy resources with Azure Resource Manager templates]
* [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
