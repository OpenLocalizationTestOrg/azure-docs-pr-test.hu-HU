---
title: "Eszközök kapcsolódó adatok megjelenítése az Azure Data Catalog |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Data Catalog kiválasztott adategységet eszközeinek kapcsolódó adatok megjelenítése."
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
ms.openlocfilehash: d45f2cabe712a7982f99a9d280fed4494fc4d377
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a><span data-ttu-id="b086b-103">Kapcsolódó adatok eszközök megtekintéséhez az Azure Data Catalog hogyan?</span><span class="sxs-lookup"><span data-stu-id="b086b-103">How to view related data assets in Azure Data Catalog?</span></span>
<span data-ttu-id="b086b-104">Az Azure Data Catalog lehetővé teszi a kijelölt adatok eszköz és a nézet köztük lévő viszonyt is kapcsolatos adatok eszközök.</span><span class="sxs-lookup"><span data-stu-id="b086b-104">Azure Data Catalog allows you to view data assets related to a selected data asset and view relationships between them.</span></span> 

## <a name="supported-data-sources"></a><span data-ttu-id="b086b-105">Támogatott adatforrások</span><span class="sxs-lookup"><span data-stu-id="b086b-105">Supported data sources</span></span> 
<span data-ttu-id="b086b-106">A következő adatforrásokból származó adatok eszközök regisztrálásakor az Azure Data Catalog automatikusan regisztrálja a kijelölt adatok eszközök közötti illesztési kapcsolatokat metaadatainak.</span><span class="sxs-lookup"><span data-stu-id="b086b-106">When you register data assets from the following data sources, Azure Data Catalog automatically registers metadata about join relationships between the selected data assets.</span></span> 

- <span data-ttu-id="b086b-107">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b086b-107">SQL Server</span></span>
- <span data-ttu-id="b086b-108">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b086b-108">Azure SQL Database</span></span>
- <span data-ttu-id="b086b-109">MySQL</span><span class="sxs-lookup"><span data-stu-id="b086b-109">MySQL</span></span>
- <span data-ttu-id="b086b-110">Oracle</span><span class="sxs-lookup"><span data-stu-id="b086b-110">Oracle</span></span>

## <a name="view-related-data-assets"></a><span data-ttu-id="b086b-111">Kapcsolódó adatok eszközök megtekintése</span><span class="sxs-lookup"><span data-stu-id="b086b-111">View related data assets</span></span>
<span data-ttu-id="b086b-112">A kiválasztott adatkészlet kapcsolódó adategységeket megtekintéséhez használja a **kapcsolatok** lapon a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="b086b-112">To view data assets that are related to a selected dataset, use the **Relationships** tab as shown in the following image:</span></span> 

![Az Azure Data Catalog - kapcsolódó adatok eszközök megtekintése](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

<span data-ttu-id="b086b-114">Ebben a példában a kiválasztott két olyan kapcsolattal vannak **ProductSubcategory** adategységet:</span><span class="sxs-lookup"><span data-stu-id="b086b-114">In this example, there are two relationships for the selected **ProductSubcategory** data asset:</span></span> 

- <span data-ttu-id="b086b-115">A termék tábla ProductSubcategoryID oszlopa Külsőkulcs-kapcsolatot a kijelölt ProductSubcategory táblázat ProductSubcategoryID oszloppal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b086b-115">ProductSubcategoryID column of the Product table has a foreign key relationship with ProductSubcategoryID column of the selected ProductSubcategory table.</span></span> 
- <span data-ttu-id="b086b-116">A ProductSubCategory tábla ProductCategoryID oszlopa Külsőkulcs-kapcsolatot a kijelölt ProductCategory táblázat ProductCategoryID oszloppal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b086b-116">ProductCategoryID column of the ProductSubCategory table has a foreign key relationship with ProductCategoryID column of the selected ProductCategory table.</span></span>

> [!NOTE]
> <span data-ttu-id="b086b-117">Figyelje meg a irányát, a kapcsolatok faszerkezetes nézetben.</span><span class="sxs-lookup"><span data-stu-id="b086b-117">Notice the direction of the arrow in the relationships tree view.</span></span>  

<span data-ttu-id="b086b-118">További részletek az oszlop teljesen minősített neve például vigye az egérmutatót, és megjelenik egy előugró ablak az alábbi képen hasonló:</span><span class="sxs-lookup"><span data-stu-id="b086b-118">To see more details such as the fully qualified name of the column, move the mouse over and you see a popup similar to the following image:</span></span> 

![Az Azure Data Catalog - kapcsolat előugró ablak](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

<span data-ttu-id="b086b-120">Adja meg, amely már regisztrált eszközök közötti kapcsolatokat, regisztrálja újra azokat az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="b086b-120">To include relationships between assets that have already been registered, re-register those assets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b086b-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b086b-121">Next steps</span></span>
- [<span data-ttu-id="b086b-122">Adategységek felügyelete</span><span class="sxs-lookup"><span data-stu-id="b086b-122">How to manage data assets</span></span>](data-catalog-how-to-manage.md)