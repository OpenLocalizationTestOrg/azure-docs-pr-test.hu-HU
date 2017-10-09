---
title: "aaaManage Azure hálózati figyelőt - PowerShell a hálózati biztonsági csoport Flow naplózza |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan naplózza az hálózati biztonsági csoport Flow toomanage az Azure hálózati figyelőt a PowerShell használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 987e8728fb6459fd6ff8eb5cd3d36ff855f2ccfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a><span data-ttu-id="48573-103">Hálózati biztonsági csoport Flow naplók konfigurálása a PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="48573-103">Configuring Network Security Group Flow logs with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="48573-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="48573-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="48573-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="48573-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="48573-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="48573-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="48573-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="48573-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="48573-108">REST API</span><span class="sxs-lookup"><span data-stu-id="48573-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="48573-109">Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="48573-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="48573-110">A folyamat naplók json formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.</span><span class="sxs-lookup"><span data-stu-id="48573-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="48573-111">Elemzések szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="48573-111">Register Insights provider</span></span>

<span data-ttu-id="48573-112">Ahhoz, hogy a folyamat sikeres legyen, hello az naplózási toowork **Microsoft.Insights** szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="48573-112">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="48573-113">Ha nem biztos benne, hogy hello **Microsoft.Insights** szolgáltató regisztrált, futtatási hello következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="48573-113">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="48573-114">Hálózati biztonsági csoport Flow naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="48573-114">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="48573-115">hello parancs tooenable folyamata naplók hello a következő példa látható:</span><span class="sxs-lookup"><span data-stu-id="48573-115">hello command tooenable flow logs is shown in hello following example:</span></span>

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG-Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="48573-116">Tiltsa le a hálózati biztonsági csoport folyamata naplók</span><span class="sxs-lookup"><span data-stu-id="48573-116">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="48573-117">A következő példa toodisable folyamata naplók használata hello:</span><span class="sxs-lookup"><span data-stu-id="48573-117">Use hello following example toodisable flow logs:</span></span>

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a><span data-ttu-id="48573-118">A folyamat napló letöltése</span><span class="sxs-lookup"><span data-stu-id="48573-118">Download a Flow log</span></span>

<span data-ttu-id="48573-119">a folyamat napló hello tárolási helye a létrehozásakor van definiálva.</span><span class="sxs-lookup"><span data-stu-id="48573-119">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="48573-120">Egy eszköz tooaccess ezen folyamat mentett naplók tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="48573-120">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="48573-121">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:</span><span class="sxs-lookup"><span data-stu-id="48573-121">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="48573-122">Hello struktúra hello napló információt [hálózati biztonsági csoport Flow napló áttekintése](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="48573-122">For information about hello structure of hello log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="48573-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48573-123">Next Steps</span></span>

<span data-ttu-id="48573-124">Ismerje meg, hogyan túl[a powerbi-jal az NSG folyamata naplók megjelenítése](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="48573-124">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="48573-125">Ismerje meg, hogyan túl[jelenítheti meg az NSG folyamata naplók nyílt forráskódú eszközökkel](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="48573-125">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
