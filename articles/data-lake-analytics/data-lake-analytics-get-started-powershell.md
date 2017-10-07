---
title: "az Azure Data Lake Analytics az Azure PowerShell lépései aaaGet |} Microsoft Docs"
description: "Azure PowerShell toocreate egy Data Lake Analytics-fiókot használja, hozzon létre egy U-SQL használatával a Data Lake Analytics-feladatot, valamint hello feladat elküldéséhez. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.openlocfilehash: cb9b35352d1cc9a78337448b1d6835875a212e08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Az Azure Data Lake Analytics használatának első lépései az Azure PowerShell-lel
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Ismerje meg, hogyan toouse Azure PowerShell toocreate Azure Data Lake Analytics fiókok majd küldje el és U-SQL feladatok futtatásához. További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).

## <a name="prerequisites"></a>Előfeltételek

Ez az oktatóanyag elkezdéséhez hello a következő információkat kell rendelkeznie:

* **Egy Azure Data Lake Analytics-fiók**. Lásd: [Ismerkedés a Data Lake Analytics szolgáltatással](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **Munkaállomás Azure PowerShell-lel**. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Ez az oktatóanyag feltételezi az Azure PowerShell használatának előzetes ismeretét. Különösen tooknow szüksége, hogy a tooAzure toolog. Lásd: hello [Ismerkedés az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) Ha segítségre van szüksége.

toolog be egy előfizetés nevét:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

Hello előfizetés neve helyett egy előfizetési azonosító toolog a is használhatja:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Ha sikeres, a parancs kimenetének hello néz hello a következő szöveget:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-hello-tutorial"></a>Hello oktatóanyag előkészítése

Ebben az oktatóanyagban hello PowerShell kódtöredékek ezeket az információkat használja a változók toostore:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Data Lake Analytics-fiókkal kapcsolatos információk beszerzése

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>U-SQL-feladat elküldése

Hozzon létre egy PowerShell-változó toohold hello U-SQL parancsfájlt.

```
$script = @"
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

"@
```

Hello parancsfájl nyújt.

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

Azt is megteheti sikerült hello parancsfájl menteni a fájlt, és küldje el a hello a következő parancsot:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


Egy adott feladat hello állapotának beolvasása. Ez a parancsmag továbbra is használatára, amíg megjelenik a hello történik.

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Helyett Get-AdlAnalyticsJob és újra amíg a feladat befejeződik, hello várakozási-AdlJob parancsmagot használhatja.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Hello kimeneti fájl letöltéséhez.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a>Lásd még:
* toosee hello ugyanaz az oktatóanyagot más eszközök használatával hello szeretné a hello hello lap tetején kattintson.
* toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).
* Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).
