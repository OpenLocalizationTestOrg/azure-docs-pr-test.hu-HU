---
title: "Hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - Azure CLI 2.0 elemzése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan Azure CLI 2.0 használatával elemezheti a virtuális gépek biztonsági a biztonsági csoport megtekintése."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 1756e14819e3b7c79361c193413a1fcd7f24a4e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-20"></a><span data-ttu-id="2a55d-103">A biztonsági csoport megtekintése az Azure CLI 2.0 verziót használja a virtuális gép biztonsági elemzése</span><span class="sxs-lookup"><span data-stu-id="2a55d-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="2a55d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2a55d-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="2a55d-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2a55d-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="2a55d-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2a55d-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="2a55d-107">REST API</span><span class="sxs-lookup"><span data-stu-id="2a55d-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="2a55d-108">Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok virtuális gép által használt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2a55d-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="2a55d-109">Ez a funkció akkor hasznos, naplózási és diagnosztizálhatja a hálózati biztonsági csoportok és annak érdekében, hogy folyamatban van a forgalom egy virtuális gépen konfigurált szabályok megfelelően engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="2a55d-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="2a55d-110">Ebben a cikkben megmutatjuk, hogyan lehet lekérni a konfigurált és hatékony biztonsági szabályokat egy virtuális gép az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="2a55d-110">In this article, we show you how to retrieve the configured and effective security rules to a virtual machine using Azure CLI</span></span>


<span data-ttu-id="2a55d-111">Ez a cikk használja a következő generációs CLI a erőforrás management üzembe helyezési modellel, Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="2a55d-111">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="2a55d-112">Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="2a55d-112">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2a55d-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2a55d-113">Before you begin</span></span>

<span data-ttu-id="2a55d-114">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="2a55d-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="2a55d-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2a55d-115">Scenario</span></span>

<span data-ttu-id="2a55d-116">A forgatókönyv a cikkben szereplő lekérdezi a konfigurált és hatékony biztonsági szabályok egy adott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="2a55d-116">The scenario covered in this article retrieves the configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="2a55d-117">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="2a55d-117">Get a VM</span></span>

<span data-ttu-id="2a55d-118">A virtuális gépek futtatásához szükséges a `vm list` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2a55d-118">A virtual machine is required to run the `vm list` cmdlet.</span></span> <span data-ttu-id="2a55d-119">A következő parancsot a virtuális gépek erőforráscsoportban sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="2a55d-119">The following command lists the virtual machines in a resource group:</span></span>

```azurecli
az vm list -resource-group resourceGroupName
```

<span data-ttu-id="2a55d-120">Ha tudja, hogy a virtuális gépet, a `vm show` parancsmagot, hogy megkapja az erőforrás-azonosítót:</span><span class="sxs-lookup"><span data-stu-id="2a55d-120">Once you know the virtual machine, you can use the `vm show` cmdlet to get its resource Id:</span></span>

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="2a55d-121">Biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="2a55d-121">Retrieve security group view</span></span>

<span data-ttu-id="2a55d-122">A program a következő lépés a biztonsági csoport megtekintése eredménye.</span><span class="sxs-lookup"><span data-stu-id="2a55d-122">The next step is to retrieve the security group view result.</span></span>

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-the-results"></a><span data-ttu-id="2a55d-123">Az eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="2a55d-123">Viewing the results</span></span>

<span data-ttu-id="2a55d-124">A következő példa egy rövidített választ adott vissza eredményt.</span><span class="sxs-lookup"><span data-stu-id="2a55d-124">The following example is a shortened response of the results returned.</span></span> <span data-ttu-id="2a55d-125">Az eredmények megjelenítése a hatékony és alkalmazott biztonsági szabályokat a virtuális gépen csoportok bontásban **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="2a55d-125">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
      "resourceGroup": "{resourceGroupName}",
      "securityRuleAssociations": {
        "defaultSecurityRules": [
          {
            "access": "Allow",
            "description": "Allow inbound traffic from all VMs in VNET",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "*",
            "direction": "Inbound",
            "etag": null,
            "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups//providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/AllowVnetInBound",
            "name": "AllowVnetInBound",
            "priority": 65000,
            "protocol": "*",
            "provisioningState": "Succeeded",
            "resourceGroup": "",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "*"
          }...
        ],
        "effectiveSecurityRules": [
          {
            "access": "Deny",
            "destinationAddressPrefix": "*",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": null,
            "expandedSourceAddressPrefix": null,
            "name": "DefaultOutboundDenyAll",
            "priority": 65500,
            "protocol": "All",
            "sourceAddressPrefix": "*",
            "sourcePortRange": "0-65535"
          },
          {
            "access": "Allow",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "expandedSourceAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "name": "DefaultRule_AllowVnetOutBound",
            "priority": 65000,
            "protocol": "All",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "0-65535"
          },...
        ],
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "resourceGroup": "{resourceGroupName}",
          "securityRules": [
            {
              "access": "Allow",
              "description": null,
              "destinationAddressPrefix": "*",
              "destinationPortRange": "3389",
              "direction": "Inbound",
              "etag": "W/\"efb606c1-2d54-475a-ab20-da3f80393577\"",
              "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "name": "default-allow-rdp",
              "priority": 1000,
              "protocol": "TCP",
              "provisioningState": "Succeeded",
              "resourceGroup": "{resourceGroupName}",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*"
            }
          ]
        },
        "subnetAssociation": null
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="2a55d-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a55d-126">Next steps</span></span>

<span data-ttu-id="2a55d-127">Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md) megtudhatja, hogyan automatizálhatja a hálózati biztonsági csoportok érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="2a55d-127">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>

<span data-ttu-id="2a55d-128">További információ a biztonsági szabályok látogasson el a hálózati erőforrások alkalmazott [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2a55d-128">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
