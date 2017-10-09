---
title: "az SQL Data Warehouse aaaAssign változók |} Microsoft Docs"
description: "Tippek a Transact-SQL változókat az Azure SQL Data Warehouse adattárházzal történő, megoldások hozzárendelése."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9de48739bb0af80ff2a117704b31512c680f78d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a><span data-ttu-id="7cc78-103">Rendelje hozzá az SQL Data Warehouse változók</span><span class="sxs-lookup"><span data-stu-id="7cc78-103">Assign variables in SQL Data Warehouse</span></span>
<span data-ttu-id="7cc78-104">Az SQL Data Warehouse változók kerülnek beállításra hello `DECLARE` utasítás vagy hello `SET` utasítást.</span><span class="sxs-lookup"><span data-stu-id="7cc78-104">Variables in SQL Data Warehouse are set using hello `DECLARE` statement or hello `SET` statement.</span></span>

<span data-ttu-id="7cc78-105">Hello következő tökéletesen érvényes módon tooset változó értékének a következők:</span><span class="sxs-lookup"><span data-stu-id="7cc78-105">All of hello following are perfectly valid ways tooset a variable value:</span></span>

## <a name="setting-variables-with-declare"></a><span data-ttu-id="7cc78-106">A DECLARE változók beállítása</span><span class="sxs-lookup"><span data-stu-id="7cc78-106">Setting variables with DECLARE</span></span>
<span data-ttu-id="7cc78-107">Változók DECLARE inicializálása az egyik hello legrugalmasabb módon tooset az SQL Data Warehouse egy változó értékét.</span><span class="sxs-lookup"><span data-stu-id="7cc78-107">Initializing variables with DECLARE is one of hello most flexible ways tooset a variable value in SQL Data Warehouse.</span></span>

```sql
DECLARE @v  int = 0
;
```

<span data-ttu-id="7cc78-108">Egyszerre több DECLARE tooset, mint egy változót is használja.</span><span class="sxs-lookup"><span data-stu-id="7cc78-108">You can also use DECLARE tooset more than one variable at a time.</span></span> <span data-ttu-id="7cc78-109">Nem használhat `SELECT` vagy `UPDATE` toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="7cc78-109">You cannot use `SELECT` or `UPDATE` toodo this:</span></span>

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

<span data-ttu-id="7cc78-110">Nem lehet a következő, és használhat egy változót hello azonos DECLARE utasítást.</span><span class="sxs-lookup"><span data-stu-id="7cc78-110">You cannot initialise and use a variable in hello same DECLARE statement.</span></span> <span data-ttu-id="7cc78-111">tooillustrate hello pont hello például az alábbi **nem** engedélyezett @p1 van inicializálva és hello használt azonos DECLARE utasítást.</span><span class="sxs-lookup"><span data-stu-id="7cc78-111">tooillustrate hello point hello example below is **not** allowed as @p1 is both initialized and used in hello same DECLARE statement.</span></span> <span data-ttu-id="7cc78-112">Ez hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="7cc78-112">This will result in an error.</span></span>

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a><span data-ttu-id="7cc78-113">A beállítás értéke beállítva</span><span class="sxs-lookup"><span data-stu-id="7cc78-113">Setting values with SET</span></span>
<span data-ttu-id="7cc78-114">Egy változó beállításához nagyon gyakori módja lesz.</span><span class="sxs-lookup"><span data-stu-id="7cc78-114">Set is a very common method for setting a single variable.</span></span>

<span data-ttu-id="7cc78-115">Az összes hello példák az alábbi módon érvényes beállítását egy változó beállítva:</span><span class="sxs-lookup"><span data-stu-id="7cc78-115">All of hello examples below are valid ways of setting a variable with SET:</span></span>

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

<span data-ttu-id="7cc78-116">Egy változó egyszerre beállítva csak beállítani.</span><span class="sxs-lookup"><span data-stu-id="7cc78-116">You can only set one variable at a time with SET.</span></span> <span data-ttu-id="7cc78-117">Azonban, a fent látható összetett operátorok nem megengedett.</span><span class="sxs-lookup"><span data-stu-id="7cc78-117">However, as can be seen above compound operators are permissable.</span></span>

## <a name="limitations"></a><span data-ttu-id="7cc78-118">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="7cc78-118">Limitations</span></span>
<span data-ttu-id="7cc78-119">Válassza ki vagy frissítés változó-hozzárendelés nem használható.</span><span class="sxs-lookup"><span data-stu-id="7cc78-119">You cannot use SELECT or UPDATE for variable assignment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cc78-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cc78-120">Next steps</span></span>
<span data-ttu-id="7cc78-121">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="7cc78-121">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
