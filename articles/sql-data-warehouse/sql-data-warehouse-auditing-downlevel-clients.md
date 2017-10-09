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
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a><span data-ttu-id="d20e7-103">Az SQL Data Warehouse - a régebbi típusú ügyfeleknek naplózási és dinamikus Adatmaszkolási támogatása</span><span class="sxs-lookup"><span data-stu-id="d20e7-103">SQL Data Warehouse -  Downlevel clients support for auditing and Dynamic Data Masking</span></span>
<span data-ttu-id="d20e7-104">[Naplózás](sql-data-warehouse-auditing-overview.md) TDS átirányítást támogató SQL-ügyfelek működik.</span><span class="sxs-lookup"><span data-stu-id="d20e7-104">[Auditing](sql-data-warehouse-auditing-overview.md) works with SQL clients that support TDS redirection.</span></span>

<span data-ttu-id="d20e7-105">Bármely olyan ügyfél, amely TDS 7.4 kell is támogatja az átirányítást.</span><span class="sxs-lookup"><span data-stu-id="d20e7-105">Any client which implements TDS 7.4 should also support redirection.</span></span> <span data-ttu-id="d20e7-106">Kivételek toothis közé tartoznak JDBC 4.0 mely hello átirányítási nem teljes mértékben támogatja és Tedious a Node.JS, amelyben átirányítás nincs megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="d20e7-106">Exceptions toothis include JDBC 4.0 in which hello redirection feature is not fully supported and Tedious for Node.JS in which redirection was not implemented.</span></span>

<span data-ttu-id="d20e7-107">"Régebbi ügyfelek" azaz TDS 7.3-as verzió támogatja és az alábbiakban - hello kiszolgálójának teljes Tartományneve hello kapcsolat-karakterláncban kell módosítani:</span><span class="sxs-lookup"><span data-stu-id="d20e7-107">For "Downlevel clients", i.e. which support TDS version 7.3 and below - hello server FQDN in hello connection string should be modified:</span></span>

<span data-ttu-id="d20e7-108">A kapcsolati karakterláncban hello eredeti kiszolgálójának teljes Tartományneve: <*kiszolgálónév*>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="d20e7-108">Original server FQDN in hello connection string: <*server name*>.database.windows.net</span></span>

<span data-ttu-id="d20e7-109">Módosított kiszolgálójának teljes Tartományneve hello kapcsolati karakterlánc: <*kiszolgálónév*> .database. **biztonságos**. windows.net</span><span class="sxs-lookup"><span data-stu-id="d20e7-109">Modified server FQDN in hello connection string: <*server name*>.database.**secure**.windows.net</span></span>

<span data-ttu-id="d20e7-110">"A régebbi típusú ügyfeleknek" részleges listáját tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="d20e7-110">A partial list of "Downlevel clients" includes:</span></span>

* <span data-ttu-id="d20e7-111">A .NET 4.0-s vagy régebbi verzió,</span><span class="sxs-lookup"><span data-stu-id="d20e7-111">.NET 4.0 and below,</span></span>
* <span data-ttu-id="d20e7-112">ODBC 10.0-s vagy régebbi verzió.</span><span class="sxs-lookup"><span data-stu-id="d20e7-112">ODBC 10.0 and below.</span></span>
* <span data-ttu-id="d20e7-113">JDBC (közben JDBC támogatja a TDS 7.4, hello TDS átirányítási nem teljes mértékben támogatja)</span><span class="sxs-lookup"><span data-stu-id="d20e7-113">JDBC (while JDBC does support TDS 7.4, hello TDS redirection feature is not fully supported)</span></span>
* <span data-ttu-id="d20e7-114">(A Node.JS) fárasztó</span><span class="sxs-lookup"><span data-stu-id="d20e7-114">Tedious (for Node.JS)</span></span>

<span data-ttu-id="d20e7-115">**Megjegyzés:** hello fent server FQDN módosítása során is hasznos egy SQL Server szint naplózási házirend alkalmazása nélkül konfiguráció szükséges lépést az egyes adatbázisok (ideiglenes megoldás).</span><span class="sxs-lookup"><span data-stu-id="d20e7-115">**Remark:** hello above server FDQN modification may be useful also for applying a SQL Server Level Auditing policy without a need for a configuration step in each database (Temporary mitigation).</span></span>     

