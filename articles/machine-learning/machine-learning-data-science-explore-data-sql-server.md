---
title: "az SQL Server virtuális gépet az Azure aaaExplore adatok |} Microsoft Docs"
description: "Hogyan tooexplore Azure SQL Server virtuális gépen tárolt adatokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fcc449fc0d0e49be9b673cfb2de347cf44804017
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Az SQL Server virtuális gépen tárolt adatok megismerése az Azure rendszerben
Ez a dokumentum hogyan tooexplore Azure SQL Server virtuális gépen tárolt adatokat. Ezt adatok wrangling SQL használatával, és Python hasonló programozási nyelv használatával is végrehajthatja.

hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toouse eszközök különböző tárolási környezetekben tooexplore adatait tootopics. Ez a feladat Ez a lépés hello Cortana Analytics folyamat (CAP).

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> Ez a dokumentum hello minta SQL-utasítások feltételezik, hogy adatokat az SQL Server. Ha nem, olvassa el az toohello felhő adatok tudományos folyamat térkép toolearn hogyan toomove az adatok tooSQL kiszolgáló.
> 
> 

## <a name="sql-dataexploration"></a>Az SQL-parancsfájlok SQL adatokba
Íme néhány példa SQL-parancsfájlok, amely az SQL Server használt tooexplore adattárolókhoz.

1. Napi megfigyeléseket hello számbavétele
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Hello szintek kategorikus oszlopban beolvasása
   
    `select  distinct <column_name> from <databasename>`
3. Hello szintek száma kombinációja kategorikus kétoszlopos lekérése 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Hello terjesztési numerikus oszlopok az beszerzése
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Gyakorlati például használhatja hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a toohello című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.
> 
> 

## <a name="python"></a>Python SQL adatokba
Python tooexplore adatokkal, és generáljon szolgáltatások Ha hello adatok SQL Server olyan hasonló tooprocessing adatok pythonos környezetekben, az Azure BLOB dokumentált módon [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md). hello adatok hello adatbázisból betölti a pandas DataFrame toobe kell, és majd dolgozhatók további. Azt a dokumentum hello folyamat toohello adatbázis csatlakoztatása és hello adatok betöltését hello DataFrame ebben a szakaszban.

a következő kapcsolati karakterlánc-formátum hello használt tooconnect tooa SQL Server-adatbázis a Python pyodbc (csere kiszolgálónév, dbname, a felhasználónevet és jelszót az adott értékek) használatával lehet:

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hello [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz. hello alábbira olvassa hello eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Most is dolgozhat hello Pandas DataFrame hello a témakörben ismertetett módon [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>A művelet példa Cortana Analytics folyamat
Hello Cortana Analytics folyamat egy nyilvános adatkészlet-végpontok közötti bemutató példa: [Team adatok tudományos folyamat hello működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).

