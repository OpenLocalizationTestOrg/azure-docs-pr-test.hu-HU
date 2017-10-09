---
title: "az Azure hálózati figyelő IP Flow ellenőrizze - Azure CLI aaaVerify forgalom |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott Azure parancssori felület használatával"
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
ms.openlocfilehash: 128a00b4296994551e7e17838a51e6d9de180e21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="60735-103">Ha a forgalom engedélyezett vagy megtagadott a tooor IP Flow ellenőrizze a virtuális gép alapján Azure hálózati figyelőt összetevője egy ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="60735-103">Check if traffic is allowed or denied tooor from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="60735-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="60735-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="60735-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="60735-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="60735-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="60735-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="60735-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="60735-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="60735-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="60735-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="60735-109">IP-adatfolyam ellenőrzése egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="60735-109">IP Flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="60735-110">Ebben a forgatókönyvben hasznos tooget e virtuális gép működik tooan külső erőforrás- vagy háttéradatbázis aktuális állapotának.</span><span class="sxs-lookup"><span data-stu-id="60735-110">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="60735-111">IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="60735-111">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="60735-112">Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.</span><span class="sxs-lookup"><span data-stu-id="60735-112">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

<span data-ttu-id="60735-113">Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="60735-113">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="60735-114">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="60735-114">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="60735-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="60735-115">Before you begin</span></span>

<span data-ttu-id="60735-116">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt, vagy hálózati figyelőt meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="60735-116">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="60735-117">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="60735-117">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="60735-118">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="60735-118">Scenario</span></span>

<span data-ttu-id="60735-119">Ez a forgatókönyv használ tooverify IP Flow győződjön meg arról, ha egy virtuális gép működik tooa ismert Bing IP-címet.</span><span class="sxs-lookup"><span data-stu-id="60735-119">This scenario uses IP Flow Verify tooverify if a virtual machine can talk tooa known Bing IP address.</span></span> <span data-ttu-id="60735-120">Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza.</span><span class="sxs-lookup"><span data-stu-id="60735-120">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="60735-121">toolearn IP Flow ellenőrzéséhez kapcsolatos további információkért látogasson el [IP-adatfolyam ellenőrizze áttekintése](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="60735-121">toolearn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="60735-122">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="60735-122">Get a VM</span></span>

<span data-ttu-id="60735-123">IP-adatfolyam tesztek forgalom tooor tartozó IP-címet a virtuális gép tooor a távoli célhelyről a ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="60735-123">IP flow verify tests traffic tooor from an IP address on a virtual machine tooor from a remote destination.</span></span> <span data-ttu-id="60735-124">A virtuális gép azonosítóját hello parancsmag szükség.</span><span class="sxs-lookup"><span data-stu-id="60735-124">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="60735-125">Ha már ismeri hello virtuális gép toouse hello azonosítója, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="60735-125">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-hello-nics"></a><span data-ttu-id="60735-126">Hálózati adapter hello beolvasása</span><span class="sxs-lookup"><span data-stu-id="60735-126">Get hello NICS</span></span>

<span data-ttu-id="60735-127">hello hello virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk hello hálózati adaptert egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="60735-127">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="60735-128">Ha már ismeri a hello IP-címet, amelyet szeretne tootest hello virtuális gépen ezt a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="60735-128">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="60735-129">Futtatási IP-adatfolyam ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="60735-129">Run IP flow verify</span></span>

<span data-ttu-id="60735-130">Most, hogy hello információ szükséges toorun hello parancsmag, hello Futtatás `az network watcher test-ip-flow` parancsmag tootest hello forgalmat.</span><span class="sxs-lookup"><span data-stu-id="60735-130">Now that we have hello information needed toorun hello cmdlet, we run hello `az network watcher test-ip-flow` cmdlet tootest hello traffic.</span></span> <span data-ttu-id="60735-131">A jelen példában használjuk hello első IP-cím hello első adapteren zajlik.</span><span class="sxs-lookup"><span data-stu-id="60735-131">In this example, we are using hello first IP address on hello first NIC.</span></span>

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> <span data-ttu-id="60735-132">Megköveteli, hogy hello VM erőforrás toorun lefoglalt IP-adatfolyam ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="60735-132">IP Flow verify requires that hello VM resource is allocated toorun.</span></span>

## <a name="review-results"></a><span data-ttu-id="60735-133">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="60735-133">Review Results</span></span>

<span data-ttu-id="60735-134">Futtatása után `az network watcher test-ip-flow` hello eredményeinek, hello alábbi példa az előző lépésben hello hello eredményének.</span><span class="sxs-lookup"><span data-stu-id="60735-134">After running `az network watcher test-ip-flow` hello results are returned, hello following example is hello results returned from hello preceding step.</span></span>

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a><span data-ttu-id="60735-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60735-135">Next steps</span></span>

<span data-ttu-id="60735-136">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello hálózati biztonsági csoport és a biztonsági szabályok definiált.</span><span class="sxs-lookup"><span data-stu-id="60735-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

<span data-ttu-id="60735-137">Látogasson el a NSG beállítások tooaudit további [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="60735-137">Learn tooaudit your NSG settings by visiting [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md).</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
