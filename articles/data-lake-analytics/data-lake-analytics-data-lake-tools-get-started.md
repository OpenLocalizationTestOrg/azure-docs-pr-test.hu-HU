---
title: "a Data Lake Tools for Visual Studio használatával aaaDevelop U-SQL-parancsfájlok |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall Data Lake Tools for Visual Studio, és hogyan toodevelop és tesztelési U-SQL-parancsfájlok."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: 7a0c02c275b8cefecbe784ba63969cbf24a150d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Megtudhatja, hogyan toouse Visual Studio toocreate Azure Data Lake Analytics fiókok-feladatok definiálásához [U-SQL](data-lake-analytics-u-sql-get-started.md), és küldje el a feladatok toohello Data Lake Analytics szolgáltatás. További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).


## <a name="prerequisites"></a>Előfeltételek

* **Visual Studio**: Az Express kivételével minden kiadás támogatott.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK for .NET** 2.7.1-es vagy újabb verzió.  Telepítheti azt a hello [webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).
* Egy **Data Lake Analytics**-fiók. egy olyan fiók, toocreate lásd [Ismerkedés az Azure Data Lake Analytics az Azure portál használatával](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Az Azure Data Lake Tools for Visual Studio telepítése 

Töltse le és telepítse az Azure Data Lake Tools for Visual Studio [a letöltőközpontból hello](http://aka.ms/adltoolsvs). A telepítést követően vegye figyelembe a következőket:
* Hello **Server Explorer** > **Azure** csomópont tartalmaz egy **Data Lake Analytics** csomópont. 
* Hello **eszközök** menü tartalmaz egy **Data Lake** elemet.

## <a name="connect-tooan-azure-data-lake-analytics-account"></a>Csatlakozás tooan Azure Data Lake Analytics-fiók

1. Nyissa meg a Visual Studiót.
2. Nyissa meg a Kiszolgálókezelőt a **Nézet** > **Kiszolgálókezelő** kiválasztásával.
3. Kattintson a jobb gombbal az **Azure** elemre. Válassza ki **csatlakozás Azure-előfizetés tooMicrosoft** és hello utasítások szerint.
4. A Kiszolgálókezelőben válassza az **Azure** > **Data Lake Analytics** elemet. Ekkor megjelenik a Data Lake Analytics-fiókok listája.


## <a name="write-your-first-u-sql-script"></a>Az első U-SQL parancsfájl megírása

a következő szöveg hello egy egyszerű U-SQL-parancsfájlt. Azt határozza meg, kisebb adatkészlet, mind az írás, amelyeket a dataset toohello alapértelmezett Data Lake Store-fájlként `/data.csv`.

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

### <a name="submit-a-data-lake-analytics-job"></a>Data Lake Analytics-feladat küldése

1. Válassza a **File** (Fájl) > **New** (Új) > **Project** (Projekt) lehetőséget.

2. Jelölje be hello **U-SQL projekt** írja be, és kattintson **OK**. A Visual Studio létrehoz egy megoldást egy **Script.usql** fájllal.

3. Hello előző parancsfájl beillesztése hello **Script.usql** ablak.

4. Hello bal felső sarkában hello **Script.usql** ablakban adja meg a hello Data Lake Analytics-fiók.

    ![U-SQL Visual Studio-projekt elküldése](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. Hello bal felső sarkában hello **Script.usql** ablakban válassza ki **Submit**.
6. Ellenőrizze a hello **Analytics-fiók**, majd válassza ki **Submit**. Után az eredmények érhetők el a Data Lake Tools for Visual Studio eredmények hello, hello küldésének befejezése után.

    ![U-SQL Visual Studio-projekt elküldése](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. toosee hello legújabb feladat állapotát és a frissítési üdvözlő képernyőt, kattintson a **frissítése**. Hello feladat sikeres, mutat hello **Feladatgrafikon**, **metaadat-művelet**, **State History**, és **diagnosztika**:

    ![U-SQL Visual Studio Data Lake Analytics-feladat teljesítménygrafikonja](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **Feladat összegzése** mutat be hello hello feladat összegzése.   
   * **Feladat részleteinek** hello feladat, többek között a hello parancsfájl, erőforrások és csúcsban konkrétabb információkat jeleníti meg.
   * **A Feladatgrafikon** visualizes hello hello feladat előrehaladását.
   * **Metaadat-művelet** jeleníti meg a hello U-SQL catalog végrehajtott összes hello-műveleteket.
   * **Adatok** összes hello bemenetekhez és kimenetekhez jeleníti meg.
   * A **Diagnosztika** részletes elemzést nyújt a feladat végrehajtásához és a teljesítmény optimalizálásához.

### <a name="toocheck-job-state"></a>toocheck feladat állapota

1. A Kiszolgálókezelőben válassza az **Azure** > **Data Lake Analytics** elemet. 
2. Bontsa ki a hello Data Lake Analytics-fiók nevét.
3. Kattintson duplán a **Feladatok** elemre.
4. Jelölje ki az előzőleg elküldött hello feladatot.

### <a name="toosee-hello-output-of-a-job"></a>feladat kimenetének toosee hello

1. A Server Explorer eszközben keresse meg az Ön által küldött toohello feladat.
2. Kattintson a hello **adatok** fülre.
3. A hello **feladatot kimenetek** lapra, jelölje be hello `"/data.csv"` fájlt.

## <a name="next-steps"></a>Következő lépések

* [U-SQL-szkript futtatása a munkaállomáson teszteléshez és hibakereséshez](data-lake-analytics-data-lake-tools-local-run.md)
* [Hibakeresés a C#-kódban – U-SQL-feladatok](data-lake-analytics-debug-u-sql-jobs.md)
* [A Visual Studio Code hello Azure Data Lake Tools használata](data-lake-analytics-data-lake-tools-for-vscode.md)
