---
title: "automatikus hangolása az Azure SQL Database aaaEnable |} Microsoft Docs"
description: "Automatikus hangolása az Azure SQL adatbázis könnyen engedélyezheti."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.openlocfilehash: af9da161eabc0f8c4cb100c050288f234efb8093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-automatic-tuning"></a><span data-ttu-id="f1dd3-103">Automatikus hangolás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f1dd3-103">Enable automatic tuning</span></span>

<span data-ttu-id="f1dd3-104">Az Azure SQL Database az automatikusan kezelt adatok szolgáltatása, amely folyamatosan figyeli a lekérdezéseket, és azonosítja, hogy elvégezhető-e a számítási feladatok teljesítményére tooimprove hello művelet.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-104">Azure SQL Database is an automatically managed data service that constantly monitors your queries and identifies hello action that you can perform tooimprove performance of your workload.</span></span> <span data-ttu-id="f1dd3-105">Áttekintheti javaslatok és manuálisan alkalmazhatja azokat, vagy lehetővé teszik az Azure SQL Database automatikusan alkalmazza a javítási műveleteket – ez az úgynevezett **automatikus hangolási módot**.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-105">You can review recommendations and manually apply them, or let Azure SQL Database automatically apply corrective actions - this is known as **automatic tuning mode**.</span></span> <span data-ttu-id="f1dd3-106">Automatikus hangolása hello kiszolgáló vagy hello adatbázis szintjén engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-106">Automatic tuning can be enabled at hello server or hello database level.</span></span>

## <a name="enable-automatic-tuning-on-server"></a><span data-ttu-id="f1dd3-107">Engedélyezze a kiszolgálón automatikus hangolása</span><span class="sxs-lookup"><span data-stu-id="f1dd3-107">Enable automatic tuning on server</span></span>

<span data-ttu-id="f1dd3-108">tooenable automatikus hangolása Azure SQL adatbázis-kiszolgálón nyissa meg a toohello server az Azure portál, és jelölje ki **automatikus hangolása** hello menüben.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-108">tooenable automatic tuning on Azure SQL Database server, navigate toohello server in Azure portal and then select **Automatic tuning** in hello menu.</span></span> <span data-ttu-id="f1dd3-109">Válassza ki a kívánt tooenable, és válassza ki az automatikus hangolási lehetőségeket hello **alkalmaz**:</span><span class="sxs-lookup"><span data-stu-id="f1dd3-109">Select hello automatic tuning options you want tooenable and select **Apply**:</span></span>

![Kiszolgáló](./media/sql-database-automatic-tuning-enable/server.png)

<span data-ttu-id="f1dd3-111">Automatikus hangolása beállítások a kiszolgálón alkalmazott tooall adatbázisok hello kiszolgálón vannak.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-111">Automatic tuning options on server are applied tooall databases on hello server.</span></span> <span data-ttu-id="f1dd3-112">Alapértelmezés szerint minden adatbázisok hello konfigurációs öröklése a fölérendelt kiszolgáló, de ez felül, és az egyes adatbázisok külön-külön megadva.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-112">By default, all databases inherit hello configuration from their parent server, but this can be overridden and specified for each database individually.</span></span>

## <a name="configure-automatic-tuning-on-database"></a><span data-ttu-id="f1dd3-113">Konfigurálja az adatbázis automatikus hangolása</span><span class="sxs-lookup"><span data-stu-id="f1dd3-113">Configure automatic tuning on database</span></span>

<span data-ttu-id="f1dd3-114">hello Azure portál lehetővé teszi, hogy tooindividually adja meg a hello automatikus hangolási beállítás, az összes adatbázisra.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-114">hello Azure portal enables you tooindividually specify hello automatic tuning configuration on each database.</span></span>

> [!NOTE]
> <span data-ttu-id="f1dd3-115">általános javaslat hello toomanage hello automatikus hangolási beállítás kiszolgálói szinten azonos konfigurációs beállítások alkalmazhatók az összes adatbázis automatikusan Igen hello.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-115">hello general recommendation is toomanage hello automatic tuning configuration at server level so hello same configuration settings can be applied on every database automatically.</span></span> <span data-ttu-id="f1dd3-116">Konfigurálja az automatikus hangolással a egyedi adatbázis Ha hello adatbázis különböző, hogy a többi hello ugyanarra a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-116">Configure automatic tuning on an individual database if hello database is different that others on hello same server.</span></span>
>

<span data-ttu-id="f1dd3-117">automatikus hangolása egy adatbázist, a tooenable toohello adatbázis hello Azure-portálon a keresse meg és majd, és válassza ki **automatikus hangolása**.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-117">tooenable automatic tuning on a single database, navigate toohello database in hello Azure portal and then and select **Automatic tuning**.</span></span> <span data-ttu-id="f1dd3-118">Egy önálló adatbázis tooinherit hello beállításokat hello adatbázisból hello jelölőnégyzet kiválasztásával konfigurálhatja, vagy külön-külön is hello konfigurációs adatbázis megadása.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-118">You can configure a single database tooinherit hello settings from hello database by selecting hello checkbox or you can specify hello configuration for a database individually.</span></span>

![Adatbázis](./media/sql-database-automatic-tuning-enable/database.png)

<span data-ttu-id="f1dd3-120">Miután kiválasztotta a megfelelő konfigurációs, kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-120">Once you have selected appropriate configuration, click **Apply**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1dd3-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1dd3-121">Next steps</span></span>
* <span data-ttu-id="f1dd3-122">Olvasási hello [automatikus hangolási cikk](sql-database-automatic-tuning.md) toolearn további automatikus hangolása és szerepéről javítják a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-122">Read hello [Automatic tuning article](sql-database-automatic-tuning.md) toolearn more about automatic tuning and how it can help you improve your performance.</span></span>
* <span data-ttu-id="f1dd3-123">Lásd: [teljesítmény javaslatok](sql-database-advisor.md) teljesítmény javaslatok az Azure SQL Database áttekintését.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-123">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="f1dd3-124">Lásd: [lekérdezési teljesítmény Insights](sql-database-query-performance.md) toolearn hello teljesítményre gyakorolt hatását a leggyakoribb lekérdezések megtekintése.</span><span class="sxs-lookup"><span data-stu-id="f1dd3-124">See [Query Performance Insights](sql-database-query-performance.md) toolearn about viewing hello performance impact of your top queries.</span></span>
