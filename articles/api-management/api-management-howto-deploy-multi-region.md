---
title: "aaaDeploy Azure API Management-szolgáltatások toomultiple Azure régiók |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy az Azure API Management szolgáltatás példány toomultiple Azure régióban."
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
ms.openlocfilehash: 04a3e762261237d73a769320a21363f99f1d20cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-an-azure-api-management-service-instance-toomultiple-azure-regions"></a><span data-ttu-id="07cb0-103">Hogyan toodeploy az Azure API Management szolgáltatás példány toomultiple Azure régiók</span><span class="sxs-lookup"><span data-stu-id="07cb0-103">How toodeploy an Azure API Management service instance toomultiple Azure regions</span></span>
<span data-ttu-id="07cb0-104">Az API Management támogatja több területi környezetben, amely lehetővé teszi, hogy az API-közzétevők toodistribute egyetlen API management szolgáltatás tetszőleges számú kívánt Azure-régiók között.</span><span class="sxs-lookup"><span data-stu-id="07cb0-104">API Management supports multi-region deployment which enables API publishers toodistribute a single API management service across any number of desired Azure regions.</span></span> <span data-ttu-id="07cb0-105">Ezzel csökkenthető a kérelem által érzékelt késleltetés földrajzilag elosztott API fogyasztók és a szolgáltatás rendelkezésre állása is támogatja, ha egy régió tartozik offline állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="07cb0-105">This helps reduce request latency perceived by geographically distributed API consumers and also improves service availability if one region goes offline.</span></span> 

<span data-ttu-id="07cb0-106">Amikor egy API-kezelés szolgáltatás a kezdetben létrejön, csak az egyik tartalmaz [egység] [ unit] és egy önálló, elsődleges régió hello van kijelölve Azure régióban található.</span><span class="sxs-lookup"><span data-stu-id="07cb0-106">When an API Management service is created initially, it contains only one [unit][unit] and resides in a single Azure region, which is designated as hello Primary Region.</span></span> <span data-ttu-id="07cb0-107">További régiókban hello Azure portálon keresztül egyszerűen lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="07cb0-107">Additional regions can be easily added through hello Azure Portal.</span></span> <span data-ttu-id="07cb0-108">Az API Management átjárókiszolgáló telepített tooeach régió, és hívás forgalom lesz irányított toohello legközelebbi átjáró.</span><span class="sxs-lookup"><span data-stu-id="07cb0-108">An API Management gateway server is deployed tooeach region and call traffic will be routed toohello closest gateway.</span></span> <span data-ttu-id="07cb0-109">Ha egy régiót offline állapotba kerül, hello akkor kimenő forgalomról automatikusan újra irányított toohello következő legközelebbi átjáró.</span><span class="sxs-lookup"><span data-stu-id="07cb0-109">If a region goes offline, hello traffic is automatically re-directed toohello next closest gateway.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="07cb0-110">Több területi telepítési csak érhető el hello  **[prémium] [ Premium]**  réteg.</span><span class="sxs-lookup"><span data-stu-id="07cb0-110">Multi-region deployment is only available in hello **[Premium][Premium]** tier.</span></span>
> 
> 

## <span data-ttu-id="07cb0-111"><a name="add-region"></a>Központi telepítése egy API-kezelés szolgáltatás példány tooa új terület</span><span class="sxs-lookup"><span data-stu-id="07cb0-111"><a name="add-region"> </a>Deploy an API Management service instance tooa new region</span></span>
> [!NOTE]
> <span data-ttu-id="07cb0-112">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="07cb0-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="07cb0-113">Hello Azure portálon lépjen a toohello **és az árképzés** az API Management szolgáltatáspéldány a lap.</span><span class="sxs-lookup"><span data-stu-id="07cb0-113">In hello Azure Portal navigate toohello **Scale and pricing** page for your API Management service instance.</span></span> 

![Skála lap][api-management-scale-service]

<span data-ttu-id="07cb0-115">toodeploy tooa új régió, kattintson a **+ Hozzáadás régió** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="07cb0-115">toodeploy tooa new region, click on **+ Add region** from hello toolbar.</span></span>

![Adja hozzá a régió][api-management-add-region]

<span data-ttu-id="07cb0-117">Hello legördülő listából válassza ki a hello helyét, és állítsa be a hello hello csúszkával egységek száma.</span><span class="sxs-lookup"><span data-stu-id="07cb0-117">Select hello location from hello drop-down list and set hello number of units for with hello slider.</span></span>

![Adja meg a egység][api-management-select-location-units]

<span data-ttu-id="07cb0-119">Kattintson a **Hozzáadás** tooplace hello helyek tábla választását.</span><span class="sxs-lookup"><span data-stu-id="07cb0-119">Click **Add** tooplace your selection in hello Locations table.</span></span> 

<span data-ttu-id="07cb0-120">Ismételje meg ezt a folyamatot, amíg a konfigurált összes hellyel rendelkezik, és kattintson a **mentése** hello eszköztár toostart hello központi telepítési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="07cb0-120">Repeat this process until you have all locations configured and click **Save** from hello toolbar toostart hello deployment process.</span></span>

## <span data-ttu-id="07cb0-121"><a name="remove-region"></a>Egy API-kezelés szolgáltatás példány törlése</span><span class="sxs-lookup"><span data-stu-id="07cb0-121"><a name="remove-region"> </a>Delete an API Management service instance from a location</span></span>
<span data-ttu-id="07cb0-122">Hello Azure portálon lépjen a toohello **és az árképzés** az API Management szolgáltatáspéldány a lap.</span><span class="sxs-lookup"><span data-stu-id="07cb0-122">In hello Azure Portal navigate toohello **Scale and pricing** page for your API Management service instance.</span></span> 

![Skála lap][api-management-scale-service]

<span data-ttu-id="07cb0-124">Milyen hello hely tooremove nyissa meg a hello helyi menü használatával hello **...**  hello tábla jobb oldali végén hello gombra.</span><span class="sxs-lookup"><span data-stu-id="07cb0-124">For hello location you would like tooremove open hello context menu using hello **...** button at hello right end of hello table.</span></span> <span data-ttu-id="07cb0-125">Jelölje be hello **törlése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="07cb0-125">Select hello **Delete** option.</span></span>

![Távolítsa el a régió][api-management-remove-region]

<span data-ttu-id="07cb0-127">Hello törlés jóváhagyásához, és kattintson a **mentése** tooapply hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="07cb0-127">Confirm hello deletion and click **Save** tooapply hello changes.</span></span>

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance tooa new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

