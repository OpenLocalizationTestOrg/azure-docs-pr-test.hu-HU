---
title: "AAA \"Azure Analysis Services Adventure Works oktatóanyag |} Microsoft dokumentumok\""
description: "Hello Adventure Works az oktatóanyag bemutatja az Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: 2df8b3ab4e8c4ffbe0086418d60fd2e2abd35e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services – Adventure Works-oktatóanyag

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez az oktatóanyag során tapasztalatokat ez hogyan toocreate és telepítését a hello 1 400 kompatibilitási szinttel rendelkező táblázatos modellek [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  

Ha új tooAnalysis szolgáltatások és a táblázatos modellezési, az oktatóprogram hello leggyorsabb módon toolearn hogyan toocreate és központi telepítése egy alapszintű táblázatos modell. Ha elvégezte a hello Előfeltételek helyben, elméletileg két toothree óra toocomplete között.  
  
## <a name="what-you-learn"></a>Ismertetett témák   
  
-   Hogyan toocreate egy új táblázatos modell: hello projekt **1 400 kompatibilitási szint** SSDT-ben.
  
-   Hogyan egy relációs adatbázis egy táblázatos modell projektbe tooimport adatait.  
  
-   Hogyan toocreate és kezelheti a hello modell táblák közötti kapcsolatokat.  
  
-   Hogyan toocreate számított oszlopokat, mértékeket és fő teljesítménymutatók, amelyek segítik a felhasználókat a kritikus fontosságú üzleti mérőszámok elemzése.  
  
-   Hogyan toocreate és perspektívák kezelhetők, és olyan hierarchiájára, amely segítségével a felhasználók könnyebben üzleti és alkalmazás-specifikus nézőpontok megadásával keresse meg a modell adatai.  
  
-   Hogyan toocreate partíciók, amely felosztani táblaadatok kisebb logikai részekkel rendelkeznek, amely független a többi partíciók feldolgozhatók.  
  
-   Hogyan toosecure modell objektumok és adatok tagjaival felhasználói szerepkörök létrehozásával.  
  
-   Hogyan toodeploy egy táblázatos modell tooan **Azure Analysis Services** vagy a helyszíni SQL Server 2017 Analysis Services-kiszolgáló.  
  
## <a name="prerequisites"></a>Előfeltételek  
toocomplete ebben az oktatóanyagban szüksége:  
  
-   Az Azure Analysis Services vagy az SQL Server 2017 Analysis Services-példány toodeploy a modellt. Regisztráljon az [Azure Analysis Services ingyenes próbaverziójára](https://azure.microsoft.com/services/analysis-services/), és [hozzon létre egy kiszolgálót](../analysis-services-create-server.md). Vagy regisztráljon, és töltse le az [SQL Server 2017 Community Technology Preview verziót](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp). 

-   Egy SQL Server Data Warehouse vagy az Azure SQL Data Warehouse hello [AdventureWorksDW2014 mintaadatbázis](http://go.microsoft.com/fwlink/?LinkID=335807). Ez a minta adatbázis hello adatok szükséges toocomplete ebben az oktatóanyagban magában foglalja. Töltse le az [SQL Server egyik ingyenes verzióját](https://www.microsoft.com/sql-server/sql-server-downloads). Vagy regisztráljon az [Azure SQL Database ingyenes próbaverziójára](https://azure.microsoft.com/services/sql-database/). 

    **Fontos:** Ha hello mintaadatbázis egy helyi SQL Server telepítése, és a modell tooan Azure Analysis Services-kiszolgálóhoz, telepít egy [helyszíni adatátjáró](../analysis-services-gateway.md) szükséges.

-   hello legújabb verziójának [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).

-   hello legújabb verziójának [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).    

-   Egy ügyfélalkalmazás (pl. [Power BI Desktop](https://powerbi.microsoft.com/desktop/) vagy Excel). 

## <a name="scenario"></a>Forgatókönyv  
Ez az oktatóanyag az Adventures Works Cycles nevű kitalált vállalatra épül. Az Adventure Works egy olyan nagyméretű, nemzetközi gyártási vállalat hoz létre, majd továbbítja a kerékpárt részek és Kellékek toocommercial piacok Észak-Amerikában, Európában és Ázsiában. hello vállalati 500 munkavállalók funkcióit használja. Az Adventure Works továbbá számos regionális értékesítési csapatot alkalmaz a különböző piacokon. A projekt toocreate egy táblázatos modell az értékesítési és marketing felhasználók tooanalyze hello AdventureWorksDW adatbázis Internet értékesítési adatait.  
  
az oktatóanyag hello toocomplete, el kell végeznie különböző során tapasztalatokat. Minden leckén belül feladatok várnak Önre. Minden egyes elvégzésével sorrendben szükség hello lecke befejezése. Előfordulhat, hogy egy bizonyos leckén belül több feladat is hasonló eredményt hoz, de a végrehajtásuk módja kismértékben különbözik. A metódus azt mutatja be, gyakran több mint egyirányú toocomplete egy feladatot, és képességek segítségével, hogy megismerte az előző megszerzett és feladatok toochallenge.  
  
hello hello megszerzett célja tooguide SSDT le a szerzői egy alapszintű táblázatos modell használatával számos hello szolgáltatást tartalmazza. Az egyes hello előző lecke épít, mert hello során tapasztalatokat, ahhoz el kell végeznie.
  
Ez az oktatóanyag nem biztosít egy kiszolgálóhoz az Azure-portálon egy kiszolgálón vagy adatbázisban kezelése az SSMS használatával, vagy használja az ügyfél felügyeletével kapcsolatos megszerzett alkalmazás toobrowse modell adatokat. 


## <a name="lessons"></a>Leckék  
Ez az oktatóanyag során tapasztalatokat következő hello tartalmazza:  
  
|Lecke|Becsült idő toocomplete|  
|----------|------------------------------|  
|[1 – Új táblázatos modellprojekt létrehozása](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 perc|  
|[2 – Az adatok beszerzése](../tutorials/aas-lesson-2-get-data.md)|10 perc|  
|[3 – Megjelölés dátumtáblaként](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 perc|  
|[4 – Kapcsolatok létrehozása](../tutorials/aas-lesson-4-create-relationships.md)|10 perc|  
|[5 – Számított oszlopok létrehozása](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 perc|
|[6 – Mértékek létrehozása](../tutorials/aas-lesson-6-create-measures.md)|30 perc|  
|[7 – Fő teljesítménymutatók (KPI) létrehozása](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 perc|  
|[8 – Perspektívák létrehozása](../tutorials/aas-lesson-8-create-perspectives.md)|5 perc|  
|[9 – Hierarchiák létrehozása](../tutorials/aas-lesson-9-create-hierarchies.md)|20 perc|  
|[10 – Partíciók létrehozása](../tutorials/aas-lesson-10-create-partitions.md)|15 perc|  
|[11 – Szerepkörök létrehozása](../tutorials/aas-lesson-11-create-roles.md)|15 perc|  
|[12 – Elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 perc| 
|[13 – Üzembe helyezés](../tutorials/aas-lesson-13-deploy.md)|5 perc|  
  
## <a name="supplemental-lessons"></a>Kiegészítő leckék  
Ezek a tapasztalatok nem szükséges toocomplete hello oktatóanyag, de jobb megértése speciális táblázatos modellben szolgáltatások szerzői hasznos lehet.  
  
|Lecke|Becsült idő toocomplete|  
|----------|------------------------------|  
|[Részletsorok](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 perc|
|[Dinamikus biztonság](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 perc|
|[Hézagos hierarchiák](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 perc| 

  
## <a name="next-steps"></a>Következő lépések  
tooget indult el, lásd: [lecke 1: hozzon létre egy új táblázatos modell projektet](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
  
  

