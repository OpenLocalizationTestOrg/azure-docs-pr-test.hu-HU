---
title: "az internetre irányuló aaaCreate terheléselosztó - klasszikus Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Internet felé néző terheléselosztót a klasszikus üzembe helyezési modell használatával hello a klasszikus Azure portálon"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 27b0d5af6e7b493fa94a9dfbfa260483ae95a2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-classic-portal"></a><span data-ttu-id="ecd30-103">Internet felé néző terheléselosztó (klasszikus) a klasszikus Azure portálon hello létrehozásához</span><span class="sxs-lookup"><span data-stu-id="ecd30-103">Get started creating an Internet facing load balancer (classic) in hello Azure classic portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ecd30-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="ecd30-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="ecd30-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecd30-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="ecd30-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ecd30-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="ecd30-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="ecd30-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="ecd30-108">Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="ecd30-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="ecd30-109">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="ecd30-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="ecd30-110">Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello.</span><span class="sxs-lookup"><span data-stu-id="ecd30-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="ecd30-111">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="ecd30-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="ecd30-112">Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ecd30-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a><span data-ttu-id="ecd30-113">Internetkapcsolattal rendelkező terheléselosztó beállítása virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="ecd30-113">Set up an Internet-facing load balancer for virtual machines</span></span>

<span data-ttu-id="ecd30-114">A sorrend tooload egyenleg hálózati forgalmat a hello Internet közötti hello virtuális gépek egy felhőalapú szolgáltatás elosztott terhelésű készletet kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="ecd30-114">In order tooload balance network traffic from hello Internet across hello virtual machines of a cloud service, you must create a load-balanced set.</span></span> <span data-ttu-id="ecd30-115">Ez az eljárás feltételezi, hogy már létrehozta hello virtuális gépek és, hogy azok minden hello belül ugyanaz a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ecd30-115">This procedure assumes that you have already created hello virtual machines and that they are all within hello same cloud service.</span></span>

<span data-ttu-id="ecd30-116">**a virtuális gépek egy elosztott terhelésű készlet tooconfigure**</span><span class="sxs-lookup"><span data-stu-id="ecd30-116">**tooconfigure a load-balanced set for virtual machines**</span></span>

1. <span data-ttu-id="ecd30-117">Hello a klasszikus Azure portálon, kattintson **virtuális gépek**, majd kattintson a virtuális gép elosztott terhelésű készlet hello hello nevére.</span><span class="sxs-lookup"><span data-stu-id="ecd30-117">In hello Azure classic portal, click **Virtual Machines**, and then click hello name of a virtual machine in hello load-balanced set.</span></span>
2. <span data-ttu-id="ecd30-118">Kattintson a **Végpontok**, majd a **Hozzáadás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ecd30-118">Click **Endpoints**, and then click **Add**.</span></span>
3. <span data-ttu-id="ecd30-119">A hello **egy végpont tooa virtuális gép felvétele elosztott** lapján hello jobbra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="ecd30-119">On hello **Add an endpoint tooa virtual machine** page, click hello right arrow.</span></span>
4. <span data-ttu-id="ecd30-120">A hello **adja meg a hello részleteket hello végpont** lap:</span><span class="sxs-lookup"><span data-stu-id="ecd30-120">On hello **Specify hello details of hello endpoint** page:</span></span>

   * <span data-ttu-id="ecd30-121">A **neve**hello végpont nevét írja be vagy válassza ki a hello nevét a hello közös protokollok előre definiált végpontok.</span><span class="sxs-lookup"><span data-stu-id="ecd30-121">In **Name**, type a name for hello endpoint or select hello name from hello list of predefined endpoints for common protocols.</span></span>
   * <span data-ttu-id="ecd30-122">A **protokoll**, igény szerint, válassza ki azt, hogy a végponthoz, az TCP vagy UDP, hello típusú által igényelt hello protokollt.</span><span class="sxs-lookup"><span data-stu-id="ecd30-122">In **Protocol**, select hello protocol required by hello type of endpoint, either TCP or UDP, as needed.</span></span>
   * <span data-ttu-id="ecd30-123">A **nyilvános portot és magánhálózati Port**, írja be a használni kívánt virtuális gép toouse hello igény szerint hello portszámokat.</span><span class="sxs-lookup"><span data-stu-id="ecd30-123">In **Public Port and Private Port**, type hello port numbers that you want hello virtual machine toouse, as needed.</span></span> <span data-ttu-id="ecd30-124">Hello magánhálózati port és a tűzfalszabályok hello virtuális gép tooredirect forgalom oly módon, hogy megfelelő-e az alkalmazás használhat.</span><span class="sxs-lookup"><span data-stu-id="ecd30-124">You can use hello private port and firewall rules on hello virtual machine tooredirect traffic in a way that is appropriate for your application.</span></span> <span data-ttu-id="ecd30-125">hello magánhálózati port ugyanaz, mint a nyilvános port hello is hello.</span><span class="sxs-lookup"><span data-stu-id="ecd30-125">hello private port can be hello same as hello public port.</span></span> <span data-ttu-id="ecd30-126">Például a webes (HTTP) forgalomnak a végpont, rendelheti port 80 tooboth hello nyilvános és magánhálózati portot.</span><span class="sxs-lookup"><span data-stu-id="ecd30-126">For example, for an endpoint for web (HTTP) traffic, you could assign port 80 tooboth hello public and private port.</span></span>

