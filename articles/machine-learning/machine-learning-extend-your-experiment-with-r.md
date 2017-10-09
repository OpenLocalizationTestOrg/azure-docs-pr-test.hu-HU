---
title: "aaaExtend a az R nyelv használatával |} Microsoft Docs"
description: "Hogyan tooextend hello R-parancsfájl végrehajtása modul használatával hello Azure Machine Learning Studio funkcióit hello R nyelv használatával."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 2c038a45-ba4d-42ea-9a88-e67391ef8c0a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 396489f26f367a744922af65e04f056c7afa1399
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extend-your-experiment-with-r"></a>A kísérletek bővítése R-rel
Hello R nyelv használatával hello Azure Machine Learning Studio funkcióit hello segítségével kiterjesztheti [R-parancsfájl végrehajtása] [ execute-r-script] modul.

Ez a modul több bemeneti adatkészletek fogad, és egyetlen dataset kimenetként eredményez. R-parancsfájl írja be a hello **R-parancsfájl** hello paramétere [R-parancsfájl végrehajtása] [ execute-r-script] modul.

Minden egyes hello modul bemeneti porthoz kód hasonló toohello alábbi használatával éri el:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Az összes jelenleg telepített csomagok listázása
módosíthatja a hello telepített csomagok listáját. A jelenleg telepített csomagok listáját található [R csomagok támogatott Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).

Is kaphat a hello befejeződött, a jelenlegi telepített csomagok listáját írja be a következő kódot a hello hello [R-parancsfájl végrehajtása] [ execute-r-script] modul:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Ez küldi hello csomagok toohello kimeneti portjára hello listája [R-parancsfájl végrehajtása] [ execute-r-script] modul.
tooview hello csomag listában, például csatlakoztassa a átalakítás modult [tooCSV átalakítása] [ convert-to-csv] hello kimenete balra toohello [R-parancsfájl végrehajtása] [ execute-r-script]modul hello kísérlet futtatásához kattintson hello kimeneti hello konverziós modul, és válassza a **letöltése**. 

![Töltse le a "TooCSV átalakítani" modul kimenete](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is hello [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Csomagok importálása
Csomagok, amelyek még nincsenek telepítve a következő parancsokat a hello hello segítségével importálhatja [R-parancsfájl végrehajtása] [ execute-r-script] modul:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Ha hello `my_favorite_package.zip` fájl tartalmazza a csomag.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
