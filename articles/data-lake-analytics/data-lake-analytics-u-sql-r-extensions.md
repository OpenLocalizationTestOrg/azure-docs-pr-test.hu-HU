---
title: "Kiterjesztése az r az Azure Data Lake Analytics U-SQL-parancsfájlok |} Microsoft Docs"
description: "Megtudhatja, hogyan R-kód U-SQL-parancsfájlok futtatása"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: d479af515566f497d9611e75426f6acb8f8276d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a><span data-ttu-id="627c0-103">Oktatóanyag: Ismerkedés a U-SQL r kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="627c0-103">Tutorial: Get started with extending U-SQL with R</span></span>

<span data-ttu-id="627c0-104">A következő példa az R-kód telepítésének lépéseit mutatja be:</span><span class="sxs-lookup"><span data-stu-id="627c0-104">The following example illustrates the basic steps for deploying R code:</span></span>
* <span data-ttu-id="627c0-105">Használja a `REFERENCE ASSEMBLY` R bővítmények engedélyezése a U-SQL parancsfájl utasítást.</span><span class="sxs-lookup"><span data-stu-id="627c0-105">Use the `REFERENCE ASSEMBLY` statement to enable R extensions for the U-SQL Script.</span></span>
* <span data-ttu-id="627c0-106">Használja a` REDUCE` művelet a bemeneti adatok a kulcs particionálásához.</span><span class="sxs-lookup"><span data-stu-id="627c0-106">Use the` REDUCE` operation to partition the input data on a key.</span></span>
* <span data-ttu-id="627c0-107">A U-SQL-R bővítmények közé tartozik egy beépített nyomáscsökkentő (`Extension.R.Reducer`) mindegyik csomópontra a nyomáscsökkentő rendelt futó R-kód.</span><span class="sxs-lookup"><span data-stu-id="627c0-107">The R extensions for U-SQL include a built-in reducer (`Extension.R.Reducer`) that runs R code on each vertex assigned to the reducer.</span></span> 
* <span data-ttu-id="627c0-108">Dedikált használatát nevű nevű adatkeretek `inputFromUSQL` és `outputToUSQL `rendre mechanizmusok adatok átadására közötti U-SQL és a R. bemeneti és kimeneti típusú azonosító neveket fix DataFrame számára (Ez azt jelenti, hogy felhasználók nem módosítható ezen előre meghatározott bemeneti neve és kimeneti DataFrame azonosítók).</span><span class="sxs-lookup"><span data-stu-id="627c0-108">Usage of dedicated named data frames called `inputFromUSQL` and `outputToUSQL `respectively to pass data between U-SQL and R. Input and output DataFrame identifier names are fixed (that is, users cannot change these predefined names of input and output DataFrame identifiers).</span></span>

## <a name="embedding-r-code-in-the-u-sql-script"></a><span data-ttu-id="627c0-109">R-kód beágyazása a U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="627c0-109">Embedding R code in the U-SQL script</span></span>

<span data-ttu-id="627c0-110">Az R-kód a U-SQL parancsfájl parancs paraméterének használatával beágyazott is a `Extension.R.Reducer`.</span><span class="sxs-lookup"><span data-stu-id="627c0-110">You can inline the R code your U-SQL script by using the command parameter of the `Extension.R.Reducer`.</span></span> <span data-ttu-id="627c0-111">Például az R-parancsfájl, mint egy karakterlánc-változó deklarálható, és adja át paraméterként a nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="627c0-111">For example, you can declare the R script as a string variable and pass it as a parameter to the Reducer.</span></span>


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a><span data-ttu-id="627c0-112">Az R-kód tartsa külön fájlt, és hivatkozzon a U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="627c0-112">Keep the R code in a separate file and reference it  the U-SQL script</span></span>

<span data-ttu-id="627c0-113">Az alábbi példában látható egy összetettebb használat.</span><span class="sxs-lookup"><span data-stu-id="627c0-113">The following example illustrates a more complex usage.</span></span> <span data-ttu-id="627c0-114">Ebben az esetben az R-kód, amely a U-SQL parancsfájl erőforrásként van telepítve.</span><span class="sxs-lookup"><span data-stu-id="627c0-114">In this case, the R code is deployed as a RESOURCE that is the U-SQL script.</span></span>

