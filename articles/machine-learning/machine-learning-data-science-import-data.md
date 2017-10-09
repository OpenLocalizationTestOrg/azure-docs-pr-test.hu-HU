---
title: "a Machine Learning Studióhoz aaaImport adatok |} Microsoft Docs"
description: "Hogyan tooimport az Azure Machine Learning Studio a különféle adatforrásokból származó adatokat. Ismerje meg, milyen típusú adatokat és az adatok formátumok támogat."
keywords: "adatok, adatformátum, adattípusok, adatforrások, betanítási adatok importálása"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: garye;bradsev
ms.openlocfilehash: 830dcdde9d43809900c520a41d6d94a65731ca3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>A betanítási adatok importálása az Azure Machine Learning Studióba különböző adatforrásokból
toouse a saját adatait a Machine Learning Studio toodevelop és vonat egy prediktív elemzési megoldások, is: 

* az adatok feltöltése egy **helyi fájl** korábbi idő a merevlemez-meghajtóról toocreate a munkaterület egy adatkészlet-modulja
* adatelérés több egyikéből **online adatforrások** hello kísérletbe futása közben [és adatokat importálhat] [ import-data] modul 
* Használjon egy másik Azure Machine learning adatait **kísérletezhet** adatkészletként mentése
* a helyszíni adatok felhasználásával **SQL Server-adatbázis**

Ezen lehetőségek ismertetett hello témaköröket az alábbi hello menü. Ezek a témakörök bemutatják, hogyan tooimport ezek különböző adatokból adatforrások toouse a Machine Learning Studióban. 

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> Nincsenek elérhető a betanítási adatok használható a Machine Learning Studio számos mintaként használható adathalmazt. Ezek az információk: [hello mintaként használható adathalmazt használja az Azure Machine Learning Studióban](machine-learning-use-sample-datasets.md)).
> 
> 

Ez a témakör bevezető is ismerteti, hogyan tooget adatok készen áll a használják, a Machine Learning Studióban, és írja le, mely adatokat formátumok és adattípusok használata támogatott. 

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Adatok használatra kész állapotba hozásához az Azure Machine Learning Studióban
A Machine Learning Studio a téglalap alakú vagy táblázatos adatok, például egy adatbázis strukturált bizonyos körülmények között nem téglalap használhatja, ha vagy tagolt szöveges adat tervezett toowork.

Az ajánlott, ha az adatok viszonylag tiszta. Ez azt jelenti, hogy érdemes például nem jegyzett karakterláncok kiszolgálásához tootake hello adatok kísérletbe való feltöltés előtt.

Van azonban modulok érhető el a Machine Learning Studióban, amelyek lehetővé teszik az adatok kísérletbe belül néhány kezeléséhez. Attól függően, hogy hello gépi tanulási algoritmusok fogja használni, esetleg toodecide hogyan fogja kezeli a adatok strukturális problémák, például a hiányzó értékeket, és a ritka adatokhoz, illetve modulokat, amelyek segíthetnek, hogy a. Hello hely **Data Transformation** hello modulpalettán ezeket a funkciókat ellátó modulok szakasza.

A kísérletben bármikor megtekintheti vagy hello kimeneti port kattintva egy modul által előállított hello adatok letöltése. Attól függően, hogy hello modul lehet különböző letöltési beállítások használható, vagy képes toovisualize hello adatok lehetnek a Machine Learning Studióban webböngészőből.

## <a name="data-formats-and-data-types-supported"></a>Támogatott formátumok és adattípusok
Adattípusok számos importálhatja a kísérletet, attól függően, hogy milyen mechanizmus tooimport adatokat, és ahol adatforrásból származó használja:

* Egyszerű szöveges (.txt)
* Vesszővel tagolt (CSV) a fejléc (.csv) vagy anélkül (. nh.csv)
* A lapon elválasztott értékeket (TSV) fejléc (.tsv) vagy anélkül (. nh.tsv)
* Excel-fájl
* Azure-tábla
* Hive tábla
* SQL-adatbázistáblában szereplő
* Az OData-értékek
* SVMLight adatok (.svmlight) (lásd: hello [SVMLight definition](http://svmlight.joachims.org/) formátum információt)
* Attribútum-kapcsolat fájlformátumra (ARFF) adatokat (.arff) (lásd: hello [ARFF definition](http://weka.wikispaces.com/ARFF) formátum információt)
* Zip-fájl (.zip)
* R-objektum vagy munkaterület fájl (. RData)

Ha például a metaadatokat tartalmazó ARFF formátumú adatokat importál, a Machine Learning Studio ezt a metaadatok toodefine hello címsor és minden egyes oszlopának adattípusa használja.

Ha importálja az adatokat, például TSV vagy CSV formátum, amely nem tartalmazza ezeket a metaadatokat, a Machine Learning Studio hello oszlop adattípusa esetében minden egyes kikövetkezteti hello adatok mintavételezéssel. Hello adatok nem rendelkezik oszlopának fejlécére kattintva rendezhető, ha a Machine Learning Studio biztosít alapértelmezett nevét.

Explicit módon adja meg vagy módosítsa hello segítségével oszlopok fejlécére kattintva rendezhető és adattípusok hello [szerkesztése metaadatok][edit-metadata].

hello következő **adattípusok** felismeri a Machine Learning Studio:

* Karakterlánc
* Egész szám
* Dupla
* Logikai érték
* Dátum és idő
* A TimeSpan

A Machine Learning Studio nevű belső adatok típust használ ***adattábla*** toopass adatok modulok között. Explicit módon átalakíthatja az adatokat adattábla formátum használatával hello [tooDataset átalakítása] [ convert-to-dataset] modul.

Bármely modul, amely fogadja a formátumok nem adattábla csendes átkonvertálja hello adatok tooData tábla előtt toohello tovább modul.

Ha szükséges, konvertálhatja adattábla CSV, TSV, ARFF programba, vagy SVMLight formátumban más átalakítás modulok használata.
Hello hely **adatok formátuma átalakítások** hello modulpalettán ezeket a funkciókat ellátó modulok szakasza.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
