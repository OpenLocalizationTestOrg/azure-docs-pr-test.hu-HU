---
title: "Azure API Management-szolgáltatások telepítése több Azure-régiók |} Microsoft Docs"
description: "Ismerje meg, hogyan lehet telepíteni az Azure API Management szolgáltatáspéldány több Azure-régiók."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 47389ad6-f865-4706-833f-846115e22e4d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 1c39fee739c2f5fd4b928e1e76e1ea57f072b5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a><span data-ttu-id="08388-103">Az Azure API Management szolgáltatáspéldány üzembe helyezése több Azure-régiók</span><span class="sxs-lookup"><span data-stu-id="08388-103">How to deploy an Azure API Management service instance to multiple Azure regions</span></span>
<span data-ttu-id="08388-104">Az API Management több területi környezetben, amely lehetővé teszi, hogy egyetlen API management szolgáltatás szét a kívánt Azure-régiók tetszőleges számú API közzétevők támogatja.</span><span class="sxs-lookup"><span data-stu-id="08388-104">API Management supports multi-region deployment which enables API publishers to distribute a single API management service across any number of desired Azure regions.</span></span> <span data-ttu-id="08388-105">Ezzel csökkenthető a kérelem által érzékelt késleltetés földrajzilag elosztott API fogyasztók és a szolgáltatás rendelkezésre állása is támogatja, ha egy régió tartozik offline állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="08388-105">This helps reduce request latency perceived by geographically distributed API consumers and also improves service availability if one region goes offline.</span></span> 

<span data-ttu-id="08388-106">Amikor egy API-kezelés szolgáltatás a kezdetben létrejön, csak az egyik tartalmaz [egység] [ unit] és egy egyetlen Azure-régió, amelyhez az elsődleges régióban kijelölt fájlcsoportban helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="08388-106">When an API Management service is created initially, it contains only one [unit][unit] and resides in a single Azure region, which is designated as the Primary Region.</span></span> <span data-ttu-id="08388-107">További régiókban könnyen felveheti az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="08388-107">Additional regions can be easily added through the Azure Portal.</span></span> <span data-ttu-id="08388-108">Az API Management átjárókiszolgáló minden régió telepít, és hívás forgalmat a rendszer a legközelebbi átjáró irányítsa.</span><span class="sxs-lookup"><span data-stu-id="08388-108">An API Management gateway server is deployed to each region and call traffic will be routed to the closest gateway.</span></span> <span data-ttu-id="08388-109">Ha egy régiót offline állapotba kerül, akkor kimenő forgalomról automatikusan újra irányított, a következő legközelebbi átjáró.</span><span class="sxs-lookup"><span data-stu-id="08388-109">If a region goes offline, the traffic is automatically re-directed to the next closest gateway.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="08388-110">Több területi telepítési érhető el csak a  **[prémium] [ Premium]**  réteg.</span><span class="sxs-lookup"><span data-stu-id="08388-110">Multi-region deployment is only available in the **[Premium][Premium]** tier.</span></span>
> 
> 

## <span data-ttu-id="08388-111"><a name="add-region"></a>API-kezelés szolgáltatás példányt telepítése egy új régióban</span><span class="sxs-lookup"><span data-stu-id="08388-111"><a name="add-region"> </a>Deploy an API Management service instance to a new region</span></span>
> [!NOTE]
> <span data-ttu-id="08388-112">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="08388-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="08388-113">Az Azure portálon keresse meg a **és az árképzés** az API Management szolgáltatáspéldány a lap.</span><span class="sxs-lookup"><span data-stu-id="08388-113">In the Azure Portal navigate to the **Scale and pricing** page for your API Management service instance.</span></span> 

![Skála lap][api-management-scale-service]

<span data-ttu-id="08388-115">Egy új régió telepíteni, kattintson a **+ Hozzáadás régió** az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="08388-115">To deploy to a new region, click on **+ Add region** from the toolbar.</span></span>

![Adja hozzá a régió][api-management-add-region]

<span data-ttu-id="08388-117">A legördülő listából válassza ki a helyet, és állítsa be a csúszkával egységek száma.</span><span class="sxs-lookup"><span data-stu-id="08388-117">Select the location from the drop-down list and set the number of units for with the slider.</span></span>

![Adja meg a egység][api-management-select-location-units]

<span data-ttu-id="08388-119">Kattintson a **Hozzáadás** helyezhető el a kiválasztott a helyek táblában.</span><span class="sxs-lookup"><span data-stu-id="08388-119">Click **Add** to place your selection in the Locations table.</span></span> 

<span data-ttu-id="08388-120">Ismételje meg ezt a folyamatot, amíg a konfigurált összes hellyel rendelkezik, és kattintson a **mentése** a telepítési folyamat elindításához az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="08388-120">Repeat this process until you have all locations configured and click **Save** from the toolbar to start the deployment process.</span></span>

## <span data-ttu-id="08388-121"><a name="remove-region"></a>Egy API-kezelés szolgáltatás példány törlése</span><span class="sxs-lookup"><span data-stu-id="08388-121"><a name="remove-region"> </a>Delete an API Management service instance from a location</span></span>
<span data-ttu-id="08388-122">Az Azure portálon keresse meg a **és az árképzés** az API Management szolgáltatáspéldány a lap.</span><span class="sxs-lookup"><span data-stu-id="08388-122">In the Azure Portal navigate to the **Scale and pricing** page for your API Management service instance.</span></span> 

![Skála lap][api-management-scale-service]

<span data-ttu-id="08388-124">Szeretné eltávolítani a helyet nyissa meg a helyi menü használatával a **...**  gomb a tábla jobb oldali végén.</span><span class="sxs-lookup"><span data-stu-id="08388-124">For the location you would like to remove open the context menu using the **...** button at the right end of the table.</span></span> <span data-ttu-id="08388-125">Válassza ki a **törlése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="08388-125">Select the **Delete** option.</span></span>

![Távolítsa el a régió][api-management-remove-region]

<span data-ttu-id="08388-127">A törlés jóváhagyásához, és kattintson a **mentése** a módosítások életbe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="08388-127">Confirm the deletion and click **Save** to apply the changes.</span></span>

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

