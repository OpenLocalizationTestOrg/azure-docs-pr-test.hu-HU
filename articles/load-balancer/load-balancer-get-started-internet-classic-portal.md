---
title: "Internetkapcsolattal rendelkező terheléselosztó létrehozása – klasszikus Azure Portal | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó klasszikus üzembehelyezési modellel a klasszikus Azure portál használatával"
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
ms.openlocfilehash: a022154f5eca6de2d2dbfc1b9aa30d2ea0a7d650
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a><span data-ttu-id="5e498-103">Bevezetés az internetkapcsolattal rendelkező terheléselosztó (klasszikus) létrehozásába a klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="5e498-103">Get started creating an Internet facing load balancer (classic) in the Azure classic portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e498-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="5e498-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="5e498-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e498-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="5e498-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5e498-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="5e498-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="5e498-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="5e498-108">Az Azure-erőforrásokkal való munka megkezdése előtt fontos megérteni, hogy az Azure jelenleg két üzembe helyezési modellel rendelkezik, a Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="5e498-108">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="5e498-109">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="5e498-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="5e498-110">A különféle eszközök dokumentációit a cikk tetején található fülekre kattintva tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="5e498-110">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span> <span data-ttu-id="5e498-111">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5e498-111">This article covers the classic deployment model.</span></span> <span data-ttu-id="5e498-112">Emellett [azt is megismerheti, hogyan lehet internetkapcsolattal rendelkező terheléselosztót létrehozni az Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5e498-112">You can also [Learn how to create an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a><span data-ttu-id="5e498-113">Internetkapcsolattal rendelkező terheléselosztó beállítása virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="5e498-113">Set up an Internet-facing load balancer for virtual machines</span></span>

<span data-ttu-id="5e498-114">Az internetről érkező hálózati forgalom terheléselosztásának megvalósításához a felhőszolgáltatás virtuális gépei között, létre kell hoznia egy elosztott terhelésű készletet.</span><span class="sxs-lookup"><span data-stu-id="5e498-114">In order to load balance network traffic from the Internet across the virtual machines of a cloud service, you must create a load-balanced set.</span></span> <span data-ttu-id="5e498-115">Ez az eljárás feltételezi, hogy már létrehozta a virtuális gépeket, és az összes ugyanazon a felhőszolgáltatáson belül van.</span><span class="sxs-lookup"><span data-stu-id="5e498-115">This procedure assumes that you have already created the virtual machines and that they are all within the same cloud service.</span></span>

<span data-ttu-id="5e498-116">**Virtuális gépek elosztott terhelésű készletének konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="5e498-116">**To configure a load-balanced set for virtual machines**</span></span>

1. <span data-ttu-id="5e498-117">A klasszikus Azure portálon kattintson a **Virtuális gépek** lehetőségre, majd kattintson a virtuális gép nevére az elosztott terhelésű készletben.</span><span class="sxs-lookup"><span data-stu-id="5e498-117">In the Azure classic portal, click **Virtual Machines**, and then click the name of a virtual machine in the load-balanced set.</span></span>
2. <span data-ttu-id="5e498-118">Kattintson a **Végpontok**, majd a **Hozzáadás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="5e498-118">Click **Endpoints**, and then click **Add**.</span></span>
3. <span data-ttu-id="5e498-119">A **Végpont hozzáadása egy virtuális géphez** lapon kattintson a jobbra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="5e498-119">On the **Add an endpoint to a virtual machine** page, click the right arrow.</span></span>
4. <span data-ttu-id="5e498-120">Az **Adja meg a végpont adatait** lapon:</span><span class="sxs-lookup"><span data-stu-id="5e498-120">On the **Specify the details of the endpoint** page:</span></span>

   * <span data-ttu-id="5e498-121">A **Név** mezőbe írjon be egy nevet a végpont számára, vagy válassza ki a nevet a közös protokollok előre definiált végpontjainak listáján.</span><span class="sxs-lookup"><span data-stu-id="5e498-121">In **Name**, type a name for the endpoint or select the name from the list of predefined endpoints for common protocols.</span></span>
   * <span data-ttu-id="5e498-122">A **Protokoll** részben szükség szerint válassza ki a végpont típusa által megkövetelt protokollt (TCP vagy UDP).</span><span class="sxs-lookup"><span data-stu-id="5e498-122">In **Protocol**, select the protocol required by the type of endpoint, either TCP or UDP, as needed.</span></span>
   * <span data-ttu-id="5e498-123">A **Nyilvános port és magánhálózati port** részbe írja be azokat a portszámokat, amelyeket a virtuális gépnek kell használnia.</span><span class="sxs-lookup"><span data-stu-id="5e498-123">In **Public Port and Private Port**, type the port numbers that you want the virtual machine to use, as needed.</span></span> <span data-ttu-id="5e498-124">A magánhálózati portot és a tűzfalszabályokat a virtuális gépen a forgalomnak az alkalmazása számára megfelelő átirányítására használhatja.</span><span class="sxs-lookup"><span data-stu-id="5e498-124">You can use the private port and firewall rules on the virtual machine to redirect traffic in a way that is appropriate for your application.</span></span> <span data-ttu-id="5e498-125">A magánhálózati port és a nyilvános port azonos lehet.</span><span class="sxs-lookup"><span data-stu-id="5e498-125">The private port can be the same as the public port.</span></span> <span data-ttu-id="5e498-126">A webes (HTTP) forgalom egyik végpontjához például hozzárendelhető a 80-as port mind nyilvános, mind magánhálózati portként.</span><span class="sxs-lookup"><span data-stu-id="5e498-126">For example, for an endpoint for web (HTTP) traffic, you could assign port 80 to both the public and private port.</span></span>

5. <span data-ttu-id="5e498-127">Válassza ki az **Elosztott terhelésű készlet létrehozása** lehetőséget, majd kattintson a jobbra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="5e498-127">Select **Create a load-balanced set**, and then click the right arrow.</span></span>
6. <span data-ttu-id="5e498-128">Az **Elosztott terhelésű készlet konfigurálása** lapon írja be az elosztott terhelésű készlet nevét, majd rendelje hozzá az Azure Load Balancer mintavételi viselkedésének értékeit.</span><span class="sxs-lookup"><span data-stu-id="5e498-128">On the **Configure the load-balanced set** page, type a name for the load-balanced set, and then assign the values for probe behavior of the Azure Load Balancer.</span></span> <span data-ttu-id="5e498-129">A terheléselosztó a mintavételezők használatával határozza meg, hogy az elosztott terhelésű készlet virtuális gépei rendelkezésre állnak-e bejövő forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="5e498-129">The Load Balancer uses probes to determine if the virtual machines in the load-balanced set are available to receive incoming traffic.</span></span>
7. <span data-ttu-id="5e498-130">Kattintson a pipa jelre az elosztott terhelésű végpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5e498-130">Click the check mark to create the load-balanced endpoint.</span></span> <span data-ttu-id="5e498-131">Az **Igen** feliratot fogja látni a virtuális gép **Végpontok** lapjának **Elosztott terhelésű készlet neve** oszlopában.</span><span class="sxs-lookup"><span data-stu-id="5e498-131">You will see **Yes** in the **Load-balanced set name** column of the **Endpoints** page for the virtual machine.</span></span>
8. <span data-ttu-id="5e498-132">A portálon kattintson a **Virtuális gépek** lehetőségre, majd kattintson egy további virtuális gép nevére az elosztott terhelésű készletben, kattintson a **Végpontok**, majd a **Hozzáadás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="5e498-132">In the portal, click **Virtual Machines**, click the name of an additional virtual machine in the load-balanced set, click **Endpoints**, and then click **Add**.</span></span>
9. <span data-ttu-id="5e498-133">A **Végpont hozzáadása egy virtuális géphez** lapon kattintson a **Végpont hozzáadása egy meglévő elosztott terhelésű készlethez** lehetőségre, válassza ki az elosztott terhelésű készlet nevét, majd kattintson a jobbra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="5e498-133">On the **Add an endpoint to a virtual machine** page, click **Add endpoint to an existing load-balanced set**, select the name of the load-balanced set, and then click the right arrow.</span></span>
10. <span data-ttu-id="5e498-134">Az **Adja meg a végpont adatait** lapon írjon be egy nevet a végpont számára, majd kattintson a pipa jelre.</span><span class="sxs-lookup"><span data-stu-id="5e498-134">On the **Specify the details of the endpoint** page, type a name for the endpoint, and then click the check mark.</span></span>

<span data-ttu-id="5e498-135">Az elosztott terhelésű készlet további virtuális gépeinek esetében ismételje meg a 8–10. lépést.</span><span class="sxs-lookup"><span data-stu-id="5e498-135">For the additional virtual machines in the load-balanced set, repeat steps 8-10.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e498-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e498-136">Next steps</span></span>

[<span data-ttu-id="5e498-137">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="5e498-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="5e498-138">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e498-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="5e498-139">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e498-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
