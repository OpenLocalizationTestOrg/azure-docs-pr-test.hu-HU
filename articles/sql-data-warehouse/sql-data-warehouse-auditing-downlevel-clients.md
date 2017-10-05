---
title: "Az SQL Data Warehouse Alsószintű ügyfelek támogatása adatok naplózását |} Microsoft Docs"
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
ms.openlocfilehash: a7ea6141285a0098339f1e071af2592dd4535c12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a><span data-ttu-id="25ddc-103">Az SQL Data Warehouse - a régebbi típusú ügyfeleknek naplózási és dinamikus Adatmaszkolási támogatása</span><span class="sxs-lookup"><span data-stu-id="25ddc-103">SQL Data Warehouse -  Downlevel clients support for auditing and Dynamic Data Masking</span></span>
<span data-ttu-id="25ddc-104">[Naplózás](sql-data-warehouse-auditing-overview.md) TDS átirányítást támogató SQL-ügyfelek működik.</span><span class="sxs-lookup"><span data-stu-id="25ddc-104">[Auditing](sql-data-warehouse-auditing-overview.md) works with SQL clients that support TDS redirection.</span></span>

<span data-ttu-id="25ddc-105">Bármely olyan ügyfél, amely TDS 7.4 kell is támogatja az átirányítást.</span><span class="sxs-lookup"><span data-stu-id="25ddc-105">Any client which implements TDS 7.4 should also support redirection.</span></span> <span data-ttu-id="25ddc-106">A kivételek közé tartozik a JDBC 4.0-s verzióját, amelyben az átirányítás nem teljes mértékben támogatja, és a Node.JS mely funkcióhoz Tedious nem lett megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="25ddc-106">Exceptions to this include JDBC 4.0 in which the redirection feature is not fully supported and Tedious for Node.JS in which redirection was not implemented.</span></span>

<span data-ttu-id="25ddc-107">Az "Alsószintű ügyfelek" azaz mely támogatási TDS verzió 7.3 és az alacsonyabb – a kiszolgáló teljes Tartománynevét a kapcsolat-karakterlánc kell módosítani:</span><span class="sxs-lookup"><span data-stu-id="25ddc-107">For "Downlevel clients", i.e. which support TDS version 7.3 and below - the server FQDN in the connection string should be modified:</span></span>

<span data-ttu-id="25ddc-108">A kapcsolódási karakterláncban eredeti kiszolgálójának teljes Tartományneve: <*kiszolgálónév*>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="25ddc-108">Original server FQDN in the connection string: <*server name*>.database.windows.net</span></span>

<span data-ttu-id="25ddc-109">A kapcsolati karakterláncban a módosított kiszolgálójának teljes Tartományneve: <*kiszolgálónév*> .database. **biztonságos**. windows.net</span><span class="sxs-lookup"><span data-stu-id="25ddc-109">Modified server FQDN in the connection string: <*server name*>.database.**secure**.windows.net</span></span>

<span data-ttu-id="25ddc-110">"A régebbi típusú ügyfeleknek" részleges listáját tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="25ddc-110">A partial list of "Downlevel clients" includes:</span></span>

* <span data-ttu-id="25ddc-111">A .NET 4.0-s vagy régebbi verzió,</span><span class="sxs-lookup"><span data-stu-id="25ddc-111">.NET 4.0 and below,</span></span>
* <span data-ttu-id="25ddc-112">ODBC 10.0-s vagy régebbi verzió.</span><span class="sxs-lookup"><span data-stu-id="25ddc-112">ODBC 10.0 and below.</span></span>
* <span data-ttu-id="25ddc-113">JDBC (JDBC támogatja a TDS 7.4, a TDS-átirányítási funkció még nem teljes mértékben támogatott)</span><span class="sxs-lookup"><span data-stu-id="25ddc-113">JDBC (while JDBC does support TDS 7.4, the TDS redirection feature is not fully supported)</span></span>
* <span data-ttu-id="25ddc-114">(A Node.JS) fárasztó</span><span class="sxs-lookup"><span data-stu-id="25ddc-114">Tedious (for Node.JS)</span></span>

<span data-ttu-id="25ddc-115">**Megjegyzés:** lehet, hogy a fenti kiszolgáló FQDN módosítását is hasznos egy SQL Server szint naplózási házirend alkalmazása nélkül konfiguráció szükséges lépést az egyes adatbázisok (ideiglenes megoldás).</span><span class="sxs-lookup"><span data-stu-id="25ddc-115">**Remark:** The above server FDQN modification may be useful also for applying a SQL Server Level Auditing policy without a need for a configuration step in each database (Temporary mitigation).</span></span>     

