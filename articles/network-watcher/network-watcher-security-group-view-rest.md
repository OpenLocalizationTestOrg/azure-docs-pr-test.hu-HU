---
title: "Hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - REST API elemzése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan lehet a virtuális gépek biztonsági biztonsági csoport megtekintése és elemzése a PowerShell használatával."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a2f418fe-f5d2-43ed-8dc3-df0ed2a4d4ac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: afced52b3ae6f3b7f400364f5ec7d049aa166590
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-rest-api"></a><span data-ttu-id="447aa-103">A virtuális gép biztonsági biztonsági csoport láthassák REST API használatával elemzése</span><span class="sxs-lookup"><span data-stu-id="447aa-103">Analyze your Virtual Machine security with Security Group View using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="447aa-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="447aa-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="447aa-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="447aa-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="447aa-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="447aa-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="447aa-107">REST API</span><span class="sxs-lookup"><span data-stu-id="447aa-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="447aa-108">Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok virtuális gép által használt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="447aa-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="447aa-109">Ez a funkció akkor hasznos, naplózási és diagnosztizálhatja a hálózati biztonsági csoportok és annak érdekében, hogy folyamatban van a forgalom egy virtuális gépen konfigurált szabályok megfelelően engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="447aa-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="447aa-110">Ebben a cikkben megmutatjuk, hogyan lehet lekérni a hatékony és alkalmazott szabályok REST API használatával virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="447aa-110">In this article, we show you how to retrieve the effective and applied security rules to a virtual machine using REST API</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="447aa-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="447aa-111">Before you begin</span></span>

<span data-ttu-id="447aa-112">Ebben az esetben hívható meg a hálózati figyelő Rest API-t a biztonsági csoport nézetet beolvasása a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="447aa-112">In this scenario, you call the Network Watcher Rest API to get the security group view for a virtual machine.</span></span> <span data-ttu-id="447aa-113">A PowerShell használatával REST API hívása ARMclient szolgál.</span><span class="sxs-lookup"><span data-stu-id="447aa-113">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="447aa-114">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="447aa-114">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="447aa-115">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="447aa-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="447aa-116">A forgatókönyv feltételezi, hogy létezik-e egy erőforráscsoportot, egy érvényes virtuális géppel használandó.</span><span class="sxs-lookup"><span data-stu-id="447aa-116">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="447aa-117">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="447aa-117">Scenario</span></span>

<span data-ttu-id="447aa-118">A forgatókönyv a cikkben szereplő lekéri a hatékony és alkalmazott szabályok egy adott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="447aa-118">The scenario covered in this article retrieves the effective and applied security rules for a given virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="447aa-119">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="447aa-119">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="447aa-120">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="447aa-120">Retrieve a virtual machine</span></span>

<span data-ttu-id="447aa-121">Futtassa a következő virtuális machineThe vissza a következő kódot változók van szüksége:</span><span class="sxs-lookup"><span data-stu-id="447aa-121">Run the following script to return a virtual machineThe following code needs variables:</span></span>

- <span data-ttu-id="447aa-122">**a subscriptionId** -előfizetés-azonosító is lehet beolvasni a a **Get-AzureRMSubscription** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="447aa-122">**subscriptionId** - The subscription id can also be retrieved with the **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="447aa-123">**resourceGroupName** -virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="447aa-123">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="447aa-124">A szükséges információk a **azonosító** típus szerinti `Microsoft.Compute/virtualMachines` válaszként, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="447aa-124">The information that is needed is the **id** under the type `Microsoft.Compute/virtualMachines` in response, as seen in the following example:</span></span>

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft
.Network/networkInterfaces/{nicName}"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Com
pute/virtualMachines/{vmName}/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute
/virtualMachines/{vmName}",
      "name": "{vmName}"
    }
  ]
}
```

## <a name="get-security-group-view-for-virtual-machine"></a><span data-ttu-id="447aa-125">A virtuális gép biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="447aa-125">Get security group view for virtual machine</span></span>

<span data-ttu-id="447aa-126">Az alábbi példa kéri a biztonsági csoport Nézet megcélzott virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="447aa-126">The following example requests the security group view of a targeted virtual machine.</span></span> <span data-ttu-id="447aa-127">Ebben a példában eredményeinek összehasonlítására, a szabályok és a konfigurációs eltéréseket kereséséhez eredetének által meghatározott biztonsági használható.</span><span class="sxs-lookup"><span data-stu-id="447aa-127">The results from this example can be used to compare to the rules and security defined by the origination to look for configuration drift.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName

$requestBody = @"
{
    'targetResourceId': '${targetUri}'

}
"@
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/securityGroupView?api-version=2016-12-01" $requestBody -verbose
```

## <a name="view-the-response"></a><span data-ttu-id="447aa-128">A válasz megtekintése</span><span class="sxs-lookup"><span data-stu-id="447aa-128">View the response</span></span>

<span data-ttu-id="447aa-129">A következő példa az előző parancs válasza.</span><span class="sxs-lookup"><span data-stu-id="447aa-129">The following sample is the response returned from the preceding command.</span></span> <span data-ttu-id="447aa-130">Az eredmények megjelenítése a hatékony és alkalmazott biztonsági szabályokat a virtuális gépen csoportok bontásban **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="447aa-130">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json

{
  "networkInterfaces": [
    {
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "securityRules": [
            {
              "name": "default-allow-rdp",
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
              "properties": {
                "provisioningState": "Succeeded",
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "3389",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 1000,
                "direction": "Inbound"
              }
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "name": "AllowVnetInBound",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/",
            "properties": {
              "provisioningState": "Succeeded",
              "description": "Allow inbound traffic from all VMs in VNET",
              "protocol": "*",
              "sourcePortRange": "*",
              "destinationPortRange": "*",
              "sourceAddressPrefix": "VirtualNetwork",
              "destinationAddressPrefix": "VirtualNetwork",
              "access": "Allow",
              "priority": 65000,
              "direction": "Inbound"
            }
          },
          ...
        ],
        "effectiveSecurityRules": [
          {
            "name": "DefaultOutboundDenyAll",
            "protocol": "All",
            "sourcePortRange": "0-65535",
            "destinationPortRange": "0-65535",
            "sourceAddressPrefix": "*",
            "destinationAddressPrefix": "*",
            "access": "Deny",
            "priority": 65500,
            "direction": "Outbound"
          },
          ...
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="447aa-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="447aa-131">Next steps</span></span>

<span data-ttu-id="447aa-132">Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-security-group-view-powershell.md) megtudhatja, hogyan automatizálhatja a hálózati biztonsági csoportok érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="447aa-132">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-security-group-view-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>


