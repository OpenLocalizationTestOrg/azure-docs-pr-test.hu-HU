---
title: "az SQL Data Warehouse szolgáltatással a Power BI aaaUse |} Microsoft Docs"
description: "Tippek a Power BI használatával az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a3a347493d07af6824a561567f05894cfe3c0471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a><span data-ttu-id="c62dd-103">A Power BI használja az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="c62dd-103">Use Power BI with SQL Data Warehouse</span></span>
<span data-ttu-id="c62dd-104">Az Azure SQL Database, a SQL Data Warehouse közvetlen kapcsolódás lehetővé teszi a felhasználó tooleverage hatékony logikai leküldéses közzététele mellett hello analitikai funkciók a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="c62dd-104">As with Azure SQL Database, SQL Data Warehouse Direct Connect allows user tooleverage powerful logical pushdown alongside hello analytical capabilities of Power BI.</span></span>  <span data-ttu-id="c62dd-105">Közvetlen csatlakozás esetén a lekérdezéseket a rendszer küldi vissza tooyour Azure SQL Data Warehouse valós idejű hello adatokba.</span><span class="sxs-lookup"><span data-stu-id="c62dd-105">With Direct Connect, queries are sent back tooyour Azure SQL Data Warehouse in real time as you explore hello data.</span></span>  <span data-ttu-id="c62dd-106">Ez, kombinálva hello méretezési az SQL Data Warehouse lehetővé teszi a számára toocreate dinamikus jelentéseket elleni terabájtos adatkészleteket percben.</span><span class="sxs-lookup"><span data-stu-id="c62dd-106">This, combined with hello scale of SQL Data Warehouse, enables users toocreate dynamic reports in minutes against terabytes of data.</span></span>  <span data-ttu-id="c62dd-107">Ezenkívül hello bevezetése hello nyissa meg a Power BI gomb lehetővé teszi a felhasználók toodirectly csatlakozni a Power BI tootheir SQL Data Warehouse Azure egyéb részeitől információk gyűjtése nélkül.</span><span class="sxs-lookup"><span data-stu-id="c62dd-107">In addition, hello introduction of hello Open in Power BI button allows users toodirectly connect Power BI tootheir SQL Data Warehouse without collecting information from other parts of Azure.</span></span>

<span data-ttu-id="c62dd-108">Ha közvetlen kapcsolódás használatával adjon Megjegyzés:</span><span class="sxs-lookup"><span data-stu-id="c62dd-108">When using Direct Connect please note:</span></span>

* <span data-ttu-id="c62dd-109">Adja meg a hello teljes kiszolgálónév (lásd alább olvashat) csatlakozáskor</span><span class="sxs-lookup"><span data-stu-id="c62dd-109">Specify hello fully qualified server name when connecting (see below for more details)</span></span>
* <span data-ttu-id="c62dd-110">Győződjön meg arról, a tűzfalszabályok hello adatbázis vannak beállítva túl "hozzáférési tooAzure-szolgáltatások engedélyezése".</span><span class="sxs-lookup"><span data-stu-id="c62dd-110">Ensure firewall rules for hello database are configured too"Allow access tooAzure services".</span></span>
* <span data-ttu-id="c62dd-111">Minden művelet, például jelöljön ki egy oszlopot, vagy hozzáad egy szűrőt közvetlenül fogja kérdezni hello data warehouse-bA</span><span class="sxs-lookup"><span data-stu-id="c62dd-111">Every action such as selecting a column or adding a filter will  directly query hello data warehouse</span></span>
* <span data-ttu-id="c62dd-112">Csempék frissítése körülbelül 15 percenként (a frissítés nem kell ütemezett toobe)</span><span class="sxs-lookup"><span data-stu-id="c62dd-112">Tiles are refreshed approximately every 15 minutes (refresh does not need toobe scheduled)</span></span>
* <span data-ttu-id="c62dd-113">A Q & A nincs közvetlen kapcsolódás adatkészletek</span><span class="sxs-lookup"><span data-stu-id="c62dd-113">Q&A is not available for Direct Connect datasets</span></span>
* <span data-ttu-id="c62dd-114">Sémaváltozások nem átveszik automatikusan</span><span class="sxs-lookup"><span data-stu-id="c62dd-114">Schema changes are not picked up automatically</span></span>
* <span data-ttu-id="c62dd-115">Minden közvetlen kapcsolódás lekérdezések időtúllépést okoz 2 perc múlva</span><span class="sxs-lookup"><span data-stu-id="c62dd-115">All Direct Connect queries will time out after 2 minutes</span></span>

<span data-ttu-id="c62dd-116">E korlátozások és a megjegyzések tooimprove hello lép folyamatos módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="c62dd-116">These restrictions and notes may change as we continue tooimprove hello experiences.</span></span> <span data-ttu-id="c62dd-117">hello lépéseket tooconnect részleteket lejjebb olvashatja.</span><span class="sxs-lookup"><span data-stu-id="c62dd-117">hello steps tooconnect are detailed below.</span></span>  

