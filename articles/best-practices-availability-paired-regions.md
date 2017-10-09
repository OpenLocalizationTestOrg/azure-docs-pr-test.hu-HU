---
title: "Üzleti folytonossági és vészhelyreállítási helyreállítási (BCDR): Azure-régiókat párosítva |} Microsoft Docs"
description: "További tudnivalók az Azure regionális párosítási, tooensure, hogy az alkalmazások legyenek rugalmas data center esetén."
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
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 68a3a33a8e768c72fa296d42c9ab97049232d169
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Üzleti folytonossági és vészhelyreállítási helyreállítási (BCDR): Azure párosítva régiók

## <a name="what-are-paired-regions"></a>Mik azok a területek párosítva?

Azure hello világ különböző földrajzi működik. Egy Azure geográfiai legalább egy Azure-régióban tartalmazó hello world meghatározott területén. Egy Azure-régió, egy földrajzi hely, egy vagy több adatközpontok tartalmazó terület.

Minden Azure-régió, egy másik régióban belül hello párosított azonos földrajzi hely, egy regionális pár együtt elvégzése. hello kivétel, Dél-Brazília, amely egy kívül a földrajzi régióban párosított.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

1. ábra – Azure regionális pár diagramja

| földrajzi hely | Párhuzamos régiók |  |
|:--- |:--- |:--- |
| Ázsia |Kelet-Ázsia |Délkelet-Ázsia |
| Ausztrália |Kelet-Ausztrália |Délkelet-Ausztrália |
| Kanada |Közép-Kanada |Kelet-Kanada |
| Kína |Észak-Kína |Kelet-Kína|
| India |Közép-India |Dél-India |
| Japán |Kelet-Japán |Nyugat-Japán |
| Korea |Korea középső régiója |Korea déli régiója |
| Észak-Amerika |USA északi középső régiója |USA déli középső régiója |
| Észak-Amerika |USA keleti régiója |USA nyugati régiója |
| Észak-Amerika |USA 2. keleti régiója |USA középső régiója |
| Észak-Amerika |USA nyugati régiója, 2. |USA nyugati középső régiója |
| Európa |Észak-Európa |Nyugat-Európa |
| Japán |Kelet-Japán |Nyugat-Japán |
| Brazília |Dél-Brazília (1) |USA déli középső régiója |
| Az USA kormányzata |USA-beli államigazgatás – Iowa |USA-beli államigazgatás – Virginia |
| Az USA kormányzata |USA-beli államigazgatás – Virginia |USA-beli államigazgatás – Texas |
| Az USA kormányzata |USA-beli államigazgatás – Texas |USA-beli államigazgatás – Arizona |
| Az USA kormányzata |USA-beli államigazgatás – Arizona |USA-beli államigazgatás – Texas |
| EGYESÜLT KIRÁLYSÁG |Az Egyesült Királyság nyugati régiója |Az Egyesült Királyság déli régiója |
| Németország |Közép-Németország |Északkelet-Németország |

1. táblázat – Azure regionális párok leképezése

> (1) Dél-Brazília nem egyedi, mert egy kívül a saját földrajzi régióban párosított. Brazíliai Dél másodlagos régióba déli középső Régiójában, de déli középső Régiójában tartozó másodlagos régióba nincs Dél-Brazília.


Azt javasoljuk, hogy a munkaterhelések az Azure-elkülönítés és a rendelkezésre állási házirendektől regionális párok toobenefit közötti replikáció. Például az Azure rendszer tervezett üzembe helyezett frissítések egymás után (nem a hello ugyanannyi időt vesz igénybe) párosított régiók között. Ez azt jelenti, hogy hello esemény ritkán fordul elő egy hibás frissítés, még akkor is, mindkét régió nem vonatkozik a szabályzat egy időben. Egy átfogó leállás valószínű eseményben hello, továbbá helyreállítási kívül minden pár legalább egy régió van prioritása.

## <a name="an-example-of-paired-regions"></a>Példa párosított régiók
2. ábra egy elméleti alkalmazást használó hello regionális pár vész-helyreállítási jeleníti meg. zöld hello számok jelöljön ki hello kereszt-régió tevékenységek három Azure-szolgáltatáshoz (Azure számítási, tárolási, és az adatbázis), és hogyan vannak konfigurálva a tooreplicate régiók között. hello egyedi előnyei üzembe helyezése a teljes párosított régiók narancssárga hello számok vannak kiemelve.

