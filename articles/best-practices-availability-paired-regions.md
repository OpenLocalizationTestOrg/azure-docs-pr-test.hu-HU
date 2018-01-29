---
title: "Üzleti folytonossági és vészhelyreállítási helyreállítási (BCDR): Azure-régiókat párosítva |} Microsoft Docs"
description: "Tudnivalók Azure regionális párosítása, ezzel biztosíthatja, hogy alkalmazások rugalmas data center esetén."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: c2d0a21c-2564-4d42-991a-bc31723f61a4
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: raynew
ms.openlocfilehash: 394f353837433e241e4da6f4accdb5eaa24bae46
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Üzleti folytonossági és vészhelyreállítási helyreállítási (BCDR): Azure párosítva régiók

## <a name="what-are-paired-regions"></a>Mik azok a területek párosítva?

A világ különböző földrajzi Azure működik. Egy Azure geográfiai egy meghatározott területen a világ, amely tartalmaz legalább egy Azure-régióban. Egy Azure-régió, egy földrajzi hely, egy vagy több adatközpontok tartalmazó terület.

Minden Azure-régió, ugyanazon a földrajzi együtt és egy regionális pár belül egy másik régióban van párosítva. A kivétel: Dél-Brazília, amely egy kívül a földrajzi régióban párosított.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

1. ábra – Azure regionális pár diagramja

| földrajzi hely | Párosított régiók |  |
|:--- |:--- |:--- |
| Ázsia |Kelet-Ázsia |Délkelet-Ázsia |
| Ausztrália |Kelet-Ausztrália |Délkelet-Ausztrália |
| Kanada |Közép-Kanada |Kelet-Kanada |
| Kína |Észak-Kína |Kelet-Kína|
| India |Közép-India |Dél-India |
| India |Nyugat-Indiában (1) |Dél-India |
| Japán |Kelet-Japán |Nyugat-Japán |
| Korea |Korea középső régiója |Korea déli régiója |
| Észak-Amerika |USA északi középső régiója |USA déli középső régiója |
| Észak-Amerika |USA keleti régiója |USA nyugati régiója |
| Észak-Amerika |USA 2. keleti régiója |USA középső régiója |
| Észak-Amerika |USA nyugati régiója, 2. |USA nyugati középső régiója |
| Európa |Észak-Európa |Nyugat-Európa |
| Japán |Kelet-Japán |Nyugat-Japán |
| Brazília |Dél-Brazília (2) |USA déli középső régiója |
| Az USA kormányzata |USA – (kormányzati) Iowa (3) |USA-beli államigazgatás – Virginia |
| Az USA kormányzata |USA – (kormányzati) Virginia (4) |USA-beli államigazgatás – Texas |
| Az USA kormányzata |USA-beli államigazgatás – Arizona |USA-beli államigazgatás – Texas |
| USA védelmi Minisztériuma |US DoD – Kelet |US DoD – Középső régió |
| EGYESÜLT KIRÁLYSÁG |Az Egyesült Királyság nyugati régiója |Az Egyesült Királyság déli régiója |
| Németország |Közép-Németország |Északkelet-Németország |

1. táblázat – Azure regionális párok leképezése

- > (1) Nyugat-India eltér, mert egy másik régióban csak egy irányban párosított. Nyugat-Indiában másodlagos régióba Dél-Indiában, Dél-Indiában másodlagos régióba viszont közép-Indiában.
- > (2) Dél-Brazília nem egyedi, mert egy kívül a saját földrajzi régióban párosított. Brazíliai Dél másodlagos régióba déli középső Régiójában, de déli középső Régiójában tartozó másodlagos régióba nincs Dél-Brazília.
- > (3) USA – (kormányzati) Iowa másodlagos régióba Velünk – (kormányzati) Virginia, de Velünk – (kormányzati) Virginia másodlagos régióba nincs Velünk – (kormányzati) Iowa.
- > (4) USA – (kormányzati) Virginia másodlagos régióba Velünk – (kormányzati) Texas, de Velünk – (kormányzati) Texas másodlagos régióba nincs Velünk – (kormányzati) Virginia.


Azt javasoljuk, hogy kihasználják az Azure-elkülönítés és a rendelkezésre állási házirendektől regionális párok közötti replikáció munkaterhelések. Például az Azure rendszer tervezett üzembe helyezett frissítések egymás után (nem egyszerre) párosított régiók között. Ez azt jelenti, hogy még abban a ritka esetben egy hibás frissítés, mindkét régió nem vonatkozik a szabályzat egy időben. A széles körű kimaradás valószínű esemény, továbbá helyreállítási kívül minden pár legalább egy régió van prioritása.

