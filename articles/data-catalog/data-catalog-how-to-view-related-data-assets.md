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
# <a name="how-tooview-related-data-assets-in-azure-data-catalog"></a>Hogyan tooview kapcsolódó adategységeket az Azure Data Catalog?
Az Azure Data Catalog lehetővé teszi tooview adatok eszközök kapcsolódó tooa kijelölt adatok eszköz és a nézet köztük lévő viszonyt is. 

## <a name="supported-data-sources"></a>Támogatott adatforrások 
A következő adatforrások hello adategységeket regisztrálásakor Azure Data Catalog automatikusan regisztrálja hello kijelölt adatok eszközök közötti illesztési kapcsolatokat metaadatait. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

## <a name="view-related-data-assets"></a>Kapcsolódó adatok eszközök megtekintése
tooview adategységeiről, amelyeket a kijelölt kapcsolódó tooa adatkészletet, amelyek használata hello **kapcsolatok** lapon látható hello kép a következő módon: 

![Az Azure Data Catalog - kapcsolódó adatok eszközök megtekintése](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

Ebben a példában a kiválasztott hello két olyan kapcsolattal vannak **ProductSubcategory** adategységet: 

- Hello termék tábla ProductSubcategoryID oszlopa Külsőkulcs-kapcsolatot a kijelölt hello ProductSubcategory tábla ProductSubcategoryID oszlopa van. 
- Hello ProductSubCategory tábla ProductCategoryID oszlopa Külsőkulcs-kapcsolatot a kiválasztott hello ProductCategory tábla ProductCategoryID oszlopa van.

> [!NOTE]
> Figyelje meg hello irányának hello nyíl hello kapcsolatok faszerkezetes nézetben.  

toosee hello teljesen minősített neve hello oszlop, például további részleteket átvitele hello egér, és megjelenik egy előugró ablak hasonló toohello kép a következő: 

![Az Azure Data Catalog - kapcsolat előugró ablak](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

eszközök, amelyek a már regisztrált, tooinclude kapcsolatai regisztrálja újra azokat az eszközöket.

## <a name="next-steps"></a>Következő lépések
- [Hogyan toomanage adategységeket](data-catalog-how-to-manage.md)
