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
# <a name="extend-your-experiment-with-r"></a><span data-ttu-id="cf23b-103">A kísérletek bővítése R-rel</span><span class="sxs-lookup"><span data-stu-id="cf23b-103">Extend your experiment with R</span></span>
<span data-ttu-id="cf23b-104">Hello R nyelv használatával hello Azure Machine Learning Studio funkcióit hello segítségével kiterjesztheti [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="cf23b-104">You can extend hello functionality of Azure Machine Learning Studio through hello R language by using hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="cf23b-105">Ez a modul több bemeneti adatkészletek fogad, és egyetlen dataset kimenetként eredményez.</span><span class="sxs-lookup"><span data-stu-id="cf23b-105">This module accepts multiple input datasets and yields a single dataset as output.</span></span> <span data-ttu-id="cf23b-106">R-parancsfájl írja be a hello **R-parancsfájl** hello paramétere [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="cf23b-106">You can type an R script into hello **R Script** parameter of hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="cf23b-107">Minden egyes hello modul bemeneti porthoz kód hasonló toohello alábbi használatával éri el:</span><span class="sxs-lookup"><span data-stu-id="cf23b-107">You access each input port of hello module by using code similar toohello following:</span></span>

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a><span data-ttu-id="cf23b-108">Az összes jelenleg telepített csomagok listázása</span><span class="sxs-lookup"><span data-stu-id="cf23b-108">Listing all currently-installed packages</span></span>
<span data-ttu-id="cf23b-109">módosíthatja a hello telepített csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="cf23b-109">hello list of installed packages can change.</span></span> <span data-ttu-id="cf23b-110">A jelenleg telepített csomagok listáját található [R csomagok támogatott Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span><span class="sxs-lookup"><span data-stu-id="cf23b-110">A list of currently installed packages can be found in [R Packages Supported by Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span></span>

<span data-ttu-id="cf23b-111">Is kaphat a hello befejeződött, a jelenlegi telepített csomagok listáját írja be a következő kódot a hello hello [R-parancsfájl végrehajtása] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="cf23b-111">You also can get hello complete, current list of installed packages by entering hello following code into hello [Execute R Script][execute-r-script] module:</span></span>

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

<span data-ttu-id="cf23b-112">Ez küldi hello csomagok toohello kimeneti portjára hello listája [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="cf23b-112">This sends hello list of packages toohello output port of hello [Execute R Script][execute-r-script] module.</span></span>
<span data-ttu-id="cf23b-113">tooview hello csomag listában, például csatlakoztassa a átalakítás modult [tooCSV átalakítása] [ convert-to-csv] hello kimenete balra toohello [R-parancsfájl végrehajtása] [ execute-r-script]modul hello kísérlet futtatásához kattintson hello kimeneti hello konverziós modul, és válassza a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="cf23b-113">tooview hello package list, connect a conversion module such as [Convert tooCSV][convert-to-csv] toohello left output of hello [Execute R Script][execute-r-script] module, run hello experiment, then click hello output of hello conversion module and select **Download**.</span></span> 

![Töltse le a "TooCSV átalakítani" modul kimenete](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is hello [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a><span data-ttu-id="cf23b-115">Csomagok importálása</span><span class="sxs-lookup"><span data-stu-id="cf23b-115">Importing packages</span></span>
<span data-ttu-id="cf23b-116">Csomagok, amelyek még nincsenek telepítve a következő parancsokat a hello hello segítségével importálhatja [R-parancsfájl végrehajtása] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="cf23b-116">You can import packages that are not already installed by using hello following commands in hello [Execute R Script][execute-r-script] module:</span></span>

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

<span data-ttu-id="cf23b-117">Ha hello `my_favorite_package.zip` fájl tartalmazza a csomag.</span><span class="sxs-lookup"><span data-stu-id="cf23b-117">where hello `my_favorite_package.zip` file contains your package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
