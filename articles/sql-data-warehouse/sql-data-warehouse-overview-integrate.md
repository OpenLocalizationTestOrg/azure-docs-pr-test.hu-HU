---
title: "aaaBuild megoldások integrálva az SQL Data Warehouse |} Microsoft Docs"
description: "Eszközök és a partnerek, amelyekbe beépül az SQL Data Warehouse-megoldás. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: c8a4202dd84305bea4e4c2faf0e4791d026e794f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a>Használja ki az egyéb szolgáltatásokat az SQL Data Warehouse szolgáltatással
Ezenkívül tooits alapvető funkcióit, az SQL Data Warehouse lehetővé teszi, hogy a felhasználók tooleverage számos hello más szolgáltatásokkal együtt, az Azure-ban.  Pontosabban jelenleg a rendszer átirányította toodeeply integrálása hello alábbi lépéseket:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Dolgozunk a további szolgáltatások tooconnect hello Azure ökoszisztéma között.

## <a name="power-bi"></a>Power BI
A Power BI-integráció lehetővé teszi tooleverage hello számítási teljesítményt az SQL Data Warehouse hello dinamikus jelentéskészítéssel és a Power BI a képi megjelenítés. A Power BI integrációs jelenleg tartalmazza:

* **Közvetlen csatlakozás**: egy speciális logikai leküldéses közzététele elleni SQL Data Warehouse-kapcsolatot.  Magasabb szinten gyorsabb elemzése biztosít.
* **Nyissa meg a Power BI**: hello "Megnyitás Power BI-ban" gombra példány információk tooPower BI, lehetővé téve a zökkenőmentesebb kapcsolat továbbítja.

Lásd: [integráció a Power BI](sql-data-warehouse-integrate-power-bi.md) vagy hello [Power BI-dokumentáció](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) további információt.

## <a name="azure-data-factory"></a>Azure Data Factory
Az Azure Data Factory lehetőséget ad a felhasználók a felügyelt platform toocreate összetett kivonat-betöltési folyamatok.  Az SQL Data Warehouse-integráció az Azure Data Factoryvel hello következőket tartalmazza:

* **Tárolt eljárások**: az SQL Data Warehouse a tárolt eljárások végrehajtása hello Levezényelni.
* **Másolás**: használata ADF toomove adatokat az SQL Data Warehouse.  Ez a művelet ADF szabványos adatok mechanizmusát vagy a PolyBase hello alatt hozzá van rendelve. 

Lásd: [integráció az Azure Data Factoryvel](sql-data-warehouse-integrate-azure-data-factory.md) vagy hello [Azure Data Factory dokumentáció](https://azure.microsoft.com/documentation/services/data-factory/) további információt.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Az Azure Machine Learning egy teljes körűen felügyelt analytics szolgáltatás, amely lehetővé teszi, hogy a felhasználók toocreate bonyolult modellek prediktív eszközök számos kihasználva.  Az SQL Data Warehouse a következő funkciókat hello támogatott, a forrás- és a célkiszolgálón, ezek a modellek:

* **Adatok olvasása:** modellek meghajtó léptékű T-SQL használatával az SQL Data Warehouse ellen.
* **Adatok írása:** véglegesítési bármely modellből módosításait tooSQL Data warehouse-bA.

Lásd: [integráció az Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) vagy hello [Azure Machine Learning dokumentációs](https://azure.microsoft.com/services/machine-learning/) további információt.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Az Azure Stream Analytics egy összetett, teljes körűen felügyelt infrastruktúra dolgoz fel, és az Azure Event Hubs alapján generált események adatainak felhasználása.  Az SQL Data Warehouse szolgáltatással integrációja lehetővé teszi az adatfolyamként történő, hatékony feldolgozott adatokat toobe és tárolt relációs adatok mélyebb engedélyezése mellett, további speciális elemzési.  

* **Job Output:** küldési kimenetét a Stream Analytics feladatok közvetlenül tooSQL Data warehouse-bA.

Lásd: [integráció az Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) vagy hello [Azure Stream Analytics-dokumentáció](https://azure.microsoft.com/documentation/services/stream-analytics/) további információt.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
