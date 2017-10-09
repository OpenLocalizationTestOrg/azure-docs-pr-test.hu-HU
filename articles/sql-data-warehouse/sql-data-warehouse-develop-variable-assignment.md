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
# <a name="assign-variables-in-sql-data-warehouse"></a>Rendelje hozzá az SQL Data Warehouse változók
Az SQL Data Warehouse változók kerülnek beállításra hello `DECLARE` utasítás vagy hello `SET` utasítást.

Hello következő tökéletesen érvényes módon tooset változó értékének a következők:

## <a name="setting-variables-with-declare"></a>A DECLARE változók beállítása
Változók DECLARE inicializálása az egyik hello legrugalmasabb módon tooset az SQL Data Warehouse egy változó értékét.

```sql
DECLARE @v  int = 0
;
```

Egyszerre több DECLARE tooset, mint egy változót is használja. Nem használhat `SELECT` vagy `UPDATE` toodo ezt:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Nem lehet a következő, és használhat egy változót hello azonos DECLARE utasítást. tooillustrate hello pont hello például az alábbi **nem** engedélyezett @p1 van inicializálva és hello használt azonos DECLARE utasítást. Ez hibát eredményez.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>A beállítás értéke beállítva
Egy változó beállításához nagyon gyakori módja lesz.

Az összes hello példák az alábbi módon érvényes beállítását egy változó beállítva:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Egy változó egyszerre beállítva csak beállítani. Azonban, a fent látható összetett operátorok nem megengedett.

## <a name="limitations"></a>Korlátozások
Válassza ki vagy frissítés változó-hozzárendelés nem használható.

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
