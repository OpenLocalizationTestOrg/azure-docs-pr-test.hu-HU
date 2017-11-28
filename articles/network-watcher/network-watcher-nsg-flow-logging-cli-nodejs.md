---
title: "Hálózati biztonsági csoport Flow naplók és Azure hálózati figyelőt - Azure CLI 1.0 kezelése |} Microsoft Docs"
description: "Ezen a lapon ismerteti, hogyan kezelheti a hálózati biztonsági csoport Flow-naplók az Azure CLI 1.0 Azure hálózati figyelőt"
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
ms.openlocfilehash: 2ea8543857c062e76f96da99fb295ce831c3c5f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli-10"></a><span data-ttu-id="00c3b-103">Hálózati biztonsági csoport Flow naplók konfigurálása az Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="00c3b-103">Configuring Network Security Group Flow logs with Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="00c3b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="00c3b-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="00c3b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="00c3b-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="00c3b-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="00c3b-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="00c3b-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="00c3b-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="00c3b-108">REST API</span><span class="sxs-lookup"><span data-stu-id="00c3b-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="00c3b-109">Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi, hogy tekintse meg a hálózati biztonsági csoporton keresztül bemenő és kimenő IP-forgalom hálózati figyelőt feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="00c3b-109">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="00c3b-110">A folyamat naplók json formátumban vannak megírva, és a kimenő és bejövő forgalom alapon egy szabályt, a hálózati adapter a folyamat vonatkozik, a folyamat (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), 5 rekordos információk megjelenítése, és ha a forgalom lett engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="00c3b-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="00c3b-111">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="00c3b-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="00c3b-112">Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.</span><span class="sxs-lookup"><span data-stu-id="00c3b-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="00c3b-113">Elemzések szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="00c3b-113">Register Insights provider</span></span>

<span data-ttu-id="00c3b-114">Ahhoz, hogy sikeresen, működjön naplózás folyamata a **Microsoft.Insights** szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="00c3b-114">In order for flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="00c3b-115">Ha nem tudja, ha a **Microsoft.Insights** szolgáltató regisztrálva van, futtassa a következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="00c3b-115">If you are not sure if the **Microsoft.Insights** provider is registered, run the following script.</span></span>

```azurecli
azure provider register --namespace Microsoft.Insights --subscription <subscriptionid>
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="00c3b-116">Hálózati biztonsági csoport Flow naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="00c3b-116">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="00c3b-117">A parancsot a folyamat naplók engedélyezéséhez az alábbi példában látható:</span><span class="sxs-lookup"><span data-stu-id="00c3b-117">The command to enable flow logs is shown in the following example:</span></span>

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e true
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="00c3b-118">Tiltsa le a hálózati biztonsági csoport folyamata naplók</span><span class="sxs-lookup"><span data-stu-id="00c3b-118">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="00c3b-119">Az alábbi példa használatával tiltsa le a naplók folyamata:</span><span class="sxs-lookup"><span data-stu-id="00c3b-119">Use the following example to disable flow logs:</span></span>

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e false
```

## <a name="download-a-flow-log"></a><span data-ttu-id="00c3b-120">A folyamat napló letöltése</span><span class="sxs-lookup"><span data-stu-id="00c3b-120">Download a Flow log</span></span>

<span data-ttu-id="00c3b-121">A folyamat napló tárolási helye a létrehozásakor van definiálva.</span><span class="sxs-lookup"><span data-stu-id="00c3b-121">The storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="00c3b-122">A folyamat naplók tárfiókba menti eléréséhez eszköz a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="00c3b-122">A convenient tool to access these flow logs saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="00c3b-123">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok kerülnek a storage-fiókok a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="00c3b-123">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="00c3b-124">A napló szerkezete információt [hálózati biztonsági csoport Flow napló áttekintése](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="00c3b-124">For information about the structure of the log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="00c3b-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00c3b-125">Next Steps</span></span>

<span data-ttu-id="00c3b-126">Megtudhatja, hogyan [a powerbi-jal az NSG folyamata naplók megjelenítése](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="00c3b-126">Learn how to [Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="00c3b-127">Megtudhatja, hogyan [jelenítheti meg az NSG folyamata naplók nyílt forráskódú eszközökkel](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="00c3b-127">Learn how to [Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
