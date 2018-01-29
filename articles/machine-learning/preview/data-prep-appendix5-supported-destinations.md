---
title: "Támogatott adatok a célok és kimenetek érhető el az Azure Machine Learning adatok előkészítése |} Microsoft Docs"
description: "Ez a dokumentum által támogatott célok teljes listája, és kiírja érhető el az Azure Machine Learning adatok előkészítése"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 50d2d481b91199630bbfbf3cfdd21a1bf3062ff0
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/18/2017
---
# <a name="supported-data-exports-for-this-preview"></a>Támogatott adatok exportálja az előzetes verzió 
Számos különböző formátumokban exportálása lehetőség. Ezek a formátumok segítségével adatok előkészítése a köztes eredmények megőrzése előtt az eredmények integrálja a Machine Learning munkafolyamat többi részétől.

## <a name="types"></a>Típusok 
### <a name="csv-file"></a>A CSV-fájl 
Vesszővel tagolt fájl írása tárhelyre.

#### <a name="options"></a>Beállítások
- Sorvégződések
- Cserélje le a NULL értékek
- Cserélje le a hibák 
- Elválasztó


### <a name="parquet"></a>Parquet 
A DataSet adatkészlet írható Storage Parquet.

Parquet, formátum különböző formát ölthetnek tároló. Kisebb adatkészletek esetében egyetlen .parquet fájl használható. Különböző Python szalagtárak olvasását és írását egyetlen .parquet fájlokat támogatja. 

Azure Machine Learning-munkaterület jelenleg írásra kimenő Parquet helyi interaktív használata során a PyArrow Python kódtár támaszkodik. Ez azt jelenti, hogy egy fájlból Parquet jelenleg az egyetlen Parquet kimeneti formátum helyi interaktív használata során támogatott.

Során kibővített fut (a Spark) Azure Machine Learning-munkaterület olvasását és írását képességek a Spark Parquet támaszkodik. A Spark alapértelmezett kimeneti Parquet (jelenleg az egyetlen támogatott) formátuma Hive adatkészletre szerkezete hasonló. Ez azt jelenti, hogy egy mappa számos .parquet fájlokat tartalmazza, amelyek minden nagyobb adatkészletek kisebb partíció. 

#### <a name="caveats"></a>Figyelmeztetések 
Parquet, formátum viszonylag fiatal, és néhány megvalósítási inkonzisztenciát között különböző szalagtárat. Spark példányhoz, amelyen karakterek érvényesek az oszlopnevek írásakor Parquet korlátozások helyezi. PyArrow tilos ezt tenniük. Egy oszlop neve nem lehet a következő karaktereket: 
- ;
- ;
- {}
- ()
- \\n
- \\t
- =

>[!NOTE]
>- Spark való kompatibilitás érdekében adatírás Parquet, amikor ezek a karakterek, az oszlopnevek előfordulását váltják fel és aláhúzásjelet (_).
>- Biztosíthatja a helyi és a kibővített fut, Parquet, az alkalmazás, a Python vagy a Spark, írt adatok között van a oszlopnevek fertőtlenítsék Spark kompatibilitás-ellenőrzése. Spark Parquet karakter írásakor várható oszlopneveket érdekében távolítsa el az érvénytelen oszlopokból rögzíti őket előtt állítsa be.



## <a name="locations"></a>Helyek 
### <a name="local"></a>Helyi 
Helyi merevlemez-meghajtóról vagy a csatlakoztatott hálózati tárolási helyét.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Az Azure Blob storage Azure-előfizetés szükséges.

