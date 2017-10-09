---
title: "az Azure Analysis Services-kiszolgáló PowerShell-lel aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Azure Analysis Services-kiszolgáló PowerShell használatával"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.openlocfilehash: 269b78983410f773d47c4cea34d6d353b19f9e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a><span data-ttu-id="bf21d-103">Azure Analysis Services-kiszolgáló létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="bf21d-103">Create an Azure Analysis Services server by using PowerShell</span></span>

<span data-ttu-id="bf21d-104">A gyors üzembe helyezés ismerteti a PowerShell használatával a hello parancssori toocreate az Azure Analysis Services-kiszolgáló a egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="bf21d-104">This quickstart describes using PowerShell from hello command line toocreate an Azure Analysis Services server in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in your Azure subscription.</span></span>

<span data-ttu-id="bf21d-105">A feladathoz az Azure PowerShell-modul 4.0-s vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="bf21d-105">This task requires Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="bf21d-106">toofind hello verzió, futtassa ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="bf21d-106">toofind hello version, run ` Get-Module -ListAvailable AzureRM`.</span></span> <span data-ttu-id="bf21d-107">tooinstall vagy frissítés, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="bf21d-107">tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

> [!NOTE]
> <span data-ttu-id="bf21d-108">A kiszolgáló létrehozása új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="bf21d-108">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="bf21d-109">több, lásd: toolearn [Analysis Services díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="bf21d-109">toolearn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf21d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bf21d-110">Prerequisites</span></span>
<span data-ttu-id="bf21d-111">toocomplete a gyors üzembe helyezés szüksége:</span><span class="sxs-lookup"><span data-stu-id="bf21d-111">toocomplete this quickstart, you need:</span></span>

* <span data-ttu-id="bf21d-112">**Azure-előfizetés**: keresse fel [Azure ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate fiókkal.</span><span class="sxs-lookup"><span data-stu-id="bf21d-112">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate an account.</span></span>
* <span data-ttu-id="bf21d-113">**Azure Active Directory**: Előfizetésének egy Azure Active Directory-bérlőhöz kell tartoznia, és abban a könyvtárban kell fiókkal rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="bf21d-113">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant and you must have an account in that directory.</span></span> <span data-ttu-id="bf21d-114">több, lásd: toolearn [hitelesítés és a felhasználói engedélyek](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="bf21d-114">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>

## <a name="import-azurermanalysisservices-module"></a><span data-ttu-id="bf21d-115">Az AzureRm.AnalysisServices modul importálása</span><span class="sxs-lookup"><span data-stu-id="bf21d-115">Import AzureRm.AnalysisServices module</span></span>
<span data-ttu-id="bf21d-116">toocreate egy kiszolgálóhoz az előfizetését, használhat hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) összetevő modul.</span><span class="sxs-lookup"><span data-stu-id="bf21d-116">toocreate a server in your subscription, you use hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)  component module.</span></span> <span data-ttu-id="bf21d-117">Hello AzureRm.AnalysisServices modul betölteni a PowerShell-munkamenetbe.</span><span class="sxs-lookup"><span data-stu-id="bf21d-117">Load hello AzureRm.AnalysisServices module into your PowerShell session.</span></span>

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-tooazure"></a><span data-ttu-id="bf21d-118">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="bf21d-118">Sign in tooAzure</span></span>

<span data-ttu-id="bf21d-119">Jelentkezzen be Azure-előfizetés tooyour hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) parancsot.</span><span class="sxs-lookup"><span data-stu-id="bf21d-119">Sign in tooyour Azure subscription by using hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command.</span></span> <span data-ttu-id="bf21d-120">Kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="bf21d-120">Follow hello on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="bf21d-121">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="bf21d-121">Create a resource group</span></span>
 
<span data-ttu-id="bf21d-122">Az [Azure-erőforráscsoport](../azure-resource-manager/resource-group-overview.md) egy olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="bf21d-122">An [Azure resource group](../azure-resource-manager/resource-group-overview.md) is a logical container where Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="bf21d-123">A kiszolgáló létrehozásakor meg kell adnia egy erőforráscsoportot az előfizetésén belül.</span><span class="sxs-lookup"><span data-stu-id="bf21d-123">When you create your server, you must specify a resource group in your subscription.</span></span> <span data-ttu-id="bf21d-124">Ha még nem rendelkezik egy erőforráscsoport, létrehozhat egy új hello segítségével [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsot.</span><span class="sxs-lookup"><span data-stu-id="bf21d-124">If you do not already have a resource group, you can create a new one by using hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="bf21d-125">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` hello USA nyugati régiója régióban.</span><span class="sxs-lookup"><span data-stu-id="bf21d-125">hello following example creates a resource group named `myResourceGroup` in hello West US region.</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a><span data-ttu-id="bf21d-126">A kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf21d-126">Create a server</span></span>

<span data-ttu-id="bf21d-127">Hozzon létre egy új kiszolgálót hello segítségével [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) parancsot.</span><span class="sxs-lookup"><span data-stu-id="bf21d-127">Create a new server by using hello [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="bf21d-128">hello alábbi példa létrehoz egy nevű myResourceGroup hello USA nyugati régiója régióban, hello D1 réteg a myServer kiszolgáló, és határozza meg philipc@adventureworks.com server rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bf21d-128">hello following example creates a server named myServer in myResourceGroup, in hello West US region, at hello D1 tier, and specifies philipc@adventureworks.com as a server administrator.</span></span>

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a><span data-ttu-id="bf21d-129">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="bf21d-129">Clean up resources</span></span>

<span data-ttu-id="bf21d-130">Eltávolíthatja hello server előfizetésből hello segítségével [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) parancsot.</span><span class="sxs-lookup"><span data-stu-id="bf21d-130">You can remove hello server from your subscription by using hello [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="bf21d-131">Ha a gyűjteménybe tartozó további rövid útmutatókkal és oktatóanyagokkal szeretné folytatni, ne távolítsa el a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="bf21d-131">If you continue with other quickstarts and tutorials in this collection, do not remove your server.</span></span> <span data-ttu-id="bf21d-132">hello következő példában eltávolítjuk hello előző lépésben létrehozott hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="bf21d-132">hello following example removes hello server created in hello previous step.</span></span>


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="bf21d-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf21d-133">Next steps</span></span>
<span data-ttu-id="bf21d-134">[Az Azure Analysis Services kezelése a PowerShell-lel](analysis-services-powershell.md) </span><span class="sxs-lookup"><span data-stu-id="bf21d-134">[Manage Azure Analysis Services with PowerShell](analysis-services-powershell.md) </span></span>  
<span data-ttu-id="bf21d-135">[Modell üzembe helyezése SSDT-ről](analysis-services-deploy.md) </span><span class="sxs-lookup"><span data-stu-id="bf21d-135">[Deploy a model from SSDT](analysis-services-deploy.md) </span></span>  
[<span data-ttu-id="bf21d-136">Modell létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="bf21d-136">Create a model in Azure portal</span></span>](analysis-services-create-model-portal.md)