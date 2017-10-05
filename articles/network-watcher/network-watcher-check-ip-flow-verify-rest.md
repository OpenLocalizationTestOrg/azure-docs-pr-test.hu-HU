---
title: "Ellenőrizze a forgalom Azure hálózati figyelő IP folyamata ellenőrizze - REST-|} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan ellenőrizhető, ha a bejövő és kimenő forgalmat a virtuális gépek engedélyezett vagy megtagadott"
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
ms.openlocfilehash: 6d3ce00a7d4f9c0cd57fa8815625a1065b03b5b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="b35f5-103">Ellenőrizze, hogy a forgalom engedélyezett vagy megtagadott IP-folyamat az Azure hálózati figyelőt összetevője ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b35f5-103">Check if traffic is allowed or denied with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="b35f5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b35f5-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="b35f5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b35f5-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="b35f5-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b35f5-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="b35f5-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b35f5-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="b35f5-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="b35f5-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="b35f5-109">IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi, hogy ellenőrizze, hogy ha a forgalom engedélyezve van-e, vagy a virtuális gép hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="b35f5-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="b35f5-110">Az érvényesítési futtathatja a bejövő vagy kimenő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="b35f5-110">The validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="b35f5-111">Ebben a forgatókönyvben akkor hasznos, ha szeretné, hogy a virtuális gép kommunikálhat külső erőforrás- vagy háttéradatbázis aktuális állapotának.</span><span class="sxs-lookup"><span data-stu-id="b35f5-111">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="b35f5-112">IP-adatfolyam győződjön meg arról is használható, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és hibáinak elhárítása az NSG-szabályok által blokkolt adatfolyamok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="b35f5-112">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="b35f5-113">Egy másik oka IP folyamat, ellenőrizze annak biztosítása, amelyet a letiltott forgalmat blokkol megfelelően az NSG.</span><span class="sxs-lookup"><span data-stu-id="b35f5-113">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b35f5-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b35f5-114">Before you begin</span></span>

<span data-ttu-id="b35f5-115">A PowerShell használatával REST API hívása ARMclient szolgál.</span><span class="sxs-lookup"><span data-stu-id="b35f5-115">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="b35f5-116">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="b35f5-116">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="b35f5-117">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="b35f5-117">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="b35f5-118">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="b35f5-118">Scenario</span></span>

