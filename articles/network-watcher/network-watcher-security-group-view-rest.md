---
title: "aaaAnalyze hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - REST API |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse PowerShell tooanalyze a virtuális gépek biztonsági a biztonsági csoport megtekintése."
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
ms.openlocfilehash: 0858a64a9454816e05f06dadb9536ad0c755e90e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-rest-api"></a><span data-ttu-id="51c45-103">A virtuális gép biztonsági biztonsági csoport láthassák REST API használatával elemzése</span><span class="sxs-lookup"><span data-stu-id="51c45-103">Analyze your Virtual Machine security with Security Group View using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="51c45-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="51c45-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="51c45-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="51c45-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="51c45-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="51c45-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="51c45-107">REST API</span><span class="sxs-lookup"><span data-stu-id="51c45-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="51c45-108">Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok, amelyek a virtuális gép alkalmazott tooa adja vissza.</span><span class="sxs-lookup"><span data-stu-id="51c45-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="51c45-109">Ez a funkció hasznos tooaudit és diagnosztizálhatja a hálózati biztonsági csoportok és a virtuális gépek tooensure forgalma konfigurált szabályok folyamatban van megfelelően engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="51c45-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="51c45-110">Ebben a cikkben megmutatjuk, hogyan tooretrieve hello hatékony és alkalmazott biztonsági szabályok tooa virtuális gépet a REST API használatával</span><span class="sxs-lookup"><span data-stu-id="51c45-110">In this article, we show you how tooretrieve hello effective and applied security rules tooa virtual machine using REST API</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="51c45-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="51c45-111">Before you begin</span></span>

<span data-ttu-id="51c45-112">Ebben a forgatókönyvben a virtuális gép meghívja a hello hálózati figyelő Rest API tooget hello biztonsági csoport megtekintése.</span><span class="sxs-lookup"><span data-stu-id="51c45-112">In this scenario, you call hello Network Watcher Rest API tooget hello security group view for a virtual machine.</span></span> <span data-ttu-id="51c45-113">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="51c45-113">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="51c45-114">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="51c45-114">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="51c45-115">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="51c45-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="51c45-116">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="51c45-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="51c45-117">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="51c45-117">Scenario</span></span>

<span data-ttu-id="51c45-118">a cikkben szereplő hello forgatókönyv hello hatékony és alkalmazott biztonsági szabályok egy adott virtuális gép kéri le.</span><span class="sxs-lookup"><span data-stu-id="51c45-118">hello scenario covered in this article retrieves hello effective and applied security rules for a given virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="51c45-119">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="51c45-119">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="51c45-120">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="51c45-120">Retrieve a virtual machine</span></span>

<span data-ttu-id="51c45-121">Futtassa a következő parancsfájl tooreturn virtuális machineThe hello változók van szüksége a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="51c45-121">Run hello following script tooreturn a virtual machineThe following code needs variables:</span></span>

- <span data-ttu-id="51c45-122">**a subscriptionId** -hello előfizetés-azonosító is lehet beolvasni a hello **Get-AzureRMSubscription** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="51c45-122">**subscriptionId** - hello subscription id can also be retrieved with hello **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="51c45-123">**resourceGroupName** – hello virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="51c45-123">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="51c45-124">hello szükséges adatokat az hello **azonosító** a hello típusa `Microsoft.Compute/virtualMachines` válaszként, hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="51c45-124">hello information that is needed is hello **id** under hello type `Microsoft.Compute/virtualMachines` in response, as seen in hello following example:</span></span>

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

## <a name="get-security-group-view-for-virtual-machine"></a><span data-ttu-id="51c45-125">A virtuális gép biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="51c45-125">Get security group view for virtual machine</span></span>

<span data-ttu-id="51c45-126">a következő példa hello kérelmek hello biztonsági csoport Nézet megcélzott virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="51c45-126">hello following example requests hello security group view of a targeted virtual machine.</span></span> <span data-ttu-id="51c45-127">Ebben a példában hello eredményeinek használt toocompare toohello szabályok és a konfigurációs eltéréseket hello eredetének toolook által meghatározott biztonsági lehet.</span><span class="sxs-lookup"><span data-stu-id="51c45-127">hello results from this example can be used toocompare toohello rules and security defined by hello origination toolook for configuration drift.</span></span>

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

## <a name="view-hello-response"></a><span data-ttu-id="51c45-128">Hello válasz megtekintése</span><span class="sxs-lookup"><span data-stu-id="51c45-128">View hello response</span></span>

<span data-ttu-id="51c45-129">a következő minta hello parancs megelőző hello hello válasza.</span><span class="sxs-lookup"><span data-stu-id="51c45-129">hello following sample is hello response returned from hello preceding command.</span></span> <span data-ttu-id="51c45-130">hello eredményeket jelenít meg minden hello hatékony és alkalmazott biztonsági szabályokat csoportok bontásban hello virtuális gépen **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="51c45-130">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="51c45-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51c45-131">Next steps</span></span>

<span data-ttu-id="51c45-132">Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-security-group-view-powershell.md) toolearn hogyan hálózati biztonsági csoportok tooautomate érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="51c45-132">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-security-group-view-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>


