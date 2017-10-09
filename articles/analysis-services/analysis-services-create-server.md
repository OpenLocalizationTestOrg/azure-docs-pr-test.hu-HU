---
title: "egy Analysis Services-kiszolgálóhoz, az Azure-ban aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Analysis Services-kiszolgáló-példány az Azure-ban."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3668f659039f79f3dd71498d1066e8682bf33228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a><span data-ttu-id="c3c60-103">Az Azure Analysis Services-kiszolgáló létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c3c60-103">Create an Azure Analysis Services server in Azure portal</span></span>
<span data-ttu-id="c3c60-104">Ez a cikk végigvezeti egy Analysis Services-kiszolgáló erőforrás létrehozása az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c3c60-104">This article walks you through creating an Analysis Services server resource in your Azure subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c3c60-105">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c3c60-105">Before you begin</span></span>
<span data-ttu-id="c3c60-106">toocomplete a gyors üzembe helyezés szüksége:</span><span class="sxs-lookup"><span data-stu-id="c3c60-106">toocomplete this quickstart, you need:</span></span>

* <span data-ttu-id="c3c60-107">**Azure-előfizetés**: keresse fel [Azure ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate fiókkal.</span><span class="sxs-lookup"><span data-stu-id="c3c60-107">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate an account.</span></span>
* <span data-ttu-id="c3c60-108">**Az Azure Active Directory**: az előfizetéshez kell tartoznia az Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="c3c60-108">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant.</span></span> <span data-ttu-id="c3c60-109">És egy olyan fiókkal, az adott Azure Active Directoryban tooAzure bejelentkezve toobe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c3c60-109">And, you need toobe signed in tooAzure with an account in that Azure Active Directory.</span></span> <span data-ttu-id="c3c60-110">Microsoft-fiókok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="c3c60-110">Microsoft accounts are not supported.</span></span> <span data-ttu-id="c3c60-111">több, lásd: toolearn [hitelesítés és a felhasználói engedélyek](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="c3c60-111">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>
* <span data-ttu-id="c3c60-112">**Erőforráscsoport**: már van erőforráscsoport használata vagy [hozzon létre egy újat](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c3c60-112">**Resource group**: Use a resource group you already have or [create a new one](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c3c60-113">A kiszolgáló létrehozása új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="c3c60-113">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="c3c60-114">több, lásd: toolearn [Analysis Services díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="c3c60-114">toolearn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
> 
> 

## <a name="toocreate-a-server-in-azure-portal"></a><span data-ttu-id="c3c60-115">a kiszolgáló Azure-portálon toocreate</span><span class="sxs-lookup"><span data-stu-id="c3c60-115">toocreate a server in Azure portal</span></span>
1. <span data-ttu-id="c3c60-116">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c3c60-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="c3c60-117">Kattintson a **+ új** > **adatok + analitika** > **Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="c3c60-117">Click **+ New** > **Data + analytics** > **Analysis Services**.</span></span>
3. <span data-ttu-id="c3c60-118">A hello **Analysis Services** panelen hello kötelező mezők kitöltésével, és nyomja le az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c3c60-118">In hello **Analysis Services** blade, fill in hello required fields, and then press **Create**.</span></span>
   
    ![Kiszolgáló létrehozása](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * <span data-ttu-id="c3c60-120">**Kiszolgálónév**: írja be egy egyedi nevet használt tooreference hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c3c60-120">**Server name**: Type a unique name used tooreference hello server.</span></span>
   * <span data-ttu-id="c3c60-121">**Előfizetés**: válassza ki a hello előfizetés ehhez a kiszolgálóhoz való váltók stb.</span><span class="sxs-lookup"><span data-stu-id="c3c60-121">**Subscription**: Select hello subscription this server bills to.</span></span>
   * <span data-ttu-id="c3c60-122">**Erőforráscsoport**: ezek a tárolók tervezett toohelp kezelheti az Azure-erőforrások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="c3c60-122">**Resource group**: These containers are designed toohelp you manage a collection of Azure resources.</span></span> <span data-ttu-id="c3c60-123">több, lásd: toolearn [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c3c60-123">toolearn more, see [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="c3c60-124">**Hely**: az Azure datacenter hely állomások hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c3c60-124">**Location**: This Azure datacenter location hosts hello server.</span></span> <span data-ttu-id="c3c60-125">Válasszon a legnagyobb felhasználói bázis legközelebbi helyet.</span><span class="sxs-lookup"><span data-stu-id="c3c60-125">Choose a location nearest your largest user base.</span></span>
   * <span data-ttu-id="c3c60-126">**IP-címek**: tarifacsomag kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="c3c60-126">**Pricing tier**: Select a pricing tier.</span></span> <span data-ttu-id="c3c60-127">Másolatot too400 GB táblázatos modellek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="c3c60-127">Tabular models up too400 GB are supported.</span></span> <span data-ttu-id="c3c60-128">több, lásd: toolearn [Azure Analysis Services díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="c3c60-128">toolearn more, see [Azure Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
4. <span data-ttu-id="c3c60-129">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c3c60-129">Click **Create**.</span></span>

<span data-ttu-id="c3c60-130">Hozzon létre egy percet; általában tesz gyakran csak néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="c3c60-130">Create usually takes under a minute; often just a few seconds.</span></span> <span data-ttu-id="c3c60-131">Ha a kiválasztott **tooPortal hozzáadása**, keresse meg a portál toosee tooyour az új kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c3c60-131">If you selected **Add tooPortal**, navigate tooyour portal toosee your new server.</span></span> <span data-ttu-id="c3c60-132">Vagy keresse meg a túl**további szolgáltatások** > **Analysis Services** toosee, ha a kiszolgáló készen áll.</span><span class="sxs-lookup"><span data-stu-id="c3c60-132">Or, navigate too**More services** > **Analysis Services** toosee if your server is ready.</span></span>

 ![Irányítópult](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a><span data-ttu-id="c3c60-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3c60-134">Next steps</span></span>
<span data-ttu-id="c3c60-135">Miután létrehozta a kiszolgáló, [a modell rendszerbe állítása](analysis-services-deploy.md) tooit SSDT vagy ssms alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="c3c60-135">Once you've created your server, you can [deploy a model](analysis-services-deploy.md) tooit by using SSDT or with SSMS.</span></span>

<span data-ttu-id="c3c60-136">Egy modell tooyour kiszolgáló telepít tooon helyi adatforrások csatlakozik, ha szüksége van-e tooinstall egy [helyszíni adatátjáró](analysis-services-gateway.md) a hálózat egyik számítógépén.</span><span class="sxs-lookup"><span data-stu-id="c3c60-136">If a model you deploy tooyour server connects tooon-premises data sources, you need tooinstall an [On-premises data gateway](analysis-services-gateway.md) on a computer in your network.</span></span>

