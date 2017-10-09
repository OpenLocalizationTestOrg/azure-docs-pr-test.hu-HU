---
title: az Advisor aaaIntroduction tooAzure |} Microsoft Docs
description: "Az Azure központi telepítéseket használ, Azure Advisor toooptimize."
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
ms.openlocfilehash: 5d796fc06366221efdb6f1bda39ab3fb676abfd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-advisor"></a>Bevezetés tooAzure Advisor

Információ az Azure Advisor és annak főbb funkcióiról, és választ toofrequently kérdések.

## <a name="what-is-advisor"></a>Mi az az Advisor?
Az Advisor, amelynek segítségével személyre szabott felhő tanácsadó kövesse az ajánlott eljárások toooptimize az Azure-környezetekhez. Az erőforrás-konfigurációhoz és használat telemetriai adatai elemzi, és az megoldások, amelyek segítségével javíthatja hello költséghatékonyság, a teljesítmény, a magas rendelkezésre állású és az Azure-erőforrások biztonsági javasolja.

Az Advisor szolgáltatásban a következőket teheti:
* Szerezze be a proaktív, hajtható végre, és személyre szabott ajánlott eljárások a. 
* Hello teljesítmény, a biztonsági és a magas rendelkezésre állás, az erőforrások javítása a teljes Azure töltött lehetőségek tooreduce meghatározása.
* A javasolt műveletek beágyazott javaslatok beszerzése.

Az Advisor hello érheti el [Azure-portálon](https://aka.ms/azureadvisordashboard). Jelentkezzen be toohello [portal](https://portal.azure.com), jelölje be **Tallózás**, majd görgessen túl**Azure Advisor**. hello Advisor irányítópulton megjelenített személyre szabott javaslatok a kiválasztott előfizetéshez. 

hello javaslatok négy kategóriába oszthatók: 

* **Magas rendelkezésre állású**: tooensure és az üzleti szempontból kritikus fontosságú alkalmazások hello folytonosságának javításához. További információkért lásd: [Advisor magas rendelkezésre állású javaslatok](advisor-high-availability-recommendations.md).

* **Biztonsági**: toodetect fenyegetések és biztonsági rések toosecurity megszegése vezethet. További információkért lásd: [Advisor biztonsági javaslatok](advisor-security-recommendations.md).

* **Teljesítmény**: az alkalmazások tooimprove hello sebességét. További információkért lásd: [Advisor teljesítmény javaslatok](advisor-performance-recommendations.md).

* **Költség**: toooptimize, ami csökkenti az általános Azure televíziózással töltenek. További információkért lásd: [Advisor költség javaslatok](advisor-cost-recommendations.md).

  ![Az Advisor javaslat típusok](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban. Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra. Ez egy *egyszeri művelet*. Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.

További információk a javaslat toolearn gombra. Is megismerheti műveletek, hogy hajtsa végre a tootake előnyeit, lehetőséget, vagy hárítsa el a problémát. 

Az Advisor ezekre vonatkozó javaslatokat beágyazott műveletek vagy dokumentáció hivatkozásokat. Egy beágyazott művelet végigvezeti Önt egy "az interaktív felhasználói út" tooimplement azt. Dokumentáció hivatkozásra kattintva megtudhatja a toodocumentation, amely leírja, hogyan toomanually megvalósítása hello művelet. 

Az Advisor óránként frissíti a javaslatokat. Ha azonnali beavatkozásra tootake ajánlása, emlékeztet, hogy egy adott időszakra vonatkozóan, vagy zárja be azt. 

## <a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="how-do-i-access-advisor"></a>Hogyan érhetem el az Advisor?
Az Advisor hello érheti el [Azure-portálon](https://aka.ms/azureadvisordashboard). Jelentkezzen be toohello [portal](https://portal.azure.com), jelölje be **Tallózás**, majd görgessen túl**Azure Advisor**. hello Advisor irányítópulton megjelenített személyre szabott javaslatok a kiválasztott előfizetéshez. 

Advisor-javaslatokra hello virtuális gép erőforráspaneljének keresztül is megtekintheti. Válassza ki a virtuális gépet, és görgessen a tooAdvisor javaslatok hello menüben. 

### <a name="what-permissions-do-i-need-tooaccess-advisor"></a>Engedélyek mire van szükségem az tooaccess Advisor?

Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban. Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra. Ez egy *egyszeri művelet*. Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.

### <a name="how-often-are-advisor-recommendations-updated"></a>Milyen gyakran történik az Advisor-javaslatokra frissíteni?

Advisor-javaslatokra óránként frissíti.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Milyen erőforrásokat Advisor nyújt javaslatokat?

Az Advisor virtuális gépek rendelkezésre állási készletek, alkalmazásátjárót, alkalmazásszolgáltatások, SQL Server-kiszolgálók, SQL-adatbázisok és Redis Cache vonatkozó javaslatokkal szolgál.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Emlékeztet vagy hagyja figyelmen kívül az ajánlás?

toosnooze vagy elvetésére vonatkozó ajánlást, kattintson a hello **emlékeztet** gombra vagy hivatkozásra. Megadhat egy időtartamot időszak vagy select **soha** toodismiss hello javaslat.

## <a name="next-steps"></a>Következő lépések

További információ az Advisor-javaslatokra toolearn lásd:

* [Bevezetés az Advisor használatába](advisor-get-started.md)
* [Magas rendelkezésre állású javaslatokat biztosít](advisor-high-availability-recommendations.md)
* [Biztonsági javaslatokat biztosít](advisor-security-recommendations.md)
* [Teljesítmény javaslatokat biztosít](advisor-performance-recommendations.md)
* [Költség javaslatokat biztosít](advisor-cost-recommendations.md)