## <a name="an-example-of-paired-regions"></a>Példa párosított régiók
2. ábra alatt egy elméleti alkalmazást, amely használja a regionális pár vész-helyreállítási jeleníti meg. A zöld számok jelölje ki a kereszt-régió tevékenységek három Azure-szolgáltatások (Azure számítási, tárolási, és az adatbázis), és azok miként vannak konfigurálva régiók közötti replikáció. Üzembe helyezése a teljes párosított régiók egyedi előnyeit a narancssárga számmal vannak kiemelve.

![Párhuzamos régió előnyt áttekintése](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

2. ábra – elméleti Azure regionális pár

## <a name="cross-region-activities"></a>Kereszt-régió tevékenységek
2. ábra az említett.

![A PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure számítás (PaaS)** – el kell juttatnia előre biztosításához erőforrások elérhetők egy másik régióban során egy olyan vészhelyzet esetén további számítási erőforrásokat. További információkért lásd: [Azure rugalmassági műszaki útmutatót](resiliency/resiliency-technical-guidance.md).

![Tárolási](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -Georedundáns tárolás (GRS) alapértelmezés szerint van konfigurálva, egy Azure Storage-fiók létrehozásakor. A GRS adatokat a rendszer automatikusan háromszor replikálja az elsődleges régióban, és állítsa be háromszor párosított régióban. További információkért lásd: [Azure tárolási redundancia lehetőségek](storage/common/storage-redundancy.md).

![Az Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL-adatbázisok** – az Azure SQL szokásos Georeplikáció, párosított régióban tranzakciók aszinkron replikálást konfigurálhat. A prémium szintű georeplikációt replikálást konfigurálhat bármely régióban a világ; azt javasoljuk azonban ezeket az erőforrásokat a legtöbb vész-helyreállítási eljárással párosított régióban telepít. További információkért lásd: [az Azure SQL Database Georeplikáció](sql-database/sql-database-geo-replication-overview.md).

![Erőforrás-kezelő](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -erőforrás-kezelő eredendően elkülönítést is biztosít logikai szolgáltatás felügyeleti összetevőinek régiók között. Ez azt jelenti, hogy egyetlen logikai hibák kevésbé valószínű, hogy egy másik hatással.

## <a name="benefits-of-paired-regions"></a>Párhuzamos régiók előnyei
2. ábra az említett.  

![Elkülönítési](./media/best-practices-availability-paired-regions/5Orange.png)
**fizikai elkülönítése** – Ha lehetséges, Azure inkább legalább 300 miles elkülönítésének egy regionális pár adatközpont között, bár ez nem gyakorlati vagy lehetséges az összes földrajzi. Fizikai datacenter elkülönítése csökkenti a természeti katasztrófa, polgári unrest, áramkimaradások vagy mindkét régió egyszerre érintő fizikai hálózati kimaradások esélyét. Az elkülönítési belül (geográfiai mérete, teljesítmény és a hálózati infrastruktúra elérhetőségét, szabályzat, stb.) a földrajzi megkötések.  

![Replikációs](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform által biztosított replikációs** -bizonyos szolgáltatások, például a Georedundáns tárolás párosított régió automatikus replikációs adja meg.

![Helyreállítási](./media/best-practices-availability-paired-regions/7Orange.png)
**régió helyreállítási rendelés** – abban az esetben, ha az egy átfogó leállás egy régió helyreállítási kívül minden pár előrébb. Központilag telepített alkalmazások kerülnek párosított régiók közötti garantáltan kell a helyre prioritással rendelkező régiók egyikéhez sem. Ha egy alkalmazás lett telepítve, amely nem van párosítva régiók között, helyreállítási előfordulhat, hogy később – a legrosszabb esetben lehet, hogy a kiválasztott régiók a utolsó két helyre kell állítani.

![Frissítések](./media/best-practices-availability-paired-regions/8Orange.png)
**egymást követő Adatfrissítés** – tervezett Azure rendszerfrissítések előkészítése már megkezdődött párosított régiók egymás után (nem egyszerre) állásidő, hibák, és a ritka esemény rossz frissítés logikai hibák hatásának minimalizálása érdekében.

![Adatok](./media/best-practices-availability-paired-regions/9Orange.png)
**adatok rezidens** – ahhoz, hogy az adó és a törvény végrehajtás szempontjából joghatóság adatok rezidens követelményeknek, a pár (kivéve a Dél-Brazília) azonos földrajzi belül található egy régiót.
