---
title: "aaaGet elindítva az Azure Data Lake Analytics az Azure portál használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure portál toocreate Data Lake Analytics-fiók, hozzon létre egy Data Lake Analytics-feladatot U-SQL használatával, valamint hello feladat elküldéséhez. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 6bb54404fa42cfed25b18bc2bfb7c72e6c361149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Az Azure Data Lake Analytics használatának első lépései az Azure Portallal
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Ismerje meg, hogyan toouse hello Azure portál toocreate Azure Data Lake Analytics-fiókok, feladatok definiálásához [U-SQL](data-lake-analytics-u-sql-get-started.md), és küldje el a feladatok toohello Data Lake Analytics szolgáltatás. További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elindításához **Azure-előfizetéssel** kell rendelkeznie. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics-fiók létrehozása

Most, létrehozhat egy Data Lake Analytics és fiókot egy Data Lake Store a hello azonos idő.  Ez a lépés egyszerű, és csak 60 másodperc toofinish vesz igénybe.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** >  **Adatok + analitika** > **Data Lake Analytics** elemre.
3. Válassza ki a következő elemek hello értékeit:
   * **Név**: Nevezze el a Data Lake Analytics-fiókot (kizárólag kisbetűk és számok használhatók).
   * **Előfizetés**: hello hello Analytics-fiókhoz használt Azure-előfizetés kiválasztása.
   * **Erőforráscsoport**. Válasszon ki egy meglévő Azure-erőforráscsoportot, vagy hozzon létre egy újat.
   * **Hely**. Válassza ki az Azure-adatközpont hello Data Lake Analytics-fiók.
   * **Data Lake Store**: hello utasítás toocreate új Data Lake Store-fiók kövesse, vagy válasszon egy meglévőt. 
4. Igény szerint tarifacsomagot is választhat a Data Lake Analytics-fiókhoz.
5. Kattintson a **Létrehozás** gombra. 


## <a name="your-first-u-sql-script"></a>Az első U-SQL-szkript

a következő szöveg hello egy nagyon egyszerű U-SQL-parancsfájlt. Minden esetben hello parancsfájlban kisebb adatkészlet meghatározása, és jegyezze meg, hogy a dataset kimenő toohello alapértelmezett Data Lake Store nevű fájlba `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a>U-SQL-feladat elküldése

1. A Data Lake Analytics-fiók hello, kattintson az **új feladat**.
2. Hello hello szövege illessze be a fenti U-SQL parancsfájlt. 
3. Kattintson a **Feladat elküldése** elemre.   
4. Várjon, amíg hello feladat állapotmódosítások túl**sikeres**.
5. Ha hello feladat sikertelen volt, lásd: [figyelése és hibaelhárítása a Data Lake Analytics-feladatok](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
6. Kattintson a hello **kimeneti** fülre, majd `data.csv`. 

## <a name="see-also"></a>Lásd még:

* megkezdődött a U-SQL-alkalmazások fejlesztésével tooget lásd [Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).
* toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).
* Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).
