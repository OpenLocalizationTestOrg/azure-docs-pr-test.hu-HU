---
title: "aaaAutomate Azure hálózati figyelő biztonsági csoport megtekintése naplózás NSG |} Microsoft Docs"
description: "Ezen a lapon útmutatás tooconfigure naplózását a hálózati biztonsági csoport"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 24fc418c433fceaf55a74b7c3b0e354dc46c8729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a><span data-ttu-id="06eb4-103">Automatizálható a NSG naplózás Azure hálózati figyelő biztonsági csoport megtekintése</span><span class="sxs-lookup"><span data-stu-id="06eb4-103">Automate NSG auditing with Azure Network Watcher Security group view</span></span>

<span data-ttu-id="06eb4-104">Az ügyfelek gyakran kihívással hello kérdéssel hello biztonsági állapotát az infrastruktúra ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="06eb4-104">Customers are often faced with hello challenge of verifying hello security posture of their infrastructure.</span></span> <span data-ttu-id="06eb4-105">Ez a kérdés ugyanolyan helyzetet teremt, a virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="06eb4-105">This challenge is no different for their VMs in Azure.</span></span> <span data-ttu-id="06eb4-106">Fontos fontos toohave hasonló biztonsági profil alapján hello hálózati biztonsági csoport (NSG) szabálya.</span><span class="sxs-lookup"><span data-stu-id="06eb4-106">It is important toohave a similar security profile based on hello Network Security Group (NSG) rules applied.</span></span> <span data-ttu-id="06eb4-107">Biztonsági csoport megtekintése hello használ, most olvashatók és szabálya tooa belül egy NSG VM hello listája.</span><span class="sxs-lookup"><span data-stu-id="06eb4-107">Using hello Security Group View, you can now get hello list of rules applied tooa VM within an NSG.</span></span> <span data-ttu-id="06eb4-108">Egy arany NSG biztonsági profil meghatározásához és biztonsági csoport megtekintése heti ütemben történik kezdeményezzen és hasonlítsa össze a hello kimeneti arany toohello profil és -jelentés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="06eb4-108">You can define a golden NSG security profile and initiate Security Group View on a weekly cadence and compare hello output toohello golden profile and create a report.</span></span> <span data-ttu-id="06eb4-109">Így azonosíthatja a könnyű összes hello virtuális gépek, amelyek nem felelnek meg a biztonsági profil előírt toohello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-109">This way you can identify with ease all hello VMs that do not conform toohello prescribed security profile.</span></span>