## <a name="using-hello-open-in-power-bi-button"></a><span data-ttu-id="c62dd-118">A hello "Megnyitás Power BI-ban" gombra kattintva</span><span class="sxs-lookup"><span data-stu-id="c62dd-118">Using hello ‘Open in Power BI’ button</span></span>
<span data-ttu-id="c62dd-119">hello legegyszerűbb módja toomove a SQL Data Warehouse között a Power BI hello nyissa meg a Power BI gomb jelenti.</span><span class="sxs-lookup"><span data-stu-id="c62dd-119">hello easiest way toomove between your SQL Data Warehouse and Power BI is with hello Open in Power BI button.</span></span> <span data-ttu-id="c62dd-120">Ez a gomb lehetővé teszi a tooseamlessly készítését új irányítópultok a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="c62dd-120">This button allows you tooseamlessly begin creating new dashboards in Power BI.</span></span>  

1. <span data-ttu-id="c62dd-121">lépések tooget tooyour SQL Data Warehouse-példányhoz a klasszikus Azure portál hello lépjen.</span><span class="sxs-lookup"><span data-stu-id="c62dd-121">tooget started navigate tooyour SQL Data Warehouse instance in hello Azure Classic Portal.</span></span>
2. <span data-ttu-id="c62dd-122">Hello "Megnyitás Power BI-ban" gombra.</span><span class="sxs-lookup"><span data-stu-id="c62dd-122">Click hello 'Open in Power BI' button.</span></span>
3. <span data-ttu-id="c62dd-123">Ha nem tudja toosign dolgozunk a közvetlenül, vagy ha nem rendelkezik Power BI-fiókja, szüksége lesz toosign a.</span><span class="sxs-lookup"><span data-stu-id="c62dd-123">If we are not able toosign you in directly, or if you do not have a Power BI account, you will need toosign-in.</span></span>  
4. <span data-ttu-id="c62dd-124">A rendszer átirányítja az SQL Data Warehouse csatlakozási oldalán, az SQL Data Warehouse hello adataival toohello előre feltöltve.</span><span class="sxs-lookup"><span data-stu-id="c62dd-124">You will be directed toohello SQL Data Warehouse connection page, with hello information from your SQL Data Warehouse pre-populated.</span></span>
5. <span data-ttu-id="c62dd-125">A hitelesítő adatok megadása után a teljes csatlakoztatott tooyour SQL Data Warehouse lesz.</span><span class="sxs-lookup"><span data-stu-id="c62dd-125">After entering your credentials you will be fully connected tooyour SQL Data Warehouse.</span></span>

## <a name="connecting-through-hello-power-bi-portal"></a><span data-ttu-id="c62dd-126">Hello Power BI portálon keresztül csatlakozó</span><span class="sxs-lookup"><span data-stu-id="c62dd-126">Connecting through hello Power BI portal</span></span>
<span data-ttu-id="c62dd-127">Hozzáadása toousing hello nyissa meg a Power BI gomb, a felhasználók is kapcsolódhatnak az SQL Data Warehouse tootheir hello Power BI-portál.</span><span class="sxs-lookup"><span data-stu-id="c62dd-127">In addition toousing hello Open in Power BI button, users can also connect tootheir SQL Data Warehouse through hello Power BI Portal.</span></span>

1. <span data-ttu-id="c62dd-128">Kattintson az "Adatok beolvasása" hello navigációs ablaktábla hello alján.</span><span class="sxs-lookup"><span data-stu-id="c62dd-128">Click 'Get Data' at hello bottom of hello navigation pane.</span></span>
2. <span data-ttu-id="c62dd-129">Válassza ki a "Adatbázisokat".</span><span class="sxs-lookup"><span data-stu-id="c62dd-129">Select 'Databases'.</span></span>
3. <span data-ttu-id="c62dd-130">Egyszer hello adatbázisok lapon válassza az "Azure SQL Data Warehouse", és kattintson a "Csatlakozás".</span><span class="sxs-lookup"><span data-stu-id="c62dd-130">Once on hello Databases page, select 'Azure SQL Data Warehouse' and then click 'Connect'.</span></span>
4. <span data-ttu-id="c62dd-131">Adja meg a hello szükséges csatlakozási adatait.</span><span class="sxs-lookup"><span data-stu-id="c62dd-131">Enter hello necessary connection information.</span></span>  <span data-ttu-id="c62dd-132">A kiszolgáló és adatbázis nevét a hello Azure portálon található.</span><span class="sxs-lookup"><span data-stu-id="c62dd-132">Your server name and database name can be found in hello Azure Portal.</span></span>
5. <span data-ttu-id="c62dd-133">A rendszer átirányítja vissza toohello főoldala a Power bi-ban, és ha a kapcsolat létrejött egy új "Adatkészletek" bejegyzést a példány hello nevű fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="c62dd-133">You will be directed back toohello main page of Power BI and after your connection is made a new entry under 'Datasets' will appear with hello name of your instance.</span></span>  
6. <span data-ttu-id="c62dd-134">Rákattinthat a hello új adatkészlet tooexplore összes hello táblák és nézetek az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c62dd-134">You can click on hello new dataset tooexplore all of hello tables and views in your database.</span></span> <span data-ttu-id="c62dd-135">Jelöljön ki egy oszlopot elküld egy lekérdezés hátsó toohello forrást, dinamikusan létrehozása a visual.</span><span class="sxs-lookup"><span data-stu-id="c62dd-135">Selecting a column will send a query back toohello source, dynamically creating your visual.</span></span> <span data-ttu-id="c62dd-136">Ezek a látványelemek egy új jelentést lehessen menteni, és vissza tooyour irányítópulton rögzítette.</span><span class="sxs-lookup"><span data-stu-id="c62dd-136">These visuals can be saved in a new report and pinned back tooyour dashboard.</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
