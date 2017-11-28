---
title: "Portálok a létrehozása és módosítása az Azure Naplóelemzés napló lekérdezések |} Microsoft Docs"
description: "Ez a cikk ismerteti a portálok, melyekkel az Azure Naplóelemzés létrehozásához és szerkesztéséhez jelentkezzen kereséseket."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 29dfa31d38f85574f84ed351bc5c26224b1a7e16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a><span data-ttu-id="fe0e6-103">Portálok a létrehozása és módosítása az Azure Naplóelemzés napló lekérdezések</span><span class="sxs-lookup"><span data-stu-id="fe0e6-103">Portals for creating and editing log queries in Azure Log Analytics</span></span>

> [!NOTE]
> <span data-ttu-id="fe0e6-104">Ez a cikk ismerteti az Azure Naplóelemzés az új lekérdezés nyelven portálok.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-104">This article describes portals in Azure Log Analytics using the new query language.</span></span>  <span data-ttu-id="fe0e6-105">További információ az új nyelv, és a frissítés a következő munkaterület eljárás első [Azure Naplóelemzési munkaterület frissítése új naplófájl-keresési](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="fe0e6-105">You can learn more about the new language and get the procedure to upgrade your workspace at [Upgrade your Azure Log Analytics workspace to new log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="fe0e6-106">Ha a munkaterületet még nem lett frissítve az új lekérdezési nyelv, tekintse át [található adatokat, és napló kereséseket a Naplóelemzési](log-analytics-log-searches.md) információt a naplófájl-keresési portál jelenlegi verziója.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-106">If your workspace hasn't been upgraded to the new query language, you should refer to [Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on the current version of the Log Search portal.</span></span>

<span data-ttu-id="fe0e6-107">A különböző pontjain lehet elérni a Naplóelemzési napló keresések segítségével adatainak lekérése a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-107">You use log searches in a variety of places throughout Log Analytics to retrieve data from the workspace.</span></span>  <span data-ttu-id="fe0e6-108">Ténylegesen létrehozásának, és a lekérdezések használata interaktív visszaadott adatok mellett, ha szerkesztésének, két lehetősége, hogy az alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-108">For actually creating and editing queries in addition to working interactively with returned data though, you have two options that are described below.</span></span>  

## <a name="log-search-portal"></a><span data-ttu-id="fe0e6-109">Naplófájl-keresési portál</span><span class="sxs-lookup"><span data-stu-id="fe0e6-109">Log search portal</span></span>
<span data-ttu-id="fe0e6-110">A naplófájl-keresési portál nem érhető el az Azure-portálon vagy az OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-110">The Log Search portal is accessible from the Azure portal or the OMS portal.</span></span>  <span data-ttu-id="fe0e6-111">Alkalmas alapvető lekérdezések külön sorba hozható létre.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-111">It's suitable for creating basic queries that can be created on a single line.</span></span>  <span data-ttu-id="fe0e6-112">A naplófájl-keresési portál egy külső portál megnyitása nélkül használható, és számos feladatot végrehajtani a napló keresések többek között a riasztási szabályok létrehozásához, számítógép-csoportok létrehozásáról és a lekérdezés eredményeit az használhatja.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-112">The Log Search portal can be used without launching an external portal, and you can use it to perform a variety of functions with log searches including creating alert rules, creating computer groups, and exporting the results of the query.</span></span>  

<span data-ttu-id="fe0e6-113">A naplófájl-keresési portál több szolgáltatásokat nyújt a lekérdezés szerkesztése a lekérdezési nyelv teljes ismerete nélkül.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-113">The Log Search portal provides multiple features for editing the query without having a full knowledge of the query language.</span></span>  <span data-ttu-id="fe0e6-114">Ezeket a funkciókat az összegzését kaphat [létrehozás napló keresi a keresési napló portálon Azure Naplóelemzés](log-analytics-log-search-log-search-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fe0e6-114">You can get a summary of these features in [Create log searches in Azure Log Analytics using the Log Search portal](log-analytics-log-search-log-search-portal.md).</span></span>


