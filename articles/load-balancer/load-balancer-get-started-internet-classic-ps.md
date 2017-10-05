---
title: "Internetkapcsolattal rendelkező terheléselosztó létrehozása – klasszikus Azure PowerShell | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó klasszikus üzemmódban a PowerShell használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 1a41f3ba199fb692c111ea6a40ddb09605f91da2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a><span data-ttu-id="8c2a0-103">Bevezetés az internetkapcsolattal rendelkező terheléselosztó (klasszikus) létrehozásába a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="8c2a0-103">Get started creating an Internet facing load balancer (classic) in PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8c2a0-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="8c2a0-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="8c2a0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c2a0-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="8c2a0-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8c2a0-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="8c2a0-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="8c2a0-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="8c2a0-108">Az Azure-erőforrásokkal való munka megkezdése előtt fontos megérteni, hogy az Azure jelenleg két üzembe helyezési modellel rendelkezik, a Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-108">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="8c2a0-109">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="8c2a0-110">A különféle eszközök dokumentációit a cikk tetején található fülekre kattintva tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-110">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span> <span data-ttu-id="8c2a0-111">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-111">This article covers the classic deployment model.</span></span> <span data-ttu-id="8c2a0-112">Emellett [azt is megismerheti, hogyan lehet internetkapcsolattal rendelkező terheléselosztót létrehozni az Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="8c2a0-112">You can also [Learn how to create an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a><span data-ttu-id="8c2a0-113">A terheléselosztó beállítása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="8c2a0-113">Set up load balancer using PowerShell</span></span>

<span data-ttu-id="8c2a0-114">Az alábbi lépéseket követve állíthat be egy terheléselosztót a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="8c2a0-114">To set up a load balancer using powershell, follow the steps below:</span></span>

1. <span data-ttu-id="8c2a0-115">Ha még nem használta az Azure PowerShellt, tekintse meg [How to Install and Configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) című részt, majd kövesse az utasításokat egészen az utolsó lépésig az Azure-ba való bejelentkezéshez és az előfizetése kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-115">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="8c2a0-116">Virtuális gép létrehozása után a PowerShell-parancsmagok segítségével hozzáadhat egy terheléselosztót egy virtuális géphez ugyanazon a felhőszolgáltatáson belül.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-116">After creating a virtual machine, you can use PowerShell cmdlets to add a load balancer to a virtual machine within the same cloud service.</span></span>

<span data-ttu-id="8c2a0-117">Az alábbi példában egy „webfarm” nevű terheléselosztó-készletet fog hozzáadni a „mytestcloud” (vagy myctestcloud.cloudapp.net) nevű felhőszolgáltatáshoz, ehhez a terheléselosztó végpontjait hozzáadja a „web1” és „web2” nevű virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-117">In the following example you will add a load balancer set called "webfarm" to cloud service "mytestcloud" (or myctestcloud.cloudapp.net) , adding the endpoints for the load balancer to virtual machines named "web1" and "web2".</span></span> <span data-ttu-id="8c2a0-118">A terheléselosztó a 80-as porton keresztül fogadja a hálózati forgalmat, és a TCP használatával elosztja a terhelést a helyi végpont (jelen esetben a 80-as port) által meghatározott virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-118">The load balancer receives network traffic on port 80 and load balances between the virtual machines defined by the local endpoint (in this case port 80) using TCP.</span></span>

### <a name="step-1"></a><span data-ttu-id="8c2a0-119">1. lépés</span><span class="sxs-lookup"><span data-stu-id="8c2a0-119">Step 1</span></span>

<span data-ttu-id="8c2a0-120">Hozzon létre egy elosztott terhelésű végpontot az első, „web1” nevű virtuális gép számára</span><span class="sxs-lookup"><span data-stu-id="8c2a0-120">Create a load balanced endpoint for the first VM "web1"</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a><span data-ttu-id="8c2a0-121">2. lépés</span><span class="sxs-lookup"><span data-stu-id="8c2a0-121">Step 2</span></span>

<span data-ttu-id="8c2a0-122">Hozzon létre egy másik végpontot a második, „web2” nevű virtuális gép számára ugyanazon terheléselosztó-készlet nevének használatával</span><span class="sxs-lookup"><span data-stu-id="8c2a0-122">Create another endpoint for the second VM  "web2" using the same load balancer set name</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a><span data-ttu-id="8c2a0-123">Virtuális gép eltávolítása a terheléselosztóból</span><span class="sxs-lookup"><span data-stu-id="8c2a0-123">Remove a virtual machine from a load balancer</span></span>

<span data-ttu-id="8c2a0-124">A Remove-AzureEndpoint paranccsal távolíthatja el egy virtuális gép végpontját a terheléselosztóból</span><span class="sxs-lookup"><span data-stu-id="8c2a0-124">You can use Remove-AzureEndpoint to remove a virtual machine endpoint from the load balancer</span></span>

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a><span data-ttu-id="8c2a0-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c2a0-125">Next steps</span></span>

<span data-ttu-id="8c2a0-126">Emellett [megkezdheti egy belső terheléselosztó létrehozását](load-balancer-get-started-ilb-classic-ps.md), és beállíthatja az [elosztási mód](load-balancer-distribution-mode.md) típusát egy adott terheléselosztó hálózati forgalmára vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-126">You can also [get started creating an internal load balancer](load-balancer-get-started-ilb-classic-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="8c2a0-127">Ha az alkalmazásának fenn kell tartania a kapcsolatot a terheléselosztó mögött lévő kiszolgálókkal, többet megtudhat a [terheléselosztó üresjárati TCP-időkorlátjának beállításairól](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="8c2a0-127">If your application needs to keep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="8c2a0-128">A segítségével jobban megismerheti az üresjárati kapcsolatok viselkedését az Azure Load Balancer használata során.</span><span class="sxs-lookup"><span data-stu-id="8c2a0-128">It will help to learn about idle connection behavior when you are using Azure Load Balancer.</span></span>
