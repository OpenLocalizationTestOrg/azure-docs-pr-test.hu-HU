---
title: "Azure Data Lake Analytics az r parancsfájlok aaaExtend U-SQL |} Microsoft Docs"
description: "Ismerje meg, hogyan toorun R-kód a U-SQL-parancsfájlok"
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
ms.openlocfilehash: 24affd4963a08d30a7111b49af388e9c1268430e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a><span data-ttu-id="efc41-103">Oktatóanyag: Ismerkedés a U-SQL r kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="efc41-103">Tutorial: Get started with extending U-SQL with R</span></span>

<span data-ttu-id="efc41-104">a következő példa hello hello R-kód telepítésének lépéseit mutatja be:</span><span class="sxs-lookup"><span data-stu-id="efc41-104">hello following example illustrates hello basic steps for deploying R code:</span></span>
* <span data-ttu-id="efc41-105">Használjon hello `REFERENCE ASSEMBLY` hello U-SQL parancsfájl-utasítás tooenable R bővítmények.</span><span class="sxs-lookup"><span data-stu-id="efc41-105">Use hello `REFERENCE ASSEMBLY` statement tooenable R extensions for hello U-SQL Script.</span></span>
* <span data-ttu-id="efc41-106">Használja a` REDUCE` művelet toopartition hello adja meg a kulcs adatokat.</span><span class="sxs-lookup"><span data-stu-id="efc41-106">Use the` REDUCE` operation toopartition hello input data on a key.</span></span>
* <span data-ttu-id="efc41-107">U-SQL hello R-bővítmények közé tartozik egy beépített nyomáscsökkentő (`Extension.R.Reducer`) minden egyes hozzárendelt csúcspont toohello nyomáscsökkentő futó R-kód.</span><span class="sxs-lookup"><span data-stu-id="efc41-107">hello R extensions for U-SQL include a built-in reducer (`Extension.R.Reducer`) that runs R code on each vertex assigned toohello reducer.</span></span> 
* <span data-ttu-id="efc41-108">Dedikált használatát nevű nevű adatkeretek `inputFromUSQL` és `outputToUSQL `rendre toopass adatok között U-SQL és R. bemeneti és kimeneti DataFrame típusú azonosító neveket fix (Ez azt jelenti, hogy felhasználók nem lehet módosítani ezeket előre definiált nevek a bemeneti és kimeneti DataFrame azonosítók).</span><span class="sxs-lookup"><span data-stu-id="efc41-108">Usage of dedicated named data frames called `inputFromUSQL` and `outputToUSQL `respectively toopass data between U-SQL and R. Input and output DataFrame identifier names are fixed (that is, users cannot change these predefined names of input and output DataFrame identifiers).</span></span>

## <a name="embedding-r-code-in-hello-u-sql-script"></a><span data-ttu-id="efc41-109">R-kód beágyazása hello U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="efc41-109">Embedding R code in hello U-SQL script</span></span>

<span data-ttu-id="efc41-110">Segítségével beágyazott hello R kód a U-SQL parancsfájl hello parancs paramétere hello `Extension.R.Reducer`.</span><span class="sxs-lookup"><span data-stu-id="efc41-110">You can inline hello R code your U-SQL script by using hello command parameter of hello `Extension.R.Reducer`.</span></span> <span data-ttu-id="efc41-111">Például deklarálható hello karakterlánc változóként R-parancsfájl, és adja át azt, mint egy paraméterrel toohello nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="efc41-111">For example, you can declare hello R script as a string variable and pass it as a parameter toohello Reducer.</span></span>


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that hello column names are hello same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-hello-r-code-in-a-separate-file-and-reference-it--hello-u-sql-script"></a><span data-ttu-id="efc41-112">Tartsa hello R-kód külön fájlt, és hivatkozni rá hello U-SQL parancsfájl</span><span class="sxs-lookup"><span data-stu-id="efc41-112">Keep hello R code in a separate file and reference it  hello U-SQL script</span></span>

<span data-ttu-id="efc41-113">a következő példa hello összetettebb használati mutatja be.</span><span class="sxs-lookup"><span data-stu-id="efc41-113">hello following example illustrates a more complex usage.</span></span> <span data-ttu-id="efc41-114">Ebben az esetben hello R-kód, amely hello U-SQL parancsfájl erőforrásként van telepítve.</span><span class="sxs-lookup"><span data-stu-id="efc41-114">In this case, hello R code is deployed as a RESOURCE that is hello U-SQL script.</span></span>

<span data-ttu-id="efc41-115">Az R-kód külön fájlt mentse.</span><span class="sxs-lookup"><span data-stu-id="efc41-115">Save this R code as a separate file.</span></span>

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

