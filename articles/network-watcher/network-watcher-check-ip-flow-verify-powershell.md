---
title: "Ellenőrizze a forgalom Azure hálózati figyelő IP-adatfolyam ellenőrizze - PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan ellenőrizhető, ha a bejövő és kimenő forgalmat a virtuális gépek engedélyezett vagy megtagadott a PowerShell használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: bf0c01a9af0e28647d11ad89a9d164716d5c8312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="559bb-103">Ellenőrizze, hogy a forgalom engedélyezett vagy megtagadott vagy a virtuális gép IP-adatfolyam és Azure hálózati figyelőt összetevője ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="559bb-103">Check if traffic is allowed or denied to or from a VM with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="559bb-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="559bb-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="559bb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="559bb-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="559bb-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="559bb-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="559bb-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="559bb-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="559bb-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="559bb-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="559bb-109">IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi, hogy ellenőrizze, hogy ha a forgalom engedélyezve van-e, vagy a virtuális gép hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="559bb-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="559bb-110">Ebben a forgatókönyvben akkor hasznos, ha szeretné, hogy a virtuális gép kommunikálhat külső erőforrás- vagy háttéradatbázis aktuális állapotának.</span><span class="sxs-lookup"><span data-stu-id="559bb-110">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="559bb-111">IP-adatfolyam győződjön meg arról is használható, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és hibáinak elhárítása az NSG-szabályok által blokkolt adatfolyamok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="559bb-111">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="559bb-112">Egy másik oka IP folyamat, ellenőrizze annak biztosítása, amelyet a letiltott forgalmat blokkol megfelelően az NSG.</span><span class="sxs-lookup"><span data-stu-id="559bb-112">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="559bb-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="559bb-113">Before you begin</span></span>

<span data-ttu-id="559bb-114">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) hozzon létre egy hálózati figyelőt, vagy egy meglévő példánya hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="559bb-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="559bb-115">A forgatókönyv feltételezi, hogy létezik-e egy erőforráscsoportot, egy érvényes virtuális géppel használandó.</span><span class="sxs-lookup"><span data-stu-id="559bb-115">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="559bb-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="559bb-116">Scenario</span></span>

<span data-ttu-id="559bb-117">Ezen forgatókönyv által használt IP-adatfolyam ellenőrizze ellenőrizheti, ha egy virtuális gép működik egy ismert Bing IP-címre.</span><span class="sxs-lookup"><span data-stu-id="559bb-117">This scenario uses IP flow verify to verify if a virtual machine can talk to a known Bing IP address.</span></span> <span data-ttu-id="559bb-118">Ha a forgalmat a rendszer megtagadja, a biztonsági szabály, amely megtagadja a forgalom adja vissza.</span><span class="sxs-lookup"><span data-stu-id="559bb-118">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="559bb-119">IP-adatfolyam bővebben ellenőrizze megismeréséhez látogasson el [IP folyamat ellenőrzése – áttekintés](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="559bb-119">To learn more about IP flow verify, visit [IP flow verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="559bb-120">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="559bb-120">Retrieve Network Watcher</span></span>

<span data-ttu-id="559bb-121">Az első lépés a hálózati figyelőt példányának lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="559bb-121">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="559bb-122">A `$networkWatcher` változó átadása az IP-adatfolyam parancsmag ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="559bb-122">The `$networkWatcher` variable is passed to the IP flow verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="559bb-123">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="559bb-123">Get a VM</span></span>

<span data-ttu-id="559bb-124">IP-adatfolyam tesztek bejövő és kimenő forgalmat a virtuális gép IP-címet vagy egy távoli céljához ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="559bb-124">IP flow verify tests traffic to or from an IP address on a virtual machine to or from a remote destination.</span></span> <span data-ttu-id="559bb-125">A virtuális gép azonosítóját a parancsmag szükség.</span><span class="sxs-lookup"><span data-stu-id="559bb-125">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="559bb-126">Ha már ismeri az Azonosítót a virtuális gép használja, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="559bb-126">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-the-nics"></a><span data-ttu-id="559bb-127">A hálózati adapterek beolvasása</span><span class="sxs-lookup"><span data-stu-id="559bb-127">Get the NICS</span></span>

<span data-ttu-id="559bb-128">A virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk a hálózati adaptert egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="559bb-128">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="559bb-129">Ha már ismeri a virtuális gépen vizsgálni kívánt IP-cím, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="559bb-129">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="559bb-130">Futtatási IP-adatfolyam ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="559bb-130">Run IP flow verify</span></span>

<span data-ttu-id="559bb-131">Most, hogy a fordítás során futtassa a parancsmagot, és futtassa azt a `Test-AzureRmNetworkWatcherIPFlow` parancsmag segítségével tesztelheti a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="559bb-131">Now that we have the information needed to run the cmdlet, we run the `Test-AzureRmNetworkWatcherIPFlow` cmdlet to test the traffic.</span></span> <span data-ttu-id="559bb-132">A jelen példában használjuk az első IP-cím első hálózati adapteren</span><span class="sxs-lookup"><span data-stu-id="559bb-132">In this example, we are using the first IP address on the first NIC.</span></span>

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> <span data-ttu-id="559bb-133">IP-adatfolyam győződjön meg arról, hogy a virtuális gép erőforrásához lefoglalt futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="559bb-133">IP flow verify requires that the VM resource is allocated to run.</span></span>

## <a name="review-results"></a><span data-ttu-id="559bb-134">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="559bb-134">Review Results</span></span>

<span data-ttu-id="559bb-135">Futtatása után `Test-AzureRmNetworkWatcherIPFlow` eredményeinek, az alábbi példa az előző lépésben adott eredmények.</span><span class="sxs-lookup"><span data-stu-id="559bb-135">After running `Test-AzureRmNetworkWatcherIPFlow` the results are returned, the following example is the results returned from the preceding step.</span></span>

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a><span data-ttu-id="559bb-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="559bb-136">Next steps</span></span>

<span data-ttu-id="559bb-137">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) nyomon követheti a hálózati biztonsági csoport és a biztonsági meghatározott szabályokat.</span><span class="sxs-lookup"><span data-stu-id="559bb-137">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













