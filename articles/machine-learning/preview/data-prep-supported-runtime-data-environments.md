---
title: "Végrehajtási és az adatok környezetek kombinációját támogatott az Azure Machine Learning adatok előkészített |} Microsoft Docs"
description: "Ez a dokumentum különböző futtatókörnyezetek és adatforrások támogatott kombinációi teljes listáját biztosít az Azure Machine Learning adatok elkezdése"
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
ms.date: 09/15/2017
ms.openlocfilehash: 248cbcfe35db646a8bc71c6f825dcaa8a4661e91
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/18/2017
---
# <a name="supported-matrix-for-this-release"></a>Ebben a kiadásban támogatott mátrix 
Ha a kód adatokat tölt az Azure Machine Learning adatforrások vagy az Azure Machine Learning adatok előkészített, vagy egy Pandas első használatával, vagy a Spark dataframe, kísérlet a következő kombinációk számítási és az adatok helyek támogatottak:

|     |Helyi fájlok  |Azure Blob Storage  |SQL Server adatbázis x  |
|---------|---------|---------|---------|---------|
|Helyi Python    |     Támogatott    |Nem támogatott         | Nem támogatott        |         |
|Python docker (Linux virtuális gép)     |Project fájlok csak a támogatott *         | Nem támogatott        |        Nem támogatott |         |
|PySpark docker (Linux virtuális gép)     |Project fájlok csak a támogatott *     |Támogatott         | Támogatott**        |         |
|Az Azure Data tudományos virtuális gép Python     |Project fájlok csak a támogatott *         |Nem támogatott         |Nem támogatott         |         |
|Az Azure Data tudományos virtuális gép PySPark     | Project fájlok csak a támogatott *        |Nem támogatott         |Nem támogatott         |         |
|Az Azure HDInsight PySpark     | Nem támogatott        |Támogatott         |Támogatott**         |         |
|Az Azure HDInsight Python     | Nem támogatott        | Nem támogatott        | Nem támogatott        |         |

Azure Data Lake Store jelenleg nem támogatott a számítási cél.

Ha a helyi elérési utat használ, a projekt fájlok átmásolja a számítási környezet, és majd olvasási hiba. A rendszer nem másolja a projekten kívül a fájlokat, és az elérési utak nem megoldja a számítási környezetben. Érdemes lehet Data Source helyettesítés, hogy a kód egy helyi fájl használatával, a helyi futtatás során. Majd átváltása egy Azure Storage-blobot egy másik futtatási konfiguráció. Is használhatja mintavételi támogatási adatforrásokon felhőkörnyezetben csak bizonyos futtatási konfigurációkat nagyméretű adatok kezeléséhez.

** Maven JDBC SQL Server-illesztőprogramot 6.2.1 használ. Gondoskodnia kell arról, hogy a csomagban (vagy egy kompatibilis egy) a számítási környezet a spark_dependencies.yml fájl tartalmazza.

Támogatja az Azure SQL Database vagy az SQL Server megadott, az adatbázis elérhető a számítási környezetből. 
