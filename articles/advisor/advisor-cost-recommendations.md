---
title: "aaaAzure Advisor költség javaslatok |} Microsoft Docs"
description: "Használja az Azure Advisor toooptimize hello költségét az Azure-környezetekhez."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 50f70c33a17f550c8753795435cdfddd51e409f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-cost-recommendations"></a>Költség javaslatokat biztosít

Az Advisor segítségével optimalizálhatja, és csökkentheti a teljes Azure töltött túl magas és üresjárati erőforrások azonosításával. Akkor is beolvasása költség hello javaslatokat **költség** hello Advisor irányítópult fület.

![Az Advisor költség lap](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>Optimalizálhatja a túl példányok átméretezésével töltött virtuális gép 
Bár egyes alkalmazás-forgatókönyvekre eredményezhet alacsony kihasználtságú úgy lett kialakítva, gyakran spórolhat a pénz hello méretét és számát a virtuális gépek kezeléséhez. Az Advisor figyeli a virtuális gépek használatának 14 napja, és majd azonosítja a kis-kihasználtságának virtuális gépek. Virtuális gépek, amelyek CPU-felhasználás csak 5 % vagy kevesebb és hálózati aktivitás a 7 MB vagy kisebb a négy vagy több napot minősülnek alacsony használt virtuális gépek.

Az Advisor toorun a virtuális gépet, a Folytatás becsült költségét hello, így kiválaszthatja azt le, illetve átméretezheti tooshut jeleníti meg.  

![Az Advisor költségeket, virtuális gépek átméretezésével kapcsolatos ajánlások](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-toomanage-performance-goals-of-multiple-sql-databases"></a>Használjon egy költséghatékony megoldás toomanage teljesítményértékeket több SQL-adatbázisok
Az Advisor azonosítja az SQL server-példányokat, amelyek kihasználhatják a rugalmas adatbáziskészlet létrehozása. A rugalmas adatbáziskészletek egy egyszerű és költséghatékony megoldást toomanage hello teljesítményértékeket több adatbázisok esetén, amelyek különböző használati szokásokról, adja meg. Az Azure rugalmas készletek kapcsolatos további információkért lásd: [Mi az Azure rugalmas készletek?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

![Az Advisor rugalmas adatbáziskészletek javaslatok költsége](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-tooaccess-cost-recommendations-in-azure-advisor"></a>Hogyan tooaccess költség javaslatok az Azure-tanácsadó

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Hello bal oldali ablaktáblában kattintson **további szolgáltatások**.

3. A hello szolgáltatás menü ablaktáblán, a **figyelés és felügyelet**, kattintson a **Azure Advisor**.  
 hello Advisor irányítópult jelenik meg.

4. Az hello Advisor irányítópultján kattintson a hello **költség** fülre.

5. Válassza ki hello előfizetést, amelyhez szeretné, hogy tooreceive javaslatokat, és kattintson **javaslatok beszerzése**.

> [!NOTE]
> Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban. Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra. Ez egy *egyszeri művelet*. Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.

## <a name="next-steps"></a>Következő lépések

További információ az Advisor-javaslatokra toolearn lásd:
* [Bevezetés tooAdvisor](advisor-overview.md)
* [Első lépések](advisor-get-started.md)
* [Teljesítmény javaslatokat biztosít](advisor-cost-recommendations.md)
* [Magas rendelkezésre állású javaslatokat biztosít](advisor-cost-recommendations.md)
* [Biztonsági javaslatokat biztosít](advisor-cost-recommendations.md)
