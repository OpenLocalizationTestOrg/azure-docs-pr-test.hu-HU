---
title: "Ellenőrizze a forgalmat az Azure hálózati figyelő IP Flow ellenőrizze - Azure CLI |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan ellenőrizhető, ha a bejövő és kimenő forgalmat a virtuális gépek engedélyezett vagy megtagadott Azure parancssori felület használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 0b52257a6c38a4392573672b7190d2269c2f145a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="a3305-103">Ha a forgalom engedélyezett vagy megtagadott vagy a virtuális gép IP Flow ellenőrizze és Azure hálózati figyelőt összetevője ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a3305-103">Check if traffic is allowed or denied to or from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a3305-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3305-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="a3305-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3305-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="a3305-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a3305-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="a3305-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a3305-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="a3305-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="a3305-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="a3305-109">IP-adatfolyam ellenőrzése egy funkciója, amely lehetővé teszi, hogy ellenőrizze, hogy ha a forgalom engedélyezve van-e, vagy a virtuális gép hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="a3305-109">IP Flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="a3305-110">Ebben a forgatókönyvben akkor hasznos, ha szeretné, hogy a virtuális gép kommunikálhat külső erőforrás- vagy háttéradatbázis aktuális állapotának.</span><span class="sxs-lookup"><span data-stu-id="a3305-110">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="a3305-111">IP-adatfolyam győződjön meg arról is használható, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és hibáinak elhárítása az NSG-szabályok által blokkolt adatfolyamok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a3305-111">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="a3305-112">Egy másik oka IP folyamat, ellenőrizze annak biztosítása, amelyet a letiltott forgalmat blokkol megfelelően az NSG.</span><span class="sxs-lookup"><span data-stu-id="a3305-112">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

<span data-ttu-id="a3305-113">Ez a cikk használja a következő generációs CLI a erőforrás management üzembe helyezési modellel, Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="a3305-113">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="a3305-114">Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="a3305-114">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a3305-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a3305-115">Before you begin</span></span>

<span data-ttu-id="a3305-116">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) hozzon létre egy hálózati figyelőt, vagy egy meglévő példánya hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="a3305-116">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="a3305-117">A forgatókönyv feltételezi, hogy létezik-e egy erőforráscsoportot, egy érvényes virtuális géppel használandó.</span><span class="sxs-lookup"><span data-stu-id="a3305-117">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="a3305-118">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="a3305-118">Scenario</span></span>

<span data-ttu-id="a3305-119">Ez a forgatókönyv használ IP Flow ellenőrizze ellenőrizheti, ha egy virtuális gép működik egy ismert Bing IP-címre.</span><span class="sxs-lookup"><span data-stu-id="a3305-119">This scenario uses IP Flow Verify to verify if a virtual machine can talk to a known Bing IP address.</span></span> <span data-ttu-id="a3305-120">Ha a forgalmat a rendszer megtagadja, a biztonsági szabály, amely megtagadja a forgalom adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a3305-120">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="a3305-121">IP Flow ellenőrizze kapcsolatos további információkért látogasson el a [IP-adatfolyam ellenőrizze áttekintése](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a3305-121">To learn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="a3305-122">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="a3305-122">Get a VM</span></span>

<span data-ttu-id="a3305-123">IP-adatfolyam tesztek bejövő és kimenő forgalmat a virtuális gép IP-címet vagy egy távoli céljához ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a3305-123">IP flow verify tests traffic to or from an IP address on a virtual machine to or from a remote destination.</span></span> <span data-ttu-id="a3305-124">A virtuális gép azonosítóját a parancsmag szükség.</span><span class="sxs-lookup"><span data-stu-id="a3305-124">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="a3305-125">Ha már ismeri az Azonosítót a virtuális gép használja, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="a3305-125">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-the-nics"></a><span data-ttu-id="a3305-126">A hálózati adapterek beolvasása</span><span class="sxs-lookup"><span data-stu-id="a3305-126">Get the NICS</span></span>

<span data-ttu-id="a3305-127">A virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk a hálózati adaptert egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="a3305-127">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="a3305-128">Ha már ismeri a virtuális gépen vizsgálni kívánt IP-cím, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="a3305-128">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="a3305-129">Futtatási IP-adatfolyam ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a3305-129">Run IP flow verify</span></span>

<span data-ttu-id="a3305-130">Most, hogy a fordítás során futtassa a parancsmagot, és futtassa azt a `az network watcher test-ip-flow` parancsmag segítségével tesztelheti a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a3305-130">Now that we have the information needed to run the cmdlet, we run the `az network watcher test-ip-flow` cmdlet to test the traffic.</span></span> <span data-ttu-id="a3305-131">A jelen példában használjuk az első IP-cím első hálózati adapteren</span><span class="sxs-lookup"><span data-stu-id="a3305-131">In this example, we are using the first IP address on the first NIC.</span></span>

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> <span data-ttu-id="a3305-132">IP-adatfolyam győződjön meg arról, hogy a virtuális gép erőforrásához lefoglalt futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="a3305-132">IP Flow verify requires that the VM resource is allocated to run.</span></span>

## <a name="review-results"></a><span data-ttu-id="a3305-133">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="a3305-133">Review Results</span></span>

<span data-ttu-id="a3305-134">Futtatása után `az network watcher test-ip-flow` eredményeinek, az alábbi példa az előző lépésben adott eredmények.</span><span class="sxs-lookup"><span data-stu-id="a3305-134">After running `az network watcher test-ip-flow` the results are returned, the following example is the results returned from the preceding step.</span></span>

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a><span data-ttu-id="a3305-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3305-135">Next steps</span></span>

<span data-ttu-id="a3305-136">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) nyomon követheti a hálózati biztonsági csoport és a biztonsági meghatározott szabályokat.</span><span class="sxs-lookup"><span data-stu-id="a3305-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

<span data-ttu-id="a3305-137">Ismerje meg, látogasson el a NSG beállítások naplózandó [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a3305-137">Learn to audit your NSG settings by visiting [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md).</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