<span data-ttu-id="627c0-115">Az R-kód külön fájlt mentse.</span><span class="sxs-lookup"><span data-stu-id="627c0-115">Save this R code as a separate file.</span></span>

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

<span data-ttu-id="627c0-116">U-SQL parancsfájl segítségével, hogy az erőforrás telepítése utasítással R-parancsfájl telepítése.</span><span class="sxs-lookup"><span data-stu-id="627c0-116">Use a U-SQL script to deploy that R script with the DEPLOY RESOURCE statement.</span></span>

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a><span data-ttu-id="627c0-117">Hogyan integrálható az R U-SQL</span><span class="sxs-lookup"><span data-stu-id="627c0-117">How R Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="627c0-118">Adattípusok</span><span class="sxs-lookup"><span data-stu-id="627c0-118">Datatypes</span></span>
* <span data-ttu-id="627c0-119">Konvertálja a karakterláncot és a numerikus oszlopot a U-SQL-R DataFrame és a U-SQL közötti [támogatott típusok: `double`, `string`, `bool`, `integer`, `byte`].</span><span class="sxs-lookup"><span data-stu-id="627c0-119">String and numeric columns from U-SQL are converted as-is between R DataFrame and U-SQL [supported types: `double`, `string`, `bool`, `integer`, `byte`].</span></span>
* <span data-ttu-id="627c0-120">A `Factor` adattípus nem támogatott a U-SQL.</span><span class="sxs-lookup"><span data-stu-id="627c0-120">The `Factor` datatype is not supported in U-SQL.</span></span>
* <span data-ttu-id="627c0-121">`byte[]`a base64-kódolású kell szerializálhatók `string`.</span><span class="sxs-lookup"><span data-stu-id="627c0-121">`byte[]` must be serialized as a base64-encoded `string`.</span></span>
* <span data-ttu-id="627c0-122">U-SQL-karakterláncok konvertálhatók tényezők az R-kód, amennyiben az U-SQL R bemeneti dataframe létrehozása vagy nyomáscsökkentő paramétert `stringsAsFactors: true`.</span><span class="sxs-lookup"><span data-stu-id="627c0-122">U-SQL strings can be converted to factors in R code, once U-SQL create R input dataframe or by setting the reducer parameter `stringsAsFactors: true`.</span></span>

### <a name="schemas"></a><span data-ttu-id="627c0-123">Sémák</span><span class="sxs-lookup"><span data-stu-id="627c0-123">Schemas</span></span>
* <span data-ttu-id="627c0-124">U-SQL adatkészletek nem lehet ismétlődő oszlopneveket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="627c0-124">U-SQL datasets cannot have duplicate column names.</span></span>
* <span data-ttu-id="627c0-125">U-SQL adatkészletek oszlopnevek karakterláncoknak kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="627c0-125">U-SQL datasets column names must be strings.</span></span>
* <span data-ttu-id="627c0-126">Oszlopnév a U-SQL és az R parancsfájlok azonosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="627c0-126">Column names must be the same in U-SQL and R scripts.</span></span>
* <span data-ttu-id="627c0-127">Csak olvasható oszlop nem lehet a kimeneti dataframe része.</span><span class="sxs-lookup"><span data-stu-id="627c0-127">Readonly column cannot be part of the output dataframe.</span></span> <span data-ttu-id="627c0-128">Mivel a csak olvasható oszlop automatikusan beszúrta vissza a U-SQL táblázatban, ha UDO kimeneti sémája része.</span><span class="sxs-lookup"><span data-stu-id="627c0-128">Because readonly columns are automatically injected back in the U-SQL table if it is a part of output schema of UDO.</span></span>

### <a name="functional-limitations"></a><span data-ttu-id="627c0-129">Működési korlátai</span><span class="sxs-lookup"><span data-stu-id="627c0-129">Functional limitations</span></span>
* <span data-ttu-id="627c0-130">Az R-motor kétszer ugyanazt a folyamatot a példánya nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="627c0-130">The R Engine can't be instantiated twice in the same process.</span></span> 
* <span data-ttu-id="627c0-131">Jelenleg U-SQL nem támogatja nyomáscsökkentő udo-k segítségével generált particionált modellek segítségével előrejelzés egyesítő udo-k.</span><span class="sxs-lookup"><span data-stu-id="627c0-131">Currently, U-SQL does not support Combiner UDOs for prediction using partitioned models generated using Reducer UDOs.</span></span> <span data-ttu-id="627c0-132">Felhasználók deklarálja a particionált modellek erőforrásként és azok az R-parancsfájl használatát (lásd: mintakód `ExtR_PredictUsingLMRawStringReducer.usql`)</span><span class="sxs-lookup"><span data-stu-id="627c0-132">Users can declare the partitioned models as resource and use them in their R Script (see sample code `ExtR_PredictUsingLMRawStringReducer.usql`)</span></span>

