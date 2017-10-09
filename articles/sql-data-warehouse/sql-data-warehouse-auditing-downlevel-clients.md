---
title: "aaaSQL adatraktár Alsószintű ügyfelek támogatása adatok naplózását |} Microsoft Docs"
description: "További tudnivalók az SQL Data Warehouse a régebbi típusú ügyfelek támogatása adatok naplózása"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 377488680eb297c3e9b1dc754c003c5b19b47996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Az SQL Data Warehouse - a régebbi típusú ügyfeleknek naplózási és dinamikus Adatmaszkolási támogatása
[Naplózás](sql-data-warehouse-auditing-overview.md) TDS átirányítást támogató SQL-ügyfelek működik.

Bármely olyan ügyfél, amely TDS 7.4 kell is támogatja az átirányítást. Kivételek toothis közé tartoznak JDBC 4.0 mely hello átirányítási nem teljes mértékben támogatja és Tedious a Node.JS, amelyben átirányítás nincs megvalósítva.

"Régebbi ügyfelek" azaz TDS 7.3-as verzió támogatja és az alábbiakban - hello kiszolgálójának teljes Tartományneve hello kapcsolat-karakterláncban kell módosítani:

A kapcsolati karakterláncban hello eredeti kiszolgálójának teljes Tartományneve: <*kiszolgálónév*>. database.windows.net

Módosított kiszolgálójának teljes Tartományneve hello kapcsolati karakterlánc: <*kiszolgálónév*> .database. **biztonságos**. windows.net

"A régebbi típusú ügyfeleknek" részleges listáját tartalmazza:

* A .NET 4.0-s vagy régebbi verzió,
* ODBC 10.0-s vagy régebbi verzió.
* JDBC (közben JDBC támogatja a TDS 7.4, hello TDS átirányítási nem teljes mértékben támogatja)
* (A Node.JS) fárasztó

**Megjegyzés:** hello fent server FQDN módosítása során is hasznos egy SQL Server szint naplózási házirend alkalmazása nélkül konfiguráció szükséges lépést az egyes adatbázisok (ideiglenes megoldás).     

