---
title: "Riasztás létrehozása, a metrika a Resource Manager sablonnal |} Microsoft Docs"
description: "Megtudhatja, hogyan metrika riasztás értesítéseket e-mailben vagy a webhook létrehozása egy Resource Manager-sablon használatával."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 41d62044-6bc5-4674-b277-45b919f58efe
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/21/2017
ms.author: johnkem
ms.openlocfilehash: ac12605636d21fd0b5c89512c454ef2d899ef6dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-metric-alert-with-a-resource-manager-template"></a><span data-ttu-id="6665b-103">Metrikariasztás létrehozása Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="6665b-103">Create a metric alert with a Resource Manager template</span></span>
<span data-ttu-id="6665b-104">Ez a cikk bemutatja, hogyan használható egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-authoring-templates.md) Azure metrika riasztások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6665b-104">This article shows how you can use an [Azure Resource Manager template](../azure-resource-manager/resource-group-authoring-templates.md) to configure Azure metric alerts.</span></span> <span data-ttu-id="6665b-105">Ez lehetővé teszi, hogy automatikusan riasztásokat állíthat be az erőforrások annak érdekében, hogy helyesen legyen-e figyelve erőforrások létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6665b-105">This enables you to automatically set up alerts on your resources when they are created to ensure that all resources are monitored correctly.</span></span>

<span data-ttu-id="6665b-106">Az alapvető lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="6665b-106">The basic steps are as follows:</span></span>

