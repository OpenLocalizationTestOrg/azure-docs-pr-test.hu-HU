---
title: "aaaHow tooview kapcsolódó adategységeket az Azure Data Catalog |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooview kapcsolatos Azure Data Catalog egy kijelölt adatok eszköz adategységeket."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: b69686737070ac563a0318f48e693215c605f90b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-related-data-assets-in-azure-data-catalog"></a><span data-ttu-id="87e0d-103">Hogyan tooview kapcsolódó adategységeket az Azure Data Catalog?</span><span class="sxs-lookup"><span data-stu-id="87e0d-103">How tooview related data assets in Azure Data Catalog?</span></span>
<span data-ttu-id="87e0d-104">Az Azure Data Catalog lehetővé teszi tooview adatok eszközök kapcsolódó tooa kijelölt adatok eszköz és a nézet köztük lévő viszonyt is.</span><span class="sxs-lookup"><span data-stu-id="87e0d-104">Azure Data Catalog allows you tooview data assets related tooa selected data asset and view relationships between them.</span></span> 

## <a name="supported-data-sources"></a><span data-ttu-id="87e0d-105">Támogatott adatforrások</span><span class="sxs-lookup"><span data-stu-id="87e0d-105">Supported data sources</span></span> 
<span data-ttu-id="87e0d-106">A következő adatforrások hello adategységeket regisztrálásakor Azure Data Catalog automatikusan regisztrálja hello kijelölt adatok eszközök közötti illesztési kapcsolatokat metaadatait.</span><span class="sxs-lookup"><span data-stu-id="87e0d-106">When you register data assets from hello following data sources, Azure Data Catalog automatically registers metadata about join relationships between hello selected data assets.</span></span> 

- <span data-ttu-id="87e0d-107">SQL Server</span><span class="sxs-lookup"><span data-stu-id="87e0d-107">SQL Server</span></span>
- <span data-ttu-id="87e0d-108">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="87e0d-108">Azure SQL Database</span></span>
- <span data-ttu-id="87e0d-109">MySQL</span><span class="sxs-lookup"><span data-stu-id="87e0d-109">MySQL</span></span>
- <span data-ttu-id="87e0d-110">Oracle</span><span class="sxs-lookup"><span data-stu-id="87e0d-110">Oracle</span></span>

## <a name="view-related-data-assets"></a><span data-ttu-id="87e0d-111">Kapcsolódó adatok eszközök megtekintése</span><span class="sxs-lookup"><span data-stu-id="87e0d-111">View related data assets</span></span>
<span data-ttu-id="87e0d-112">tooview adategységeiről, amelyeket a kijelölt kapcsolódó tooa adatkészletet, amelyek használata hello **kapcsolatok** lapon látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="87e0d-112">tooview data assets that are related tooa selected dataset, use hello **Relationships** tab as shown in hello following image:</span></span> 

![Az Azure Data Catalog - kapcsolódó adatok eszközök megtekintése](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

<span data-ttu-id="87e0d-114">Ebben a példában a kiválasztott hello két olyan kapcsolattal vannak **ProductSubcategory** adategységet:</span><span class="sxs-lookup"><span data-stu-id="87e0d-114">In this example, there are two relationships for hello selected **ProductSubcategory** data asset:</span></span> 

- <span data-ttu-id="87e0d-115">Hello termék tábla ProductSubcategoryID oszlopa Külsőkulcs-kapcsolatot a kijelölt hello ProductSubcategory tábla ProductSubcategoryID oszlopa van.</span><span class="sxs-lookup"><span data-stu-id="87e0d-115">ProductSubcategoryID column of hello Product table has a foreign key relationship with ProductSubcategoryID column of hello selected ProductSubcategory table.</span></span> 
- <span data-ttu-id="87e0d-116">Hello ProductSubCategory tábla ProductCategoryID oszlopa Külsőkulcs-kapcsolatot a kiválasztott hello ProductCategory tábla ProductCategoryID oszlopa van.</span><span class="sxs-lookup"><span data-stu-id="87e0d-116">ProductCategoryID column of hello ProductSubCategory table has a foreign key relationship with ProductCategoryID column of hello selected ProductCategory table.</span></span>

> [!NOTE]
> <span data-ttu-id="87e0d-117">Figyelje meg hello irányának hello nyíl hello kapcsolatok faszerkezetes nézetben.</span><span class="sxs-lookup"><span data-stu-id="87e0d-117">Notice hello direction of hello arrow in hello relationships tree view.</span></span>  

<span data-ttu-id="87e0d-118">toosee hello teljesen minősített neve hello oszlop, például további részleteket átvitele hello egér, és megjelenik egy előugró ablak hasonló toohello kép a következő:</span><span class="sxs-lookup"><span data-stu-id="87e0d-118">toosee more details such as hello fully qualified name of hello column, move hello mouse over and you see a popup similar toohello following image:</span></span> 

![Az Azure Data Catalog - kapcsolat előugró ablak](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

<span data-ttu-id="87e0d-120">eszközök, amelyek a már regisztrált, tooinclude kapcsolatai regisztrálja újra azokat az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="87e0d-120">tooinclude relationships between assets that have already been registered, re-register those assets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87e0d-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87e0d-121">Next steps</span></span>
- [<span data-ttu-id="87e0d-122">Hogyan toomanage adategységeket</span><span class="sxs-lookup"><span data-stu-id="87e0d-122">How toomanage data assets</span></span>](data-catalog-how-to-manage.md)
