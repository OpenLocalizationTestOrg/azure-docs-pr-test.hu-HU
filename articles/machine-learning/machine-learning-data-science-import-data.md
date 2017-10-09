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
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a><span data-ttu-id="f4395-105">A betanítási adatok importálása az Azure Machine Learning Studióba különböző adatforrásokból</span><span class="sxs-lookup"><span data-stu-id="f4395-105">Import your training data into Azure Machine Learning Studio from various data sources</span></span>
<span data-ttu-id="f4395-106">toouse a saját adatait a Machine Learning Studio toodevelop és vonat egy prediktív elemzési megoldások, is:</span><span class="sxs-lookup"><span data-stu-id="f4395-106">toouse your own data in Machine Learning Studio toodevelop and train a predictive analytics solution, you can:</span></span> 

* <span data-ttu-id="f4395-107">az adatok feltöltése egy **helyi fájl** korábbi idő a merevlemez-meghajtóról toocreate a munkaterület egy adatkészlet-modulja</span><span class="sxs-lookup"><span data-stu-id="f4395-107">upload data from a **local file** ahead of time from your hard drive toocreate a dataset module in your workspace</span></span>
* <span data-ttu-id="f4395-108">adatelérés több egyikéből **online adatforrások** hello kísérletbe futása közben [és adatokat importálhat] [ import-data] modul</span><span class="sxs-lookup"><span data-stu-id="f4395-108">access data from one of several **online data sources** while your experiment is running using hello [Import Data][import-data] module</span></span> 
* <span data-ttu-id="f4395-109">Használjon egy másik Azure Machine learning adatait **kísérletezhet** adatkészletként mentése</span><span class="sxs-lookup"><span data-stu-id="f4395-109">use data from another Azure Machine learning **experiment** saved as a dataset</span></span>
* <span data-ttu-id="f4395-110">a helyszíni adatok felhasználásával **SQL Server-adatbázis**</span><span class="sxs-lookup"><span data-stu-id="f4395-110">use data from an on-premises **SQL Server database**</span></span>

<span data-ttu-id="f4395-111">Ezen lehetőségek ismertetett hello témaköröket az alábbi hello menü.</span><span class="sxs-lookup"><span data-stu-id="f4395-111">Each of these options is described in one of hello topics on hello menu below.</span></span> <span data-ttu-id="f4395-112">Ezek a témakörök bemutatják, hogyan tooimport ezek különböző adatokból adatforrások toouse a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="f4395-112">These topics show you how tooimport data from these various data sources toouse in Machine Learning Studio.</span></span> 

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> <span data-ttu-id="f4395-113">Nincsenek elérhető a betanítási adatok használható a Machine Learning Studio számos mintaként használható adathalmazt.</span><span class="sxs-lookup"><span data-stu-id="f4395-113">There are a number of sample datasets available in Machine Learning Studio that you can use for training data.</span></span> <span data-ttu-id="f4395-114">Ezek az információk: [hello mintaként használható adathalmazt használja az Azure Machine Learning Studióban](machine-learning-use-sample-datasets.md)).</span><span class="sxs-lookup"><span data-stu-id="f4395-114">For information on these, see [Use hello sample datasets in Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).</span></span>
> 
> 

<span data-ttu-id="f4395-115">Ez a témakör bevezető is ismerteti, hogyan tooget adatok készen áll a használják, a Machine Learning Studióban, és írja le, mely adatokat formátumok és adattípusok használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="f4395-115">This introductory topic also discusses how tooget data ready for use in Machine Learning Studio and describes which data formats and data types are supported.</span></span> 

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a><span data-ttu-id="f4395-116">Adatok használatra kész állapotba hozásához az Azure Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="f4395-116">Get data ready for use in Azure Machine Learning Studio</span></span>
<span data-ttu-id="f4395-117">A Machine Learning Studio a téglalap alakú vagy táblázatos adatok, például egy adatbázis strukturált bizonyos körülmények között nem téglalap használhatja, ha vagy tagolt szöveges adat tervezett toowork.</span><span class="sxs-lookup"><span data-stu-id="f4395-117">Machine Learning Studio is designed toowork with rectangular or tabular data, such as text data that's delimited or structured data from a database, though in some circumstances non-rectangular data may be used.</span></span>

<span data-ttu-id="f4395-118">Az ajánlott, ha az adatok viszonylag tiszta.</span><span class="sxs-lookup"><span data-stu-id="f4395-118">It's best if your data is relatively clean.</span></span> <span data-ttu-id="f4395-119">Ez azt jelenti, hogy érdemes például nem jegyzett karakterláncok kiszolgálásához tootake hello adatok kísérletbe való feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="f4395-119">That is, you'll want tootake care of issues such as unquoted strings before you upload hello data into your experiment.</span></span>