1. <span data-ttu-id="6665b-107">Hozzon létre egy sablont, amely leírja a riasztás létrehozása, JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="6665b-107">Create a template as a JSON file that describes how to create the alert.</span></span>
2. <span data-ttu-id="6665b-108">[A sablon bármely olyan telepítési módszerrel telepítéséhez](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="6665b-108">[Deploy the template using any deployment method](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

<span data-ttu-id="6665b-109">Az alábbiakban azt ismertetjük Resource Manager-sablonok létrehozásához először riasztás egyedül a riasztást egy másik erőforrás létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="6665b-109">Below we describe how to create a Resource Manager template first for an alert alone, then for an alert during the creation of another resource.</span></span>

## <a name="resource-manager-template-for-a-metric-alert"></a><span data-ttu-id="6665b-110">A metrika riasztások Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="6665b-110">Resource Manager template for a metric alert</span></span>
<span data-ttu-id="6665b-111">Hozzon létre egy riasztást, a Resource Manager-sablonnal, hozzon létre egy típusú erőforrást `Microsoft.Insights/alertRules` , és töltse ki az összes kapcsolódó tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="6665b-111">To create an alert using a Resource Manager template, you create a resource of type `Microsoft.Insights/alertRules` and fill in all related properties.</span></span> <span data-ttu-id="6665b-112">Az alábbiakban van a sablont, amely egy riasztási szabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6665b-112">Below is a template that creates an alert rule.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "alertName": {
            "type": "string",
            "metadata": {
                "description": "Name of alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether alerts are enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Resource ID of the resource emitting the metric that will be used for the comparison."
            }
        },
        "metricName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "aggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Last",
                "Maximum",
                "Minimum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "sendToServiceOwners": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether alerts are sent to service owners"
            }
        },
        "customEmailAddresses": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Comma-delimited email addresses where the alerts are also sent"
            }
        },
        "webhookUrl": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "URL of a webhook that will receive an HTTP POST when the alert activates."
            }
        }
    },
    "variables": {
        "customEmails": "[split(parameters('customEmailAddresses'), ',')]"
    },
    "resources": [
        {
            "type": "Microsoft.Insights/alertRules",
            "name": "[parameters('alertName')]",
            "location": "[resourceGroup().location]",
            "apiVersion": "2016-03-01",
            "properties": {
                "name": "[parameters('alertName')]",
                "description": "[parameters('alertDescription')]",
                "isEnabled": "[parameters('isEnabled')]",
                "condition": {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource": {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri": "[parameters('resourceId')]",
                        "metricName": "[parameters('metricName')]"
                    },
                    "operator": "[parameters('operator')]",
                    "threshold": "[parameters('threshold')]",
                    "windowSize": "[parameters('windowSize')]",
                    "timeAggregation": "[parameters('aggregation')]"
                },
                "actions": [
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "sendToServiceOwners": "[parameters('sendToServiceOwners')]",
                        "customEmails": "[variables('customEmails')]"
                    },
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleWebhookAction",
                        "serviceUri": "[parameters('webhookUrl')]",
                        "properties": {}
                    }
                ]
            }
        }
    ]
}
```

<span data-ttu-id="6665b-113">Riasztási szabály a séma és a Tulajdonságok magyarázatot [érhető el itt](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span><span class="sxs-lookup"><span data-stu-id="6665b-113">An explanation of the schema and properties for an alert rule [is available here](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span></span>

## <a name="resource-manager-template-for-a-resource-with-an-alert"></a><span data-ttu-id="6665b-114">Riasztás erőforrás Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="6665b-114">Resource Manager template for a resource with an alert</span></span>
<span data-ttu-id="6665b-115">A Resource Manager-sablon riasztást általában akkor hasznos, ha létre riasztást erőforrás létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="6665b-115">An alert on a Resource Manager template is most often useful when creating an alert while creating a resource.</span></span> <span data-ttu-id="6665b-116">Például, érdemes lehet ahhoz, hogy a "CPU % > 80-as" szabály be van állítva, minden alkalommal, amikor a virtuális gép telepítése.</span><span class="sxs-lookup"><span data-stu-id="6665b-116">For example, you may want to ensure that a “CPU % > 80” rule is set up every time you deploy a Virtual Machine.</span></span> <span data-ttu-id="6665b-117">Ehhez adja hozzá a riasztási szabály a erőforrás tömbben erőforrásként a Virtuálisgép-sablonhoz, és adja hozzá a függőség használatával a `dependsOn` tulajdonságot a VM erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="6665b-117">To do this, you add the alert rule as a resource in the resource array for your VM template and add a dependency using the `dependsOn` property to the VM resource ID.</span></span> <span data-ttu-id="6665b-118">Ez egy teljes példa, amely egy Windows virtuális Gépet hoz létre, és hozzáadja egy riasztást, mely előfizetés rendszergazdái értesítése, ha a CPU-kihasználtsága a 80 % fölé megy.</span><span class="sxs-lookup"><span data-stu-id="6665b-118">Here’s a full example that creates a Windows VM and adds an alert that notifies subscription admins when the CPU utilization goes above 80%.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "newStorageAccountName": {
            "type": "string",
            "metadata": {
                "Description": "The name of the storage account where the VM disk is stored."
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "Description": "The name of the administrator account on the VM."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "Description": "The administrator account password on the VM."
            }
        },
        "dnsNameForPublicIP": {
            "type": "string",
            "metadata": {
                "Description": "The name of the public IP address used to access the VM."
            }
        }
    },
    "variables": {
        "location": "Central US",
        "imagePublisher": "MicrosoftWindowsServer",
        "imageOffer": "WindowsServer",
        "windowsOSVersion": "2012-R2-Datacenter",
        "OSDiskName": "osdisk1",
        "nicName": "nc1",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "sn1",
        "subnetPrefix": "10.0.0.0/24",
        "storageAccountType": "Standard_LRS",
        "publicIPAddressName": "ip1",
        "publicIPAddressType": "Dynamic",
        "vmStorageAccountContainerName": "vhds",
        "vmName": "vm1",
        "vmSize": "Standard_A0",
        "virtualNetworkName": "vn1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "vmID":"[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]",
        "alertName": "highCPUOnVM",
        "alertDescription":"CPU is over 80%",
        "alertIsEnabled": true,
        "resourceId": "",
        "metricName": "Percentage CPU",
        "operator": "GreaterThan",
        "threshold": "80",
        "windowSize": "PT5M",
        "aggregation": "Average",
        "customEmails": "",
        "sendToServiceOwners": true,
        "webhookUrl": "http://testwebhook.test"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('newStorageAccountName')]",
            "apiVersion": "2015-06-15",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[variables('publicIPAddressName')]",
            "location": "[variables('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                }
            }
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[variables('virtualNetworkName')]",
            "location": "[variables('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[variables('nicName')]",
            "location": "[variables('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
                            },
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[variables('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[variables('vmSize')]"
                },
                "osProfile": {
                    "computername": "[variables('vmName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "[variables('imagePublisher')]",
                        "offer": "[variables('imageOffer')]",
                        "sku": "[variables('windowsOSVersion')]",
                        "version": "latest"
                    },
                    "osDisk": {
                        "name": "osdisk",
                        "vhd": {
                            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
                        },
                        "caching": "ReadWrite",
                        "createOption": "FromImage"
                    }
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                        }
                    ]
                }
            }
        },
        {
            "type": "Microsoft.Insights/alertRules",
            "name": "[variables('alertName')]",
            "dependsOn": [
                "[variables('vmID')]"
            ],
            "location": "[variables('location')]",
            "apiVersion": "2016-03-01",
            "properties": {
                "name": "[variables('alertName')]",
                "description": "variables('alertDescription')",
                "isEnabled": "[variables('alertIsEnabled')]",
                "condition": {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource": {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri": "[variables('vmID')]",
                        "metricName": "[variables('metricName')]"
                    },
                    "operator": "[variables('operator')]",
                    "threshold": "[variables('threshold')]",
                    "windowSize": "[variables('windowSize')]",
                    "timeAggregation": "[variables('aggregation')]"
                },
                "actions": [
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "sendToServiceOwners": "[variables('sendToServiceOwners')]",
                        "customEmails": "[variables('customEmails')]"
                    },
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleWebhookAction",
                        "serviceUri": "[variables('webhookUrl')]",
                        "properties": {}
                    }
                ]
            }
        }
    ]
}
```

## <a name="next-steps"></a><span data-ttu-id="6665b-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6665b-119">Next Steps</span></span>
* [<span data-ttu-id="6665b-120">Tudjon meg többet a riasztások</span><span class="sxs-lookup"><span data-stu-id="6665b-120">Read more about Alerts</span></span>](insights-receive-alert-notifications.md)
* <span data-ttu-id="6665b-121">[Diagnosztikai beállítások hozzáadása](monitoring-enable-diagnostic-logs-using-template.md) a Resource Manager sablonhoz</span><span class="sxs-lookup"><span data-stu-id="6665b-121">[Add Diagnostic Settings](monitoring-enable-diagnostic-logs-using-template.md) to your Resource Manager template</span></span>

