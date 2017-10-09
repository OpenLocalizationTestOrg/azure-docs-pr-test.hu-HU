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
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a>Oktatóanyag: Ismerkedés a U-SQL r kiterjesztése

a következő példa hello hello R-kód telepítésének lépéseit mutatja be:
* Használjon hello `REFERENCE ASSEMBLY` hello U-SQL parancsfájl-utasítás tooenable R bővítmények.
* Használja a` REDUCE` művelet toopartition hello adja meg a kulcs adatokat.
* U-SQL hello R-bővítmények közé tartozik egy beépített nyomáscsökkentő (`Extension.R.Reducer`) minden egyes hozzárendelt csúcspont toohello nyomáscsökkentő futó R-kód. 
* Dedikált használatát nevű nevű adatkeretek `inputFromUSQL` és `outputToUSQL `rendre toopass adatok között U-SQL és R. bemeneti és kimeneti DataFrame típusú azonosító neveket fix (Ez azt jelenti, hogy felhasználók nem lehet módosítani ezeket előre definiált nevek a bemeneti és kimeneti DataFrame azonosítók).

## <a name="embedding-r-code-in-hello-u-sql-script"></a>R-kód beágyazása hello U-SQL-parancsfájl

Segítségével beágyazott hello R kód a U-SQL parancsfájl hello parancs paramétere hello `Extension.R.Reducer`. Például deklarálható hello karakterlánc változóként R-parancsfájl, és adja át azt, mint egy paraméterrel toohello nyomáscsökkentő.


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

## <a name="keep-hello-r-code-in-a-separate-file-and-reference-it--hello-u-sql-script"></a>Tartsa hello R-kód külön fájlt, és hivatkozni rá hello U-SQL parancsfájl

a következő példa hello összetettebb használati mutatja be. Ebben az esetben hello R-kód, amely hello U-SQL parancsfájl erőforrásként van telepítve.

Az R-kód külön fájlt mentse.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

Használja a U-SQL parancsfájl toodeploy adott R-parancsfájl hello telepítése erőforrás utasítást.

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

## <a name="how-r-integrates-with-u-sql"></a>Hogyan integrálható az R U-SQL

### <a name="datatypes"></a>Adattípusok
* Konvertálja a karakterláncot és a numerikus oszlopot a U-SQL-R DataFrame és a U-SQL közötti [támogatott típusok: `double`, `string`, `bool`, `integer`, `byte`].
* Hello `Factor` adattípus nem támogatott a U-SQL.
* `byte[]`a base64-kódolású kell szerializálhatók `string`.
* U-SQL-karakterláncok U-SQL R bemeneti dataframe létrehozása után, vagy hello nyomáscsökkentő paraméter beállítása lehet az R-kód, átalakított toofactors `stringsAsFactors: true`.

### <a name="schemas"></a>Sémák
* U-SQL adatkészletek nem lehet ismétlődő oszlopneveket tartalmaz.
* U-SQL adatkészletek oszlopnevek karakterláncoknak kell lenniük.
* Oszlopnevek ugyanazt a U-SQL és az R parancsfájlok kell hello.
* Csak olvasható oszlop nem lehet hello kimeneti dataframe része. Mivel a csak olvasható oszlop automatikusan beszúrta vissza táblázatban hello U-SQL, ha UDO kimeneti sémája része.

### <a name="functional-limitations"></a>Működési korlátai
* hello R motor példánya nem hozható létre kétszer a hello ugyanazt a folyamatot. 
* Jelenleg U-SQL nem támogatja nyomáscsökkentő udo-k segítségével generált particionált modellek segítségével előrejelzés egyesítő udo-k. Felhasználók deklarálnia particionálva hello modellek erőforrásként és azok az R-parancsfájl használatát (lásd: mintakód `ExtR_PredictUsingLMRawStringReducer.usql`)

### <a name="r-versions"></a>R-verziók
Csak R 3.2.2 esetén támogatott.

### <a name="standard-r-modules"></a>Standard R modul

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

### <a name="input-and-output-size-limitations"></a>Bemeneti és kimeneti méretkorlátai
Minden csomópont számára tooit rendelt memória korlátozott mennyiségű. Hello bemeneti és kimeneti DataFrames léteznie kell a memóriában hello R-kódban, mert hello bemeneti és kimeneti hello teljes mérete nem lehet nagyobb, mint 500 MB.

### <a name="sample-code"></a>Mintakód
További mintakód megtalálható a Data Lake Store-fiók hello U-SQL Advanced Analytics extensions telepítését követően. További mintakód hello elérési útja: `<your_account_address>/usqlext/samples/R`. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>A U-SQL egyéni R modul telepítése

Először hozzon létre egy egyéni R modult, és azt a zip-, majd töltse fel az R modul egyéni tooyour ADL fájltároló zip hello. Hello példában azt fel kell töltenie magittr_1.5.zip toohello gyökere hello alapértelmezett ADLS-fiók hello ADLA fiókot használunk. Hello modul tooADL tároló feltöltése után deklarálja azt TELEPÍTENI erőforrás toomake használja azt a U-SQL parancsfájlt és a hívás `install.packages` tooinstall azt.

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

## <a name="next-steps"></a>Következő lépések
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md)
* [U-SQL ablak függvények használata az Azure Data Lake Analytics-feladatok](data-lake-analytics-use-window-functions.md)
