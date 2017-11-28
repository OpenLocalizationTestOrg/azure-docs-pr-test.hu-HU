---
title: "az internetre irányuló aaaCreate terheléselosztó - klasszikus Azure PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre terheléselosztó klasszikus módban PowerShell használatával"
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
ms.openlocfilehash: 76d9a712a0acda223fc86b80be9c35c0ed9f3a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a><span data-ttu-id="4f36e-103">Bevezetés az internetkapcsolattal rendelkező terheléselosztó (klasszikus) létrehozásába a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="4f36e-103">Get started creating an Internet facing load balancer (classic) in PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4f36e-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="4f36e-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="4f36e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f36e-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="4f36e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4f36e-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="4f36e-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="4f36e-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="4f36e-108">Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="4f36e-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="4f36e-109">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="4f36e-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="4f36e-110">Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello.</span><span class="sxs-lookup"><span data-stu-id="4f36e-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="4f36e-111">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="4f36e-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="4f36e-112">Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4f36e-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a><span data-ttu-id="4f36e-113">A terheléselosztó beállítása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="4f36e-113">Set up load balancer using PowerShell</span></span>

<span data-ttu-id="4f36e-114">tooset fel egy terhelés-kiegyenlítő powershellel, kövesse a hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4f36e-114">tooset up a load balancer using powershell, follow hello steps below:</span></span>

1. <span data-ttu-id="4f36e-115">Ha még sosem használta az Azure PowerShell, lásd: [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az útmutatás hello összes hello módon toohello toosign befejezése az Azure, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="4f36e-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="4f36e-116">A virtuális gép létrehozása utáni használhatja a PowerShell-parancsmagok tooadd load balancer tooa virtuális gép hello belül ugyanaz a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4f36e-116">After creating a virtual machine, you can use PowerShell cmdlets tooadd a load balancer tooa virtual machine within hello same cloud service.</span></span>

<span data-ttu-id="4f36e-117">Hello adhat egy terheléselosztási terheléselosztó-készlet neve "webfarm" toocloud szolgáltatás "mytestcloud" (vagy myctestcloud.cloudapp.net), hello hello végpontjainak betöltése terheléselosztó toovirtual hozzáadása gépek alábbi példa neve "web1" és a "weben 2".</span><span class="sxs-lookup"><span data-stu-id="4f36e-117">In hello following example you will add a load balancer set called "webfarm" toocloud service "mytestcloud" (or myctestcloud.cloudapp.net) , adding hello endpoints for hello load balancer toovirtual machines named "web1" and "web2".</span></span> <span data-ttu-id="4f36e-118">hello terheléselosztó fogad hálózati forgalmat a 80-as porton és terhelés kiegyensúlyozza a helyi végpont hello (az az eset 80-as port) által meghatározott hello virtuális gépek közötti TCP használatával.</span><span class="sxs-lookup"><span data-stu-id="4f36e-118">hello load balancer receives network traffic on port 80 and load balances between hello virtual machines defined by hello local endpoint (in this case port 80) using TCP.</span></span>

### <a name="step-1"></a><span data-ttu-id="4f36e-119">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4f36e-119">Step 1</span></span>

<span data-ttu-id="4f36e-120">A web1"hello első virtuális gép" elosztott terhelésű végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f36e-120">Create a load balanced endpoint for hello first VM "web1"</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a><span data-ttu-id="4f36e-121">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4f36e-121">Step 2</span></span>

<span data-ttu-id="4f36e-122">Hozzon létre egy másik végpont a hello második virtuális gép "web2" hello azonos betölteni a terheléselosztó-készlet neve</span><span class="sxs-lookup"><span data-stu-id="4f36e-122">Create another endpoint for hello second VM  "web2" using hello same load balancer set name</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a><span data-ttu-id="4f36e-123">Virtuális gép eltávolítása a terheléselosztóból</span><span class="sxs-lookup"><span data-stu-id="4f36e-123">Remove a virtual machine from a load balancer</span></span>

<span data-ttu-id="4f36e-124">Használhatja a Remove-AzureEndpoint tooremove egy virtuális gép végpontjának hello terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="4f36e-124">You can use Remove-AzureEndpoint tooremove a virtual machine endpoint from hello load balancer</span></span>

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a><span data-ttu-id="4f36e-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f36e-125">Next steps</span></span>

<span data-ttu-id="4f36e-126">Emellett [megkezdheti egy belső terheléselosztó létrehozását](load-balancer-get-started-ilb-classic-ps.md), és beállíthatja az [elosztási mód](load-balancer-distribution-mode.md) típusát egy adott terheléselosztó hálózati forgalmára vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="4f36e-126">You can also [get started creating an internal load balancer](load-balancer-get-started-ilb-classic-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="4f36e-127">Ha az alkalmazásnak tookeep kapcsolatok életben kiszolgálók egy terheléselosztó mögött, akkor is jobban átláthatja kapcsolatos [üresjárati TCP időtúllépési beállítások terheléselosztó](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="4f36e-127">If your application needs tookeep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="4f36e-128">Az Azure Load Balancer használatakor segítségével toolearn kapcsolat üresjárati működése.</span><span class="sxs-lookup"><span data-stu-id="4f36e-128">It will help toolearn about idle connection behavior when you are using Azure Load Balancer.</span></span>