<span data-ttu-id="f4395-120">Van azonban modulok érhető el a Machine Learning Studióban, amelyek lehetővé teszik az adatok kísérletbe belül néhány kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="f4395-120">However, there are modules available in Machine Learning Studio that enable some manipulation of data within your experiment.</span></span> <span data-ttu-id="f4395-121">Attól függően, hogy hello gépi tanulási algoritmusok fogja használni, esetleg toodecide hogyan fogja kezeli a adatok strukturális problémák, például a hiányzó értékeket, és a ritka adatokhoz, illetve modulokat, amelyek segíthetnek, hogy a.</span><span class="sxs-lookup"><span data-stu-id="f4395-121">Depending on hello machine learning algorithms you'll be using, you may need toodecide how you'll handle data structural issues such as missing values and sparse data, and there are modules that can help with that.</span></span> <span data-ttu-id="f4395-122">Hello hely **Data Transformation** hello modulpalettán ezeket a funkciókat ellátó modulok szakasza.</span><span class="sxs-lookup"><span data-stu-id="f4395-122">Look in hello **Data Transformation** section of hello module palette for modules that perform these functions.</span></span>

<span data-ttu-id="f4395-123">A kísérletben bármikor megtekintheti vagy hello kimeneti port kattintva egy modul által előállított hello adatok letöltése.</span><span class="sxs-lookup"><span data-stu-id="f4395-123">At any point in your experiment you can view or download hello data that's produced by a module by clicking hello output port.</span></span> <span data-ttu-id="f4395-124">Attól függően, hogy hello modul lehet különböző letöltési beállítások használható, vagy képes toovisualize hello adatok lehetnek a Machine Learning Studióban webböngészőből.</span><span class="sxs-lookup"><span data-stu-id="f4395-124">Depending on hello module, there may be different download options available, or you may be able toovisualize hello data within your web browser in Machine Learning Studio.</span></span>

## <a name="data-formats-and-data-types-supported"></a><span data-ttu-id="f4395-125">Támogatott formátumok és adattípusok</span><span class="sxs-lookup"><span data-stu-id="f4395-125">Data formats and data types supported</span></span>
<span data-ttu-id="f4395-126">Adattípusok számos importálhatja a kísérletet, attól függően, hogy milyen mechanizmus tooimport adatokat, és ahol adatforrásból származó használja:</span><span class="sxs-lookup"><span data-stu-id="f4395-126">You can import a number of data types into your experiment, depending on what mechanism you use tooimport data and where it's coming from:</span></span>

