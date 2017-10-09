---
title: az SQL Data Warehouse SQL aaaDynamic |} Microsoft Docs
description: "Tippek az Azure SQL Data Warehouse adattárházzal történő, megoldások dinamikus SQL használatát."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: a948c2c3-3cd1-4373-90a9-79e59414b778
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 4d66eecb37621510f657d1ec9a2a935daaa16052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a><span data-ttu-id="15fb1-103">Az SQL Data Warehouse dinamikus SQL</span><span class="sxs-lookup"><span data-stu-id="15fb1-103">Dynamic SQL in SQL Data Warehouse</span></span>
<span data-ttu-id="15fb1-104">Az SQL Data Warehouse, akkor előfordulhat, hogy alkalmazáskód fejlesztésekor kell toouse dinamikus sql toohelp kínálja rugalmas, általános és moduláris.</span><span class="sxs-lookup"><span data-stu-id="15fb1-104">When developing application code for SQL Data Warehouse you may need toouse dynamic sql toohelp deliver flexible, generic and modular solutions.</span></span> <span data-ttu-id="15fb1-105">SQL Data Warehouse jelenleg nem támogatja a blob adattípusokat.</span><span class="sxs-lookup"><span data-stu-id="15fb1-105">SQL Data Warehouse does not support blob data types at this time.</span></span> <span data-ttu-id="15fb1-106">Ez lehet, hogy a karakterláncok hello méretének korlátozása a blob típusok varchar(max) és a típus: nvarchar(max) típusok közé.</span><span class="sxs-lookup"><span data-stu-id="15fb1-106">This may limit hello size of your strings as blob types include both varchar(max) and nvarchar(max) types.</span></span> <span data-ttu-id="15fb1-107">Ha ezek a típusok már használta az alkalmazás kódjában, nagyon nagy karakterláncok fejlesztéskor, szüksége lesz toobreak hello kód adattömbök és use hello EXEC utasítás helyette.</span><span class="sxs-lookup"><span data-stu-id="15fb1-107">If you have used these types in your application code when building very large strings, you will need toobreak hello code into chunks and use hello EXEC statement instead.</span></span>

<span data-ttu-id="15fb1-108">Egy egyszerű példa:</span><span class="sxs-lookup"><span data-stu-id="15fb1-108">A simple example:</span></span>

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

<span data-ttu-id="15fb1-109">Ha hello karakterlánc rövid használhatja [sp_executesql] [ sp_executesql] normál.</span><span class="sxs-lookup"><span data-stu-id="15fb1-109">If hello string is short you can use [sp_executesql][sp_executesql] as normal.</span></span>

> [!NOTE]
> <span data-ttu-id="15fb1-110">Az utasítás dinamikus SQL végrehajtásra is tulajdonos tooall TSQL ellenőrzési szabályok.</span><span class="sxs-lookup"><span data-stu-id="15fb1-110">Statements executed as dynamic SQL will still be subject tooall TSQL validation rules.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="15fb1-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15fb1-111">Next steps</span></span>
<span data-ttu-id="15fb1-112">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="15fb1-112">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
