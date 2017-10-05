---
title: "Egy Analysis Services-kiszolgáló létrehozása az Azure-ban |} Microsoft Docs"
description: "Útmutató: az Analysis Services server-példány létrehozása az Azure-ban."
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
ms.openlocfilehash: 95b367e7cd74405088190c1fe19cf92990759d90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a><span data-ttu-id="5fa2a-103">Az Azure Analysis Services-kiszolgáló létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5fa2a-103">Create an Azure Analysis Services server in Azure portal</span></span>
<span data-ttu-id="5fa2a-104">Ez a cikk végigvezeti egy Analysis Services-kiszolgáló erőforrás létrehozása az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-104">This article walks you through creating an Analysis Services server resource in your Azure subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5fa2a-105">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5fa2a-105">Before you begin</span></span>
<span data-ttu-id="5fa2a-106">A rövid útmutató elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="5fa2a-106">To complete this quickstart, you need:</span></span>

* <span data-ttu-id="5fa2a-107">**Azure-előfizetés**: A fiók létrehozásával kapcsolatban lásd: [Ingyenes Azure-próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p/).</span><span class="sxs-lookup"><span data-stu-id="5fa2a-107">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) to create an account.</span></span>
* <span data-ttu-id="5fa2a-108">**Az Azure Active Directory**: az előfizetéshez kell tartoznia az Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-108">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant.</span></span> <span data-ttu-id="5fa2a-109">És meg kell jelentkeznie az Azure-bA egy olyan fiókkal, az adott Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-109">And, you need to be signed in to Azure with an account in that Azure Active Directory.</span></span> <span data-ttu-id="5fa2a-110">Microsoft-fiókok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-110">Microsoft accounts are not supported.</span></span> <span data-ttu-id="5fa2a-111">További információ: [Hitelesítés és felhasználói engedélyek](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="5fa2a-111">To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>
* <span data-ttu-id="5fa2a-112">**Erőforráscsoport**: már van erőforráscsoport használata vagy [hozzon létre egy újat](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5fa2a-112">**Resource group**: Use a resource group you already have or [create a new one](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5fa2a-113">A kiszolgáló létrehozása új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-113">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="5fa2a-114">További tudnivalókért lásd: [Analysis Services-díjszabás](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="5fa2a-114">To learn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
> 
> 

## <a name="to-create-a-server-in-azure-portal"></a><span data-ttu-id="5fa2a-115">A kiszolgáló létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5fa2a-115">To create a server in Azure portal</span></span>
1. <span data-ttu-id="5fa2a-116">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5fa2a-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="5fa2a-117">Kattintson a **+ új** > **adatok + analitika** > **Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-117">Click **+ New** > **Data + analytics** > **Analysis Services**.</span></span>
3. <span data-ttu-id="5fa2a-118">Az a **Analysis Services** panelen adja meg a kötelező mezőket, és nyomja le az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-118">In the **Analysis Services** blade, fill in the required fields, and then press **Create**.</span></span>
   
    ![Kiszolgáló létrehozása](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * <span data-ttu-id="5fa2a-120">**Kiszolgálónév**: Adjon meg egy egyedi nevet mutató hivatkozás a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-120">**Server name**: Type a unique name used to reference the server.</span></span>
   * <span data-ttu-id="5fa2a-121">**Előfizetés**: Jelölje ki az ehhez a kiszolgálóhoz való váltók stb.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-121">**Subscription**: Select the subscription this server bills to.</span></span>
   * <span data-ttu-id="5fa2a-122">**Erőforráscsoport**: ezek a tárolók úgy tervezték, hogy az Azure erőforrások kezeléséhez nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-122">**Resource group**: These containers are designed to help you manage a collection of Azure resources.</span></span> <span data-ttu-id="5fa2a-123">További tudnivalókért lásd: [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5fa2a-123">To learn more, see [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="5fa2a-124">**Hely**: az Azure-adatközpont hely üzemelteti a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-124">**Location**: This Azure datacenter location hosts the server.</span></span> <span data-ttu-id="5fa2a-125">Válasszon a legnagyobb felhasználói bázis legközelebbi helyet.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-125">Choose a location nearest your largest user base.</span></span>
   * <span data-ttu-id="5fa2a-126">**IP-címek**: tarifacsomag kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-126">**Pricing tier**: Select a pricing tier.</span></span> <span data-ttu-id="5fa2a-127">Táblázatos modellek legfeljebb 400 GB támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-127">Tabular models up to 400 GB are supported.</span></span> <span data-ttu-id="5fa2a-128">További tudnivalókért lásd: [Azure Analysis Services díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="5fa2a-128">To learn more, see [Azure Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
4. <span data-ttu-id="5fa2a-129">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-129">Click **Create**.</span></span>

<span data-ttu-id="5fa2a-130">Hozzon létre egy percet; általában tesz gyakran csak néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-130">Create usually takes under a minute; often just a few seconds.</span></span> <span data-ttu-id="5fa2a-131">Ha a kiválasztott **portálon**, keresse meg a portálhoz, és az új kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-131">If you selected **Add to Portal**, navigate to your portal to see your new server.</span></span> <span data-ttu-id="5fa2a-132">Vagy keresse meg **további szolgáltatások** > **Analysis Services** megjelenítéséhez, ha a kiszolgáló készen áll-e.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-132">Or, navigate to **More services** > **Analysis Services** to see if your server is ready.</span></span>

 ![Irányítópult](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a><span data-ttu-id="5fa2a-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5fa2a-134">Next steps</span></span>
<span data-ttu-id="5fa2a-135">Miután létrehozta a kiszolgáló, [a modell rendszerbe állítása](analysis-services-deploy.md) hozzá SSDT vagy ssms alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-135">Once you've created your server, you can [deploy a model](analysis-services-deploy.md) to it by using SSDT or with SSMS.</span></span>

<span data-ttu-id="5fa2a-136">A modell központi telepítése a kiszolgálóra a helyi adatforrások csatlakozik, ha telepíteni szeretné egy [helyszíni adatátjáró](analysis-services-gateway.md) a hálózat egyik számítógépén.</span><span class="sxs-lookup"><span data-stu-id="5fa2a-136">If a model you deploy to your server connects to on-premises data sources, you need to install an [On-premises data gateway](analysis-services-gateway.md) on a computer in your network.</span></span>