![Párhuzamos régió előnyt áttekintése](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

2. ábra – elméleti Azure regionális pár

## <a name="cross-region-activities"></a>Kereszt-régió tevékenységek
A hivatkozott tooin, a 2. ábra.

![A PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure számítás (PaaS)** – el kell juttatnia további számítási erőforrásokat előre tooensure erőforrások elérhetők egy másik régióban során egy olyan vészhelyzet esetén. További információkért lásd: [Azure rugalmassági műszaki útmutatót](resiliency/resiliency-technical-guidance.md).

![Tárolási](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -Georedundáns tárolás (GRS) alapértelmezés szerint van konfigurálva, egy Azure Storage-fiók létrehozásakor. A GRS adatokat a rendszer automatikusan háromszor replikálja hello elsődleges régión belül, és állítsa be háromszor hello párosított régióban. További információkért lásd: [Azure tárolási redundancia lehetőségek](storage/common/storage-redundancy.md).

![Az Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL-adatbázisok** – az Azure SQL szokásos Georeplikáció, tranzakciók tooa párosított régió aszinkron replikálást konfigurálhat. A prémium szintű georeplikációt konfigurálható replikációs tooany régió hello world; azt javasoljuk azonban ezeket az erőforrásokat a legtöbb vész-helyreállítási eljárással párosított régióban telepít. További információkért lásd: [az Azure SQL Database Georeplikáció](sql-database/sql-database-geo-replication-overview.md).

![Erőforrás-kezelő](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -erőforrás-kezelő eredendően elkülönítést is biztosít logikai szolgáltatás felügyeleti összetevőinek régiók között. Ez azt jelenti, hogy egyetlen logikai hibák esélyét tooimpact egy másik.

## <a name="benefits-of-paired-regions"></a>Párhuzamos régiók előnyei
A hivatkozott tooin, a 2. ábra.  

![Elkülönítési](./media/best-practices-availability-paired-regions/5Orange.png)
**fizikai elkülönítése** – Ha lehetséges, Azure inkább legalább 300 miles elkülönítésének egy regionális pár adatközpont között, bár ez nem gyakorlati vagy lehetséges az összes földrajzi. Fizikai datacenter elkülönítése csökkenti természeti katasztrófa, polgári unrest, áramkimaradások vagy mindkét régió egyszerre érintő fizikai hálózati kimaradások hello esélyét. Elkülönítési tulajdonos toohello megkötések hello geográfiai (geográfiai mérete, teljesítmény és a hálózati infrastruktúra elérhetőségét, szabályzat, stb.) belül.  

![Replikációs](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform által biztosított replikációs** -egyes szolgáltatások, például a Georedundáns tárolást biztosítanak automatikus replikációs toohello párosított régióban.

![Helyreállítási](./media/best-practices-availability-paired-regions/7Orange.png)
**régió helyreállítási rendelés** – egy átfogó leállás hello esetben egy régió helyreállítási kívül minden pár előrébb. Központilag telepített alkalmazások kerülnek párosított régiók közötti garantáltan toohave hello régiók egyikéhez sem prioritású helyre. Ha egy alkalmazás, amely nem van párosítva régiók közötti lett telepítve, a helyreállítási később – a legrosszabb eset hello választott régiók előfordulhat, hogy hello helyre utolsó két toobe hello.

![Frissítések](./media/best-practices-availability-paired-regions/8Orange.png)
**egymást követő Adatfrissítés** – tervezett Azure rendszerfrissítések előkészítése már megkezdődött toopaired régiók egymás után (nem a hello ugyanannyi időt vesz igénybe) toominimize állásidő, hibák, és a logikai hibák hello ritka esetben hello hatása hibás frissítés.

![Adatok](./media/best-practices-availability-paired-regions/9Orange.png)
**adatok rezidens** – egy régióban található hello azonos földrajzi hely, mint a kulcspár (hello kivételt, Dél-Brazília) a rendelés toomeet rezidens követelményeit az adó és a törvény végrehajtás szempontjából joghatóság alá.
