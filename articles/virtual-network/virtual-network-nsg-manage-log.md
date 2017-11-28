---
title: "aaaMonitor műveleteket, eseményeket és NSG-ket számlálói |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable számlálók, eseményeket és NSG-ket műveleti naplózás"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: f16f1a0ad693028ee7aba21574b5c8ddfcd27096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a><span data-ttu-id="b4a58-103">Naplóelemzés hálózati biztonsági csoportokhoz</span><span class="sxs-lookup"><span data-stu-id="b4a58-103">Log analytics for network security groups (NSGs)</span></span>

<span data-ttu-id="b4a58-104">Az NSG-ket a diagnosztikai naplófájl kategóriák következő hello engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="b4a58-104">You can enable hello following diagnostic log categories for NSGs:</span></span>

* <span data-ttu-id="b4a58-105">**Esemény:** mely NSG szabályok lettek alkalmazott tooVMs és a MAC-címe alapján példány szerepkörök bejegyzést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4a58-105">**Event:** Contains entries for which NSG rules are applied tooVMs and instance roles based on MAC address.</span></span> <span data-ttu-id="b4a58-106">Ezek a szabályok hello állapota 60 másodpercenként gyűjti.</span><span class="sxs-lookup"><span data-stu-id="b4a58-106">hello status for these rules is collected every 60 seconds.</span></span>
* <span data-ttu-id="b4a58-107">**A szabály számláló:** Contains bejegyzéseket hányszor minden NSG-szabályok alkalmazott toodeny vagy -kezelési forgalom engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b4a58-107">**Rule counter:** Contains entries for how many times each NSG rule is applied toodeny or allow traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="b4a58-108">Diagnosztikai naplók esetén csak az NSG-k hello Azure Resource Manager üzembe helyezési modellben telepített érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b4a58-108">Diagnostic logs are only available for NSGs deployed through hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="b4a58-109">Diagnosztikai naplózás hello klasszikus üzembe helyezési modellben telepített NSG-k nem engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="b4a58-109">You cannot enable diagnostic logging for NSGs deployed through hello classic deployment model.</span></span> <span data-ttu-id="b4a58-110">Kra hello két modell, hivatkozhat hello [ismertetése Azure üzembe helyezési modellel](../resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4a58-110">For a better understanding of hello two models, reference hello [Understanding Azure deployment models](../resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="b4a58-111">(Korábbi nevén naplózási és a műveleti naplókat) naplózása alapértelmezés szerint engedélyezve van a az NSG-k vagy az Azure-telepítés modell használatával létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b4a58-111">Activity logging (previously known as audit or operational logs) is enabled by default for NSGs created through either Azure deployment model.</span></span> <span data-ttu-id="b4a58-112">milyen műveletek befejeződtek az NSG-ket hello műveletnaplóban, keresse meg a következő típusú erőforrások hello tartalmazó toodetermine:</span><span class="sxs-lookup"><span data-stu-id="b4a58-112">toodetermine which operations were completed on NSGs in hello activity log, look for entries that contain hello following resource types:</span></span> 

- <span data-ttu-id="b4a58-113">Microsoft.ClassicNetwork/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="b4a58-113">Microsoft.ClassicNetwork/networkSecurityGroups</span></span> 
- <span data-ttu-id="b4a58-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="b4a58-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span></span>
- <span data-ttu-id="b4a58-115">Microsoft.Network/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="b4a58-115">Microsoft.Network/networkSecurityGroups</span></span>
- <span data-ttu-id="b4a58-116">Microsoft.Network/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="b4a58-116">Microsoft.Network/networkSecurityGroups/securityRules</span></span> 

<span data-ttu-id="b4a58-117">Olvasási hello [hello Azure tevékenységnapló áttekintése](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) cikk toolearn tevékenységi naplóit többet.</span><span class="sxs-lookup"><span data-stu-id="b4a58-117">Read hello [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) article toolearn more about activity logs.</span></span> 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="b4a58-118">Diagnosztikai naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b4a58-118">Enable diagnostic logging</span></span>

<span data-ttu-id="b4a58-119">Diagnosztikai naplózás engedélyezni kell a *minden* NSG kívánt toocollect adatait.</span><span class="sxs-lookup"><span data-stu-id="b4a58-119">Diagnostic logging must be enabled for *each* NSG you want toocollect data for.</span></span> <span data-ttu-id="b4a58-120">Hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) a cikk azt ismerteti, ahol a diagnosztikai naplók küldhető.</span><span class="sxs-lookup"><span data-stu-id="b4a58-120">hello [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article explains where diagnostic logs can be sent.</span></span> <span data-ttu-id="b4a58-121">Ha nem rendelkezik egy meglévő NSG, teljes hello hello szükséges lépések [hálózati biztonsági csoport létrehozása](virtual-networks-create-nsg-arm-pportal.md) egy cikk toocreate.</span><span class="sxs-lookup"><span data-stu-id="b4a58-121">If you don't have an existing NSG, complete hello steps in hello [Create a network security group](virtual-networks-create-nsg-arm-pportal.md) article toocreate one.</span></span> <span data-ttu-id="b4a58-122">Diagnosztikai naplózás hello a következő módszerek bármelyikével NSG engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="b4a58-122">You can enable NSG diagnostic logging using any of hello following methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="b4a58-123">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b4a58-123">Azure portal</span></span>

<span data-ttu-id="b4a58-124">toouse hello portál tooenable naplózás, bejelentkezési toohello [portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b4a58-124">toouse hello portal tooenable logging, login toohello [portal](https://portal.azure.com).</span></span> <span data-ttu-id="b4a58-125">Kattintson a **további szolgáltatások**, majd írja be a *hálózati biztonsági csoportok*.</span><span class="sxs-lookup"><span data-stu-id="b4a58-125">Click **More services**, then type *network security groups*.</span></span> <span data-ttu-id="b4a58-126">Válassza ki a kívánt naplózása tooenable NSG hello.</span><span class="sxs-lookup"><span data-stu-id="b4a58-126">Select hello NSG you want tooenable logging for.</span></span> <span data-ttu-id="b4a58-127">Hello utasítások nem számítási erőforrások hello [hello portálon diagnosztikai naplók engedélyezése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4a58-127">Follow hello instructions for non-compute resources in hello [Enable diagnostic logs in hello portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="b4a58-128">Válassza ki **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, vagy mindkét kategóriák a naplók.</span><span class="sxs-lookup"><span data-stu-id="b4a58-128">Select **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, or both categories of logs.</span></span>

### <a name="powershell"></a><span data-ttu-id="b4a58-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b4a58-129">PowerShell</span></span>

<span data-ttu-id="b4a58-130">naplózás, toouse PowerShell tooenable hello hello utasításokat követve [PowerShell diagnosztikai naplók engedélyezése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4a58-130">toouse PowerShell tooenable logging, follow hello instructions in hello [Enable diagnostic logs via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="b4a58-131">Értékelje ki a következő információ, mielőtt belép a parancs hello cikkből hello:</span><span class="sxs-lookup"><span data-stu-id="b4a58-131">Evaluate hello following information before entering a command from hello article:</span></span>

- <span data-ttu-id="b4a58-132">Azt is meghatározhatja, hello érték toouse a hello `-ResourceId` hello következő [szöveg] megfelelő, majd a hello parancs bevitele lecserélésével paraméter `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span><span class="sxs-lookup"><span data-stu-id="b4a58-132">You can determine hello value toouse for hello `-ResourceId` parameter by replacing hello following [text], as appropriate, then entering hello command `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span></span> <span data-ttu-id="b4a58-133">hello parancs kimenetében hello azonosító hasonlít túl*következő [előfizetés Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG neve]*.</span><span class="sxs-lookup"><span data-stu-id="b4a58-133">hello ID output from hello command looks similar too*/subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="b4a58-134">Ha csak a napló kategória toocollect adatait szeretné hozzáadni `-Categories [category]` hello parancs hello cikkben, ahol a kategóriát vagy a toohello végére *NetworkSecurityGroupEvent* vagy *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="b4a58-134">If you only want toocollect data from log category add `-Categories [category]` toohello end of hello command in hello article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="b4a58-135">Ha nem adja meg hello `-Categories` paraméter, adatgyűjtés engedélyezve van, az kategóriák naplófájl.</span><span class="sxs-lookup"><span data-stu-id="b4a58-135">If you don't use hello `-Categories` parameter, data collection is enabled for both log categories.</span></span>

### <a name="azure-command-line-interface-cli"></a><span data-ttu-id="b4a58-136">Azure parancssori felület (CLI)</span><span class="sxs-lookup"><span data-stu-id="b4a58-136">Azure command-line interface (CLI)</span></span>

<span data-ttu-id="b4a58-137">toouse hello CLI tooenable naplózási, hello hello utasításait követve [parancssori felületen keresztül diagnosztikai naplók engedélyezése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4a58-137">toouse hello CLI tooenable logging, follow hello instructions in hello [Enable diagnostic logs via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="b4a58-138">Értékelje ki a következő információ, mielőtt belép a parancs hello cikkből hello:</span><span class="sxs-lookup"><span data-stu-id="b4a58-138">Evaluate hello following information before entering a command from hello article:</span></span>

- <span data-ttu-id="b4a58-139">Azt is meghatározhatja, hello érték toouse a hello `-ResourceId` hello következő [szöveg] megfelelő, majd a hello parancs bevitele lecserélésével paraméter `azure network nsg show [resource-group-name] [nsg-name]`.</span><span class="sxs-lookup"><span data-stu-id="b4a58-139">You can determine hello value toouse for hello `-ResourceId` parameter by replacing hello following [text], as appropriate, then entering hello command `azure network nsg show [resource-group-name] [nsg-name]`.</span></span> <span data-ttu-id="b4a58-140">hello parancs kimenetében hello azonosító hasonlít túl*következő [előfizetés Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG neve]*.</span><span class="sxs-lookup"><span data-stu-id="b4a58-140">hello ID output from hello command looks similar too*/subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="b4a58-141">Ha csak a napló kategória toocollect adatait szeretné hozzáadni `-Categories [category]` hello parancs hello cikkben, ahol a kategóriát vagy a toohello végére *NetworkSecurityGroupEvent* vagy *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="b4a58-141">If you only want toocollect data from log category add `-Categories [category]` toohello end of hello command in hello article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="b4a58-142">Ha nem adja meg hello `-Categories` paraméter, adatgyűjtés engedélyezve van, az kategóriák naplófájl.</span><span class="sxs-lookup"><span data-stu-id="b4a58-142">If you don't use hello `-Categories` parameter, data collection is enabled for both log categories.</span></span>

## <a name="logged-data"></a><span data-ttu-id="b4a58-143">A naplózott adatok</span><span class="sxs-lookup"><span data-stu-id="b4a58-143">Logged data</span></span>

<span data-ttu-id="b4a58-144">JSON-formátumú adatot ír a mindkét naplókhoz.</span><span class="sxs-lookup"><span data-stu-id="b4a58-144">JSON-formatted data is written for both logs.</span></span> <span data-ttu-id="b4a58-145">hello adatokat az egyes napló írása, a következő szakaszok hello szerepel:</span><span class="sxs-lookup"><span data-stu-id="b4a58-145">hello specific data written for each log type is listed in hello following sections:</span></span>

### <a name="event-log"></a><span data-ttu-id="b4a58-146">Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="b4a58-146">Event log</span></span>
<span data-ttu-id="b4a58-147">A napló tartalmaz információkat, mely NSG vonatkozó szabályokat alkalmazott tooVMs, és a felhőalapú szolgáltatás szerepkörpéldányt beállítani, a MAC-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="b4a58-147">This log contains information about which NSG rules are applied tooVMs and cloud service role instances, based on MAC address.</span></span> <span data-ttu-id="b4a58-148">a következő példa adatok hello minden esemény kerül:</span><span class="sxs-lookup"><span data-stu-id="b4a58-148">hello following example data is logged for each event:</span></span>

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a><span data-ttu-id="b4a58-149">A szabály számláló napló</span><span class="sxs-lookup"><span data-stu-id="b4a58-149">Rule counter log</span></span>

<span data-ttu-id="b4a58-150">Ez a napló minden alkalmazott szabály tooresources információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4a58-150">This log contains information about each rule applied tooresources.</span></span> <span data-ttu-id="b4a58-151">hello alábbi példa a rendszer adatokat naplózza minden alkalommal, amikor egy szabály alkalmazásakor:</span><span class="sxs-lookup"><span data-stu-id="b4a58-151">hello following example data is logged each time a rule is applied:</span></span>

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a><span data-ttu-id="b4a58-152">Megtekintése és elemzése a naplókat</span><span class="sxs-lookup"><span data-stu-id="b4a58-152">View and analyze logs</span></span>

<span data-ttu-id="b4a58-153">toolearn hogyan tooview tevékenységet naplózni, olvassa el a hello [hello Azure tevékenységnapló áttekintése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4a58-153">toolearn how tooview activity log data, read hello [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="b4a58-154">toolearn hogyan tooview diagnosztikai naplózni, olvassa el a hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4a58-154">toolearn how tooview diagnostic log data, read hello [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="b4a58-155">Ha diagnosztikai adatokat tooLog Analytics küldi el, használhatja a hello [Azure hálózati biztonsági csoport analytics](../log-analytics/log-analytics-azure-networking-analytics.md) továbbfejlesztett insights (előzetes verzió) felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="b4a58-155">If you send diagnostics data tooLog Analytics, you can use hello [Azure Network Security Group analytics](../log-analytics/log-analytics-azure-networking-analytics.md) (preview) management solution for enhanced insights.</span></span> 