<span data-ttu-id="efc41-116">Használja a U-SQL parancsfájl toodeploy adott R-parancsfájl hello telepítése erőforrás utasítást.</span><span class="sxs-lookup"><span data-stu-id="efc41-116">Use a U-SQL script toodeploy that R script with hello DEPLOY RESOURCE statement.</span></span>

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
        OUTPUT @RScriptOutput too@OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a><span data-ttu-id="efc41-117">Hogyan integrálható az R U-SQL</span><span class="sxs-lookup"><span data-stu-id="efc41-117">How R Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="efc41-118">Adattípusok</span><span class="sxs-lookup"><span data-stu-id="efc41-118">Datatypes</span></span>
* <span data-ttu-id="efc41-119">Konvertálja a karakterláncot és a numerikus oszlopot a U-SQL-R DataFrame és a U-SQL közötti [támogatott típusok: `double`, `string`, `bool`, `integer`, `byte`].</span><span class="sxs-lookup"><span data-stu-id="efc41-119">String and numeric columns from U-SQL are converted as-is between R DataFrame and U-SQL [supported types: `double`, `string`, `bool`, `integer`, `byte`].</span></span>
* <span data-ttu-id="efc41-120">Hello `Factor` adattípus nem támogatott a U-SQL.</span><span class="sxs-lookup"><span data-stu-id="efc41-120">hello `Factor` datatype is not supported in U-SQL.</span></span>
* <span data-ttu-id="efc41-121">`byte[]`a base64-kódolású kell szerializálhatók `string`.</span><span class="sxs-lookup"><span data-stu-id="efc41-121">`byte[]` must be serialized as a base64-encoded `string`.</span></span>
* <span data-ttu-id="efc41-122">U-SQL-karakterláncok U-SQL R bemeneti dataframe létrehozása után, vagy hello nyomáscsökkentő paraméter beállítása lehet az R-kód, átalakított toofactors `stringsAsFactors: true`.</span><span class="sxs-lookup"><span data-stu-id="efc41-122">U-SQL strings can be converted toofactors in R code, once U-SQL create R input dataframe or by setting hello reducer parameter `stringsAsFactors: true`.</span></span>

### <a name="schemas"></a><span data-ttu-id="efc41-123">Sémák</span><span class="sxs-lookup"><span data-stu-id="efc41-123">Schemas</span></span>
* <span data-ttu-id="efc41-124">U-SQL adatkészletek nem lehet ismétlődő oszlopneveket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="efc41-124">U-SQL datasets cannot have duplicate column names.</span></span>
* <span data-ttu-id="efc41-125">U-SQL adatkészletek oszlopnevek karakterláncoknak kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="efc41-125">U-SQL datasets column names must be strings.</span></span>
* <span data-ttu-id="efc41-126">Oszlopnevek ugyanazt a U-SQL és az R parancsfájlok kell hello.</span><span class="sxs-lookup"><span data-stu-id="efc41-126">Column names must be hello same in U-SQL and R scripts.</span></span>
* <span data-ttu-id="efc41-127">Csak olvasható oszlop nem lehet hello kimeneti dataframe része.</span><span class="sxs-lookup"><span data-stu-id="efc41-127">Readonly column cannot be part of hello output dataframe.</span></span> <span data-ttu-id="efc41-128">Mivel a csak olvasható oszlop automatikusan beszúrta vissza táblázatban hello U-SQL, ha UDO kimeneti sémája része.</span><span class="sxs-lookup"><span data-stu-id="efc41-128">Because readonly columns are automatically injected back in hello U-SQL table if it is a part of output schema of UDO.</span></span>

### <a name="functional-limitations"></a><span data-ttu-id="efc41-129">Működési korlátai</span><span class="sxs-lookup"><span data-stu-id="efc41-129">Functional limitations</span></span>
* <span data-ttu-id="efc41-130">hello R motor példánya nem hozható létre kétszer a hello ugyanazt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="efc41-130">hello R Engine can't be instantiated twice in hello same process.</span></span> 
* <span data-ttu-id="efc41-131">Jelenleg U-SQL nem támogatja nyomáscsökkentő udo-k segítségével generált particionált modellek segítségével előrejelzés egyesítő udo-k.</span><span class="sxs-lookup"><span data-stu-id="efc41-131">Currently, U-SQL does not support Combiner UDOs for prediction using partitioned models generated using Reducer UDOs.</span></span> <span data-ttu-id="efc41-132">Felhasználók deklarálnia particionálva hello modellek erőforrásként és azok az R-parancsfájl használatát (lásd: mintakód `ExtR_PredictUsingLMRawStringReducer.usql`)</span><span class="sxs-lookup"><span data-stu-id="efc41-132">Users can declare hello partitioned models as resource and use them in their R Script (see sample code `ExtR_PredictUsingLMRawStringReducer.usql`)</span></span>