<span data-ttu-id="b35f5-119">Ebben a forgatókönyvben IP folyamat, ellenőrizze használatával győződjön meg arról, ha egy virtuális gép működik egy másik 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b35f5-119">This scenario uses IP flow Verify to verify if a virtual machine can talk to another machine over port 443.</span></span> <span data-ttu-id="b35f5-120">Ha a forgalmat a rendszer megtagadja, a biztonsági szabály, amely megtagadja a forgalom adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b35f5-120">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="b35f5-121">IP-adatfolyam ellenőrizze kapcsolatos további információkért látogasson el a [IP folyamat ellenőrzése – áttekintés](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b35f5-121">To learn more about IP flow Verify, visit [IP flow verify overview](network-watcher-ip-flow-verify-overview.md)</span></span>

<span data-ttu-id="b35f5-122">Ebben a forgatókönyvben azt:</span><span class="sxs-lookup"><span data-stu-id="b35f5-122">In this scenario, you:</span></span>

* <span data-ttu-id="b35f5-123">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="b35f5-123">Retrieve a virtual machine</span></span>
* <span data-ttu-id="b35f5-124">Hívás IP folyamat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b35f5-124">Call IP flow verify</span></span>
* <span data-ttu-id="b35f5-125">Eredmények ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b35f5-125">Verify results</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="b35f5-126">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="b35f5-126">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="b35f5-127">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="b35f5-127">Retrieve a virtual machine</span></span>

<span data-ttu-id="b35f5-128">Futtassa a következő parancsfájl egy virtuális gép visszaállítására.</span><span class="sxs-lookup"><span data-stu-id="b35f5-128">Run the following script to return a virtual machine.</span></span> <span data-ttu-id="b35f5-129">A következő kódot a változók értékeit kell:</span><span class="sxs-lookup"><span data-stu-id="b35f5-129">The following code needs values for the variables:</span></span>

* <span data-ttu-id="b35f5-130">**a subscriptionId** -az előfizetési azonosító használatával.</span><span class="sxs-lookup"><span data-stu-id="b35f5-130">**subscriptionId** - The subscription Id to use.</span></span>
* <span data-ttu-id="b35f5-131">**resourceGroupName** -virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="b35f5-131">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="b35f5-132">A szükséges adatokat a rendszer a azonosítóját típus szerinti `Microsoft.Compute/virtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="b35f5-132">The information that is needed is the id under the type `Microsoft.Compute/virtualMachines`.</span></span> <span data-ttu-id="b35f5-133">Az eredményeket a következő példakód hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="b35f5-133">The results should be similar to the following code sample:</span></span>

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

## <a name="call-ip-flow-verify"></a><span data-ttu-id="b35f5-134">Hívás IP folyamat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b35f5-134">Call IP flow Verify</span></span>

<span data-ttu-id="b35f5-135">Az alábbi példakód létrehozza a forgalmat egy adott virtuális gép kérelmet.</span><span class="sxs-lookup"><span data-stu-id="b35f5-135">The following example creates a request to verify the traffic for a specified virtual machine.</span></span> <span data-ttu-id="b35f5-136">A válasz adja vissza, ha a forgalom engedélyezve van, vagy ha a forgalmat a rendszer megtagadja.</span><span class="sxs-lookup"><span data-stu-id="b35f5-136">The response returns if the traffic is allowed or if the traffic is denied.</span></span> <span data-ttu-id="b35f5-137">Ha a forgalom megtagadva is adja vissza, milyen szabály blokkolja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="b35f5-137">If traffic is denied it also returns what rule blocks the traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="b35f5-138">IP-adatfolyam ellenőrizze, hogy megköveteli, hogy a virtuális gép erőforrásához van lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="b35f5-138">IP flow verify requires that the VM resource is allocated.</span></span>

<span data-ttu-id="b35f5-139">A parancsfájl az erőforrás-azonosítót a virtuális gépek és a hálózati kártyát a virtuális gépen van szükség.</span><span class="sxs-lookup"><span data-stu-id="b35f5-139">The script requires the resource Id of a virtual machine and of a network interface card on the virtual machine.</span></span> <span data-ttu-id="b35f5-140">Ezeket az értékeket az előző kimeneti által biztosított.</span><span class="sxs-lookup"><span data-stu-id="b35f5-140">These values are provided by the preceding output.</span></span>

> [!Important]
> <span data-ttu-id="b35f5-141">Az összes hálózati figyelő REST-hívást az erőforráscsoport neve a kérés URI, a hálózati figyelőt példányt tartalmazó, nem az erőforrásokat a diagnosztikai műveleteket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="b35f5-141">For all Network Watcher REST calls the resource group name in the request URI is the one that contains the Network Watcher instance, not the resources you are performing the diagnostic actions on.</span></span>

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

## <a name="understanding-the-results"></a><span data-ttu-id="b35f5-142">Az eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="b35f5-142">Understanding the results</span></span>

<span data-ttu-id="b35f5-143">A válasz vissza azt ismerteti, hogy a forgalom engedélyezett vagy megtagadott.</span><span class="sxs-lookup"><span data-stu-id="b35f5-143">The response you get back tells you whether the traffic is allowed or denied.</span></span> <span data-ttu-id="b35f5-144">A válasz a következőképpen néz egyet az alábbi példákat:</span><span class="sxs-lookup"><span data-stu-id="b35f5-144">The response looks like one of the following examples:</span></span>

<span data-ttu-id="b35f5-145">**Engedélyezett**</span><span class="sxs-lookup"><span data-stu-id="b35f5-145">**Allowed**</span></span>

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

<span data-ttu-id="b35f5-146">**Megtagadva**</span><span class="sxs-lookup"><span data-stu-id="b35f5-146">**Denied**</span></span>

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a><span data-ttu-id="b35f5-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b35f5-147">Next steps</span></span>

<span data-ttu-id="b35f5-148">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) további információt a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="b35f5-148">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to learn more about Network Security Groups.</span></span>












