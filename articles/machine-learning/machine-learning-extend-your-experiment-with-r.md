---
title: "Kiterjeszti a az R nyelv használatával |} Microsoft Docs"
description: "How Azure Machine Learning Studio funkcióit az R nyelv használatával kiterjesztése az R-parancsfájl végrehajtása modul használatával."
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
ms.openlocfilehash: fe207ef917980be8b554ad9c08176d108b19fb71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="extend-your-experiment-with-r"></a><span data-ttu-id="ba1eb-103">A kísérletek bővítése R-rel</span><span class="sxs-lookup"><span data-stu-id="ba1eb-103">Extend your experiment with R</span></span>
<span data-ttu-id="ba1eb-104">Azure Machine Learning Studio funkcióit keresztül az R nyelv használatával kiterjesztheti a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="ba1eb-104">You can extend the functionality of Azure Machine Learning Studio through the R language by using the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="ba1eb-105">Ez a modul több bemeneti adatkészletek fogad, és egyetlen dataset kimenetként eredményez.</span><span class="sxs-lookup"><span data-stu-id="ba1eb-105">This module accepts multiple input datasets and yields a single dataset as output.</span></span> <span data-ttu-id="ba1eb-106">Az R-parancsfájl beírhatja a **R-parancsfájl** paramétere a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="ba1eb-106">You can type an R script into the **R Script** parameter of the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="ba1eb-107">Minden modul bemeneti portját a következőhöz hasonló kód használatával éri el:</span><span class="sxs-lookup"><span data-stu-id="ba1eb-107">You access each input port of the module by using code similar to the following:</span></span>

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a><span data-ttu-id="ba1eb-108">Az összes jelenleg telepített csomagok listázása</span><span class="sxs-lookup"><span data-stu-id="ba1eb-108">Listing all currently-installed packages</span></span>
<span data-ttu-id="ba1eb-109">Módosíthatja a telepített csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="ba1eb-109">The list of installed packages can change.</span></span> <span data-ttu-id="ba1eb-110">A jelenleg telepített csomagok listáját található [R csomagok támogatott Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba1eb-110">A list of currently installed packages can be found in [R Packages Supported by Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span></span>

<span data-ttu-id="ba1eb-111">Is kaphat a teljes, a jelenlegi telepített csomagok listáját az alábbi kódot írja be a [R-parancsfájl végrehajtása] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="ba1eb-111">You also can get the complete, current list of installed packages by entering the following code into the [Execute R Script][execute-r-script] module:</span></span>

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

<span data-ttu-id="ba1eb-112">Ez a csomagok listájában küld kimeneti portját a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="ba1eb-112">This sends the list of packages to the output port of the [Execute R Script][execute-r-script] module.</span></span>
<span data-ttu-id="ba1eb-113">Tekintse meg a csomag listáját, csatlakoztassa a átalakítás modult például [CSV átalakítása] [ convert-to-csv] a bal oldali kimeneti a [R-parancsfájl végrehajtása] [ execute-r-script] modul, Futtassa a kísérletet, majd kattintson a konverziós modul kimenete, és válassza ki **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="ba1eb-113">To view the package list, connect a conversion module such as [Convert to CSV][convert-to-csv] to the left output of the [Execute R Script][execute-r-script] module, run the experiment, then click the output of the conversion module and select **Download**.</span></span> 

![Töltse le a "CSV való átalakítása" modul kimenete](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a><span data-ttu-id="ba1eb-115">Csomagok importálása</span><span class="sxs-lookup"><span data-stu-id="ba1eb-115">Importing packages</span></span>
<span data-ttu-id="ba1eb-116">Csomagok, amelyek még nincsenek telepítve a következő parancsok segítségével importálhatja a [R-parancsfájl végrehajtása] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="ba1eb-116">You can import packages that are not already installed by using the following commands in the [Execute R Script][execute-r-script] module:</span></span>

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

<span data-ttu-id="ba1eb-117">Ha a `my_favorite_package.zip` fájl tartalmazza a csomag.</span><span class="sxs-lookup"><span data-stu-id="ba1eb-117">where the `my_favorite_package.zip` file contains your package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
