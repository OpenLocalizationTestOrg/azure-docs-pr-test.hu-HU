---
title: "aaaManage hálózati biztonsági csoport Flow naplózza az Azure hálózati figyelőt - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan naplózza az hálózati biztonsági csoport Flow toomanage az Azure hálózati figyelőt Azure parancssori felülettel"
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
ms.openlocfilehash: 2d0b02e7d0a5a9ab20beb491d49a5747f976a079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a><span data-ttu-id="74dc5-103">Hálózati biztonsági csoport Flow naplók konfigurálása az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="74dc5-103">Configuring Network Security Group Flow logs with Azure CLI</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="74dc5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="74dc5-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="74dc5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="74dc5-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="74dc5-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="74dc5-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="74dc5-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="74dc5-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="74dc5-108">REST API</span><span class="sxs-lookup"><span data-stu-id="74dc5-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="74dc5-109">Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="74dc5-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="74dc5-110">A folyamat naplók json formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.</span><span class="sxs-lookup"><span data-stu-id="74dc5-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="74dc5-111">Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="74dc5-111">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="74dc5-112">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="74dc5-112">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="74dc5-113">Elemzések szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="74dc5-113">Register Insights provider</span></span>

<span data-ttu-id="74dc5-114">Ahhoz, hogy a folyamat sikeres legyen, hello az naplózási toowork **Microsoft.Insights** szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="74dc5-114">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="74dc5-115">Ha nem biztos benne, hogy hello **Microsoft.Insights** szolgáltató regisztrált, futtatási hello következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="74dc5-115">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```azurecli
az provider register --namespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="74dc5-116">Hálózati biztonsági csoport Flow naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="74dc5-116">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="74dc5-117">hello parancs tooenable folyamata naplók hello a következő példa látható:</span><span class="sxs-lookup"><span data-stu-id="74dc5-117">hello command tooenable flow logs is shown in hello following example:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="74dc5-118">Tiltsa le a hálózati biztonsági csoport folyamata naplók</span><span class="sxs-lookup"><span data-stu-id="74dc5-118">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="74dc5-119">A következő példa toodisable folyamata naplók használata hello:</span><span class="sxs-lookup"><span data-stu-id="74dc5-119">Use hello following example toodisable flow logs:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled false --nsg nsgName
```

## <a name="download-a-flow-log"></a><span data-ttu-id="74dc5-120">A folyamat napló letöltése</span><span class="sxs-lookup"><span data-stu-id="74dc5-120">Download a Flow log</span></span>

<span data-ttu-id="74dc5-121">a folyamat napló hello tárolási helye a létrehozásakor van definiálva.</span><span class="sxs-lookup"><span data-stu-id="74dc5-121">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="74dc5-122">Egy eszköz tooaccess ezen folyamat mentett naplók tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="74dc5-122">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="74dc5-123">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:</span><span class="sxs-lookup"><span data-stu-id="74dc5-123">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="74dc5-124">Hello struktúra hello napló információt [hálózati biztonsági csoport Flow napló áttekintése](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="74dc5-124">For information about hello structure of hello log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="74dc5-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="74dc5-125">Next Steps</span></span>

<span data-ttu-id="74dc5-126">Ismerje meg, hogyan túl[a powerbi-jal az NSG folyamata naplók megjelenítése](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="74dc5-126">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="74dc5-127">Ismerje meg, hogyan túl[jelenítheti meg az NSG folyamata naplók nyílt forráskódú eszközökkel](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="74dc5-127">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
