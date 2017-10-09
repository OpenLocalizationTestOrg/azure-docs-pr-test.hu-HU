---
title: "Ellenőrizze a aaaverify forgalom Azure hálózati figyelő IP-adatfolyam - PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott a PowerShell használatával"
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
ms.openlocfilehash: 924da1de1bd554e15816886f8e51d7f170f0e7ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="7cf26-103">Ellenőrizze, hogy a forgalom engedélyezett vagy tiltott tooor a virtuális gép IP-adatfolyam és Azure hálózati figyelőt összetevője ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7cf26-103">Check if traffic is allowed or denied tooor from a VM with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="7cf26-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7cf26-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="7cf26-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7cf26-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="7cf26-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7cf26-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="7cf26-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7cf26-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="7cf26-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="7cf26-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="7cf26-109">IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="7cf26-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="7cf26-110">Ebben a forgatókönyvben hasznos tooget e virtuális gép működik tooan külső erőforrás- vagy háttéradatbázis aktuális állapotának.</span><span class="sxs-lookup"><span data-stu-id="7cf26-110">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="7cf26-111">IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="7cf26-111">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="7cf26-112">Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.</span><span class="sxs-lookup"><span data-stu-id="7cf26-112">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7cf26-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="7cf26-113">Before you begin</span></span>

<span data-ttu-id="7cf26-114">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt, vagy hálózati figyelőt meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="7cf26-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="7cf26-115">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="7cf26-115">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="7cf26-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="7cf26-116">Scenario</span></span>

<span data-ttu-id="7cf26-117">Ezen forgatókönyv által használt IP-adatfolyam tooverify ellenőrizze-e a virtuális gép működik tooa ismert Bing IP-címet.</span><span class="sxs-lookup"><span data-stu-id="7cf26-117">This scenario uses IP flow verify tooverify if a virtual machine can talk tooa known Bing IP address.</span></span> <span data-ttu-id="7cf26-118">Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7cf26-118">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="7cf26-119">toolearn kapcsolatos IP folyamata győződjön meg arról, látogasson el [IP folyamat ellenőrzése – áttekintés](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="7cf26-119">toolearn more about IP flow verify, visit [IP flow verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="7cf26-120">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="7cf26-120">Retrieve Network Watcher</span></span>

<span data-ttu-id="7cf26-121">hello első lépéseként tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="7cf26-121">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="7cf26-122">Hello `$networkWatcher` változó átadása toohello IP folyamata parancsmag ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="7cf26-122">hello `$networkWatcher` variable is passed toohello IP flow verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="7cf26-123">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="7cf26-123">Get a VM</span></span>

<span data-ttu-id="7cf26-124">IP-adatfolyam tesztek forgalom tooor tartozó IP-címet a virtuális gép tooor a távoli célhelyről a ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="7cf26-124">IP flow verify tests traffic tooor from an IP address on a virtual machine tooor from a remote destination.</span></span> <span data-ttu-id="7cf26-125">A virtuális gép azonosítóját hello parancsmag szükség.</span><span class="sxs-lookup"><span data-stu-id="7cf26-125">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="7cf26-126">Ha már ismeri hello virtuális gép toouse hello azonosítója, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="7cf26-126">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-hello-nics"></a><span data-ttu-id="7cf26-127">Hálózati adapter hello beolvasása</span><span class="sxs-lookup"><span data-stu-id="7cf26-127">Get hello NICS</span></span>

<span data-ttu-id="7cf26-128">hello hello virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk hello hálózati adaptert egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="7cf26-128">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="7cf26-129">Ha már ismeri a hello IP-címet, amelyet szeretne tootest hello virtuális gépen ezt a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="7cf26-129">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="7cf26-130">Futtatási IP-adatfolyam ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7cf26-130">Run IP flow verify</span></span>

<span data-ttu-id="7cf26-131">Most, hogy hello információ szükséges toorun hello parancsmag, hello Futtatás `Test-AzureRmNetworkWatcherIPFlow` parancsmag tootest hello forgalmat.</span><span class="sxs-lookup"><span data-stu-id="7cf26-131">Now that we have hello information needed toorun hello cmdlet, we run hello `Test-AzureRmNetworkWatcherIPFlow` cmdlet tootest hello traffic.</span></span> <span data-ttu-id="7cf26-132">A jelen példában használjuk hello első IP-cím hello első adapteren zajlik.</span><span class="sxs-lookup"><span data-stu-id="7cf26-132">In this example, we are using hello first IP address on hello first NIC.</span></span>

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> <span data-ttu-id="7cf26-133">IP-adatfolyam ellenőrizze, hogy megköveteli, hogy hello VM erőforrás toorun van lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="7cf26-133">IP flow verify requires that hello VM resource is allocated toorun.</span></span>

## <a name="review-results"></a><span data-ttu-id="7cf26-134">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="7cf26-134">Review Results</span></span>

<span data-ttu-id="7cf26-135">Futtatása után `Test-AzureRmNetworkWatcherIPFlow` hello eredményeinek, hello alábbi példa az előző lépésben hello hello eredményének.</span><span class="sxs-lookup"><span data-stu-id="7cf26-135">After running `Test-AzureRmNetworkWatcherIPFlow` hello results are returned, hello following example is hello results returned from hello preceding step.</span></span>

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a><span data-ttu-id="7cf26-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cf26-136">Next steps</span></span>

<span data-ttu-id="7cf26-137">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello hálózati biztonsági csoport és a biztonsági szabályok definiált.</span><span class="sxs-lookup"><span data-stu-id="7cf26-137">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













