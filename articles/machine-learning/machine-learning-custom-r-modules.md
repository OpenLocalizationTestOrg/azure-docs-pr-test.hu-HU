---
title: "Egyéni R modul létrehozása az Azure Machine Learning aaaAuthor |} Microsoft Docs"
description: "Gyors üzembe helyezési egyéni R modul az Azure Machine Learning-szerzésre vonatkozó információ."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 03/24/2017
ms.author: bradsev;ankarlof
ms.openlocfilehash: 8007c2abe20a4ab990f38b6d09bc4e6834ad2082
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a><span data-ttu-id="3118d-103">Egyéni R-modul létrehozása az Azure Machine Learningben</span><span class="sxs-lookup"><span data-stu-id="3118d-103">Author custom R modules in Azure Machine Learning</span></span>
<span data-ttu-id="3118d-104">Ez a témakör ismerteti, hogyan tooauthor és központi telepítése az Azure Machine Learning egy egyéni R modult.</span><span class="sxs-lookup"><span data-stu-id="3118d-104">This topic describes how tooauthor and deploy a custom R module in Azure Machine Learning.</span></span> <span data-ttu-id="3118d-105">Egyéni R modul vannak, és milyen fájlok használt toodefine ismerteti azokat.</span><span class="sxs-lookup"><span data-stu-id="3118d-105">It explains what custom R modules are and what files are used toodefine them.</span></span> <span data-ttu-id="3118d-106">Azt mutatja be, hogyan tooconstruct hello fájlok, amelyek meghatározzák egy modult, és hogyan tooregister hello modul a Machine Learning-munkaterület központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3118d-106">It illustrates how tooconstruct hello files that define a module and how tooregister hello module for deployment in a Machine Learning workspace.</span></span> <span data-ttu-id="3118d-107">hello elemek és attribútumok hello definíciójában hello egyéni modult használja majd ismerteti részletesen.</span><span class="sxs-lookup"><span data-stu-id="3118d-107">hello elements and attributes used in hello definition of hello custom module are then described in more detail.</span></span> <span data-ttu-id="3118d-108">Hogyan toouse kiegészítő funkciók és a fájlok és a több kimenet van is ismertet.</span><span class="sxs-lookup"><span data-stu-id="3118d-108">How toouse auxiliary functionality and files and multiple outputs is also discussed.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a><span data-ttu-id="3118d-109">Mi az, hogy egy egyéni R modult?</span><span class="sxs-lookup"><span data-stu-id="3118d-109">What is a custom R module?</span></span>
<span data-ttu-id="3118d-110">A **egyéni modul** egy felhasználó által definiált modul, amely lehet feltöltve tooyour munkaterület és az Azure Machine Learning kísérlet részeként végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="3118d-110">A **custom module** is a user-defined module that can be uploaded tooyour workspace and executed as part of an Azure Machine Learning experiment.</span></span> <span data-ttu-id="3118d-111">A **egyéni R modul** egy egyéni modul, amely végrehajtja a felhasználó által definiált R függvény.</span><span class="sxs-lookup"><span data-stu-id="3118d-111">A **custom R module** is a custom module that executes a user-defined R function.</span></span> <span data-ttu-id="3118d-112">**R** statisztikai számítások és grafikus statisztikusok és az adatok kutatók által algoritmusok megvalósításának széles körben használt programozási nyelv.</span><span class="sxs-lookup"><span data-stu-id="3118d-112">**R** is a programming language for statistical computing and graphics that is widely used by statisticians and data scientists for implementing algorithms.</span></span> <span data-ttu-id="3118d-113">R jelenleg hello nyelvi további nyelveket a jövőbeli kiadások van ütemezve. az egyéni modulok, de támogatja a támogatott.</span><span class="sxs-lookup"><span data-stu-id="3118d-113">Currently, R is hello only language supported in custom modules, but support for additional languages is scheduled for future releases.</span></span>

<span data-ttu-id="3118d-114">Az egyéni modulok rendelkezik **első osztályú állapot** az Azure Machine Learning hello értelemben, hogy azok csakúgy, mint bármely más modul használható.</span><span class="sxs-lookup"><span data-stu-id="3118d-114">Custom modules have **first-class status** in Azure Machine Learning in hello sense that they can be used just like any other module.</span></span> <span data-ttu-id="3118d-115">Az egyéb modulok, közzétett kísérletek vagy a képi megjelenítések végrehajthatók.</span><span class="sxs-lookup"><span data-stu-id="3118d-115">They can be executed with other modules, included in published experiments or in visualizations.</span></span> <span data-ttu-id="3118d-116">Hello modul által megvalósított hello algoritmus szabályozhatják, a használt bemeneti és kimeneti portok toobe hello, hello modellezési paramétereket és egyéb különböző futásidejű viselkedések.</span><span class="sxs-lookup"><span data-stu-id="3118d-116">You have control over hello algorithm implemented by hello module, hello input and output ports toobe used, hello modeling parameters, and other various runtime behaviors.</span></span> <span data-ttu-id="3118d-117">A kísérlet, amely tartalmazza az egyéni modulok az egyszerű is a Cortana Intelligence Gallery hello tehetők közzé.</span><span class="sxs-lookup"><span data-stu-id="3118d-117">An experiment that contains custom modules can also be published into hello Cortana Intelligence Gallery for easy sharing.</span></span>

## <a name="files-in-a-custom-r-module"></a><span data-ttu-id="3118d-118">Egy egyéni R modul fájlok</span><span class="sxs-lookup"><span data-stu-id="3118d-118">Files in a custom R module</span></span>
<span data-ttu-id="3118d-119">Egy egyéni R modul egy .zip fájlt, amely legalább két fájlt tartalmaz határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="3118d-119">A custom R module is defined by a .zip file that contains, at a minimum, two files:</span></span>

* <span data-ttu-id="3118d-120">A **forrásfájl** , amely megvalósítja hello modul által elérhetővé tett hello R függvény</span><span class="sxs-lookup"><span data-stu-id="3118d-120">A **source file** that implements hello R function exposed by hello module</span></span>
* <span data-ttu-id="3118d-121">Egy **XML-definíciós fájljának** , amely leírja, hogy hello egyéni modul felület</span><span class="sxs-lookup"><span data-stu-id="3118d-121">An **XML definition file** that describes hello custom module interface</span></span>