### <a name="r-versions"></a><span data-ttu-id="efc41-133">R-verziók</span><span class="sxs-lookup"><span data-stu-id="efc41-133">R Versions</span></span>
<span data-ttu-id="efc41-134">Csak R 3.2.2 esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="efc41-134">Only R 3.2.2 is supported.</span></span>

### <a name="standard-r-modules"></a><span data-ttu-id="efc41-135">Standard R modul</span><span class="sxs-lookup"><span data-stu-id="efc41-135">Standard R modules</span></span>

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

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="efc41-136">Bemeneti és kimeneti méretkorlátai</span><span class="sxs-lookup"><span data-stu-id="efc41-136">Input and Output size limitations</span></span>
<span data-ttu-id="efc41-137">Minden csomópont számára tooit rendelt memória korlátozott mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="efc41-137">Every vertex has a limited amount of memory assigned tooit.</span></span> <span data-ttu-id="efc41-138">Hello bemeneti és kimeneti DataFrames léteznie kell a memóriában hello R-kódban, mert hello bemeneti és kimeneti hello teljes mérete nem lehet nagyobb, mint 500 MB.</span><span class="sxs-lookup"><span data-stu-id="efc41-138">Because hello input and output DataFrames must exist in memory in hello R code, hello total size for hello input and output cannot exceed 500 MB.</span></span>

### <a name="sample-code"></a><span data-ttu-id="efc41-139">Mintakód</span><span class="sxs-lookup"><span data-stu-id="efc41-139">Sample Code</span></span>
<span data-ttu-id="efc41-140">További mintakód megtalálható a Data Lake Store-fiók hello U-SQL Advanced Analytics extensions telepítését követően.</span><span class="sxs-lookup"><span data-stu-id="efc41-140">More sample code is available in your Data Lake Store account after you install hello U-SQL Advanced Analytics extensions.</span></span> <span data-ttu-id="efc41-141">További mintakód hello elérési útja: `<your_account_address>/usqlext/samples/R`.</span><span class="sxs-lookup"><span data-stu-id="efc41-141">hello path for more sample code is: `<your_account_address>/usqlext/samples/R`.</span></span> 

## <a name="deploying-custom-r-modules-with-u-sql"></a><span data-ttu-id="efc41-142">A U-SQL egyéni R modul telepítése</span><span class="sxs-lookup"><span data-stu-id="efc41-142">Deploying Custom R modules with U-SQL</span></span>

<span data-ttu-id="efc41-143">Először hozzon létre egy egyéni R modult, és azt a zip-, majd töltse fel az R modul egyéni tooyour ADL fájltároló zip hello.</span><span class="sxs-lookup"><span data-stu-id="efc41-143">First, create an R custom module and zip it and then upload hello zipped R custom module file tooyour ADL store.</span></span> <span data-ttu-id="efc41-144">Hello példában azt fel kell töltenie magittr_1.5.zip toohello gyökere hello alapértelmezett ADLS-fiók hello ADLA fiókot használunk.</span><span class="sxs-lookup"><span data-stu-id="efc41-144">In hello example, we will upload magittr_1.5.zip toohello root of hello default ADLS account for hello ADLA account we are using.</span></span> <span data-ttu-id="efc41-145">Hello modul tooADL tároló feltöltése után deklarálja azt TELEPÍTENI erőforrás toomake használja azt a U-SQL parancsfájlt és a hívás `install.packages` tooinstall azt.</span><span class="sxs-lookup"><span data-stu-id="efc41-145">Once you upload hello module tooADL store, declare it as use DEPLOY RESOURCE toomake it available in your U-SQL script and call `install.packages` tooinstall it.</span></span>

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script toorun
    DECLARE @myRScript = @"
    # install hello magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load hello magrittr package,
    require(magrittr),
    # demonstrate use of hello magrittr package,
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

    OUTPUT @RScriptOutput too@OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a><span data-ttu-id="efc41-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="efc41-146">Next Steps</span></span>
* [<span data-ttu-id="efc41-147">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="efc41-147">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="efc41-148">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="efc41-148">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="efc41-149">U-SQL ablak függvények használata az Azure Data Lake Analytics-feladatok</span><span class="sxs-lookup"><span data-stu-id="efc41-149">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)
