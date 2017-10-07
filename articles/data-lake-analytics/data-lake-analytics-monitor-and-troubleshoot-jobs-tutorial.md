---
title: "Azure portál használatával aaaTroubleshoot Azure Data Lake Analytics-feladatok |} Microsoft Docs"
description: 'Ismerje meg, hogyan toouse hello Azure Portal tootroubleshoot Data Lake Analytics-feladatok. '
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e810d56bab8f1a8254721ec9906bb6a4508dc22a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Azure portál használata az Azure Data Lake Analytics-feladatok hibaelhárítása
Ismerje meg, hogyan toouse hello Azure Portal tootroubleshoot Data Lake Analytics-feladatok.

Ebben az oktatóanyagban meg fog beállítani egy hiányzó forrás fájl probléma, valamint hello Azure Portal tootroubleshoot hello probléma használja.

## <a name="submit-a-data-lake-analytics-job"></a>Data Lake Analytics-feladat küldése

Küldje el a U-SQL-feladatot a következő hello:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   too"/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
hello forrásfájl hello parancsfájl definiálva van **/Samples/Data/SearchLog.tsv1**, ahol meg kell **/Samples/Data/SearchLog.tsv**.


## <a name="troubleshoot-hello-job"></a>Hello-feladat hibaelhárítása

**toosee összes hello feladatok**

1. A hello Azure-portálon, kattintson az **Microsoft Azure** hello bal felső sarokban található.
2. Kattintson a hello csempe, amely a Data Lake Analytics-fiók nevét.  összegző hello feladat jelenik meg hello **feladatkezelés** csempére.

    ![Azure Data Lake Analytics-feladatok kezelése](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    hello feladat felügyeleti lehetővé teszi egy pillanat alatt a hello feladat állapotát. Figyelje meg, nincs a sikertelen feladatokat.
3. Kattintson a hello **feladatkezelés** toosee hello feladatok csempére. hello feladat szerint vannak kategóriába sorolva **futtató**, **várakozik**, és **befejezve**. Ekkor megjelenik a sikertelen feladat a hello **befejezve** szakasz. Hello lista első tanúsítványt kell lennie. Ha nagy mennyiségű feladatok, rákattinthat **szűrő** toohelp toolocate feladatok meg.

    ![Azure Data Lake Analytics szűrheti a feladatokat](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Kattintson a sikertelen feladatot hello hello lista tooopen hello feladat részleteit egy új panelen:

    ![Az Azure Data Lake Analytics feladat sikertelen volt.](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Értesítés hello **küldje el újra** gombra. Hello probléma megoldása után újraküldése a hello feladat.
5. Kattintson a kijelölt részben hello előző képernyőkép tooopen hello hiba részletes adatait.  Látni fogja hasonlót:

    ![Az Azure Data Lake Analytics nem sikerült a feladat részletei](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Jelzi, hogy hello forrásmappa nem található.
6. Kattintson a **parancsfájl ismétlődő**.
7. Frissítés hello **FROM** toohello következő elérési út:

    "/ Samples/Data/SearchLog.tsv"
8. Kattintson a **Feladat elküldése** elemre.

## <a name="see-also"></a>Lásd még:
* [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [Ismerkedés az Azure Data Lake Analytics Azure PowerShell használatával](data-lake-analytics-get-started-powershell.md)
* [Ismerkedés az Azure Data Lake Analytics és a U-SQL Visual Studio használatával](data-lake-analytics-u-sql-get-started.md)
* [Az Azure Data Lake Analytics kezelése az Azure Portal használatával](data-lake-analytics-manage-use-portal.md)
