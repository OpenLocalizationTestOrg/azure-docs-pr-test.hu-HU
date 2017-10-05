---
title: "Hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - Azure CLI 1.0 elemzése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan Azure CLI 1.0 használatával elemezheti a virtuális gépek biztonsági a biztonsági csoport megtekintése."
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
ms.openlocfilehash: 2c4c494dcc4fe1a85c5feb29506c35fb03066479
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a><span data-ttu-id="03b47-103">A virtuális gép biztonsági a biztonsági csoport nézet segítségével az Azure CLI 1.0 elemzése</span><span class="sxs-lookup"><span data-stu-id="03b47-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="03b47-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="03b47-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="03b47-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="03b47-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="03b47-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="03b47-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="03b47-107">REST API</span><span class="sxs-lookup"><span data-stu-id="03b47-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="03b47-108">Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok virtuális gép által használt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="03b47-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="03b47-109">Ez a funkció akkor hasznos, naplózási és diagnosztizálhatja a hálózati biztonsági csoportok és annak érdekében, hogy folyamatban van a forgalom egy virtuális gépen konfigurált szabályok megfelelően engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="03b47-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="03b47-110">Ebben a cikkben megmutatjuk, hogyan lehet lekérni a konfigurált és hatékony biztonsági szabályokat egy virtuális gép az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="03b47-110">In this article, we show you how to retrieve the configured and effective security rules to a virtual machine using Azure CLI</span></span>

<span data-ttu-id="03b47-111">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="03b47-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="03b47-112">Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.</span><span class="sxs-lookup"><span data-stu-id="03b47-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="03b47-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="03b47-113">Before you begin</span></span>

<span data-ttu-id="03b47-114">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="03b47-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="03b47-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="03b47-115">Scenario</span></span>

<span data-ttu-id="03b47-116">A forgatókönyv a cikkben szereplő lekérdezi a konfigurált és hatékony biztonsági szabályok egy adott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="03b47-116">The scenario covered in this article retrieves the configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="03b47-117">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="03b47-117">Get a VM</span></span>

<span data-ttu-id="03b47-118">A virtuális gépek futtatásához szükséges a `vm list` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="03b47-118">A virtual machine is required to run the `vm list` cmdlet.</span></span> <span data-ttu-id="03b47-119">A következő parancsot egy erőforráscsoportban található virtuális machinese sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="03b47-119">The following command lists the virtual machinese in a resource group:</span></span>

```azurecli
azure vm list -g resourceGroupName
```

<span data-ttu-id="03b47-120">Ha tudja, hogy a virtuális gépet, a `vm show` parancsmagot, hogy megkapja az erőforrás-azonosítót:</span><span class="sxs-lookup"><span data-stu-id="03b47-120">Once you know the virtual machine, you can use the `vm show` cmdlet to get its resource Id:</span></span>

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="03b47-121">Biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="03b47-121">Retrieve security group view</span></span>

<span data-ttu-id="03b47-122">A program a következő lépés a biztonsági csoport megtekintése eredménye.</span><span class="sxs-lookup"><span data-stu-id="03b47-122">The next step is to retrieve the security group view result.</span></span> <span data-ttu-id="03b47-123">Hozzáadása a "--json" jelző formázza az eredmények json-ban.</span><span class="sxs-lookup"><span data-stu-id="03b47-123">Adding the "--json" flag will format the results in json.</span></span>

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-the-results"></a><span data-ttu-id="03b47-124">Az eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="03b47-124">Viewing the results</span></span>

<span data-ttu-id="03b47-125">A következő példa egy rövidített választ adott vissza eredményt.</span><span class="sxs-lookup"><span data-stu-id="03b47-125">The following example is a shortened response of the results returned.</span></span> <span data-ttu-id="03b47-126">Az eredmények megjelenítése a hatékony és alkalmazott biztonsági szabályokat a virtuális gépen csoportok bontásban **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="03b47-126">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="03b47-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03b47-127">Next steps</span></span>

<span data-ttu-id="03b47-128">Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md) megtudhatja, hogyan automatizálhatja a hálózati biztonsági csoportok érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="03b47-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>

<span data-ttu-id="03b47-129">További információ a biztonsági szabályok látogasson el a hálózati erőforrások alkalmazott [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="03b47-129">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
