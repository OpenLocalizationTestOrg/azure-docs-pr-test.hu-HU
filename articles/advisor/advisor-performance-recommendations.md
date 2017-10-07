---
title: "aaaAzure Advisor teljesítmény javaslatok |} Microsoft Docs"
description: "Használja az Azure-környezetekhez Advisor toooptimize hello teljesítményét."
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
ms.openlocfilehash: eb3d928664717f6f322132ac740f42015f56b76e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-performance-recommendations"></a>Teljesítmény javaslatokat biztosít

Az Azure teljesítménye javaslatokat biztosít hello sebesség és az üzleti szempontból kritikus fontosságú alkalmazások válaszképességének javítása érdekében. Letölthető teljesítmény javaslatok Advisor a hello **teljesítmény** hello Advisor irányítópult lapja.

![Az Advisor teljesítmény lap](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a>Az SQL DB Advisor adatbázis teljesítményének növelése

Az Advisor egy egységes, összevont nézetének összes Azure-erőforrások javaslatokat biztosít. Azt integrálódik az SQL Database Advisor toobring meg az SQL Azure database hello teljesítményének javítására vonatkozó javaslatokat. SQL Database Advisor segédprogramot az SQL Azure-adatbázisok hello teljesítményét értékeli a használati előzmények elemzésével. Lehetőséget kínál a javaslatok legmegfelelőbb, átlagos munkaterhelésre hello adatbázis futtatásához. 

> [!NOTE]
> tooget ajánlásokat, egy adatbázist kell rendelkeznie kapcsolatos használati hetente, és a hét belül kell konzisztens tevékenységet észleltünk a fiókjában. SQL Database Advisor segédprogramot a lekérdezés konzisztens mintára mint a véletlenszerű felszakadásáig tevékenység könnyebben optimalizálható.

SQL Database Advisor kapcsolatos további információkért lásd: [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![SQL-adatbázissal kapcsolatos javaslatok](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a>Redis gyorsítótár teljesítményének és megbízhatóságának növelése

Az Advisor azonosítja a Redis Cache példány ahol teljesítményt hátrányosan érintheti által magas memóriahasználat, a kiszolgáló terhelését, a hálózati sávszélesség vagy a nagyszámú ügyfél kapcsolatok. Az Advisor is biztosítja a legjobb potenciális problémák elkerülése javaslatok toohelp eljárásokat. További információ a Redis Cache javaslatok: [Redis gyorsítótár Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>App Service-teljesítmény és megbízhatóság javítása

Azure Advisor integrálja a alkalmazásszolgáltatások felhasználói élmény javítása és felderítésére vonatkozó platform képességei ajánlott eljárásait. Például App Services ajánlások a következők:
* Ahol memória vagy a Processzor-erőforrások elfogytak a megoldás beállítások app futtatókörnyezetek által példányok észlelése.
* Ahol helymegosztást erőforrások – például webes alkalmazásokat és adatbázisokat példányok észlelésének javíthatja a teljesítményt és az alacsonyabb költségek. 

App Service szolgáltatások javaslatok kapcsolatos további információkért lásd: [ajánlott eljárások az Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![App Services javaslatok](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-tooaccess-performance-recommendations-in-advisor"></a>Hogyan tooaccess Advisor teljesítmény javaslatok

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Hello bal oldali ablaktáblában kattintson **további szolgáltatások**.

3. A hello szolgáltatás menü ablaktáblán, a **figyelés és felügyelet**, kattintson a **Azure Advisor**.  
 hello Advisor irányítópult jelenik meg.

4. Az hello Advisor irányítópultján kattintson a hello **teljesítmény** fülre.

5. Válassza ki hello előfizetést, amelyhez szeretné, hogy tooreceive javaslatokat, és kattintson **javaslatok beszerzése**.

> [!NOTE]
> Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban. Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra. Ez egy *egyszeri művelet*. Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.

## <a name="next-steps"></a>Következő lépések

További információ az Advisor-javaslatokra toolearn lásd:

* [Bevezetés tooAdvisor](advisor-overview.md)
* [Bevezetés az Advisor használatába](advisor-get-started.md)
* [Költség javaslatokat biztosít](advisor-performance-recommendations.md)
* [Magas rendelkezésre állású javaslatokat biztosít](advisor-high-availability-recommendations.md)
* [Biztonsági javaslatokat biztosít](advisor-security-recommendations.md)