5. <span data-ttu-id="ecd30-127">Válassza ki **hozzon létre egy elosztott terhelésű készletet**, majd kattintson a hello jobbra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="ecd30-127">Select **Create a load-balanced set**, and then click hello right arrow.</span></span>
6. <span data-ttu-id="ecd30-128">A hello **hello elosztott terhelésű készlet konfigurálása** lapon hello elosztott terhelésű készlet nevét, és a mintavételi viselkedését hello Azure terheléselosztó hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="ecd30-128">On hello **Configure hello load-balanced set** page, type a name for hello load-balanced set, and then assign hello values for probe behavior of hello Azure Load Balancer.</span></span> <span data-ttu-id="ecd30-129">Terheléselosztó hello mintavételt toodetermine használja, elérhető tooreceive bejövő forgalom hello elosztott terhelésű készlet hello virtuális gépek esetén.</span><span class="sxs-lookup"><span data-stu-id="ecd30-129">hello Load Balancer uses probes toodetermine if hello virtual machines in hello load-balanced set are available tooreceive incoming traffic.</span></span>
7. <span data-ttu-id="ecd30-130">Kattintson a hello pipa toocreate hello elosztott terhelésű végpont.</span><span class="sxs-lookup"><span data-stu-id="ecd30-130">Click hello check mark toocreate hello load-balanced endpoint.</span></span> <span data-ttu-id="ecd30-131">Látni fogja **Igen** a hello **elosztott terhelésű készlet nevét** hello oszlopa **végpontok** lap hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="ecd30-131">You will see **Yes** in hello **Load-balanced set name** column of hello **Endpoints** page for hello virtual machine.</span></span>
8. <span data-ttu-id="ecd30-132">Hello portálon kattintson **virtuális gépek**, kattintson egy további virtuális gép elosztott terhelésű készlet hello hello nevét, majd **végpontok**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ecd30-132">In hello portal, click **Virtual Machines**, click hello name of an additional virtual machine in hello load-balanced set, click **Endpoints**, and then click **Add**.</span></span>
9. <span data-ttu-id="ecd30-133">A hello **egy végpont tooa virtuális gép felvétele elosztott** lapján kattintson **adja hozzá a végpont tooan elosztott terhelésű készlet**, válassza ki az elosztott terhelésű készlet hello hello nevét, és kattintson a hello jobbra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="ecd30-133">On hello **Add an endpoint tooa virtual machine** page, click **Add endpoint tooan existing load-balanced set**, select hello name of hello load-balanced set, and then click hello right arrow.</span></span>
10. <span data-ttu-id="ecd30-134">A hello **adja meg a hello részleteket hello végpont** lapon, írja be a hello végpont nevét, és kattintson a pipa hello.</span><span class="sxs-lookup"><span data-stu-id="ecd30-134">On hello **Specify hello details of hello endpoint** page, type a name for hello endpoint, and then click hello check mark.</span></span>

<span data-ttu-id="ecd30-135">Hello további virtuális gépek hello elosztott terhelésű készlet esetében ismételje meg a 8 – 10.</span><span class="sxs-lookup"><span data-stu-id="ecd30-135">For hello additional virtual machines in hello load-balanced set, repeat steps 8-10.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecd30-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecd30-136">Next steps</span></span>

[<span data-ttu-id="ecd30-137">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="ecd30-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="ecd30-138">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ecd30-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="ecd30-139">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ecd30-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