### <a name="r-versions"></a><span data-ttu-id="627c0-133">R-verziók</span><span class="sxs-lookup"><span data-stu-id="627c0-133">R Versions</span></span>
<span data-ttu-id="627c0-134">Csak R 3.2.2 esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="627c0-134">Only R 3.2.2 is supported.</span></span>

### <a name="standard-r-modules"></a><span data-ttu-id="627c0-135">Standard R modul</span><span class="sxs-lookup"><span data-stu-id="627c0-135">Standard R modules</span></span>

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="627c0-136">Bemeneti és kimeneti méretkorlátai</span><span class="sxs-lookup"><span data-stu-id="627c0-136">Input and Output size limitations</span></span>
<span data-ttu-id="627c0-137">Minden csomópont csak korlátozott mennyiségű memória rendelve van.</span><span class="sxs-lookup"><span data-stu-id="627c0-137">Every vertex has a limited amount of memory assigned to it.</span></span> <span data-ttu-id="627c0-138">A bemeneti és kimeneti DataFrames már léteznie kell az R-kód memóriája, mert a bemeneti és kimeneti teljes mérete legfeljebb 500 MB.</span><span class="sxs-lookup"><span data-stu-id="627c0-138">Because the input and output DataFrames must exist in memory in the R code, the total size for the input and output cannot exceed 500 MB.</span></span>

### <a name="sample-code"></a><span data-ttu-id="627c0-139">Mintakód</span><span class="sxs-lookup"><span data-stu-id="627c0-139">Sample Code</span></span>
<span data-ttu-id="627c0-140">További mintakód áll rendelkezésre a Data Lake Store-fiókot, a U-SQL Advanced Analytics extensions telepítését követően.</span><span class="sxs-lookup"><span data-stu-id="627c0-140">More sample code is available in your Data Lake Store account after you install the U-SQL Advanced Analytics extensions.</span></span> <span data-ttu-id="627c0-141">További mintakód elérési útja: `<your_account_address>/usqlext/samples/R`.</span><span class="sxs-lookup"><span data-stu-id="627c0-141">The path for more sample code is: `<your_account_address>/usqlext/samples/R`.</span></span> 

## <a name="deploying-custom-r-modules-with-u-sql"></a><span data-ttu-id="627c0-142">A U-SQL egyéni R modul telepítése</span><span class="sxs-lookup"><span data-stu-id="627c0-142">Deploying Custom R modules with U-SQL</span></span>

<span data-ttu-id="627c0-143">Először hozzon létre egy egyéni R modult, és azt a zip-, majd töltse fel az R egyéni modul zip fájlt az ADL áruházba.</span><span class="sxs-lookup"><span data-stu-id="627c0-143">First, create an R custom module and zip it and then upload the zipped R custom module file to your ADL store.</span></span> <span data-ttu-id="627c0-144">A példában magittr_1.5.zip feltölti azt használjuk ADLA fiók alapértelmezett ADLS-fiók gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="627c0-144">In the example, we will upload magittr_1.5.zip to the root of the default ADLS account for the ADLA account we are using.</span></span> <span data-ttu-id="627c0-145">Ha a modul ADL tárolójába feltöltött, deklarálja azt, tegye elérhetővé a U-SQL parancsfájl és hívás erőforrás központi telepítése segítségével `install.packages` a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="627c0-145">Once you upload the module to ADL store, declare it as use DEPLOY RESOURCE to make it available in your U-SQL script and call `install.packages` to install it.</span></span>

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a><span data-ttu-id="627c0-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="627c0-146">Next Steps</span></span>
* [<span data-ttu-id="627c0-147">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="627c0-147">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="627c0-148">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="627c0-148">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="627c0-149">U-SQL ablak függvények használata az Azure Data Lake Analytics-feladatok</span><span class="sxs-lookup"><span data-stu-id="627c0-149">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)
