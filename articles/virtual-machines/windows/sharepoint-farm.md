---
title: "SharePoint kiszolgálófarmok aaaCreate az Azure-ban |} Microsoft Docs"
description: "Gyorsan létrehozhat egy új SharePoint 2013-at vagy a SharePoint 2016-farm az Azure-ban az Azure portál piactér hello."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d71c7177d9b99cf377bab767342508007285b452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-sharepoint-server-farms-using-hello-azure-portal-marketplace"></a><span data-ttu-id="0c3f9-103">SharePoint használata az Azure portál piactér hello kiszolgálófarmok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0c3f9-103">Create SharePoint server farms using hello Azure portal marketplace</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a><span data-ttu-id="0c3f9-104">A SharePoint 2013-farmok</span><span class="sxs-lookup"><span data-stu-id="0c3f9-104">SharePoint 2013 farms</span></span>
<span data-ttu-id="0c3f9-105">A Microsoft Azure portál Piactéri hello gyorsan létre tud hozni előre beállított a SharePoint Server 2013-farmok.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-105">With hello Microsoft Azure portal marketplace, you can quickly create pre-configured SharePoint Server 2013 farms.</span></span> <span data-ttu-id="0c3f9-106">Ez takaríthat meg sok időt Ha alapszintű vagy magas rendelkezésre állású SharePoint-farm fejlesztési/tesztelési környezetben kell, vagy ha éppen kipróbálja a SharePoint Server 2013 együttműködés megoldásként a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-106">This can save you a lot of time when you need a basic or high-availability SharePoint farm for a dev/test environment or if you are evaluating SharePoint Server 2013 as a collaboration solution for your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="0c3f9-107">Hello **SharePoint-kiszolgálófarm** hello Azure-portálon az Azure piactér hello elem el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-107">hello **SharePoint Server Farm** item in hello Azure Marketplace of hello Azure portal has been removed.</span></span> <span data-ttu-id="0c3f9-108">Helyette a hello **a SharePoint 2013 nem magas rendelkezésre ÁLLÁSÚ Farm** és **SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** elemeket.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-108">It has been replaced with hello **SharePoint 2013 non-HA Farm** and **SharePoint 2013 HA Farm** items.</span></span>
>
>

<span data-ttu-id="0c3f9-109">Alapszintű hello SharePoint-farm ebben a konfigurációban a három virtuális gépet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-109">hello basic SharePoint farm consists of three virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

<span data-ttu-id="0c3f9-111">A farm konfigurációja használhat egy egyszerűbb beállítása a SharePoint-alkalmazások fejlesztéséhez vagy a SharePoint 2013 első értékelését.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-111">You can use this farm configuration for a simplified setup for SharePoint app development or your first-time evaluation of SharePoint 2013.</span></span>

<span data-ttu-id="0c3f9-112">toocreate hello alapvető (Háromkiszolgálós) SharePoint-farm:</span><span class="sxs-lookup"><span data-stu-id="0c3f9-112">toocreate hello basic (three-server) SharePoint farm:</span></span>