![Naplófájl-keresési portál](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a><span data-ttu-id="fe0e6-116">Speciális Analytics portál</span><span class="sxs-lookup"><span data-stu-id="fe0e6-116">Advanced Analytics portal</span></span>
<span data-ttu-id="fe0e6-117">A speciális elemzés portál egy dedikált portál, amely nem érhető el, a keresési napló portálon speciális funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-117">The Advanced Analytics portal is a dedicated portal that provides advanced functionality not available in the Log Search portal.</span></span>  <span data-ttu-id="fe0e6-118">Szolgáltatások közé tartozik a lekérdezés több sorban szerkesztése, szelektív módon hajtható végre a kódot, környezetfüggő Intellisense és intelligens Analytics képes.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-118">Features include the ability to edit a query on multiple lines, selectively execute code, context sensitive Intellisense, and Smart Analytics.</span></span>  <span data-ttu-id="fe0e6-119">Az Advanced Analytics portál legalkalmasabb összetett lekérdezések, amelyek vagy mentett napló keresést vagy másolja és illeszthetők be más Naplóelemzési elemek.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-119">The Advanced Analytics portal is most suitable for designing complex queries that are either saved as a log search or copied and pasted into other Log Analytics elements.</span></span>  <span data-ttu-id="fe0e6-120">A naplófájl-keresési portál tartalmaz egy hivatkozást a Advanced Analytics portálon indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-120">You launch the Advanced Analytics portal from a link in the Log Search portal.</span></span>

![Speciális Analytics portál](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


<span data-ttu-id="fe0e6-122">A speciális funkciók miatt általában használni a speciális elemzés portal az elsődleges eszközként létrehozása és szerkesztése a lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-122">Because of its advanced features, you'll usually use the Advanced Analytics portal as your primary tool for creating and editing queries.</span></span>  <span data-ttu-id="fe0e6-123">Egyszer meghatározta, hogy a lekérdezés megfelelően működik-e, majd lesz másolja és illessze be máshol például a naplófájl-keresési portál vagy az adatforrásnézet-tervezőből.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-123">Once you've determined that the query works as expected, then you'll copy and paste it elsewhere such as the Log Search portal or View Designer.</span></span>  <span data-ttu-id="fe0e6-124">Mivel az Advanced Analytics portál már támogatja több sort tartalmazó lekérdezések ellenére kell venni a figyelembe a következőket, amikor a lekérdezés másol ezen a portálon.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-124">Because the Advanced Analytics portal supports queries with multiple lines though, you need to take the following into consideration when copying a query from this portal.</span></span>

- <span data-ttu-id="fe0e6-125">Megjegyzések a lekérdezés kell távolítania, mielőtt másolni és beilleszteni egy másik helyre.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-125">Comments must be removed from the query before it's copied and pasted into another location.</span></span>  <span data-ttu-id="fe0e6-126">Egy sor Megjegyzés két perjeleket az azt megelőző (/ /).</span><span class="sxs-lookup"><span data-stu-id="fe0e6-126">You can comment a line by preceding it with two slashes (//).</span></span>  <span data-ttu-id="fe0e6-127">Ha több sor lekérdezés beillesztése egy egysoros, a sortörések törlődnek.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-127">When you paste a multiple line query into a single line, line breaks are removed.</span></span>  <span data-ttu-id="fe0e6-128">Megjegyzések érhetők el, ha az összes karakter után az első Megjegyzés a Megjegyzés részének számít.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-128">If comments are included, all characters after the first comment are considered part of the comment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="fe0e6-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe0e6-129">Next steps</span></span>

- <span data-ttu-id="fe0e6-130">Az oktatóanyag bízná a [naplófájl-keresési portál](log-analytics-log-search-log-search-portal.md) vagy a [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) létrehozhat olyan lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-130">Walk through a tutorial on using the [Log Search portal](log-analytics-log-search-log-search-portal.md) or the [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) to create queries.</span></span>
- <span data-ttu-id="fe0e6-131">Tekintse meg a [útmutató a lekérdezések írásáról](https://go.microsoft.com/fwlink/?linkid=856078) az új lekérdezés nyelven.</span><span class="sxs-lookup"><span data-stu-id="fe0e6-131">Check out a [tutorial on writing queries](https://go.microsoft.com/fwlink/?linkid=856078) using the new query language.</span></span>