<span data-ttu-id="3118d-122">További kiegészítő fájlok hello .zip fájlt, amely elérhető az hello egyéni modult is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="3118d-122">Additional auxiliary files can also be included in hello .zip file that provides functionality that can be accessed from hello custom module.</span></span> <span data-ttu-id="3118d-123">Ez a beállítás hello ismertet **argumentumok** hello útmutató szakaszban része **hello XML-definíciós fájljának elemeinek** hello gyors üzembe helyezési példában a következő.</span><span class="sxs-lookup"><span data-stu-id="3118d-123">This option is discussed in hello **Arguments** part of hello reference section **Elements in hello XML definition file** following hello quickstart example.</span></span>

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a><span data-ttu-id="3118d-124">Gyors üzembe helyezési példa: határozza meg, a csomagot, majd egy egyéni R modult regisztrálni</span><span class="sxs-lookup"><span data-stu-id="3118d-124">Quickstart example: define, package, and register a custom R module</span></span>
<span data-ttu-id="3118d-125">Ez a példa bemutatja, hogyan tooconstruct hello egy egyéni R modult szükséges fájlokat, a csomagolni egy zip-fájl, és a Machine Learning-munkaterület hello modul regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="3118d-125">This example illustrates how tooconstruct hello files required by a custom R module, package them into a zip file, and then register hello module in your Machine Learning workspace.</span></span> <span data-ttu-id="3118d-126">hello például zip csomag és a minta-fájlok letölthető [letöltése CustomAddRows.zip fájl](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="3118d-126">hello example zip package and sample files can be downloaded from [Download CustomAddRows.zip file](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span></span>

## <a name="hello-source-file"></a><span data-ttu-id="3118d-127">hello forrásfájl</span><span class="sxs-lookup"><span data-stu-id="3118d-127">hello source file</span></span>
<span data-ttu-id="3118d-128">Vegye figyelembe a hello példa egy **egyéni hozzáadása sorok** modul, amely módosítja a hello szabványos végrehajtásának hello **sorok hozzáadása** modul használt tooconcatenate sorát (megfigyelések) két adatkészletet (adatkeretek).</span><span class="sxs-lookup"><span data-stu-id="3118d-128">Consider hello example of a **Custom Add Rows** module that modifies hello standard implementation of hello **Add Rows** module used tooconcatenate rows (observations) from two datasets (data frames).</span></span> <span data-ttu-id="3118d-129">hello standard **sorok hozzáadása** modul hozzáfűzi hello sor végének hello második bemeneti adatkészlet toohello hello első bemeneti adatkészletet hello segítségével `rbind` algoritmus.</span><span class="sxs-lookup"><span data-stu-id="3118d-129">hello standard **Add Rows** module appends hello rows of hello second input dataset toohello end of hello first input dataset using hello `rbind` algorithm.</span></span> <span data-ttu-id="3118d-130">testre szabott hello `CustomAddRows` függvény hasonlóképpen fogad el két adatkészletet, de egy logikai swap paraméter további bemenetként is fogad.</span><span class="sxs-lookup"><span data-stu-id="3118d-130">hello customized `CustomAddRows` function similarly accepts two datasets, but also accepts a Boolean swap parameter as an additional input.</span></span> <span data-ttu-id="3118d-131">Ha hello swap paraméter értéke túl**hamis**, akkor adja vissza hello azonos adatkészlet, szabványos implementációjától hello.</span><span class="sxs-lookup"><span data-stu-id="3118d-131">If hello swap parameter is set too**FALSE**, it returns hello same data set as hello standard implementation.</span></span> <span data-ttu-id="3118d-132">De ha hello swap paraméter **igaz**, hello függvény első bemeneti adatkészlet toohello végét hello második dataset sorát helyette hozzáfűzi.</span><span class="sxs-lookup"><span data-stu-id="3118d-132">But if hello swap parameter is **TRUE**, hello function appends rows of first input dataset toohello end of hello second dataset instead.</span></span> <span data-ttu-id="3118d-133">hello CustomAddRows.R hello R hello végrehajtásának tartalmazó fájl `CustomAddRows` hello által elérhetővé tett függvény **egyéni hozzáadása sorok** modul rendelkezik a következő R-kód hello.</span><span class="sxs-lookup"><span data-stu-id="3118d-133">hello CustomAddRows.R file that contains hello implementation of hello R `CustomAddRows` function exposed by hello **Custom Add Rows** module has hello following R code.</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="hello-xml-definition-file"></a><span data-ttu-id="3118d-134">hello XML-definíciós fájlt</span><span class="sxs-lookup"><span data-stu-id="3118d-134">hello XML definition file</span></span>
<span data-ttu-id="3118d-135">tooexpose ez `CustomAddRows` függvényt egy Azure Machine Learning-modul, egy XML-definíciós fájljának, létre kell hozni toospecify hogyan hello **egyéni hozzáadása sorok** modul kell kinézete és viselkedése.</span><span class="sxs-lookup"><span data-stu-id="3118d-135">tooexpose this `CustomAddRows` function as an Azure Machine Learning module, an XML definition file must be created toospecify how hello **Custom Add Rows** module should look and behave.</span></span> 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother. Dataset 2 is concatenated tooDataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify hello base language, script file and R function toouse for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: hello values of hello id attributes in hello Input and Arg elements must match hello parameter names in hello R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>hello combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


<span data-ttu-id="3118d-136">Kritikus toonote, amely hello értékének hello **azonosító** hello attribútumait **bemeneti** és **Arg** hello XML-fájl elemeinek meg kell egyeznie a hello függvény paramétereinek nevére hello R hello CustomAddRows.R fájlban pontosan kód: (*dataset1*, *dataset2*, és *swap* hello példában).</span><span class="sxs-lookup"><span data-stu-id="3118d-136">It is critical toonote that hello value of hello **id** attributes of hello **Input** and **Arg** elements in hello XML file must match hello function parameter names of hello R code in hello CustomAddRows.R file EXACTLY: (*dataset1*, *dataset2*, and *swap* in hello example).</span></span> <span data-ttu-id="3118d-137">Hasonlóképpen, a hello értékének hello **belépési pont** hello attribútumának **nyelvi** elem pontosan meg kell egyeznie a hello R-parancsfájl hello függvény hello neve: (*CustomAddRows* hello példában).</span><span class="sxs-lookup"><span data-stu-id="3118d-137">Similarly, hello value of hello **entryPoint** attribute of hello **Language** element must match hello name of hello function in hello R script EXACTLY: (*CustomAddRows* in hello example).</span></span> 

<span data-ttu-id="3118d-138">Ezzel szemben hello **azonosító** hello attribútuma **kimeneti** elem nem felel meg a hello R-parancsfájl tooany változókat.</span><span class="sxs-lookup"><span data-stu-id="3118d-138">In contrast, hello **id** attribute for hello **Output** element does not correspond tooany variables in hello R script.</span></span> <span data-ttu-id="3118d-139">Ha több kimenet szükség, egyszerűen listáját adja vissza hello R függvényből elhelyezett eredményekkel *hello az ugyanabban a sorrendben* , **kimenetek** hello XML-fájlban deklarált elemeket.</span><span class="sxs-lookup"><span data-stu-id="3118d-139">When more than one output is required, simply return a list from hello R function with results placed *in hello same order* as **Outputs** elements are declared in hello XML file.</span></span>

### <a name="package-and-register-hello-module"></a><span data-ttu-id="3118d-140">Csomag és a nyilvántartás hello modul</span><span class="sxs-lookup"><span data-stu-id="3118d-140">Package and register hello module</span></span>
<span data-ttu-id="3118d-141">Mentés másként két fájlt *CustomAddRows.R* és *CustomAddRows.xml* és majd zip hello két fájlt együtt történő egy *CustomAddRows.zip* fájlt.</span><span class="sxs-lookup"><span data-stu-id="3118d-141">Save these two files as *CustomAddRows.R* and *CustomAddRows.xml* and then zip hello two files together into a *CustomAddRows.zip* file.</span></span>

<span data-ttu-id="3118d-142">a Machine Learning munkaterületen, a Machine Learning Studio hello lépjen tooyour munkaterületén kattintson hello tooregister **+ új** hello alján gombra, majd válassza a **modul ZIP-csomag a ->** tooupload új hello **egyéni hozzáadása sorok** modul.</span><span class="sxs-lookup"><span data-stu-id="3118d-142">tooregister them in your Machine Learning workspace, go tooyour workspace in hello Machine Learning Studio, click hello **+NEW** button on hello bottom and choose **MODULE -> FROM ZIP PACKAGE** tooupload hello new **Custom Add Rows** module.</span></span>

![Zip feltöltése](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

<span data-ttu-id="3118d-144">Hello **egyéni hozzáadása sorok** modul mostantól készen áll a toobe érhetők el a Machine Learning kísérleteket.</span><span class="sxs-lookup"><span data-stu-id="3118d-144">hello **Custom Add Rows** module is now ready toobe accessed by your Machine Learning experiments.</span></span>

## <a name="elements-in-hello-xml-definition-file"></a><span data-ttu-id="3118d-145">XML-definíciós fájl hello elemei</span><span class="sxs-lookup"><span data-stu-id="3118d-145">Elements in hello XML definition file</span></span>
### <a name="module-elements"></a><span data-ttu-id="3118d-146">A modul elemei</span><span class="sxs-lookup"><span data-stu-id="3118d-146">Module elements</span></span>
<span data-ttu-id="3118d-147">Hello **modul** elem használt toodefine egy egyéni modul hello XML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="3118d-147">hello **Module** element is used toodefine a custom module in hello XML file.</span></span> <span data-ttu-id="3118d-148">Több modul adható meg egy XML-fájl több **modul** elemek.</span><span class="sxs-lookup"><span data-stu-id="3118d-148">Multiple modules can be defined in one XML file using multiple **module** elements.</span></span> <span data-ttu-id="3118d-149">A munkaterület minden modulja egy egyedi névvel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3118d-149">Each module in your workspace must have a unique name.</span></span> <span data-ttu-id="3118d-150">Egy egyéni modult regisztrálni azonos nevet, egy meglévő egyéni modulként hello és hello meglévő modul lecseréli hello újat.</span><span class="sxs-lookup"><span data-stu-id="3118d-150">Register a custom module with hello same name as an existing custom module and it replaces hello existing module with hello new one.</span></span> <span data-ttu-id="3118d-151">Azonban az egyéni modulok lehet hello azonos nevet, egy meglévő Azure Machine Learning-modul regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="3118d-151">Custom modules can, however, be registered with hello same name as an existing Azure Machine Learning module.</span></span> <span data-ttu-id="3118d-152">Ha igen, azok megjelennek hello **egyéni** hello modulpalettán kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="3118d-152">If so, they appear in hello **Custom** category of hello module palette.</span></span>

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother...</Description>/> 


<span data-ttu-id="3118d-153">Hello belül **modul** elem, két további választható elem adhat meg:</span><span class="sxs-lookup"><span data-stu-id="3118d-153">Within hello **Module** element, you can specify two additional optional elements:</span></span>

* <span data-ttu-id="3118d-154">egy **tulajdonos** hello modulba beágyazott elem</span><span class="sxs-lookup"><span data-stu-id="3118d-154">an **Owner** element that is embedded into hello module</span></span>  
* <span data-ttu-id="3118d-155">egy **leírás** elem, amely tartalmazza a szöveg, amely gyors súgó hello modul jelenik meg, és ha mutat a Machine Learning felhasználói felület hello hello modul.</span><span class="sxs-lookup"><span data-stu-id="3118d-155">a **Description** element that contains text that is displayed in quick help for hello module and when you hover over hello module in hello Machine Learning UI.</span></span>

<span data-ttu-id="3118d-156">Karakterek korlátok hello modul elemekben szabályokat:</span><span class="sxs-lookup"><span data-stu-id="3118d-156">Rules for characters limits in hello Module elements:</span></span>

* <span data-ttu-id="3118d-157">hello értékének hello **neve** hello attribútum **modul** elem legfeljebb 64 karakternél hosszabb.</span><span class="sxs-lookup"><span data-stu-id="3118d-157">hello value of hello **name** attribute in hello **Module** element must not exceed 64 characters in length.</span></span> 
* <span data-ttu-id="3118d-158">hello tartalmának hello **leírás** elem legfeljebb 128 karakter hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="3118d-158">hello content of hello **Description** element must not exceed 128 characters in length.</span></span>
* <span data-ttu-id="3118d-159">hello tartalmának hello **tulajdonos** elem legfeljebb 32 karakter hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="3118d-159">hello content of hello **Owner** element must not exceed 32 characters in length.</span></span>

<span data-ttu-id="3118d-160">Lehet, hogy egy modul eredmények determinisztikus vagy nondeterministic.* * alapértelmezés szerint, minden modul minősülnek toobe determinisztikus.</span><span class="sxs-lookup"><span data-stu-id="3118d-160">A module's results can be deterministic or nondeterministic.** By default, all modules are considered toobe deterministic.</span></span> <span data-ttu-id="3118d-161">Ez azt jelenti, hogy a megadott bemeneti paraméterek és egy nem változó készletét, hello modul kell visszaadnia hello azonos eredmények eacRAND vagy egy functionh futtatáskor.</span><span class="sxs-lookup"><span data-stu-id="3118d-161">That is, given an unchanging set of input parameters and data, hello module should return hello same results eacRAND or a functionh time it is run.</span></span> <span data-ttu-id="3118d-162">Ez a viselkedés, Azure Machine Learning Studio csak Újrafuttatja determinisztikus, ha a paraméter jelölésű modulok vagy hello bemeneti adatok változásairól.</span><span class="sxs-lookup"><span data-stu-id="3118d-162">Given this behavior, Azure Machine Learning Studio only reruns modules marked as deterministic if a parameter or hello input data has changed.</span></span> <span data-ttu-id="3118d-163">Gyorsítótárazott hello eredményeket ad vissza a kísérletek sokkal gyorsabb végrehajtását is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3118d-163">Returning hello cached results also provides much faster execution of experiments.</span></span>

<span data-ttu-id="3118d-164">Nincsenek determinált, például VÉL vagy az aktuális dátum vagy idő hello függvény funkciókat.</span><span class="sxs-lookup"><span data-stu-id="3118d-164">There are functions that are nondeterministic, such as RAND or a function that returns hello current date or time.</span></span> <span data-ttu-id="3118d-165">Ha a modul determinált függvényt tartalmaz, megadhatja, hello modult nem determinisztikus által választható beállítás hello **isDeterministic** túl attribútum**hamis**.</span><span class="sxs-lookup"><span data-stu-id="3118d-165">If your module uses a nondeterministic function, you can specify that hello module is non-deterministic by setting hello optional **isDeterministic** attribute too**FALSE**.</span></span> <span data-ttu-id="3118d-166">Ez biztosítja, hogy hello modult akkor fut újra, amikor hello kísérlet fut, akkor is, ha hello modul bemeneti és a paraméterek nem változtak.</span><span class="sxs-lookup"><span data-stu-id="3118d-166">This insures that hello module is rerun whenever hello experiment is run, even if hello module input and parameters have not changed.</span></span> 

### <a name="language-definition"></a><span data-ttu-id="3118d-167">Nyelv meghatározása</span><span class="sxs-lookup"><span data-stu-id="3118d-167">Language Definition</span></span>
<span data-ttu-id="3118d-168">Hello **nyelvi** az XML-definíciós fájljának eleme használt toospecify hello egyéni modul nyelv.</span><span class="sxs-lookup"><span data-stu-id="3118d-168">hello **Language** element in your XML definition file is used toospecify hello custom module language.</span></span> <span data-ttu-id="3118d-169">R jelenleg csak a támogatott nyelvi hello.</span><span class="sxs-lookup"><span data-stu-id="3118d-169">Currently, R is hello only supported language.</span></span> <span data-ttu-id="3118d-170">hello értékének hello **sourceFile** attribútum hello R-fájl hello függvény toocall hello modul futtatásakor hello nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3118d-170">hello value of hello **sourceFile** attribute must be hello name of hello R file that contains hello function toocall when hello module is run.</span></span> <span data-ttu-id="3118d-171">Ez a fájl hello zip-csomagját részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3118d-171">This file must be part of hello zip package.</span></span> <span data-ttu-id="3118d-172">hello értékének hello **entryPoint** attribútum meghívott hello függvény hello nevét, és meg kell egyeznie egy érvényes ellátott függvényt hívják hello forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="3118d-172">hello value of hello **entryPoint** attribute is hello name of hello function being called and must match a valid function defined with in hello source file.</span></span>

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a><span data-ttu-id="3118d-173">Portok</span><span class="sxs-lookup"><span data-stu-id="3118d-173">Ports</span></span>
<span data-ttu-id="3118d-174">hello egy egyéni modul bemeneti és kimeneti port meg van határozva a hello gyermekelemei **portok** hello XML-definíciós fájljának szakasza.</span><span class="sxs-lookup"><span data-stu-id="3118d-174">hello input and output ports for a custom module are specified in child elements of hello **Ports** section of hello XML definition file.</span></span> <span data-ttu-id="3118d-175">Ezek az elemek sorrendjét hello hello elrendezés tapasztalt (UX) felhasználók határozza meg.</span><span class="sxs-lookup"><span data-stu-id="3118d-175">hello order of these elements determines hello layout experienced (UX) by users.</span></span> <span data-ttu-id="3118d-176">hello első gyermekét **bemeneti** vagy **kimeneti** hello felsorolt **portok** hello XML-fájl eleme lesz hello bal szélső bemeneti portját a Machine Learning UX hello</span><span class="sxs-lookup"><span data-stu-id="3118d-176">hello first child **input** or **output** listed in hello **Ports** element of hello XML file becomes hello left-most input port in hello Machine Learning UX.</span></span>
<span data-ttu-id="3118d-177">Adja meg mindegyik, és előfordulhat, hogy a kimeneti portra egy nem kötelező **leírás** hello szöveg látható, ha hello egérmutatót rámutat hello port a Machine Learning felhasználói felület hello megadó gyermekelemet.</span><span class="sxs-lookup"><span data-stu-id="3118d-177">Each input and output port may have an optional **Description** child element that specifies hello text shown when you hover hello mouse cursor over hello port in hello Machine Learning UI.</span></span>

<span data-ttu-id="3118d-178">**Szabályok portok**:</span><span class="sxs-lookup"><span data-stu-id="3118d-178">**Ports Rules**:</span></span>

* <span data-ttu-id="3118d-179">Maximális száma **bemeneti és kimeneti portok** minden 8 van.</span><span class="sxs-lookup"><span data-stu-id="3118d-179">Maximum number of **input and output ports** is 8 for each.</span></span>

### <a name="input-elements"></a><span data-ttu-id="3118d-180">Bemeneti elemei</span><span class="sxs-lookup"><span data-stu-id="3118d-180">Input elements</span></span>
<span data-ttu-id="3118d-181">A bemeneti portok lehetővé teszik toopass adatok tooyour R függvény és munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="3118d-181">Input ports allow you toopass data tooyour R function and workspace.</span></span> <span data-ttu-id="3118d-182">Hello **adattípusok** , amely a bemeneti portok a következők támogatottak:</span><span class="sxs-lookup"><span data-stu-id="3118d-182">hello **data types** that are supported for input ports are as follows:</span></span> 

<span data-ttu-id="3118d-183">**A DataTable:** ehhez a típushoz lett átadva tooyour R függvény bemeneti egy data.frame.</span><span class="sxs-lookup"><span data-stu-id="3118d-183">**DataTable:** This type is passed tooyour R function as a data.frame.</span></span> <span data-ttu-id="3118d-184">Tulajdonképpen bármely típusa (például a CSV-fájlok vagy a ARFF fájlok), és hogy Machine Learning által támogatott kompatibilisek-e **DataTable** konvertált tooa data.frame automatikusan vannak.</span><span class="sxs-lookup"><span data-stu-id="3118d-184">In fact, any types (for example, CSV files or ARFF files) that are supported by Machine Learning and that are compatible with **DataTable** are converted tooa data.frame automatically.</span></span> 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

<span data-ttu-id="3118d-185">Hello **azonosító** társított minden egyes attribútum **DataTable** bemeneti porthoz egyedi értéknek kell tartoznia, és ezt az értéket meg kell egyeznie a megfelelő nevű az R-függvény paramétere.</span><span class="sxs-lookup"><span data-stu-id="3118d-185">hello **id** attribute associated with each **DataTable** input port must have a unique value and this value must match its corresponding named parameter in your R function.</span></span>
<span data-ttu-id="3118d-186">Nem kötelező **DataTable** nem átadott kísérlet a bemeneti portok hello értéke lehet **NULL** átadott toohello R függvény és választható zip portok figyelmen kívül lesznek hagyva hello bemeneti nincs csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="3118d-186">Optional **DataTable** ports that are not passed as input in an experiment have hello value **NULL** passed toohello R function and optional zip ports are ignored if hello input is not connected.</span></span> <span data-ttu-id="3118d-187">Hello **isOptional** attribútum nem kötelező megadni mindkét hello **DataTable** és **Zip-** , és meg kell adnia *hamis* alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="3118d-187">hello **isOptional** attribute is optional for both hello **DataTable** and **Zip** types and is *false* by default.</span></span>

<span data-ttu-id="3118d-188">**Zip:** egyéni modulok fogad el bemenetként egy zip-fájlt.</span><span class="sxs-lookup"><span data-stu-id="3118d-188">**Zip:** Custom modules can accept a zip file as input.</span></span> <span data-ttu-id="3118d-189">A bemeneti van csomagolva, a függvény hello R működő könyvtárba</span><span class="sxs-lookup"><span data-stu-id="3118d-189">This input is unpacked into hello R working directory of your function</span></span>

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files toobe extracted toohello R working directory.</Description>
           </Input>

<span data-ttu-id="3118d-190">Az egyéni R modul hello azonosító egy Zip-porthoz tartozik toomatch paraméter hello R függvény.</span><span class="sxs-lookup"><span data-stu-id="3118d-190">For custom R modules, hello id for a Zip port does not have toomatch any parameters of hello R function.</span></span> <span data-ttu-id="3118d-191">Ez azért, mert hello zip-fájl automatikusan kibontott toohello R munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="3118d-191">This is because hello zip file is automatically extracted toohello R working directory.</span></span>

<span data-ttu-id="3118d-192">**Bemeneti szabályok:**</span><span class="sxs-lookup"><span data-stu-id="3118d-192">**Input Rules:**</span></span>

* <span data-ttu-id="3118d-193">hello értékének hello **azonosító** hello attribútumának **bemeneti** elemnek kell lennie egy érvényes R változónevet.</span><span class="sxs-lookup"><span data-stu-id="3118d-193">hello value of hello **id** attribute of hello **Input** element must be a valid R variable name.</span></span>
* <span data-ttu-id="3118d-194">hello értékének hello **azonosító** hello attribútumának **bemeneti** elem nem lehet hosszabb 64 karakternél.</span><span class="sxs-lookup"><span data-stu-id="3118d-194">hello value of hello **id** attribute of hello **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="3118d-195">hello értékének hello **neve** hello attribútumának **bemeneti** elem nem lehet hosszabb 64 karakternél.</span><span class="sxs-lookup"><span data-stu-id="3118d-195">hello value of hello **name** attribute of hello **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="3118d-196">hello tartalmának hello **leírás** elem nem lehet hosszabb 128 karakternél</span><span class="sxs-lookup"><span data-stu-id="3118d-196">hello content of hello **Description** element must not be longer than 128 characters</span></span>
* <span data-ttu-id="3118d-197">hello értékének hello **típus** hello attribútumának **bemeneti** elemnek kell lennie *Zip-* vagy *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="3118d-197">hello value of hello **type** attribute of hello **Input** element must be *Zip* or *DataTable*.</span></span>
* <span data-ttu-id="3118d-198">hello értékének hello **isOptional** hello attribútumának **bemeneti** elem nincs szükség (és *hamis* alapértelmezés szerint ha nincs megadva); de ha van megadva, kelllennie*igaz* vagy *hamis*.</span><span class="sxs-lookup"><span data-stu-id="3118d-198">hello value of hello **isOptional** attribute of hello **Input** element is not required (and is *false* by default when not specified); but if it is specified, it must be *true* or *false*.</span></span>

### <a name="output-elements"></a><span data-ttu-id="3118d-199">Kimeneti elemei</span><span class="sxs-lookup"><span data-stu-id="3118d-199">Output elements</span></span>
<span data-ttu-id="3118d-200">**Standard kimeneti portot:** kimeneti portjait csatlakoztatott toohello visszatérési értékek a R függvényből, amelyek ezután felhasználhatók a további modulokat.</span><span class="sxs-lookup"><span data-stu-id="3118d-200">**Standard output ports:** Output ports are mapped toohello return values from your R function, which can then be used by subsequent modules.</span></span> <span data-ttu-id="3118d-201">*A DataTable* hello csak a standard kimeneti port típus jelenleg támogatott.</span><span class="sxs-lookup"><span data-stu-id="3118d-201">*DataTable* is hello only standard output port type supported currently.</span></span> <span data-ttu-id="3118d-202">(Támogatása *tanulókkal* és *átalakítja* kapja.) A *DataTable* output típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="3118d-202">(Support for *Learners* and *Transforms* is forthcoming.) A *DataTable* output is defined as:</span></span>

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

<span data-ttu-id="3118d-203">Az egyéni R modul kimenetek, a hello hello értékének **azonosító** attribútumnak nincs bármi toocorrespond hello R-parancsfájl, de ennek egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3118d-203">For outputs in custom R modules, hello value of hello **id** attribute does not have toocorrespond with anything in hello R script, but it must be unique.</span></span> <span data-ttu-id="3118d-204">Egy egy modul kimeneti hello visszatérési érték a hello R függvénynek kell lennie egy *data.frame*.</span><span class="sxs-lookup"><span data-stu-id="3118d-204">For a single module output, hello return value from hello R function must be a *data.frame*.</span></span> <span data-ttu-id="3118d-205">A rendezés toooutput egynél több objektum támogatott adattípusú, hello megfelelő kimeneti portokat kell toobe hello XML-definíciós fájljának megadott és hello objektumok listáját adja vissza a toobe kell.</span><span class="sxs-lookup"><span data-stu-id="3118d-205">In order toooutput more than one object of a supported data type, hello appropriate output ports need toobe specified in hello XML definition file and hello objects need toobe returned as a list.</span></span> <span data-ttu-id="3118d-206">hello kimeneti objektumok toooutput portok bal oldali tooright tükröző hello-listát adott vissza helyet hello objektumok hello ahhoz rendeli.</span><span class="sxs-lookup"><span data-stu-id="3118d-206">hello output objects are assigned toooutput ports from left tooright, reflecting hello order in which hello objects are placed in hello returned list.</span></span>

<span data-ttu-id="3118d-207">Például, ha azt szeretné, hogy toomodify hello **egyéni hozzáadása sorok** modul toooutput hello eredeti két adatkészletet, *dataset1* és *dataset2*, továbbá új toohello csatlakoztatva adatkészlet, *adatkészlet*, (sorrendje, a bal oldali tooright,: *adatkészlet*, *dataset1*, *dataset2*), majd adja meg a hello a kimeneti portok hello CustomAddRows.xml fájlban az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3118d-207">For example, if you want toomodify hello **Custom Add Rows** module toooutput hello original two datasets, *dataset1* and *dataset2*, in addition toohello new joined dataset, *dataset*, (in an order, from left tooright, as: *dataset*, *dataset1*, *dataset2*), then define hello output ports in hello CustomAddRows.xml file as follows:</span></span>

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


<span data-ttu-id="3118d-208">És a "CustomAddRows.R" hello megfelelő sorrendben listájaként hello objektumok listáját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="3118d-208">And return hello list of objects in a list in hello correct order in ‘CustomAddRows.R’:</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

<span data-ttu-id="3118d-209">**A képi megjelenítés kimeneti:** azt is megadhatja a kimeneti portra típusú *képi megjelenítés*, amely hello R grafikus eszköz és a konzol kimeneti hello kimenet megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="3118d-209">**Visualization output:** You can also specify an output port of type *Visualization*, which displays hello output from hello R graphics device and console output.</span></span> <span data-ttu-id="3118d-210">Ez a port nem hello R függvény kimeneti része, és nem ütközik más hello hello sorrendjének kimeneti port típusok.</span><span class="sxs-lookup"><span data-stu-id="3118d-210">This port is not part of hello R function output and does not interfere with hello order of hello other output port types.</span></span> <span data-ttu-id="3118d-211">tooadd a képi megjelenítés port toohello egyéni modulokkal, adja hozzá egy **kimeneti** elem értéke az *képi megjelenítés* a saját **típus** attribútum:</span><span class="sxs-lookup"><span data-stu-id="3118d-211">tooadd a visualization port toohello custom modules, add an **Output** element with a value of *Visualization* for its **type** attribute:</span></span>

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View hello R console graphics device output.</Description>
    </Output>

<span data-ttu-id="3118d-212">**Kimeneti szabályok:**</span><span class="sxs-lookup"><span data-stu-id="3118d-212">**Output Rules:**</span></span>

* <span data-ttu-id="3118d-213">hello értékének hello **azonosító** hello attribútumának **kimeneti** elemnek kell lennie egy érvényes R változónevet.</span><span class="sxs-lookup"><span data-stu-id="3118d-213">hello value of hello **id** attribute of hello **Output** element must be a valid R variable name.</span></span>
* <span data-ttu-id="3118d-214">hello értékének hello **azonosító** hello attribútumának **kimeneti** elem nem lehet hosszabb 32 karakternél.</span><span class="sxs-lookup"><span data-stu-id="3118d-214">hello value of hello **id** attribute of hello **Output** element must not be longer than 32 characters.</span></span>
* <span data-ttu-id="3118d-215">hello értékének hello **neve** hello attribútumának **kimeneti** elem nem lehet hosszabb 64 karakternél.</span><span class="sxs-lookup"><span data-stu-id="3118d-215">hello value of hello **name** attribute of hello **Output** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="3118d-216">hello értékének hello **típus** hello attribútumának **kimeneti** elemnek kell lennie *képi megjelenítés*.</span><span class="sxs-lookup"><span data-stu-id="3118d-216">hello value of hello **type** attribute of hello **Output** element must be *Visualization*.</span></span>

### <a name="arguments"></a><span data-ttu-id="3118d-217">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="3118d-217">Arguments</span></span>
<span data-ttu-id="3118d-218">További adatok átadhatók toohello R függvény keresztül modul paraméterei hello meghatározott **argumentumok** elemet.</span><span class="sxs-lookup"><span data-stu-id="3118d-218">Additional data can be passed toohello R function via module parameters which are defined in hello **Arguments** element.</span></span> <span data-ttu-id="3118d-219">Hello modul kiválasztásakor hello jobb szélső tulajdonságok ablaktáblájában hello Machine Learning felhasználói felületén ezek a paraméterek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3118d-219">These parameters appear in hello rightmost properties pane of hello Machine Learning UI when hello module is selected.</span></span> <span data-ttu-id="3118d-220">Az argumentumok hello támogatott típusok lehetnek, vagy létrehozhat egy egyéni számbavételi szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="3118d-220">Arguments can be any of hello supported types or you can create a custom enumeration when needed.</span></span> <span data-ttu-id="3118d-221">Hasonló toohello **portok** elemek, **argumentumok** elemek lehet egy nem kötelező **leírás** elem, amely itt jelenik meg, ha hello egérrel hello szöveg keresztül hello paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="3118d-221">Similar toohello **Ports** elements, **Arguments** elements can have an optional **Description** element that specifies hello text that appears when you hover hello mouse over hello parameter name.</span></span>
<span data-ttu-id="3118d-222">Egy modul, például a defaultValue, minValue és maxValue választható tulajdonságok adhatók hozzá, az attribútumok tooa tooany argumentum **tulajdonságok** elemet.</span><span class="sxs-lookup"><span data-stu-id="3118d-222">Optional properties for a module, such as defaultValue, minValue, and maxValue can be added tooany argument as attributes tooa **Properties** element.</span></span> <span data-ttu-id="3118d-223">Érvényes tulajdonságainak hello **tulajdonságok** elem hello argumentum típusa attól függ, és a támogatott hello argumentumtípus hello a következő szakaszban ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="3118d-223">Valid properties for hello **Properties** element depend on hello argument type and are described with hello supported argument types in hello next section.</span></span> <span data-ttu-id="3118d-224">Hello argumentumokat **isOptional** tulajdonsága túl**"true"** nem igényelnek hello felhasználói tooenter értéket.</span><span class="sxs-lookup"><span data-stu-id="3118d-224">Arguments with hello **isOptional** property set too**"true"** do not require hello user tooenter a value.</span></span> <span data-ttu-id="3118d-225">Ha az érték nem szerepel a toohello argumentum, majd hello argumentum nem toohello belépési pont függvény lett átadva.</span><span class="sxs-lookup"><span data-stu-id="3118d-225">If a value is not provided toohello argument, then hello argument is not passed toohello entry point function.</span></span> <span data-ttu-id="3118d-226">Argumentumokat hello belépési pont függvény nem kötelező kell toobe hello függvény, explicit módon kezeli pl. hello belépési pont függvény definícióját a NULL alapértelmezett értéket rendelni hozzá.</span><span class="sxs-lookup"><span data-stu-id="3118d-226">Arguments of hello entry point function that are optional need toobe explicitly handled by hello function, e.g. assigned a default value of NULL in hello entry point function definition.</span></span> <span data-ttu-id="3118d-227">Egy nem kötelező argumentumában csak kényszeríti hello egyéb argumentum megkötések, azaz a min vagy max, ha hello felhasználó által megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="3118d-227">An optional argument will only enforce hello other argument constraints, i.e. min or max, if a value is provided by hello user.</span></span>
<span data-ttu-id="3118d-228">Csakúgy, mint a be- és kimenetekkel, rendkívül fontos, hogy rendelkezik-e hello paraméterek hozzájuk rendelt egyedi azonosító érték.</span><span class="sxs-lookup"><span data-stu-id="3118d-228">As with inputs and outputs, it is critical that each of hello parameters have unique id values associated with them.</span></span> <span data-ttu-id="3118d-229">A gyors üzembe helyezési példa hello tartozó azonosító/paraméter volt *swap*.</span><span class="sxs-lookup"><span data-stu-id="3118d-229">In our quick start example hello associated id/parameter was *swap*.</span></span>

### <a name="arg-element"></a><span data-ttu-id="3118d-230">Arg elem</span><span class="sxs-lookup"><span data-stu-id="3118d-230">Arg element</span></span>
<span data-ttu-id="3118d-231">Egy modul paraméter van definiálva hello segítségével **Arg** hello gyermekeleme **argumentumok** hello XML-definíciós fájljának szakasza.</span><span class="sxs-lookup"><span data-stu-id="3118d-231">A module parameter is defined using hello **Arg** child element of hello **Arguments** section of hello XML definition file.</span></span> <span data-ttu-id="3118d-232">A hello gyermekelemek a hello **portok** területen hello paraméterek rendelési hello **argumentumok** szakasz határozza meg a metódusban hello UX hello elrendezés</span><span class="sxs-lookup"><span data-stu-id="3118d-232">As with hello child elements in hello **Ports** section, hello ordering of parameters in hello **Arguments** section defines hello layout encountered in hello UX.</span></span> <span data-ttu-id="3118d-233">hello paraméterek jelennek meg felülről lefelé hello azonos rendelés nincsenek meghatározva a felhasználói felület hello hello XML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="3118d-233">hello parameters appear from top down in hello UI in hello same order in which they are defined in hello XML file.</span></span> <span data-ttu-id="3118d-234">az itt felsorolt paramétereket támogatja a Machine Learning hello típusok.</span><span class="sxs-lookup"><span data-stu-id="3118d-234">hello types supported by Machine Learning for parameters are listed here.</span></span> 

<span data-ttu-id="3118d-235">**int** – egész szám (32 bites) típusú paramétert.</span><span class="sxs-lookup"><span data-stu-id="3118d-235">**int** – an Integer (32-bit) type parameter.</span></span>

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* <span data-ttu-id="3118d-236">*Választható tulajdonságok*: **min**, **maximális**, **alapértelmezett** és **isOptional**</span><span class="sxs-lookup"><span data-stu-id="3118d-236">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="3118d-237">**kettős** – dupla típusú paraméter.</span><span class="sxs-lookup"><span data-stu-id="3118d-237">**double** – a double type parameter.</span></span>

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* <span data-ttu-id="3118d-238">*Választható tulajdonságok*: **min**, **maximális**, **alapértelmezett** és **isOptional**</span><span class="sxs-lookup"><span data-stu-id="3118d-238">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="3118d-239">**logikai** – a logikai paraméter által a UX jelölőnégyzettel jelölt</span><span class="sxs-lookup"><span data-stu-id="3118d-239">**bool** – a Boolean parameter that is represented by a check-box in UX.</span></span>

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* <span data-ttu-id="3118d-240">*Választható tulajdonságok*: **alapértelmezett** -hamis, ha nincs beállítva.</span><span class="sxs-lookup"><span data-stu-id="3118d-240">*Optional Properties*: **default** - false if not set</span></span>

<span data-ttu-id="3118d-241">**karakterlánc**: szabványos karakterláncok</span><span class="sxs-lookup"><span data-stu-id="3118d-241">**string**: a standard string</span></span>

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* <span data-ttu-id="3118d-242">*Választható tulajdonságok*: **alapértelmezett** és **isOptional**</span><span class="sxs-lookup"><span data-stu-id="3118d-242">*Optional Properties*: **default** and **isOptional**</span></span>

<span data-ttu-id="3118d-243">**ColumnPicker**: egy oszlop kiválasztási paraméter.</span><span class="sxs-lookup"><span data-stu-id="3118d-243">**ColumnPicker**: a column selection parameter.</span></span> <span data-ttu-id="3118d-244">Ez a típus a hello UX egy Oszlopválasztó kezeli.</span><span class="sxs-lookup"><span data-stu-id="3118d-244">This type renders in hello UX as a column chooser.</span></span> <span data-ttu-id="3118d-245">Hello **tulajdonság** elem, amelyből oszlop van kijelölve, ahol hello port céltípus kell hello port használt ide toospecify hello azonosítóját *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="3118d-245">hello **Property** element is used here toospecify hello id of hello port from which columns are selected, where hello target port type must be *DataTable*.</span></span> <span data-ttu-id="3118d-246">hello Oszlopválasztás hello eredményét toohello R függvény van át a kiválasztott hello oszlopnevek tartalmazó karakterláncok listáját.</span><span class="sxs-lookup"><span data-stu-id="3118d-246">hello result of hello column selection is passed toohello R function as a list of strings containing hello selected column names.</span></span> 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* <span data-ttu-id="3118d-247">*Kötelező tulajdonságokban*: **portId** -megegyezik egy bemeneti elem azonosítója hello típusú *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="3118d-247">*Required Properties*: **portId** - matches hello id of an Input element with type *DataTable*.</span></span>
* <span data-ttu-id="3118d-248">*Választható tulajdonságok*:</span><span class="sxs-lookup"><span data-stu-id="3118d-248">*Optional Properties*:</span></span>
  
  * <span data-ttu-id="3118d-249">**allowedTypes** -szűrők hello oszlop típusokat, amelyek ki tudja választani a.</span><span class="sxs-lookup"><span data-stu-id="3118d-249">**allowedTypes** - Filters hello column types from which you can pick.</span></span> <span data-ttu-id="3118d-250">Érvényes értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="3118d-250">Valid values include:</span></span> 
    
    * <span data-ttu-id="3118d-251">Numerikus</span><span class="sxs-lookup"><span data-stu-id="3118d-251">Numeric</span></span>
    * <span data-ttu-id="3118d-252">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="3118d-252">Boolean</span></span>
    * <span data-ttu-id="3118d-253">Kategorikus</span><span class="sxs-lookup"><span data-stu-id="3118d-253">Categorical</span></span>
    * <span data-ttu-id="3118d-254">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="3118d-254">String</span></span>
    * <span data-ttu-id="3118d-255">Címke</span><span class="sxs-lookup"><span data-stu-id="3118d-255">Label</span></span>
    * <span data-ttu-id="3118d-256">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3118d-256">Feature</span></span>
    * <span data-ttu-id="3118d-257">Pontszám</span><span class="sxs-lookup"><span data-stu-id="3118d-257">Score</span></span>
    * <span data-ttu-id="3118d-258">Összes</span><span class="sxs-lookup"><span data-stu-id="3118d-258">All</span></span>
  * <span data-ttu-id="3118d-259">**alapértelmezett** -hello Oszlopválasztó kiválasztott érvényes alapértelmezett beállításokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3118d-259">**default** - Valid default selections for hello column picker include:</span></span> 
    
    * <span data-ttu-id="3118d-260">None</span><span class="sxs-lookup"><span data-stu-id="3118d-260">None</span></span>
    * <span data-ttu-id="3118d-261">NumericFeature</span><span class="sxs-lookup"><span data-stu-id="3118d-261">NumericFeature</span></span>
    * <span data-ttu-id="3118d-262">NumericLabel</span><span class="sxs-lookup"><span data-stu-id="3118d-262">NumericLabel</span></span>
    * <span data-ttu-id="3118d-263">NumericScore</span><span class="sxs-lookup"><span data-stu-id="3118d-263">NumericScore</span></span>
    * <span data-ttu-id="3118d-264">NumericAll</span><span class="sxs-lookup"><span data-stu-id="3118d-264">NumericAll</span></span>
    * <span data-ttu-id="3118d-265">BooleanFeature</span><span class="sxs-lookup"><span data-stu-id="3118d-265">BooleanFeature</span></span>
    * <span data-ttu-id="3118d-266">BooleanLabel</span><span class="sxs-lookup"><span data-stu-id="3118d-266">BooleanLabel</span></span>
    * <span data-ttu-id="3118d-267">BooleanScore</span><span class="sxs-lookup"><span data-stu-id="3118d-267">BooleanScore</span></span>
    * <span data-ttu-id="3118d-268">BooleanAll</span><span class="sxs-lookup"><span data-stu-id="3118d-268">BooleanAll</span></span>
    * <span data-ttu-id="3118d-269">CategoricalFeature</span><span class="sxs-lookup"><span data-stu-id="3118d-269">CategoricalFeature</span></span>
    * <span data-ttu-id="3118d-270">CategoricalLabel</span><span class="sxs-lookup"><span data-stu-id="3118d-270">CategoricalLabel</span></span>
    * <span data-ttu-id="3118d-271">CategoricalScore</span><span class="sxs-lookup"><span data-stu-id="3118d-271">CategoricalScore</span></span>
    * <span data-ttu-id="3118d-272">CategoricalAll</span><span class="sxs-lookup"><span data-stu-id="3118d-272">CategoricalAll</span></span>
    * <span data-ttu-id="3118d-273">StringFeature</span><span class="sxs-lookup"><span data-stu-id="3118d-273">StringFeature</span></span>
    * <span data-ttu-id="3118d-274">StringLabel</span><span class="sxs-lookup"><span data-stu-id="3118d-274">StringLabel</span></span>
    * <span data-ttu-id="3118d-275">StringScore</span><span class="sxs-lookup"><span data-stu-id="3118d-275">StringScore</span></span>
    * <span data-ttu-id="3118d-276">StringAll</span><span class="sxs-lookup"><span data-stu-id="3118d-276">StringAll</span></span>
    * <span data-ttu-id="3118d-277">AllLabel</span><span class="sxs-lookup"><span data-stu-id="3118d-277">AllLabel</span></span>
    * <span data-ttu-id="3118d-278">AllFeature</span><span class="sxs-lookup"><span data-stu-id="3118d-278">AllFeature</span></span>
    * <span data-ttu-id="3118d-279">AllScore</span><span class="sxs-lookup"><span data-stu-id="3118d-279">AllScore</span></span>
    * <span data-ttu-id="3118d-280">Összes</span><span class="sxs-lookup"><span data-stu-id="3118d-280">All</span></span>

<span data-ttu-id="3118d-281">**Legördülő lista**: egy felhasználó által megadott felsorolt (legördülő) listán.</span><span class="sxs-lookup"><span data-stu-id="3118d-281">**DropDown**: a user-specified enumerated (dropdown) list.</span></span> <span data-ttu-id="3118d-282">hello legördülő elemek hello belül vannak megadva **tulajdonságok** elem használatával egy **elem** elemet.</span><span class="sxs-lookup"><span data-stu-id="3118d-282">hello dropdown items are specified within hello **Properties** element using an **Item** element.</span></span> <span data-ttu-id="3118d-283">Hello **azonosító** minden **elem** egyedinek kell lennie, és egy érvényes R változó.</span><span class="sxs-lookup"><span data-stu-id="3118d-283">hello **id** for each **Item** must be unique and a valid R variable.</span></span> <span data-ttu-id="3118d-284">hello értékének hello **neve** , egy **elem** hello látható szöveg és a toohello R függvénynek átadott hello érték funkcionál.</span><span class="sxs-lookup"><span data-stu-id="3118d-284">hello value of hello **name** of an **Item** serves as both hello text that you see and hello value that is passed toohello R function.</span></span>

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* <span data-ttu-id="3118d-285">*Választható tulajdonságok*:</span><span class="sxs-lookup"><span data-stu-id="3118d-285">*Optional Properties*:</span></span>
  * <span data-ttu-id="3118d-286">**alapértelmezett** – hello hello alapértelmezett tulajdonságot meg kell felelnie az azonosító értéke egy hello értéke **elem** elemek.</span><span class="sxs-lookup"><span data-stu-id="3118d-286">**default** - hello value for hello default property must correspond with an id value from one of hello **Item** elements.</span></span>

### <a name="auxiliary-files"></a><span data-ttu-id="3118d-287">Külső fájlok</span><span class="sxs-lookup"><span data-stu-id="3118d-287">Auxiliary Files</span></span>
<span data-ttu-id="3118d-288">Minden olyan fájlt, amely az egyéni modul ZIP-fájlja kerül folyamatos toobe használható végrehajtási idő alatt.</span><span class="sxs-lookup"><span data-stu-id="3118d-288">Any file that is placed in your custom module ZIP file is going toobe available for use during execution time.</span></span> <span data-ttu-id="3118d-289">A jelen könyvtárstruktúrák megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="3118d-289">Any directory structures present are preserved.</span></span> <span data-ttu-id="3118d-290">Ez azt jelenti, hogy fájl forrás works hello azonos helyileg, és az Azure Machine Learning végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="3118d-290">This means that file sourcing works hello same locally and in Azure Machine Learning execution.</span></span> 

> [!NOTE]
> <span data-ttu-id="3118d-291">Figyelje meg, hogy a fájlok is kibontott too'src "directory, az összes elérési utat kell" src / "előtag.</span><span class="sxs-lookup"><span data-stu-id="3118d-291">Notice that all files are extracted too‘src’ directory so all paths should have ‘src/’ prefix.</span></span>
> 
> 

<span data-ttu-id="3118d-292">Tegyük fel szeretné tooremove hello adatkészletből NAs a sorokat, és is távolítsa el az ismétlődő sorokat előtt ellenőrizze a CustomAddRows, és már leírt egy R függvény, amelyet, amely egy fájlban RemoveDupNARows.R:</span><span class="sxs-lookup"><span data-stu-id="3118d-292">For example, say you want tooremove any rows with NAs from hello dataset, and also remove any duplicate rows, before outputting it into CustomAddRows, and you’ve already written an R function that does that in a file RemoveDupNARows.R:</span></span>

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
<span data-ttu-id="3118d-293">Hello kiegészítő fájlt RemoveDupNARows.R hello CustomAddRows függvény is forrás:</span><span class="sxs-lookup"><span data-stu-id="3118d-293">You can source hello auxiliary file RemoveDupNARows.R in hello CustomAddRows function:</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

<span data-ttu-id="3118d-294">Ezt követően töltse fel az "CustomAddRows.R", "CustomAddRows.xml" és "RemoveDupNARows.R" egy egyéni R modult tartalmazó zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="3118d-294">Next, upload a zip file containing ‘CustomAddRows.R’, ‘CustomAddRows.xml’, and ‘RemoveDupNARows.R’ as a custom R module.</span></span>

## <a name="execution-environment"></a><span data-ttu-id="3118d-295">Végrehajtási környezet</span><span class="sxs-lookup"><span data-stu-id="3118d-295">Execution Environment</span></span>
<span data-ttu-id="3118d-296">hello végrehajtási környezet hello R-parancsfájl által használt verziójával megegyező verzióra r hello hello **R-parancsfájl végrehajtása** modul is használható hello azonos és csomagok alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="3118d-296">hello execution environment for hello R script uses hello same version of R as hello **Execute R Script** module and can use hello same default packages.</span></span> <span data-ttu-id="3118d-297">Hozzáadhat további R csomagok tooyour egyéni modult is belefoglalja ezeket hello egyéni modul zip-csomagját.</span><span class="sxs-lookup"><span data-stu-id="3118d-297">You can also add additional R packages tooyour custom module by including them in hello custom module zip package.</span></span> <span data-ttu-id="3118d-298">Csak töltődnek be azokat az R-parancsfájl, mint az R környezetben.</span><span class="sxs-lookup"><span data-stu-id="3118d-298">Just load them in your R script as you would in your own R environment.</span></span> 

<span data-ttu-id="3118d-299">**Hello végrehajtási környezet korlátai** tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3118d-299">**Limitations of hello execution environment** include:</span></span>

* <span data-ttu-id="3118d-300">Nem állandó fájlrendszer: több frissítési kísérletei során hello között nem maradnak meg fájlokat, ha az egyéni modul hello fut ugyanabban a modulban.</span><span class="sxs-lookup"><span data-stu-id="3118d-300">Non-persistent file system: Files written when hello custom module is run are not persisted across multiple runs of hello same module.</span></span>
* <span data-ttu-id="3118d-301">Nincs hálózati hozzáférés</span><span class="sxs-lookup"><span data-stu-id="3118d-301">No network access</span></span>