* <span data-ttu-id="f4395-127">Egyszerű szöveges (.txt)</span><span class="sxs-lookup"><span data-stu-id="f4395-127">Plain text (.txt)</span></span>
* <span data-ttu-id="f4395-128">Vesszővel tagolt (CSV) a fejléc (.csv) vagy anélkül (. nh.csv)</span><span class="sxs-lookup"><span data-stu-id="f4395-128">Comma-separated values (CSV) with a header (.csv) or without (.nh.csv)</span></span>
* <span data-ttu-id="f4395-129">A lapon elválasztott értékeket (TSV) fejléc (.tsv) vagy anélkül (. nh.tsv)</span><span class="sxs-lookup"><span data-stu-id="f4395-129">Tab-separated values (TSV) with a header (.tsv) or without (.nh.tsv)</span></span>
* <span data-ttu-id="f4395-130">Excel-fájl</span><span class="sxs-lookup"><span data-stu-id="f4395-130">Excel file</span></span>
* <span data-ttu-id="f4395-131">Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="f4395-131">Azure table</span></span>
* <span data-ttu-id="f4395-132">Hive tábla</span><span class="sxs-lookup"><span data-stu-id="f4395-132">Hive table</span></span>
* <span data-ttu-id="f4395-133">SQL-adatbázistáblában szereplő</span><span class="sxs-lookup"><span data-stu-id="f4395-133">SQL database table</span></span>
* <span data-ttu-id="f4395-134">Az OData-értékek</span><span class="sxs-lookup"><span data-stu-id="f4395-134">OData values</span></span>
* <span data-ttu-id="f4395-135">SVMLight adatok (.svmlight) (lásd: hello [SVMLight definition](http://svmlight.joachims.org/) formátum információt)</span><span class="sxs-lookup"><span data-stu-id="f4395-135">SVMLight data (.svmlight) (see hello [SVMLight definition](http://svmlight.joachims.org/) for format information)</span></span>
* <span data-ttu-id="f4395-136">Attribútum-kapcsolat fájlformátumra (ARFF) adatokat (.arff) (lásd: hello [ARFF definition](http://weka.wikispaces.com/ARFF) formátum információt)</span><span class="sxs-lookup"><span data-stu-id="f4395-136">Attribute Relation File Format (ARFF) data (.arff) (see hello [ARFF definition](http://weka.wikispaces.com/ARFF) for format information)</span></span>
* <span data-ttu-id="f4395-137">Zip-fájl (.zip)</span><span class="sxs-lookup"><span data-stu-id="f4395-137">Zip file (.zip)</span></span>
* <span data-ttu-id="f4395-138">R-objektum vagy munkaterület fájl (. RData)</span><span class="sxs-lookup"><span data-stu-id="f4395-138">R object or workspace file (.RData)</span></span>

<span data-ttu-id="f4395-139">Ha például a metaadatokat tartalmazó ARFF formátumú adatokat importál, a Machine Learning Studio ezt a metaadatok toodefine hello címsor és minden egyes oszlopának adattípusa használja.</span><span class="sxs-lookup"><span data-stu-id="f4395-139">If you import data in a format such as ARFF that includes metadata, Machine Learning Studio uses this metadata toodefine hello heading and data type of each column.</span></span>

<span data-ttu-id="f4395-140">Ha importálja az adatokat, például TSV vagy CSV formátum, amely nem tartalmazza ezeket a metaadatokat, a Machine Learning Studio hello oszlop adattípusa esetében minden egyes kikövetkezteti hello adatok mintavételezéssel.</span><span class="sxs-lookup"><span data-stu-id="f4395-140">If you import data such as TSV or CSV format that doesn't include this metadata, Machine Learning Studio infers hello data type for each column by sampling hello data.</span></span> <span data-ttu-id="f4395-141">Hello adatok nem rendelkezik oszlopának fejlécére kattintva rendezhető, ha a Machine Learning Studio biztosít alapértelmezett nevét.</span><span class="sxs-lookup"><span data-stu-id="f4395-141">If hello data also doesn't have column headings, Machine Learning Studio provides default names.</span></span>

<span data-ttu-id="f4395-142">Explicit módon adja meg vagy módosítsa hello segítségével oszlopok fejlécére kattintva rendezhető és adattípusok hello [szerkesztése metaadatok][edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="f4395-142">You can explicitly specify or change hello headings and data types for columns using hello [Edit Metadata][edit-metadata].</span></span>

<span data-ttu-id="f4395-143">hello következő **adattípusok** felismeri a Machine Learning Studio:</span><span class="sxs-lookup"><span data-stu-id="f4395-143">hello following **data types** are recognized by Machine Learning Studio:</span></span>

* <span data-ttu-id="f4395-144">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f4395-144">String</span></span>
* <span data-ttu-id="f4395-145">Egész szám</span><span class="sxs-lookup"><span data-stu-id="f4395-145">Integer</span></span>
* <span data-ttu-id="f4395-146">Dupla</span><span class="sxs-lookup"><span data-stu-id="f4395-146">Double</span></span>
* <span data-ttu-id="f4395-147">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="f4395-147">Boolean</span></span>
* <span data-ttu-id="f4395-148">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f4395-148">DateTime</span></span>
* <span data-ttu-id="f4395-149">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="f4395-149">TimeSpan</span></span>

<span data-ttu-id="f4395-150">A Machine Learning Studio nevű belső adatok típust használ ***adattábla*** toopass adatok modulok között.</span><span class="sxs-lookup"><span data-stu-id="f4395-150">Machine Learning Studio uses an internal data type called ***Data Table*** toopass data between modules.</span></span> <span data-ttu-id="f4395-151">Explicit módon átalakíthatja az adatokat adattábla formátum használatával hello [tooDataset átalakítása] [ convert-to-dataset] modul.</span><span class="sxs-lookup"><span data-stu-id="f4395-151">You can explicitly convert your data into Data Table format using hello [Convert tooDataset][convert-to-dataset] module.</span></span>

<span data-ttu-id="f4395-152">Bármely modul, amely fogadja a formátumok nem adattábla csendes átkonvertálja hello adatok tooData tábla előtt toohello tovább modul.</span><span class="sxs-lookup"><span data-stu-id="f4395-152">Any module that accepts formats other than Data Table will convert hello data tooData Table silently before passing it toohello next module.</span></span>

<span data-ttu-id="f4395-153">Ha szükséges, konvertálhatja adattábla CSV, TSV, ARFF programba, vagy SVMLight formátumban más átalakítás modulok használata.</span><span class="sxs-lookup"><span data-stu-id="f4395-153">If necessary, you can convert Data Table format back into CSV, TSV, ARFF, or SVMLight format using other conversion modules.</span></span>
<span data-ttu-id="f4395-154">Hello hely **adatok formátuma átalakítások** hello modulpalettán ezeket a funkciókat ellátó modulok szakasza.</span><span class="sxs-lookup"><span data-stu-id="f4395-154">Look in hello **Data Format Conversions** section of hello module palette for modules that perform these functions.</span></span>

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
