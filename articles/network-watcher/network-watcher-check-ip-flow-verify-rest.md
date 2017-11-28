---
title: "Ellenőrizze a aaaVerify forgalom Azure hálózati figyelő IP-adatfolyam - REST |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 956db0d326db597c6c402a9e8d4a5522c47c02d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="87921-103">Ellenőrizze, hogy a forgalom engedélyezett vagy megtagadott IP-folyamat az Azure hálózati figyelőt összetevője ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="87921-103">Check if traffic is allowed or denied with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="87921-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="87921-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="87921-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="87921-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="87921-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="87921-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="87921-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="87921-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="87921-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="87921-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="87921-109">IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="87921-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="87921-110">hello érvényesítési futtathatja a bejövő vagy kimenő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="87921-110">hello validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="87921-111">Ebben a forgatókönyvben hasznos tooget e virtuális gép működik tooan külső erőforrás- vagy háttéradatbázis aktuális állapotának.</span><span class="sxs-lookup"><span data-stu-id="87921-111">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="87921-112">IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="87921-112">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="87921-113">Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.</span><span class="sxs-lookup"><span data-stu-id="87921-113">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="87921-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="87921-114">Before you begin</span></span>

<span data-ttu-id="87921-115">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87921-115">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="87921-116">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="87921-116">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="87921-117">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="87921-117">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="87921-118">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="87921-118">Scenario</span></span>

<span data-ttu-id="87921-119">Ez a forgatókönyv IP folyamata ellenőrizze tooverify használ, ha egy virtuális gép működik tooanother gép 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="87921-119">This scenario uses IP flow Verify tooverify if a virtual machine can talk tooanother machine over port 443.</span></span> <span data-ttu-id="87921-120">Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza.</span><span class="sxs-lookup"><span data-stu-id="87921-120">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="87921-121">További információ az IP-adatfolyam győződjön meg arról, toolearn látogasson el [IP folyamat ellenőrzése – áttekintés](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="87921-121">toolearn more about IP flow Verify, visit [IP flow verify overview](network-watcher-ip-flow-verify-overview.md)</span></span>

<span data-ttu-id="87921-122">Ebben a forgatókönyvben azt:</span><span class="sxs-lookup"><span data-stu-id="87921-122">In this scenario, you:</span></span>

* <span data-ttu-id="87921-123">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="87921-123">Retrieve a virtual machine</span></span>
* <span data-ttu-id="87921-124">Hívás IP folyamat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="87921-124">Call IP flow verify</span></span>
* <span data-ttu-id="87921-125">Eredmények ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="87921-125">Verify results</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="87921-126">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="87921-126">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="87921-127">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="87921-127">Retrieve a virtual machine</span></span>

<span data-ttu-id="87921-128">A következő parancsfájl tooreturn hello egy virtuális gép futtatásához.</span><span class="sxs-lookup"><span data-stu-id="87921-128">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="87921-129">a következő kódot kell hello változók értékeit hello:</span><span class="sxs-lookup"><span data-stu-id="87921-129">hello following code needs values for hello variables:</span></span>

* <span data-ttu-id="87921-130">**a subscriptionId** -előfizetési azonosító toouse hello.</span><span class="sxs-lookup"><span data-stu-id="87921-130">**subscriptionId** - hello subscription Id toouse.</span></span>
* <span data-ttu-id="87921-131">**resourceGroupName** – hello virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="87921-131">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="87921-132">hello szükséges adatokat a rendszer hello azonosítóját a hello típusa `Microsoft.Compute/virtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="87921-132">hello information that is needed is hello id under hello type `Microsoft.Compute/virtualMachines`.</span></span> <span data-ttu-id="87921-133">hello eredményeket kell lennie a következő példakód hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="87921-133">hello results should be similar toohello following code sample:</span></span>

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a><span data-ttu-id="87921-134">Hívás IP folyamat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="87921-134">Call IP flow Verify</span></span>

<span data-ttu-id="87921-135">hello alábbi példa létrehoz egy kérelem tooverify hello forgalmat egy adott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="87921-135">hello following example creates a request tooverify hello traffic for a specified virtual machine.</span></span> <span data-ttu-id="87921-136">hello választ ad vissza, ha hello forgalom engedélyezve van, vagy ha hello forgalom megtagadva.</span><span class="sxs-lookup"><span data-stu-id="87921-136">hello response returns if hello traffic is allowed or if hello traffic is denied.</span></span> <span data-ttu-id="87921-137">Ha a forgalom is adja vissza, milyen szabály blokkok hello forgalom megtagadva.</span><span class="sxs-lookup"><span data-stu-id="87921-137">If traffic is denied it also returns what rule blocks hello traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="87921-138">IP-adatfolyam ellenőrizze, hogy megköveteli, hogy a virtuális gép erőforrásához hello le van foglalva.</span><span class="sxs-lookup"><span data-stu-id="87921-138">IP flow verify requires that hello VM resource is allocated.</span></span>

<span data-ttu-id="87921-139">hello parancsfájlhoz szükséges hello erőforrás-azonosítót a virtuális gépek és a hálózati kártya hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="87921-139">hello script requires hello resource Id of a virtual machine and of a network interface card on hello virtual machine.</span></span> <span data-ttu-id="87921-140">Ezek az értékek kimeneti megelőző hello által biztosított.</span><span class="sxs-lookup"><span data-stu-id="87921-140">These values are provided by hello preceding output.</span></span>

> [!Important]
> <span data-ttu-id="87921-141">Az összes hálózati figyelő REST hívások hello hello kérelem URI hello egy hello hálózati figyelőt példányát, nem hello olyan erőforrásokat tartalmaz, a hello diagnosztikai műveleteket hajt végre, az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="87921-141">For all Network Watcher REST calls hello resource group name in hello request URI is hello one that contains hello Network Watcher instance, not hello resources you are performing hello diagnostic actions on.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-hello-results"></a><span data-ttu-id="87921-142">Hello eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="87921-142">Understanding hello results</span></span>

<span data-ttu-id="87921-143">hello válasz vissza azt ismerteti, hogy hello forgalom engedélyezett vagy megtagadott.</span><span class="sxs-lookup"><span data-stu-id="87921-143">hello response you get back tells you whether hello traffic is allowed or denied.</span></span> <span data-ttu-id="87921-144">hello választ egyet a következő példák hello néz:</span><span class="sxs-lookup"><span data-stu-id="87921-144">hello response looks like one of hello following examples:</span></span>

<span data-ttu-id="87921-145">**Engedélyezett**</span><span class="sxs-lookup"><span data-stu-id="87921-145">**Allowed**</span></span>

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

<span data-ttu-id="87921-146">**Megtagadva**</span><span class="sxs-lookup"><span data-stu-id="87921-146">**Denied**</span></span>

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a><span data-ttu-id="87921-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87921-147">Next steps</span></span>

<span data-ttu-id="87921-148">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn további hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="87921-148">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn more about Network Security Groups.</span></span>