1. <span data-ttu-id="0c3f9-113">Kattintson a [Itt](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span><span class="sxs-lookup"><span data-stu-id="0c3f9-113">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span></span>
2. <span data-ttu-id="0c3f9-114">Kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-114">Click **Deploy**.</span></span>
3. <span data-ttu-id="0c3f9-115">A hello **a SharePoint 2013 nem magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-115">On hello **SharePoint 2013 non-HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="0c3f9-116">Adja meg a beállításokat a hello hello lépésein **létrehozása a SharePoint 2013-as nem magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-116">Specify settings on hello steps of hello **Create SharePoint 2013 non-HA Farm** pane, and then click **Create**.</span></span>

<span data-ttu-id="0c3f9-117">Ebben a konfigurációban kilenc virtuális gépek magas rendelkezésre állású SharePoint-farm hello áll.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-117">hello high-availability SharePoint farm consists of nine virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

<span data-ttu-id="0c3f9-119">Használhatja a farm konfigurációs tootest magasabb ügyfél terhelés, magas rendelkezésre állású hello külső SharePoint-webhely, az SQL Server AlwaysOn rendelkezésre állási csoportok SharePoint-farm.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-119">You can use this farm configuration tootest higher client loads, high availability of hello external SharePoint site, and SQL Server AlwaysOn Availability Groups for a SharePoint farm.</span></span> <span data-ttu-id="0c3f9-120">A SharePoint-alkalmazások fejlesztéséhez a magas rendelkezésre állású környezetekben is használhatja ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-120">You can also use this configuration for SharePoint app development in a high-availability environment.</span></span>

<span data-ttu-id="0c3f9-121">toocreate hello magas rendelkezésre állású (kilenc-kiszolgáló) SharePoint-farm:</span><span class="sxs-lookup"><span data-stu-id="0c3f9-121">toocreate hello high-availability (nine-server) SharePoint farm:</span></span>

1. <span data-ttu-id="0c3f9-122">Kattintson a [Itt](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span><span class="sxs-lookup"><span data-stu-id="0c3f9-122">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span></span>
2. <span data-ttu-id="0c3f9-123">Kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-123">Click **Deploy**.</span></span>
3. <span data-ttu-id="0c3f9-124">A hello **SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-124">On hello **SharePoint 2013 HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="0c3f9-125">Beállításainak megadása az hello hello hét lépésein **létrehozása a SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-125">Specify settings on hello seven steps of hello **Create SharePoint 2013 HA Farm** pane, and then click **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="0c3f9-126">Nem hozható létre hello **a SharePoint 2013 nem magas rendelkezésre ÁLLÁSÚ Farm** vagy **SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** egy Azure ingyenes próbaverziót.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-126">You cannot create hello **SharePoint 2013 non-HA Farm** or **SharePoint 2013 HA Farm** with an Azure Free Trial.</span></span>
>
>

<span data-ttu-id="0c3f9-127">hello Azure-portálon létrehoz mindkét gazdaság az Internet felé néző webes jelenlét csak felhőalapú virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-127">hello Azure portal creates both of these farms in a cloud-only virtual network with an Internet-facing web presence.</span></span> <span data-ttu-id="0c3f9-128">Nincs pont-pont VPN- vagy ExpressRoute kapcsolat hátsó tooyour szervezeti hálózat.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-128">There is no site-to-site VPN or ExpressRoute connection back tooyour organization network.</span></span>

> [!NOTE]
> <span data-ttu-id="0c3f9-129">Ha alapszintű létrehozása hello, vagy a magas rendelkezésre állású SharePoint-farmok hello Azure-portál használatával, nem adható meg egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-129">When you create hello basic or high-availability SharePoint farms using hello Azure portal, you cannot specify an existing resource group.</span></span> <span data-ttu-id="0c3f9-130">a korlátozás toowork gazdaság létrehozása az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-130">toowork around this limitation, create these farms with Azure PowerShell.</span></span> <span data-ttu-id="0c3f9-131">További információkért lásd: [létrehozása a SharePoint 2013-as fejlesztési és tesztelési célú farmok az Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span><span class="sxs-lookup"><span data-stu-id="0c3f9-131">For more information, see [Create SharePoint 2013 dev/test farms with Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span></span>
>
>

## <a name="sharepoint-2016-farms"></a><span data-ttu-id="0c3f9-132">A SharePoint 2016-farmok</span><span class="sxs-lookup"><span data-stu-id="0c3f9-132">SharePoint 2016 farms</span></span>
<span data-ttu-id="0c3f9-133">Lásd: [Ez a cikk](https://technet.microsoft.com/library/mt723354.aspx) hello utasításokat toobuild hello egykiszolgálós SharePoint Server 2016 következő farm.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-133">See [this article](https://technet.microsoft.com/library/mt723354.aspx) for hello instructions toobuild hello following single-server SharePoint Server 2016 farm.</span></span>

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-hello-sharepoint-farms"></a><span data-ttu-id="0c3f9-135">SharePoint-farmok hello kezelése</span><span class="sxs-lookup"><span data-stu-id="0c3f9-135">Managing hello SharePoint farms</span></span>
<span data-ttu-id="0c3f9-136">A távoli asztali kapcsolaton keresztül a gazdaság hello-kiszolgálókat is felügyelhet.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-136">You can administer hello servers of these farms through Remote Desktop connections.</span></span> <span data-ttu-id="0c3f9-137">További információkért lásd: [jelentkezzen be a virtuális gép toohello](quick-create-portal.md#connect-to-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="0c3f9-137">For more information, see [Log on toohello virtual machine](quick-create-portal.md#connect-to-virtual-machine).</span></span>

<span data-ttu-id="0c3f9-138">Hello a SharePoint központi felügyeleti hely konfigurálhatja a helyeket, SharePoint-alkalmazások és egyéb funkciókat.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-138">From hello Central Administration SharePoint site, you can configure My sites, SharePoint applications, and other functionality.</span></span> <span data-ttu-id="0c3f9-139">További információkért lásd: [SharePoint konfigurálása](http://technet.microsoft.com/library/ee836142.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c3f9-139">For more information, see [Configure SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c3f9-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c3f9-140">Next steps</span></span>
* <span data-ttu-id="0c3f9-141">Fedezze fel további [SharePoint konfigurációk](https://technet.microsoft.com/library/dn635309.aspx) az Azure infrastruktúra-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="0c3f9-141">Discover additional [SharePoint configurations](https://technet.microsoft.com/library/dn635309.aspx) in Azure infrastructure services.</span></span>
