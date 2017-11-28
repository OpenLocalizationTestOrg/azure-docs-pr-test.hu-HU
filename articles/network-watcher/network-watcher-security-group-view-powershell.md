---
title: "Hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - PowerShell elemzése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan lehet a virtuális gépek biztonsági biztonsági csoport megtekintése és elemzése a PowerShell használatával."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 04e76b49-6a1b-4d0f-9a9b-51cf2f4df5a2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 363fdd9f1de933bb4050f91e1e111aaf3e419058
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a><span data-ttu-id="a0946-103">A biztonsági csoport megtekintése a PowerShell használatával a virtuális gép biztonsági elemzése</span><span class="sxs-lookup"><span data-stu-id="a0946-103">Analyze your Virtual Machine security with Security Group View using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a0946-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0946-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="a0946-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a0946-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="a0946-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a0946-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="a0946-107">REST API</span><span class="sxs-lookup"><span data-stu-id="a0946-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="a0946-108">Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok virtuális gép által használt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a0946-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="a0946-109">Ez a funkció akkor hasznos, naplózási és diagnosztizálhatja a hálózati biztonsági csoportok és annak érdekében, hogy folyamatban van a forgalom egy virtuális gépen konfigurált szabályok megfelelően engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="a0946-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="a0946-110">Ebben a cikkben megmutatjuk, hogyan lehet lekérni a konfigurált és hatékony biztonsági szabályokat egy virtuális gép PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a0946-110">In this article, we show you how to retrieve the configured and effective security rules to a virtual machine using PowerShell</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a0946-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a0946-111">Before you begin</span></span>

<span data-ttu-id="a0946-112">Ebben a forgatókönyvben futtatja a `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag a biztonsági szabály információinak lekérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="a0946-112">In this scenario, you run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet to retrieve the security rule information.</span></span>

<span data-ttu-id="a0946-113">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="a0946-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="a0946-114">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="a0946-114">Scenario</span></span>

<span data-ttu-id="a0946-115">A forgatókönyv a cikkben szereplő lekérdezi a konfigurált és hatékony biztonsági szabályok egy adott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="a0946-115">The scenario covered in this article retrieves the configured and effective security rules for a given virtual machine.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="a0946-116">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="a0946-116">Retrieve Network Watcher</span></span>

<span data-ttu-id="a0946-117">Az első lépés a hálózati figyelőt példányának lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="a0946-117">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="a0946-118">Ez a változó átadott a `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a0946-118">This variable is passed to the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a><span data-ttu-id="a0946-119">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="a0946-119">Get a VM</span></span>

<span data-ttu-id="a0946-120">A virtuális gépek futtatásához szükséges a `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag ellen.</span><span class="sxs-lookup"><span data-stu-id="a0946-120">A virtual machine is required to run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="a0946-121">A következő példa egy Virtuálisgép-objektum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a0946-121">The following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="a0946-122">Biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="a0946-122">Retrieve security group view</span></span>

<span data-ttu-id="a0946-123">A program a következő lépés a biztonsági csoport megtekintése eredménye.</span><span class="sxs-lookup"><span data-stu-id="a0946-123">The next step is to retrieve the security group view result.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-the-results"></a><span data-ttu-id="a0946-124">Az eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="a0946-124">Viewing the results</span></span>

<span data-ttu-id="a0946-125">A következő példa egy rövidített választ adott vissza eredményt.</span><span class="sxs-lookup"><span data-stu-id="a0946-125">The following example is a shortened response of the results returned.</span></span> <span data-ttu-id="a0946-126">Az eredmények megjelenítése a hatékony és alkalmazott biztonsági szabályokat a virtuális gépen csoportok bontásban **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="a0946-126">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```
NetworkInterfaces : [
                      {
                        "NetworkInterfaceSecurityRules": [
                          {
                            "Name": "default-allow-rdp",
                            "Etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
                            "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/securityRules/default-allow-rdp",
                            "Protocol": "TCP",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "3389",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "Access": "Allow",
                            "Priority": 1000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "DefaultSecurityRules": [
                          {
                            "Name": "AllowVnetInBound",
                            "Id": "/subscriptions00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/defaultSecurityRules/",
                            "Description": "Allow inbound traffic from all VMs in VNET",
                            "Protocol": "*",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "*",
                            "SourceAddressPrefix": "VirtualNetwork",
                            "DestinationAddressPrefix": "VirtualNetwork",
                            "Access": "Allow",
                            "Priority": 65000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "EffectiveSecurityRules": [
                          {
                            "Name": "DefaultOutboundDenyAll",
                            "Protocol": "All",
                            "SourcePortRange": "0-65535",
                            "DestinationPortRange": "0-65535",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "ExpandedSourceAddressPrefix": [],
                            "ExpandedDestinationAddressPrefix": [],
                            "Access": "Deny",
                            "Priority": 65500,
                            "Direction": "Outbound"
                          },
                          ...
                        ]
                      }
                    ]
```

## <a name="next-steps"></a><span data-ttu-id="a0946-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0946-127">Next steps</span></span>

<span data-ttu-id="a0946-128">Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md) megtudhatja, hogyan automatizálhatja a hálózati biztonsági csoportok érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="a0946-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>


