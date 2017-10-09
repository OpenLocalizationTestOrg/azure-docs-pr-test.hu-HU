---
title: "aaaAnalyze hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse PowerShell tooanalyze a virtuális gépek biztonsági a biztonsági csoport megtekintése."
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
ms.openlocfilehash: 5e1990d97899bd8585025ec13dd556ab2e034c3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a><span data-ttu-id="2c94d-103">A biztonsági csoport megtekintése a PowerShell használatával a virtuális gép biztonsági elemzése</span><span class="sxs-lookup"><span data-stu-id="2c94d-103">Analyze your Virtual Machine security with Security Group View using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="2c94d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c94d-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="2c94d-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2c94d-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="2c94d-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2c94d-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="2c94d-107">REST API</span><span class="sxs-lookup"><span data-stu-id="2c94d-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="2c94d-108">Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok, amelyek a virtuális gép alkalmazott tooa adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2c94d-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="2c94d-109">Ez a funkció hasznos tooaudit és diagnosztizálhatja a hálózati biztonsági csoportok és a virtuális gépek tooensure forgalma konfigurált szabályok folyamatban van megfelelően engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="2c94d-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="2c94d-110">Ebben a cikkben megmutatjuk, hogyan tooretrieve hello konfigurálva és hatékony biztonsági szabályok tooa virtuális gépet a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2c94d-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using PowerShell</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2c94d-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2c94d-111">Before you begin</span></span>

<span data-ttu-id="2c94d-112">Ebben a forgatókönyvben hello futtatása `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag tooretrieve hello biztonsági szabályra vonatkozó információt.</span><span class="sxs-lookup"><span data-stu-id="2c94d-112">In this scenario, you run hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet tooretrieve hello security rule information.</span></span>

<span data-ttu-id="2c94d-113">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="2c94d-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="2c94d-114">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2c94d-114">Scenario</span></span>

<span data-ttu-id="2c94d-115">a cikkben szereplő hello forgatókönyv lekéri a konfigurált hello és a hatékony biztonsági szabályok egy adott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="2c94d-115">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="2c94d-116">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="2c94d-116">Retrieve Network Watcher</span></span>

<span data-ttu-id="2c94d-117">hello első lépéseként tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="2c94d-117">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="2c94d-118">Ez a változó átadása toohello `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2c94d-118">This variable is passed toohello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a><span data-ttu-id="2c94d-119">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="2c94d-119">Get a VM</span></span>

<span data-ttu-id="2c94d-120">A virtuális gép szükség toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag ellen.</span><span class="sxs-lookup"><span data-stu-id="2c94d-120">A virtual machine is required toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="2c94d-121">a következő példa hello lekérdezi a VM-objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="2c94d-121">hello following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="2c94d-122">Biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="2c94d-122">Retrieve security group view</span></span>

<span data-ttu-id="2c94d-123">hello tovább tooretrieve hello biztonsági csoport megtekintése eredménye.</span><span class="sxs-lookup"><span data-stu-id="2c94d-123">hello next step is tooretrieve hello security group view result.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-hello-results"></a><span data-ttu-id="2c94d-124">Hello eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="2c94d-124">Viewing hello results</span></span>

<span data-ttu-id="2c94d-125">hello következő példa egy rövidített választ hello eredményt adott vissza.</span><span class="sxs-lookup"><span data-stu-id="2c94d-125">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="2c94d-126">hello eredményeket jelenít meg minden hello hatékony és alkalmazott biztonsági szabályokat csoportok bontásban hello virtuális gépen **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="2c94d-126">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2c94d-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c94d-127">Next steps</span></span>

<span data-ttu-id="2c94d-128">Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md) toolearn hogyan hálózati biztonsági csoportok tooautomate érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="2c94d-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>


