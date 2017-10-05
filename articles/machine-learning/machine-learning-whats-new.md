---
title: What's new in Azure Machine Learning |} Microsoft Docs
description: "Az Azure Machine Learning elérhető új szolgáltatások."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 551b977b90612ddbfa1514a9c2358ebf8179c385
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-azure-machine-learning"></a><span data-ttu-id="11e9e-103">Az Azure Machine Learning újdonságai</span><span class="sxs-lookup"><span data-stu-id="11e9e-103">What's New in Azure Machine Learning</span></span>

### <a name="the-march-2017-release-of-microsoft-azure-machine-learning-updates-provides-the-following-feature"></a><span data-ttu-id="11e9e-104">A Microsoft Azure Machine Learning frissítések március 2017 kiadása a következő funkcióval rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="11e9e-104">The March 2017 release of Microsoft Azure Machine Learning updates provides the following feature:</span></span>



* <span data-ttu-id="11e9e-105">Az Azure Machine Learning BES feladatok dedikált kapacitás</span><span class="sxs-lookup"><span data-stu-id="11e9e-105">Dedicated Capacity for Azure Machine Learning BES Jobs</span></span>

    <span data-ttu-id="11e9e-106">Számítógép által használt feldolgozása tanulási Batch-készlet a [Azure Batch](../batch/batch-technical-overview.md) ügyfél által felügyelt méretezési biztosít az Azure Machine Learning kötegelt végrehajtási szolgáltatás szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="11e9e-106">Machine Learning Batch Pool processing uses the [Azure Batch](../batch/batch-technical-overview.md) service to provide customer-managed scale for the Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="11e9e-107">Készlet kötegfeldolgozási hozhat létre Azure Batch készletek amelyen kötegelt feladatok elküldéséhez és azok kiszámítható módon hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="11e9e-107">Batch Pool processing allows you to create Azure Batch pools on which you can submit batch jobs and have them execute in a predictable manner.</span></span>

    <span data-ttu-id="11e9e-108">További információkért lásd: [Azure Batch szolgáltatás gépi tanulási feladatok](machine-learning-dedicated-capacity-for-bes-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="11e9e-108">For more information, see [Azure Batch service for Machine Learning jobs](machine-learning-dedicated-capacity-for-bes-jobs.md).</span></span>


### <a name="the-august-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a><span data-ttu-id="11e9e-109">A Microsoft Azure Machine Learning frissítések 2016 augusztusától kiadás az alábbi szolgáltatásokat biztosítja:</span><span class="sxs-lookup"><span data-stu-id="11e9e-109">The August 2016 release of Microsoft Azure Machine Learning updates provide the following features:</span></span>
* <span data-ttu-id="11e9e-110">Klasszikus webszolgáltatások mostantól kezelhető az új [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/) portál, amely egy helyen kezelhetők a webes szolgáltatás minden elemét.</span><span class="sxs-lookup"><span data-stu-id="11e9e-110">Classic Web services can now be managed in the new [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/) portal that provides one place to manage all aspects of your Web service.</span></span>    
  * <span data-ttu-id="11e9e-111">Webszolgáltatás biztosító [kihasználtságának statisztikai adatai](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="11e9e-111">Which provides web service [usage statistics](machine-learning-manage-new-webservice.md).</span></span>
  * <span data-ttu-id="11e9e-112">Leegyszerűsíti az Azure Machine Learning távoli-kérelem hívásokon mintaadatok vizsgálatát.</span><span class="sxs-lookup"><span data-stu-id="11e9e-112">Simplifies testing of Azure Machine Learning Remote-Request calls using sample data.</span></span>
  * <span data-ttu-id="11e9e-113">Új kötegelt végrehajtási szolgáltatás tesztoldalt biztosít a minta adatok és a feladat elküldése előzményeit.</span><span class="sxs-lookup"><span data-stu-id="11e9e-113">Provides a new Batch Execution Service test page with sample data and job submission history.</span></span>
  * <span data-ttu-id="11e9e-114">Egyszerűbb végpont-felügyeletet biztosít.</span><span class="sxs-lookup"><span data-stu-id="11e9e-114">Provides easier endpoint management.</span></span>

### <a name="the-july-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a><span data-ttu-id="11e9e-115">A Microsoft Azure Machine Learning frissítések 2016. július kiadásában az alábbi szolgáltatásokat biztosítja:</span><span class="sxs-lookup"><span data-stu-id="11e9e-115">The July 2016 release of Microsoft Azure Machine Learning updates provide the following features:</span></span>
* <span data-ttu-id="11e9e-116">Webszolgáltatások most már felügyelt, Azure-erőforrások kezelhetők [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) felületek, ami a következő fejlesztéseket:</span><span class="sxs-lookup"><span data-stu-id="11e9e-116">Web services are now managed as Azure resources managed through [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) interfaces, allowing for the following enhancements:</span></span>
  * <span data-ttu-id="11e9e-117">Új [REST API-k](https://msdn.microsoft.com/library/azure/Dn950030.aspx) telepítéséhez és kezeléséhez a Resource Manager-alapú webes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="11e9e-117">There are new [REST APIs](https://msdn.microsoft.com/library/azure/Dn950030.aspx) to deploy and manage your Resource Manager based Web services.</span></span>
  * <span data-ttu-id="11e9e-118">Új [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/) portál, amely egy helyen kezelhetők a webes szolgáltatás minden elemét.</span><span class="sxs-lookup"><span data-stu-id="11e9e-118">There is a new [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/) portal that provides one place to manage all aspects of your Web service.</span></span>
* <span data-ttu-id="11e9e-119">A szerződés magában foglalja egy új előfizetés-alapú, több területi webes szolgáltatás telepítési modell erőforrás-kezelő használatával az erőforrás-kezelő erőforrás-szolgáltató kihasználva webszolgáltatások API-k alapján.</span><span class="sxs-lookup"><span data-stu-id="11e9e-119">Incorporates a new subscription-based, multi-region web service deployment model using Resource Manager based APIs leveraging the Resource Manager Resource Provider for Web Services.</span></span>
* <span data-ttu-id="11e9e-120">Bevezeti az új [díjszabások](https://azure.microsoft.com/pricing/details/machine-learning/) tervezze meg az új erőforrás-kezelő RP használ számlázási felügyeleti képességek.</span><span class="sxs-lookup"><span data-stu-id="11e9e-120">Introduces new [pricing plans](https://azure.microsoft.com/pricing/details/machine-learning/) and plan management capabilities using the new Resource Manager RP for Billing.</span></span>
  * <span data-ttu-id="11e9e-121">Most [a webszolgáltatás telepítése több régióba](machine-learning-how-to-deploy-to-multiple-regions.md) anélkül, hogy minden régióban egy előfizetés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="11e9e-121">You can now [deploy your web service to multiple regions](machine-learning-how-to-deploy-to-multiple-regions.md) without needing to create a subscription in each region.</span></span>
* <span data-ttu-id="11e9e-122">Webszolgáltatás biztosít [kihasználtságának statisztikai adatai](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="11e9e-122">Provides web service [usage statistics](machine-learning-manage-new-webservice.md).</span></span>
* <span data-ttu-id="11e9e-123">Leegyszerűsíti az Azure Machine Learning távoli-kérelem hívásokon mintaadatok vizsgálatát.</span><span class="sxs-lookup"><span data-stu-id="11e9e-123">Simplifies testing of Azure Machine Learning Remote-Request calls using sample data.</span></span>
* <span data-ttu-id="11e9e-124">Új kötegelt végrehajtási szolgáltatás tesztoldalt biztosít a minta adatok és a feladat elküldése előzményeit.</span><span class="sxs-lookup"><span data-stu-id="11e9e-124">Provides a new Batch Execution Service test page with sample data and job submission history.</span></span>

<span data-ttu-id="11e9e-125">Emellett a Machine Learning Studio léptethet érvénybe az új webes modell környezetébe, vagy továbbra is a hagyományos webes modell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="11e9e-125">In addition, the Machine Learning Studio has been updated to allow you to deploy to the new Web service model or continue to deploy to the classic Web service model.</span></span> 