<span data-ttu-id="06eb4-110">Ha nem ismeri a hálózati biztonsági csoportokkal, látogasson el a [hálózati biztonsági – áttekintés](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="06eb4-110">If you are unfamiliar with Network Security Groups, visit [Network Security Overview](../virtual-network/virtual-networks-nsg.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="06eb4-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="06eb4-111">Before you begin</span></span>

<span data-ttu-id="06eb4-112">Ebben a forgatókönyvben egy ismert helyes alapterv toohello biztonsági csoport összehasonlítja a virtuális gép eredményének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="06eb4-112">In this scenario, you compare a known good baseline toohello security group view results returned for a virtual machine.</span></span>

<span data-ttu-id="06eb4-113">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="06eb4-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="06eb4-114">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="06eb4-114">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="06eb4-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="06eb4-115">Scenario</span></span>

<span data-ttu-id="06eb4-116">a cikkben szereplő hello forgatókönyv hello biztonsági csoport megtekintése a virtuális gép lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="06eb4-116">hello scenario covered in this article gets hello security group view for a virtual machine.</span></span>

<span data-ttu-id="06eb4-117">Ebben a forgatókönyvben a tartalma:</span><span class="sxs-lookup"><span data-stu-id="06eb4-117">In this scenario, you will:</span></span>

- <span data-ttu-id="06eb4-118">Egy ismert helyes szabálykészletben beolvasása</span><span class="sxs-lookup"><span data-stu-id="06eb4-118">Retrieve a known good rule set</span></span>
- <span data-ttu-id="06eb4-119">Rest API-hoz egy virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="06eb4-119">Retrieve a virtual machine with Rest API</span></span>
- <span data-ttu-id="06eb4-120">A virtuális gép biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="06eb4-120">Get security group view for virtual machine</span></span>
- <span data-ttu-id="06eb4-121">Értékelje ki a válasz</span><span class="sxs-lookup"><span data-stu-id="06eb4-121">Evaluate Response</span></span>

## <a name="retrieve-rule-set"></a><span data-ttu-id="06eb4-122">Szabály beolvasása</span><span class="sxs-lookup"><span data-stu-id="06eb4-122">Retrieve rule set</span></span>

<span data-ttu-id="06eb4-123">Ez a példa első lépéseként hello együtt egy meglévő toowork.</span><span class="sxs-lookup"><span data-stu-id="06eb4-123">hello first step in this example is toowork with an existing baseline.</span></span> <span data-ttu-id="06eb4-124">hello alábbi példa néhány hello használata meglévő hálózati biztonsági csoport kinyert json `Get-AzureRmNetworkSecurityGroup` parancsmag, amely ehhez a példához hello alapjául szolgál.</span><span class="sxs-lookup"><span data-stu-id="06eb4-124">hello following example is some json extracted from an existing Network Security Group using hello `Get-AzureRmNetworkSecurityGroup` cmdlet that is used as hello baseline for this example.</span></span>

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-toopowershell-objects"></a><span data-ttu-id="06eb4-125">Alakítsa át a szabály beállított tooPowerShell objektumok</span><span class="sxs-lookup"><span data-stu-id="06eb4-125">Convert rule set tooPowerShell objects</span></span>

<span data-ttu-id="06eb4-126">Ebben a lépésben egy json-fájl, amely a hálózati biztonsági csoport hello ehhez a példához várt toobe hello szabályokat a korábban létrehozott olvassák azt.</span><span class="sxs-lookup"><span data-stu-id="06eb4-126">In this step, we are reading a json file that was created earlier with hello rules that are expected toobe on hello Network Security Group for this example.</span></span>

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a><span data-ttu-id="06eb4-127">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="06eb4-127">Retrieve Network Watcher</span></span>

<span data-ttu-id="06eb4-128">hello tovább tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="06eb4-128">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="06eb4-129">Hello `$networkWatcher` változó átadása toohello `AzureRmNetworkWatcherSecurityGroupView` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="06eb4-129">hello `$networkWatcher` variable is passed toohello `AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="06eb4-130">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="06eb4-130">Get a VM</span></span>

<span data-ttu-id="06eb4-131">A virtuális gép szükség toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag ellen.</span><span class="sxs-lookup"><span data-stu-id="06eb4-131">A virtual machine is required toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="06eb4-132">a következő példa hello lekérdezi a VM-objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="06eb4-132">hello following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="06eb4-133">Biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="06eb4-133">Retrieve security group view</span></span>

<span data-ttu-id="06eb4-134">hello tovább tooretrieve hello biztonsági csoport megtekintése eredménye.</span><span class="sxs-lookup"><span data-stu-id="06eb4-134">hello next step is tooretrieve hello security group view result.</span></span> <span data-ttu-id="06eb4-135">Az eredménye, hogy a korábban bemutatott összehasonlított toohello "eredeti" json.</span><span class="sxs-lookup"><span data-stu-id="06eb4-135">This result is compared toohello "baseline" json that was shown earlier.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-hello-results"></a><span data-ttu-id="06eb4-136">Hello eredmények elemzése</span><span class="sxs-lookup"><span data-stu-id="06eb4-136">Analyzing hello results</span></span>

<span data-ttu-id="06eb4-137">hello válasz hálózati illesztők szerint vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="06eb4-137">hello response is grouped by Network interfaces.</span></span> <span data-ttu-id="06eb4-138">különböző típusú hello visszaadott szabályok érvényben, és az alapértelmezett biztonsági szabályokat.</span><span class="sxs-lookup"><span data-stu-id="06eb4-138">hello different types of rules returned are effective and default security rules.</span></span> <span data-ttu-id="06eb4-139">hello eredmény további oszlanak meg az alkalmazásának módját, egy alhálózatot vagy egy virtuális hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="06eb4-139">hello result is further broken down by how it is applied, either on a subnet or a virtual NIC.</span></span>

<span data-ttu-id="06eb4-140">hello következő PowerShell-parancsfájl összehasonlítja hello eredményeit hello biztonsági csoport megtekintése tooan meglévő kimenete egy NSG.</span><span class="sxs-lookup"><span data-stu-id="06eb4-140">hello following PowerShell script compares hello results of hello Security Group View tooan existing output of an NSG.</span></span> <span data-ttu-id="06eb4-141">hello alábbi példa: hogyan hello eredmények is össze kell hasonlítani egy egyszerű példa `Compare-Object` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="06eb4-141">hello following example is a simple example of how hello results can be compared with `Compare-Object` cmdlet.</span></span>

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

<span data-ttu-id="06eb4-142">a következő példa hello hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="06eb4-142">hello following example is hello result.</span></span> <span data-ttu-id="06eb4-143">Láthatja, hogy két szereplő először szabálykészleten hello hello szabályok nem volt jelen hello szemben.</span><span class="sxs-lookup"><span data-stu-id="06eb4-143">You can see two of hello rules that were in hello first rule set were not present in hello comparison.</span></span>

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a><span data-ttu-id="06eb4-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06eb4-144">Next steps</span></span>

<span data-ttu-id="06eb4-145">Ha a beállítások módosítása, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek a szóban forgó tootrack.</span><span class="sxs-lookup"><span data-stu-id="06eb4-145">If settings have been changed, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are in question.</span></span>













