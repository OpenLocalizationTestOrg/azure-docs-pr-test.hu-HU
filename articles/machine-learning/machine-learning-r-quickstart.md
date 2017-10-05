---
title: "Az R nyelv a Machine Learning gyors üzembe helyezési útmutató |} Microsoft Docs"
description: "Az R programozási oktatóanyag segítségével gyorsan az előrejelzési megoldások létrehozásához Azure Machine Learning Studio az R nyelv első lépéseiben."
keywords: "gyors üzembe helyezés, r nyelven, r programozási nyelv, r programozási útmutató"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 598f5ce445e520b6cdc347c80f7f3dcbc9c2c9e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="quickstart-tutorial-for-the-r-programming-language-for-azure-machine-learning"></a><span data-ttu-id="7cf1d-104">Bevezető oktatóprogram az Azure Machine Learning rendszerrel használt R programozási nyelvbe</span><span class="sxs-lookup"><span data-stu-id="7cf1d-104">Quickstart tutorial for the R programming language for Azure Machine Learning</span></span>

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a><span data-ttu-id="7cf1d-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="7cf1d-105">Introduction</span></span>
<span data-ttu-id="7cf1d-106">Gyors üzembe helyezési oktatóanyag segítségével gyorsan az Azure Machine Learning kiterjesztése az R programozási nyelv használatával indíthat el.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-106">This quickstart tutorial helps you quickly start extending Azure Machine Learning by using the R programming language.</span></span> <span data-ttu-id="7cf1d-107">Ez az oktatóanyag R programozási létrehozása, tesztelése és R kódban az Azure Machine Learning segítségével hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-107">Follow this R programming tutorial to create, test and execute R code within Azure Machine Learning.</span></span> <span data-ttu-id="7cf1d-108">Oktatóanyag munka, az R nyelv használatával az Azure Machine Learning előrejelzési teljes körű megoldást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-108">As you work through tutorial, you will create a complete forecasting solution by using the R language in Azure Machine Learning.</span></span>  

<span data-ttu-id="7cf1d-109">Számos hatékony gépi tanulási és adatok adatkezelési modulok Microsoft Azure Machine Learning tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-109">Microsoft Azure Machine Learning contains many powerful machine learning and data manipulation modules.</span></span> <span data-ttu-id="7cf1d-110">A hatékony R nyelv, a nyelv franca Analytics leírása.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-110">The powerful R language has been described as the lingua franca of analytics.</span></span> <span data-ttu-id="7cf1d-111">Elemzés és az adatok kezelése az Azure Machine Learning boldogan, bővíthető R. használatával Ez a kombináció biztosít a méretezhetőséget és a központi telepítés az Azure Machine Learning könnyű a rugalmasságot és az r mély elemzés</span><span class="sxs-lookup"><span data-stu-id="7cf1d-111">Happily, analytics and data manipulation in Azure Machine Learning can be extended by using R. This combination provides the scalability and ease of deployment of Azure Machine Learning with the flexibility and deep analytics of R.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-the-dataset"></a><span data-ttu-id="7cf1d-112">Az előrejelzés és az adatkészletet</span><span class="sxs-lookup"><span data-stu-id="7cf1d-112">Forecasting and the dataset</span></span>
<span data-ttu-id="7cf1d-113">Az előrejelzés a széles körben munkavállalók és elég hasznos lehet analitikai módszer.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-113">Forecasting is a widely employed and quite useful analytical method.</span></span> <span data-ttu-id="7cf1d-114">Közös közé határozza cikkek optimális készlet szintek történő előrejelzésére makrogazdasági változók meghatározása előrejelzésére használja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-114">Common uses range from predicting sales of seasonal items, determining optimal inventory levels, to predicting macroeconomic variables.</span></span> <span data-ttu-id="7cf1d-115">Az előrejelzés idő adatsorozat modellekkel való általában történik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-115">Forecasting is typically done with time series models.</span></span>

<span data-ttu-id="7cf1d-116">Adatsorozat időadatok az adatai, amelyben az értékek rendelkezik idő indexszel.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-116">Time series data is data in which the values have a time index.</span></span> <span data-ttu-id="7cf1d-117">Az idő index rendszeres, lehet, például minden hónap vagy percben, vagy szabálytalan.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-117">The time index can be regular, e.g. every month or every minute, or irregular.</span></span> <span data-ttu-id="7cf1d-118">Egy idősorozat-modellbe idő adatsorozat adatokon alapul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-118">A time series model is based on time series data.</span></span> <span data-ttu-id="7cf1d-119">Az R programozási nyelvet egy rugalmas keretrendszer és az idő adatsorozat adatok széles körű elemzés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-119">The R programming language contains a flexible framework and extensive analytics for time series data.</span></span>

<span data-ttu-id="7cf1d-120">Az első lépések útmutató rendszer kell kaliforniai tejtermelésre használata és díjszabási adatok.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-120">In this quickstart guide we will be working with California dairy production and pricing data.</span></span> <span data-ttu-id="7cf1d-121">Ezek az adatok több tejtermékek és az ár tejzsír, egy teljesítményteszt hagyományos havi információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-121">This data includes monthly information on the production of several dairy products and the price of milk fat, a benchmark commodity.</span></span>

<span data-ttu-id="7cf1d-122">A cikkben, és az R parancsfájlokat használt adatok [itt letöltött][download].</span><span class="sxs-lookup"><span data-stu-id="7cf1d-122">The data used in this article, along with R scripts, can be [downloaded here][download].</span></span> <span data-ttu-id="7cf1d-123">Ezeket az adatokat eredetileg volt synthesized http://future.aae.wisc.edu/tab/production.html, a Wisconsin egyetemi érhető el információ.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-123">This data was originally synthesized from information available from the University of Wisconsin at http://future.aae.wisc.edu/tab/production.html.</span></span>

### <a name="organization"></a><span data-ttu-id="7cf1d-124">Szervezet</span><span class="sxs-lookup"><span data-stu-id="7cf1d-124">Organization</span></span>
<span data-ttu-id="7cf1d-125">Megismerheti, hogyan hozhat létre, tesztelheti és elemzés és az adatok R adatkezelési kód végrehajtása az Azure Machine Learning környezetben, azt fogja előrehaladás számos lépésen.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-125">We will progress through several steps as you learn how to create, test and execute analytics and data manipulation R code in the Azure Machine Learning environment.</span></span>  

* <span data-ttu-id="7cf1d-126">Először néhány a alapjait, az Azure Machine Learning Studio környezet az R nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-126">First we will explore the basics of using the R language in the Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="7cf1d-127">Ezután azt előrehaladás megvitatása adatok, az R-kód és az Azure Machine Learning környezetben grafikus i/o különböző jellemzőinek megtekintését.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-127">Then we progress to discussing various aspects of I/O for data, R code and graphics in the Azure Machine Learning environment.</span></span>
* <span data-ttu-id="7cf1d-128">Azt fogja majd összeállítani az előrejelzési megoldás első része adatok tisztítása és átalakítása kódjának létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-128">We will then construct the first part of our forecasting solution by creating code for data cleaning and transformation.</span></span>
* <span data-ttu-id="7cf1d-129">Az előkészített adataink végezzük el az adathalmaz változók számos közötti összefüggések elemzése.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-129">With our data prepared we will perform an analysis of the correlations between several of the variables in our dataset.</span></span>
* <span data-ttu-id="7cf1d-130">Végül létre fogunk hozni egy határozza idő series előrejelzési modell tejtermelésre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-130">Finally, we will create a seasonal time series forecasting model for milk production.</span></span>

## <span data-ttu-id="7cf1d-131"><a id="mlstudio"></a>Együttműködhet az R nyelv a Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="7cf1d-131"><a id="mlstudio"></a>Interact with R language in Machine Learning Studio</span></span>
<span data-ttu-id="7cf1d-132">Ez a szakasz végigvezeti az R programozási nyelv, a Machine Learning Studio környezetben való interakció néhány alapjait.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-132">This section takes you through some basics of interacting with the R programming language in the Machine Learning Studio environment.</span></span> <span data-ttu-id="7cf1d-133">Az R nyelv hozzon létre egyéni elemzés és adatok adatkezelési modulok az Azure Machine Learning környezeten belül hatékony eszközt biztosít.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-133">The R language provides a powerful tool to create customized analytics and data manipulation modules within the Azure Machine Learning environment.</span></span>

<span data-ttu-id="7cf1d-134">Rstudióból történő fejlesztéséhez, tesztelése és hibakeresése kis méretű R-kód fogja használni.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-134">I will use RStudio to develop, test and debug R code on a small scale.</span></span> <span data-ttu-id="7cf1d-135">Ez a kód vágjon és a beillesztési műveleteket egy [R-parancsfájl végrehajtása] [ execute-r-script] modul a Machine Learning Studióban készen áll a futtatásra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-135">This code is then cut and paste into an [Execute R Script][execute-r-script] module in Machine Learning Studio ready to run.</span></span>  

### <a name="the-execute-r-script-module"></a><span data-ttu-id="7cf1d-136">Az R-parancsfájl végrehajtása modul</span><span class="sxs-lookup"><span data-stu-id="7cf1d-136">The Execute R Script module</span></span>
<span data-ttu-id="7cf1d-137">A Machine Learning Studio belül R parancsfájlok belül a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-137">Within Machine Learning Studio, R scripts are run within the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-138">Egy példa a [R-parancsfájl végrehajtása] [ execute-r-script] modul a Machine Learning Studióban az 1. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-138">An example of the [Execute R Script][execute-r-script] module in Machine Learning Studio is shown in Figure 1.</span></span>

 ![R programozási nyelv: végrehajtás R-parancsfájl a modul a Machine Learning Studióban kiválasztva][1]

<span data-ttu-id="7cf1d-140">*1. ábra. A Machine Learning Studio környezet az R-parancsfájl végrehajtása modul kiválasztva megjelenítő.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-140">*Figure 1. The Machine Learning Studio environment showing the Execute R Script module selected.*</span></span>

<span data-ttu-id="7cf1d-141">1. ábra hivatkozik, vizsgáljuk meg néhány használata a Machine Learning Studio környezet legfontosabb részeit a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-141">Referring to Figure 1, let's look at some of the key parts of the Machine Learning Studio environment for working with the [Execute R Script][execute-r-script] module.</span></span>

* <span data-ttu-id="7cf1d-142">A modul a kísérletben a középső ablaktáblán jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-142">The modules in the experiment are shown in the center pane.</span></span>
* <span data-ttu-id="7cf1d-143">A jobb oldali ablaktábla felső részén egy ablakban megtekintheti és szerkesztheti az R parancsfájlok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-143">The upper part of the right pane contains a window to view and edit your R scripts.</span></span>  
* <span data-ttu-id="7cf1d-144">A jobb oldali alsó részén látható néhány tulajdonságát a [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="7cf1d-144">The lower part of right pane shows some properties of the [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="7cf1d-145">A hiba- és kimeneti naplókat, ha a panel megfelelő tesztüzeméhez kattintva tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-145">You can view the error and output logs by clicking on the appropriate spots of this pane.</span></span>

<span data-ttu-id="7cf1d-146">Azt, természetesen megvitatása lesz, a [R-parancsfájl végrehajtása] [ execute-r-script] Ez a dokumentum további részében részletesebben.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-146">We will, of course, be discussing the [Execute R Script][execute-r-script] in greater detail in the rest of this document.</span></span>

<span data-ttu-id="7cf1d-147">Az összetett R funkciók használatakor javasolt, hogy szerkesztése, tesztelése és hibakeresése a Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-147">When working with complex R functions, I recommend that you edit, test and debug in RStudio.</span></span> <span data-ttu-id="7cf1d-148">Csakúgy, mint bármely szoftverfejlesztői Növekményesen kiterjesztése a kódot, és kis egyszerű vizsgálati eseteknél az tesztelik azt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-148">As with any software development, extend your code incrementally and test it on small simple test cases.</span></span> <span data-ttu-id="7cf1d-149">Kivágás, majd az R parancsfájl ablakban illessze be a funkciók a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-149">Then cut and paste your functions into the R script window of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-150">Ez a megközelítés lehetővé teszi az Rstudióból integrált fejlesztési környezeti (IDE) és a hatványra emelésének Azure Machine Learning hasznosítására.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-150">This approach allows you to harness both the RStudio integrated development environment (IDE) and the power of Azure Machine Learning.</span></span>  

#### <a name="execute-r-code"></a><span data-ttu-id="7cf1d-151">R-kód végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-151">Execute R code</span></span>
<span data-ttu-id="7cf1d-152">Bármely az R-kód a [R-parancsfájl végrehajtása] [ execute-r-script] modul hajtja végre, amikor a kísérlet futtatásához kattintson a a **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-152">Any R code in the [Execute R Script][execute-r-script] module will execute when you run the experiment by clicking on the **Run** button.</span></span> <span data-ttu-id="7cf1d-153">Végrehajtás befejezése után egy pipa jelenik meg a [R-parancsfájl végrehajtása] [ execute-r-script] ikonra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-153">When execution has completed, a check mark will appear on the [Execute R Script][execute-r-script] icon.</span></span>

#### <a name="defensive-r-coding-for-azure-machine-learning"></a><span data-ttu-id="7cf1d-154">Defenzív R kódolási Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7cf1d-154">Defensive R coding for Azure Machine Learning</span></span>
<span data-ttu-id="7cf1d-155">Tegyük fel például, egy webszolgáltatás-bővítmény az R-kód fejlesztői Azure Machine Learning segítségével, meg kell mindenképpen terveznie módját a kód foglalkozni fog váratlan adatok bemeneti és a kivételek.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-155">If you are developing R code for, say, a web service by using Azure Machine Learning, you should definitely plan how your code will deal with an unexpected data input and exceptions.</span></span> <span data-ttu-id="7cf1d-156">Érthetőség kedvéért I nem szereplő nagy átszállítást megakadályozzák ellenőrzik, illetve a legtöbb látható példák kivételkezelő.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-156">To maintain clarity, I have not included much in the way of checking or exception handling in most of the code examples shown.</span></span> <span data-ttu-id="7cf1d-157">Azonban, a Folytatás I fogja diktálni funkciók számos példát R-hez tartozó kivételkezelő funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-157">However, as we proceed I will give you several examples of functions by using R's exception handling capability.</span></span>  

<span data-ttu-id="7cf1d-158">R kivételkezelést teljesebb kezelést van szüksége, ha szeretnék javasoljuk, hogy olvassa el a megfelelő szakaszok a könyv felsorolt Wickham által [B függelék – további olvasási](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="7cf1d-158">If you need a more complete treatment of R exception handling, I recommend you read the applicable sections of the book by Wickham listed in [Appendix B - Further Reading](#appendixb).</span></span>

#### <a name="debug-and-test-r-in-machine-learning-studio"></a><span data-ttu-id="7cf1d-159">Hibakeresését és tesztelését R a Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="7cf1d-159">Debug and test R in Machine Learning Studio</span></span>
<span data-ttu-id="7cf1d-160">Megújítja, a javasolt tesztelése és hibakeresése az R-kód az Rstudióból kis méretű.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-160">To reiterate, I recommend you test and debug your R code on a small scale in RStudio.</span></span> <span data-ttu-id="7cf1d-161">Azonban, vannak esetek, ahová el kell nyomon követheti az R kódproblémáknak az [R-parancsfájl végrehajtása] [ execute-r-script] magát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-161">However, there are cases where you will need to track down R code problems in the [Execute R Script][execute-r-script] itself.</span></span> <span data-ttu-id="7cf1d-162">Ezenkívül ajánlott ellenőrizni a Machine Learning Studio eredményezi.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-162">In addition, it is good practice to check your results in Machine Learning Studio.</span></span>

<span data-ttu-id="7cf1d-163">Az R-kód és az Azure Machine Learning platformon végrehajtása kimenete elsősorban megtalálható kimenetét.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-163">Output from the execution of your R code and on the Azure Machine Learning platform is found primarily in output.log.</span></span> <span data-ttu-id="7cf1d-164">További információk a error.log fogja látni.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-164">Some additional information will be seen in error.log.</span></span>  

<span data-ttu-id="7cf1d-165">Ha a hiba akkor fordul elő, a Machine Learning Studióban az R-kód futtatása során, az első lépések error.log lássunk kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-165">If an error occurs in Machine Learning Studio while running your R code, your first course of action should be to look at error.log.</span></span> <span data-ttu-id="7cf1d-166">Ez a fájl hasznos hibaüzenetek segítenek megérteni, és javítsa ki a hibát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-166">This file can contain useful error messages to help you understand and correct your error.</span></span> <span data-ttu-id="7cf1d-167">Error.log megtekintéséhez kattintson a **nézet hibanapló** a a **tulajdonságok panelen** a a [R-parancsfájl végrehajtása] [ execute-r-script] hiba.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-167">To view error.log, click on **View error log** on the **properties pane** for the [Execute R Script][execute-r-script] containing the error.</span></span>

<span data-ttu-id="7cf1d-168">Például futtattam a következő R-kód egy nem definiált változó y a egy [R-parancsfájl végrehajtása] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-168">For example, I ran the following R code, with an undefined variable y, in an [Execute R Script][execute-r-script] module:</span></span>

    x <- 1.0
    z <- x + y

<span data-ttu-id="7cf1d-169">Ez a kód nem tud végrehajtani, hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-169">This code fails to execute, resulting in an error condition.</span></span> <span data-ttu-id="7cf1d-170">Kattintson a **nézet hibanapló** a a **tulajdonságok panelen** hozza létre a képernyőt a 2. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-170">Clicking on **View error log** on the **properties pane** produces the display shown in Figure 2.</span></span>

  ![Felugró hibaüzenet][2]

<span data-ttu-id="7cf1d-172">*2. ábra. Előugró hibaüzenet.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-172">*Figure 2. Error message pop-up.*</span></span>

<span data-ttu-id="7cf1d-173">Úgy tűnik, igazolnia kell a hely kimenetét a R hibaüzenet megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-173">It looks like we need to look in output.log to see the R error message.</span></span> <span data-ttu-id="7cf1d-174">Kattintson a [R-parancsfájl végrehajtása] [ execute-r-script] majd kattintson a a **kimenetét megtekintése** elemet a **tulajdonságok panelen** jobb.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-174">Click on the [Execute R Script][execute-r-script] and then click on the **View output.log** item on the **properties pane** to the right.</span></span> <span data-ttu-id="7cf1d-175">Egy új böngészőablakban nyílik meg, és a következő látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-175">A new browser window opens, and I see the following.</span></span>

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

<span data-ttu-id="7cf1d-176">Ez a hibaüzenet nem meglepetések számát tartalmazza, és egyértelműen azonosítja a probléma.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-176">This error message contains no surprises and clearly identifies the problem.</span></span>

<span data-ttu-id="7cf1d-177">Az érték az R bármely objektum vizsgálata, kinyomtathatja ezeket az értékeket a kimenetét fájlba.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-177">To inspect the value of any object in R, you can print these values to the output.log file.</span></span> <span data-ttu-id="7cf1d-178">A szabályok objektum értékek vizsgálata azonosak lényegében egy interaktív R munkamenet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-178">The rules for examining object values are essentially the same as in an interactive R session.</span></span> <span data-ttu-id="7cf1d-179">Például ha sorban adjon meg egy változónevet, az objektum értékének nyomtat a kimenetét fájlba.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-179">For example, if you type a variable name on a line, the value of the object will be printed to the output.log file.</span></span>  

#### <a name="packages-in-machine-learning-studio"></a><span data-ttu-id="7cf1d-180">A Machine Learning Studióban csomagok</span><span class="sxs-lookup"><span data-stu-id="7cf1d-180">Packages in Machine Learning Studio</span></span>
<span data-ttu-id="7cf1d-181">Az Azure Machine Learning több mint 350 előre telepített R nyelvi csomagokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-181">Azure Machine Learning comes with over 350 preinstalled R language packages.</span></span> <span data-ttu-id="7cf1d-182">A következő kódot használhatja a [R-parancsfájl végrehajtása] [ execute-r-script] modul előre telepített csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-182">You can use the following code in the [Execute R Script][execute-r-script] module to retrieve a list of the preinstalled packages.</span></span>

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

<span data-ttu-id="7cf1d-183">Ha ez a kód utolsó sora jelenleg nem világos, olvassa el a.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-183">If you don't understand the last line of this code at the moment, read on.</span></span> <span data-ttu-id="7cf1d-184">Ez a dokumentum többi része nagymértékben ismertetik az Azure Machine Learning-környezet az R nyelv használatát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-184">In the rest of this document we will extensively discuss using R in the Azure Machine Learning environment.</span></span>

### <a name="introduction-to-rstudio"></a><span data-ttu-id="7cf1d-185">Rstudióból bemutatása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-185">Introduction to RStudio</span></span>
<span data-ttu-id="7cf1d-186">Rstudióból egy széles körben használt IDE az R. A Szerkesztés, a tesztelés és a hibakereséshez az R-kód gyors üzembe helyezési útmutatóban használt egyes használom Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-186">RStudio is a widely used IDE for R. I will use RStudio for editing, testing and debugging some of the R code used in this quick start guide.</span></span> <span data-ttu-id="7cf1d-187">Ha tesztelt, és készen áll, az R-kód egyszerűen kimásolni, majd a Rstudióból szerkesztő illessze be a Machine Learning Studio [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-187">Once R code is tested and ready, you simply cut and paste from the RStudio editor into a Machine Learning Studio [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="7cf1d-188">Ha nem rendelkezik az R programozási nyelv, a asztali gépen telepítve van, akkor tegye meg most javasolt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-188">If you do not have the R programming language installed on your desktop machine, I recommend you do so now.</span></span> <span data-ttu-id="7cf1d-189">Szabad nyílt forráskódú R nyelv érhető el, az átfogó R archív hálózati (CRAN), [http://www.r-project.org/](http://www.r-project.org/).</span><span class="sxs-lookup"><span data-stu-id="7cf1d-189">Free downloads of open source R language are available at the Comprehensive R Archive Network (CRAN) at [http://www.r-project.org/](http://www.r-project.org/).</span></span> <span data-ttu-id="7cf1d-190">Nincsenek elérhető a Windows, Mac OS és Linux/UNIX letöltések.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-190">There are downloads available for Windows, Mac OS, and Linux/UNIX.</span></span> <span data-ttu-id="7cf1d-191">Válassza ki a közeli tükrözött, és kövesse a letöltési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-191">Choose a nearby mirror and follow the download directions.</span></span> <span data-ttu-id="7cf1d-192">Ezenkívül a CRAN számos hasznos elemzés és az adatok adatkezelési csomagok olyan tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-192">In addition, CRAN contains a wealth of useful analytics and data manipulation packages.</span></span>

<span data-ttu-id="7cf1d-193">Ha most ismerkedik a Rstudióból, töltse le és telepítse az asztali verzióját.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-193">If you are new to RStudio, you should download and install the desktop version.</span></span> <span data-ttu-id="7cf1d-194">A Windows, Mac OS és Linux/UNIX, http://www.rstudio.com/products/RStudio/ tölti le az Rstudióból találja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-194">You can find the RStudio downloads for Windows, Mac OS, and Linux/UNIX at http://www.rstudio.com/products/RStudio/.</span></span> <span data-ttu-id="7cf1d-195">Kövesse az utasításokat, az asztali számítógépre telepítéséhez Rstudióból megadott.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-195">Follow the directions provided to install RStudio on your desktop machine.</span></span>  

<span data-ttu-id="7cf1d-196">Egy oktatóanyag bemutatása Rstudióból https://support.rstudio.com/hc/sections/200107586-Using-RStudio érhető el.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-196">A tutorial introduction to RStudio is available at https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span></span>

<span data-ttu-id="7cf1d-197">I néhány kiegészítő információt nyújt az Rstudióból használatával [függelék][appendixa].</span><span class="sxs-lookup"><span data-stu-id="7cf1d-197">I provide some additional information on using RStudio in [Appendix A][appendixa].</span></span>  

## <span data-ttu-id="7cf1d-198"><a id="scriptmodule"></a>Az R-parancsfájl végrehajtása modul mindkét adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-198"><a id="scriptmodule"></a>Get data in and out of the Execute R Script module</span></span>
<span data-ttu-id="7cf1d-199">Ebben a szakaszban ismertetjük be és ki hogyan be adatok a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-199">In this section we will discuss how you get data into and out of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-200">Azt a rendszer tekintse át kezelését a különböző adattípusok olvassa be és ki a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-200">We will review how to handle various data types read into and out of the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="7cf1d-201">A teljes kód látható az ebben a szakaszban korábban letöltött zip-fájlban van.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-201">The complete code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="load-and-check-data-in-machine-learning-studio"></a><span data-ttu-id="7cf1d-202">Betölteni, és ellenőrizze az adatokat a Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="7cf1d-202">Load and check data in Machine Learning Studio</span></span>
#### <span data-ttu-id="7cf1d-203"><a id="loading"></a>Az adatkészlet betöltése</span><span class="sxs-lookup"><span data-stu-id="7cf1d-203"><a id="loading"></a>Load the dataset</span></span>
<span data-ttu-id="7cf1d-204">Először tölt a **csdairydata.csv** Azure Machine Learning Studio fájlból.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-204">We will start by loading the **csdairydata.csv** file into Azure Machine Learning Studio.</span></span>

* <span data-ttu-id="7cf1d-205">Indítsa el az Azure Machine Learning Studio környezetet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-205">Start your Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="7cf1d-206">Kattintson a **+ új** a képernyő, és válassza ki a bal alsó **Dataset**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-206">Click on **+ NEW** at the lower left of your screen and select **Dataset**.</span></span>
* <span data-ttu-id="7cf1d-207">Válassza ki **helyi fájlból**, majd **Tallózás** válassza ki a fájlt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-207">Select **From Local File**, and then **Browse** to select the file.</span></span>
* <span data-ttu-id="7cf1d-208">Győződjön meg arról, hogy a kijelölt **általános CSV-fájl (.csv) fejlécű** az adatkészlet típusként.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-208">Make sure you have selected **Generic CSV file with header (.csv)** as the type for the dataset.</span></span>
* <span data-ttu-id="7cf1d-209">Kattintson a pipa jelre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-209">Click the check mark.</span></span>
* <span data-ttu-id="7cf1d-210">A dataset feltöltése után megtekintheti az új adatkészlet parancsával a **adatkészletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-210">After the dataset has been uploaded, you should see the new dataset by clicking on the **Datasets** tab.</span></span>  

#### <a name="create-an-experiment"></a><span data-ttu-id="7cf1d-211">A kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-211">Create an experiment</span></span>
<span data-ttu-id="7cf1d-212">Most, hogy néhány adat a Machine Learning Studióban, igazolnia kell hozni egy kísérletet az elemzés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-212">Now that we have some data in Machine Learning Studio, we need to create an experiment to do the analysis.</span></span>  

* <span data-ttu-id="7cf1d-213">Kattintson a **+ új** az alsó balra, és válassza ki **kísérlet**, majd **üres kísérlet**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-213">Click on **+ NEW** at the lower left and select **Experiment**, then **Blank Experiment**.</span></span>
* <span data-ttu-id="7cf1d-214">Jelölje ki, és módosítja, a kísérlet nevet adhat a **kísérlet létre...**  cím az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-214">You can name your experiment by selecting, and modifying, the **Experiment created on ...** title at the top of the page.</span></span> <span data-ttu-id="7cf1d-215">Például megváltoztathatja úgy, hogy **hitelesítésszolgáltató tejtermék Analysis**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-215">For example, changing it to **CA Dairy Analysis**.</span></span>
* <span data-ttu-id="7cf1d-216">Bontsa ki a kísérlet lap bal oldalán **mentett adatkészletek**, majd **saját adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-216">On the left of the experiment page, expand **Saved Datasets**, and then **My Datasets**.</span></span> <span data-ttu-id="7cf1d-217">Megjelenik a **cadairydata.csv** korábban feltöltött.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-217">You should see the **cadairydata.csv** that you uploaded earlier.</span></span>
* <span data-ttu-id="7cf1d-218">Húzza a **csdairydata.csv dataset** alakzatot a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-218">Drag and drop the **csdairydata.csv dataset** onto the experiment.</span></span>
* <span data-ttu-id="7cf1d-219">Az a **keresési kísérletezhet elemek** mező a bal oldali ablaktáblán, a típus tetején [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="7cf1d-219">In the **Search experiment items** box on the top of the left pane, type [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="7cf1d-220">A modul jelennek meg a keresési listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-220">You will see the module appear in the search list.</span></span>
* <span data-ttu-id="7cf1d-221">Húzza a [R-parancsfájl végrehajtása] [ execute-r-script] modul alakzatot a sorát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-221">Drag and drop the [Execute R Script][execute-r-script] module onto your pallet.</span></span>  
* <span data-ttu-id="7cf1d-222">Csatlakoztassa a kimenetét a **csdairydata.csv dataset** a bal oldali bemeneti (**Dataset1**), a [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="7cf1d-222">Connect the output of the **csdairydata.csv dataset** to the leftmost input (**Dataset1**) of the [Execute R Script][execute-r-script].</span></span>
* <span data-ttu-id="7cf1d-223">**Ne feledje kattintson a "Mentés"!**</span><span class="sxs-lookup"><span data-stu-id="7cf1d-223">**Don't forget to click on 'Save'!**</span></span>  

<span data-ttu-id="7cf1d-224">Ezen a ponton a kísérletben hasonlóan kell kinéznie 3. ábra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-224">At this point your experiment should look something like Figure 3.</span></span>

![A hitelesítésszolgáltató tejtermék elemzés kísérletezhet a DataSet adatkészlet és R-parancsfájl végrehajtása modul][3]

<span data-ttu-id="7cf1d-226">*3. ábra. A hitelesítésszolgáltató tejtermék elemzés a DataSet adatkészlet és R-parancsfájl végrehajtása modul kísérletezhet.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-226">*Figure 3. The CA Dairy Analysis experiment with dataset and Execute R Script module.*</span></span>

#### <a name="check-on-the-data"></a><span data-ttu-id="7cf1d-227">Az adatok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7cf1d-227">Check on the data</span></span>
<span data-ttu-id="7cf1d-228">Most rá egy pillantást az adatok azt töltött be a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-228">Let's have a look at the data we have loaded into our experiment.</span></span> <span data-ttu-id="7cf1d-229">A kísérletben, kattintson a kimenetét a **cadairydata.csv dataset** válassza **megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-229">In the experiment, click on the output of the **cadairydata.csv dataset** and select **visualize**.</span></span> <span data-ttu-id="7cf1d-230">4. ábra hasonlót meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-230">You should see something like Figure 4.</span></span>  

![A cadairydata.csv dataset összegzése][4]

<span data-ttu-id="7cf1d-232">*4. ábra. A cadairydata.csv adatkészlet összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-232">*Figure 4. Summary of the cadairydata.csv dataset.*</span></span>

<span data-ttu-id="7cf1d-233">Ebben a nézetben látható nagy mennyiségű hasznos információkat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-233">In this view we see a lot of useful information.</span></span> <span data-ttu-id="7cf1d-234">Láthatja, hogy a dataset több első sorát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-234">We can see the first several rows of that dataset.</span></span> <span data-ttu-id="7cf1d-235">Ha olyan oszlopot válasszon ki, hogy az a statisztika szakasz az oszlop további információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-235">If we select a column, the Statistics section shows more information about the column.</span></span> <span data-ttu-id="7cf1d-236">Például a szolgáltatástípus sor látható az Azure Machine Learning Studio rendelt az oszlop milyen adatokat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-236">For example, the Feature Type row shows us what data types Azure Machine Learning Studio assigned to the column.</span></span> <span data-ttu-id="7cf1d-237">Ehhez hasonló gyorsan át, akkor az a jó megerősítést ellenőrzés előtt először bármely súlyos munkájuk elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-237">Having a quick look like this is a good sanity check before we start to do any serious work.</span></span>

### <a name="first-r-script"></a><span data-ttu-id="7cf1d-238">Első R-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="7cf1d-238">First R script</span></span>
<span data-ttu-id="7cf1d-239">Hozzon létre egy egyszerű első R-parancsfájl az Azure Machine Learning Studióban kísérletezhet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-239">Let's create a simple first R script to experiment with in Azure Machine Learning Studio.</span></span> <span data-ttu-id="7cf1d-240">Létrehozott és a következő parancsfájl tesztelve az Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-240">I have created and tested the following script in RStudio.</span></span>  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="7cf1d-241">Most át kell adnom a parancsfájl az Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-241">Now I need to transfer this script to Azure Machine Learning Studio.</span></span> <span data-ttu-id="7cf1d-242">Sikerült egyszerűen Kivágás, majd illessze be.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-242">I could simply cut and paste.</span></span> <span data-ttu-id="7cf1d-243">Azonban ebben az esetben fogja átvinni az R-parancsfájl egy zip-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-243">However, in this case, I will transfer my R script via a zip file.</span></span>

### <a name="data-input-to-the-execute-r-script-module"></a><span data-ttu-id="7cf1d-244">Adatbevitel az R-parancsfájl végrehajtása modulhoz</span><span class="sxs-lookup"><span data-stu-id="7cf1d-244">Data input to the Execute R Script module</span></span>
<span data-ttu-id="7cf1d-245">Most rendelkezik a bemeneti adatok számára egy pillantást a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-245">Let's have a look at the inputs to the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-246">Ebben a példában a kaliforniai tejelő adatok olvasását azt a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-246">In this example we will read the California dairy data into the [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="7cf1d-247">Három lehetséges bemeneteit van a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-247">There are three possible inputs for the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-248">Bármely egyikét vagy mindegyikét a bemenetek, attól függően, hogy az alkalmazás használhat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-248">You may use any one or all of these inputs, depending on your application.</span></span> <span data-ttu-id="7cf1d-249">Egyúttal tökéletesen nagy valószínűséggel nem bemenetből fogad adatokat minden R-parancsfájl használata.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-249">It is also perfectly reasonable to use an R script that takes no input at all.</span></span>  

<span data-ttu-id="7cf1d-250">Vizsgáljuk meg mindegyik e ráfordítások fog balról jobbra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-250">Let's look at each of these inputs, going from left to right.</span></span> <span data-ttu-id="7cf1d-251">A bemenetek nevei helyezi el a kurzor fölé a bemeneti és a hozzájuk tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-251">You can see the names of each of the inputs by placing your cursor over the input and reading the tooltip.</span></span>  

#### <a name="script-bundle"></a><span data-ttu-id="7cf1d-252">Parancsfájl-csomag</span><span class="sxs-lookup"><span data-stu-id="7cf1d-252">Script Bundle</span></span>
<span data-ttu-id="7cf1d-253">A parancsfájl csomagot adjon meg lehetővé teszi a azokat egy zip-fájl tartalmát adja át [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-253">The Script Bundle input allows you to pass the contents of a zip file into [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-254">A zip-fájl tartalmának olvassa be az R-kód a következő parancsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-254">You can use one of the following commands to read the contents of the zip file into your R code.</span></span>

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> <span data-ttu-id="7cf1d-255">Az Azure Machine Learning kezeli a zip fájlokat, ha azok src / könyvtár, ezért szüksége lesz a fájlneveket, a könyvtárnév előtagja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-255">Azure Machine Learning treats files in the zip as if they are in the src/ directory, so you need to prefix your file names with this directory name.</span></span> <span data-ttu-id="7cf1d-256">Például, ha a zip-fájlokat tartalmazza `yourfile.R` és `yourData.rdata` a zip-gyökérben, meg kellene cím ezek `src/yourfile.R` és `src/yourData.rdata` használatakor `source` és `load`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-256">For example, if the zip contains the files `yourfile.R` and `yourData.rdata` in the root of the zip, you would address these as `src/yourfile.R` and `src/yourData.rdata` when using `source` and `load`.</span></span>
> 
> 

<span data-ttu-id="7cf1d-257">Ismertettük már adathalmaz betöltése [betöltése a dataset](#loading).</span><span class="sxs-lookup"><span data-stu-id="7cf1d-257">We already discussed loading datasets in [Loading the dataset](#loading).</span></span> <span data-ttu-id="7cf1d-258">Miután létrehozta és tesztelte az R-parancsfájl az előző szakaszban látható, a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-258">Once you have created and tested the R script shown in the previous section, do the following:</span></span>

1. <span data-ttu-id="7cf1d-259">Mentse az R-parancsfájl egy. R-fájl.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-259">Save the R script into a .R file.</span></span> <span data-ttu-id="7cf1d-260">A parancsfájl "simpleplot jelentkezem. R".</span><span class="sxs-lookup"><span data-stu-id="7cf1d-260">I call my script file "simpleplot.R".</span></span> <span data-ttu-id="7cf1d-261">Ez a tartalom.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-261">Here's the contents.</span></span>
   
        ## Only one of the following two lines should be used
        ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
        ## If in RStudio, use the second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## The following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. <span data-ttu-id="7cf1d-262">Hozzon létre egy zip fájlt, és másolja a parancsfájlt a zip-fájlba.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-262">Create a zip file and copy your script into this zip file.</span></span> <span data-ttu-id="7cf1d-263">A Windows, kattintson a jobb gombbal a fájlra és kiválasztása **küldése**, majd **tömörített mappa**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-263">On Windows, you can right-click on the file and select **Send to**, and then **Compressed folder**.</span></span> <span data-ttu-id="7cf1d-264">Ezzel létrehoz egy új zip-fájl, amely tartalmazza a "simpleplot. R"fájl.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-264">This will create a new zip file containing the "simpleplot.R" file.</span></span>
3. <span data-ttu-id="7cf1d-265">Adja hozzá a fájlt, hogy a **adatkészletek** a Machine Learning Studióban, mint típusának megadásával **zip**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-265">Add your file to the **datasets** in Machine Learning Studio, specifying the type as **zip**.</span></span> <span data-ttu-id="7cf1d-266">Most látnia kell a zip-fájl a adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-266">You should now see the zip file in your datasets.</span></span>
4. <span data-ttu-id="7cf1d-267">A zip-fájl a húzással **adatkészletek** alakzatot a **ML Studio vászonra**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-267">Drag and drop the zip file from **datasets** onto the **ML Studio canvas**.</span></span>
5. <span data-ttu-id="7cf1d-268">Csatlakoztassa a kimenetét a **zip-adatok** ikonra a **parancsfájl köteg** a bemeneti a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-268">Connect the output of the **zip data** icon to the **Script Bundle** input of the [Execute R Script][execute-r-script] module.</span></span>
6. <span data-ttu-id="7cf1d-269">Típus a `source()` a kód ablakának zip fájlt nevű függvény a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-269">Type the `source()` function with your zip file name into the code window for the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-270">Abban az esetben, ha a beírt `source("src/simpleplot.R")`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-270">In my case I typed `source("src/simpleplot.R")`.</span></span>  
7. <span data-ttu-id="7cf1d-271">Ellenőrizze, hogy **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-271">Make sure you click **Save**.</span></span>

<span data-ttu-id="7cf1d-272">Ha ezeket a lépéseket befejeződött, a [R-parancsfájl végrehajtása] [ execute-r-script] modul végrehajtja az R-parancsfájl a zip-fájlban szereplő a kísérlet futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-272">Once these steps are complete, the [Execute R Script][execute-r-script] module will execute the R script in the zip file when the experiment is run.</span></span> <span data-ttu-id="7cf1d-273">Ezen a ponton a kísérletben hasonlóan kell kinéznie 5. ábra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-273">At this point your experiment should look something like Figure 5.</span></span>

![Kísérlet zip R-parancsfájl használatával][6]

<span data-ttu-id="7cf1d-275">*5. ábra. A kísérletben zip R-parancsfájl használatával.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-275">*Figure 5. Experiment using zipped R script.*</span></span>

#### <a name="dataset1"></a><span data-ttu-id="7cf1d-276">Dataset1</span><span class="sxs-lookup"><span data-stu-id="7cf1d-276">Dataset1</span></span>
<span data-ttu-id="7cf1d-277">Az R-kód továbbíthatja a Dataset1 bemeneti használatával adatok téglalap alakú tábla.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-277">You can pass a rectangular table of data to your R code by using the Dataset1 input.</span></span> <span data-ttu-id="7cf1d-278">Az egyszerű parancsfájlban a `maml.mapInputPort(1)` függvény olvassa be az adatok %1 porton.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-278">In our simple script the `maml.mapInputPort(1)` function reads the data from port 1.</span></span> <span data-ttu-id="7cf1d-279">Ezek az adatok majd hozzá van rendelve egy dataframe változó neve, a kódban.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-279">This data is then assigned to a dataframe variable name in your code.</span></span> <span data-ttu-id="7cf1d-280">Az egyszerű parancsprogram az első kódsort a hozzárendelés hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-280">In our simple script the first line of code performs the assignment.</span></span>

    cadairydata <- maml.mapInputPort(1)

<span data-ttu-id="7cf1d-281">Hajtsa végre a kísérletben kattintva a **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-281">Execute your experiment by clicking on the **Run** button.</span></span> <span data-ttu-id="7cf1d-282">A végrehajtás befejezése után kattintson a a [R-parancsfájl végrehajtása] [ execute-r-script] modul, és kattintson **kimeneti napló megtekintése** a Tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-282">When the execution finishes, click on the [Execute R Script][execute-r-script] module and then click **View output log** on the properties pane.</span></span> <span data-ttu-id="7cf1d-283">Egy új lapot a böngészőben a kimenetét fájl tartalmát megjelenítő meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-283">A new page should appear in your browser showing the contents of the output.log file.</span></span> <span data-ttu-id="7cf1d-284">Előfordulhat, hogy le kell megjelennie a következő hasonlót.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-284">When you scroll down you should see something like the following.</span></span>

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

<span data-ttu-id="7cf1d-285">Megtalálhatók a lap le további információk a oszlop, amely a következő hasonlót fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-285">Farther down the page is more detailed information on the columns, which will look something like the following.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

<span data-ttu-id="7cf1d-286">Ezekkel az eredményekkel többnyire 228 megfigyelés és a dataframe 9 oszlopai a várt módon vannak.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-286">These results are mostly as expected, with 228 observations and 9 columns in the dataframe.</span></span> <span data-ttu-id="7cf1d-287">Az oszlopok neveit, az R adattípus és egy minta oszlopok láthatja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-287">We can see the column names, the R data type and a sample of each column.</span></span>

> [!NOTE]
> <span data-ttu-id="7cf1d-288">Az azonos nyomtatásra kényelmesen érhető el R eszköz kimenetében a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-288">This same printed output is conveniently available from the R Device output of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-289">A kimenetének ismertetik a [R-parancsfájl végrehajtása] [ execute-r-script] modul a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-289">We will discuss the outputs of the [Execute R Script][execute-r-script] module in the next section.</span></span>  
> 
> 

#### <a name="dataset2"></a><span data-ttu-id="7cf1d-290">Dataset2</span><span class="sxs-lookup"><span data-stu-id="7cf1d-290">Dataset2</span></span>
<span data-ttu-id="7cf1d-291">A Dataset2 bemeneti működése megegyezik-e Dataset1.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-291">The behavior of the Dataset2 input is identical to that of Dataset1.</span></span> <span data-ttu-id="7cf1d-292">Használja a bemeneti átviheti az adatok egy második téglalap alakú tábla az R-kódba.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-292">Using this input you can pass a second rectangular table of data into your R code.</span></span> <span data-ttu-id="7cf1d-293">A függvény `maml.mapInputPort(2)`, az argumentumnak 2 használatával továbbítja ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-293">The function `maml.mapInputPort(2)`, with the argument 2, is used to pass this data.</span></span>  

### <a name="execute-r-script-outputs"></a><span data-ttu-id="7cf1d-294">R-parancsfájl kimenetek végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-294">Execute R Script outputs</span></span>
#### <a name="output-a-dataframe"></a><span data-ttu-id="7cf1d-295">Egy dataframe kimeneti</span><span class="sxs-lookup"><span data-stu-id="7cf1d-295">Output a dataframe</span></span>
<span data-ttu-id="7cf1d-296">A kimenetre küldheti az R dataframe tartalmát és az eredmény Dataset1 porton keresztül téglalap alakú táblázatként használatával a `maml.mapOutputPort()` függvény.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-296">You can output the contents of an R dataframe as a rectangular table through the Result Dataset1 port by using the `maml.mapOutputPort()` function.</span></span> <span data-ttu-id="7cf1d-297">Az egyszerű R-parancsfájl ez hajtható végre a következő sort.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-297">In our simple R script this is performed by the following line.</span></span>

    maml.mapOutputPort('cadairydata')

<span data-ttu-id="7cf1d-298">Miután a kísérletet, kattintson a az eredmény Dataset1 kimeneti portra, és kattintson a **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-298">After running the experiment, click on the Result Dataset1 output port and then click on **Visualize**.</span></span> <span data-ttu-id="7cf1d-299">6. ábra hasonlót meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-299">You should see something like Figure 6.</span></span>

![A képi megjelenítés a kimeneti kaliforniai tejelő adatok][7]

<span data-ttu-id="7cf1d-301">*6. ábra. A képi megjelenítés a kimeneti kaliforniai tejelő adatok.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-301">*Figure 6. The visualization of the output of the California dairy data.*</span></span>

<span data-ttu-id="7cf1d-302">A kimeneti keres a bemeneti azonos, pontosan úgy várt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-302">This output looks identical to the input, exactly as we expected.</span></span>  

### <a name="r-device-output"></a><span data-ttu-id="7cf1d-303">R eszköz kimeneti</span><span class="sxs-lookup"><span data-stu-id="7cf1d-303">R Device output</span></span>
<span data-ttu-id="7cf1d-304">Az eszköz kimenetét a [R-parancsfájl végrehajtása] [ execute-r-script] modul üzenetek és grafikus kimeneti tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-304">The Device output of the [Execute R Script][execute-r-script] module contains messages and graphics output.</span></span> <span data-ttu-id="7cf1d-305">Mindkét standard kimenet és a standard hiba R üzenetei R eszköz kimeneti portjával.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-305">Both standard output and standard error messages from R are sent to the R Device output port.</span></span>  

<span data-ttu-id="7cf1d-306">Az R eszköz kimeneti megtekintéséhez kattintson a porton majd a **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-306">To view the R Device output, click on the port and then on **Visualize**.</span></span> <span data-ttu-id="7cf1d-307">A standard kimenet és a standard hiba a 7. ábra az R-parancsfájl látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-307">We see the standard output and standard error from the R script in Figure 7.</span></span>

![Standard kimenet és a standard hiba R eszköz port][8]

<span data-ttu-id="7cf1d-309">*7. ábra. Standard kimenet és a standard hiba R eszköz port.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-309">*Figure 7. Standard output and standard error from the R Device port.*</span></span>

<span data-ttu-id="7cf1d-310">Azt lefelé görgetéshez használható tekintse meg a 8. ábrán az R-parancsfájl kimenete grafikus.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-310">Scrolling down we see the graphics output from our R script in Figure 8.</span></span>  

![Az R eszköz port grafikus kimenete][9]

<span data-ttu-id="7cf1d-312">*8. ábra. A grafikus az R eszköz port kimenetét.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-312">*Figure 8. Graphics output from the R Device port.*</span></span>  

## <span data-ttu-id="7cf1d-313"><a id="filtering"></a>Adatok szűrése és átalakítás</span><span class="sxs-lookup"><span data-stu-id="7cf1d-313"><a id="filtering"></a>Data filtering and transformation</span></span>
<span data-ttu-id="7cf1d-314">Ebben a szakaszban végezzük el a néhány alapvető adatok szűrése és átalakítási műveletek a kaliforniai tejelő adatokon.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-314">In this section we will perform some basic data filtering and transformation operations on the California dairy data.</span></span> <span data-ttu-id="7cf1d-315">Ez a szakasz végén azt fogja tudni adatok formátuma nem megfelelő az elemzési modell készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-315">By the end of this section we will have data in a format suitable for building an analytic model.</span></span>  

<span data-ttu-id="7cf1d-316">Pontosabban, ez a szakasz azt feladatokat kell elvégezni több közös adatok tisztítása és átalakítás: Adja meg a szűrést dataframes, hozzáadása új számított oszlop, az átalakítási és átalakítások értéket.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-316">More specifically, in this section we will perform several common data cleaning and transformation tasks: type transformation, filtering on dataframes, adding new computed columns, and value transformations.</span></span> <span data-ttu-id="7cf1d-317">A háttérben segítséget valós problémákat észlelt a számos változata foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-317">This background should help you deal with the many variations encountered in real-world problems.</span></span>

<span data-ttu-id="7cf1d-318">Az ebben a szakaszban a teljes R-kód nem áll rendelkezésre a korábban letöltött zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-318">The complete R code for this section is available in the zip file you downloaded earlier.</span></span>

### <a name="type-transformations"></a><span data-ttu-id="7cf1d-319">Típus átalakítások</span><span class="sxs-lookup"><span data-stu-id="7cf1d-319">Type transformations</span></span>
<span data-ttu-id="7cf1d-320">Most, hogy azt a kaliforniai tejelő adatok olvashatja be az R-kód a [R-parancsfájl végrehajtása] [ execute-r-script] modul, igazolnia kell, hogy az oszlopok adatainak legyen-e a kívánt típusát és formátumát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-320">Now that we can read the California dairy data into the R code in the [Execute R Script][execute-r-script] module, we need to ensure that the data in the columns has the intended type and format.</span></span>  

<span data-ttu-id="7cf1d-321">R az egy dinamikusan gépelt nyelv, ami azt jelenti, hogy adattípusok kényszerített vannak-e egy másik szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-321">R is a dynamically typed language, which means that data types are coerced from one to another as required.</span></span> <span data-ttu-id="7cf1d-322">Az R atomi adattípusait numerikus, logikai és karakter közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-322">The atomic data types in R include numeric, logical and character.</span></span> <span data-ttu-id="7cf1d-323">A tényező típusú compactly kategorikus adatok tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-323">The factor type is used to compactly store categorical data.</span></span> <span data-ttu-id="7cf1d-324">Adattípusok sokkal további információk találhatók a hivatkozások [B függelék – további információ](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="7cf1d-324">You can find much more information on data types in the references in [Appendix B - Further reading](#appendixb).</span></span>

<span data-ttu-id="7cf1d-325">A táblázatos adatolvasás a külső forrásból származó R, esetén mindig egy célszerű ellenőrizni az eredményül kapott típusok az oszlopok.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-325">When tabular data is read into R from an external source, it is always a good idea to check the resulting types in the columns.</span></span> <span data-ttu-id="7cf1d-326">Érdemes lehet egy oszlophoz Típusjelző karakter, de sok esetben ez fog megjelenni tényezőként vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-326">You may want a column of type character, but in many cases this will show up as factor or vice versa.</span></span> <span data-ttu-id="7cf1d-327">Más esetekben egy oszlopot úgy gondolja, hogy legyen numerikus által képviselt karakter adatok, például az 1,23, lebegőpontos helyett a "1,23" pont számát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-327">In other cases a column you think should be numeric is represented by character data, e.g. '1.23' rather than 1.23 as a floating point number.</span></span>  

<span data-ttu-id="7cf1d-328">Szerencsére is könnyen egy típuskonverzió a másikra, mindaddig, amíg lesz lehetséges.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-328">Fortunately, it is easy to convert one type to another, as long as mapping is possible.</span></span> <span data-ttu-id="7cf1d-329">Például "Nevada" konvertálása nem egy numerikus érték, de akkor átalakíthatja egy tényező (kategorikus változó).</span><span class="sxs-lookup"><span data-stu-id="7cf1d-329">For example, you cannot convert 'Nevada' into a numeric value, but you can convert it to a factor (categorical variable).</span></span> <span data-ttu-id="7cf1d-330">Másik példaként egy numerikus 1 átalakíthatja a következő karaktert: "1" vagy egy tényező.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-330">As another example, you can convert a numeric 1 into a character '1' or a factor.</span></span>  

<span data-ttu-id="7cf1d-331">Ezek az átalakítások bármelyikét szintaxisa a következő egyszerű: `as.datatype()`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-331">The syntax for any of these conversions is simple: `as.datatype()`.</span></span> <span data-ttu-id="7cf1d-332">A típuskonverziós a következők:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-332">These type conversion functions include the following.</span></span>

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

<span data-ttu-id="7cf1d-333">Azt az előző szakaszban bemeneti oszlopok adattípusai megnézi: az összes oszlop sem típusú numerikus, kivéve az oszlop, "Month", amelynek a típuskarakter címkével.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-333">Looking at the data types of the columns we input in the previous section: all columns are of type numeric, except for the column labeled 'Month', which is of type character.</span></span> <span data-ttu-id="7cf1d-334">Most konvertálja az tényező, és a teszteredmények.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-334">Let's convert this to a factor and test the results.</span></span>  

<span data-ttu-id="7cf1d-335">Rendelkezik a sort, amelyben a scatterplot mátrix létrehozza és hozzáadja a "Month" oszlop átalakítása tényezőként sor törölve</span><span class="sxs-lookup"><span data-stu-id="7cf1d-335">I have deleted the line that created the scatterplot matrix and added a line converting the 'Month' column to a factor.</span></span> <span data-ttu-id="7cf1d-336">A kísérletemben I fog csak kivágása és R illessze be a kódját ablakot, a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-336">In my experiment I will just cut and paste the R code into the code window of the [Execute R Script][execute-r-script] Module.</span></span> <span data-ttu-id="7cf1d-337">Is nem sikerült frissíteni a zip-fájl, és töltse fel az Azure Machine Learning Studio, de ez számos lépést időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-337">You could also update the zip file and upload it to Azure Machine Learning Studio, but this takes several steps.</span></span>  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check the result
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="7cf1d-338">Most futtassa ezt a kódot, és tekintse meg a kimeneti naplót, az R-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-338">Let's execute this code and look at the output log for the R script.</span></span> <span data-ttu-id="7cf1d-339">9. ábra jelenik meg a vonatkozó adatokat a naplóból.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-339">The relevant data from the log is shown in Figure 9.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="7cf1d-340">*9. ábra. A dataframe tényező változóhoz összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-340">*Figure 9. Summary of the dataframe with a factor variable.*</span></span>

<span data-ttu-id="7cf1d-341">A hónap típusú most üzenetnek kell megjelennie "**keresztüli 14 szintek rendelkező tényező**".</span><span class="sxs-lookup"><span data-stu-id="7cf1d-341">The type for Month should now say '**Factor w/ 14 levels**'.</span></span> <span data-ttu-id="7cf1d-342">Ez az probléma, mivel az év csak 12 hónapon keresztül.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-342">This is a problem since there are only 12 months in the year.</span></span> <span data-ttu-id="7cf1d-343">Is ellenőrizheti, hogy a típus **Visualize** eredmény DataSet port van "**Categorical**".</span><span class="sxs-lookup"><span data-stu-id="7cf1d-343">You can also check to see that the type in **Visualize** of the Result Dataset port is '**Categorical**'.</span></span>

<span data-ttu-id="7cf1d-344">A probléma oka, hogy a "Month" oszlop nem lett a kódolt rendszeresen.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-344">The problem is that the 'Month' column has not been coded systematically.</span></span> <span data-ttu-id="7cf1d-345">Bizonyos esetekben egy hónap. április nevezik, és más, ápr rövidítéseket tartalmaz. A karakterlánc 3 karakter segítségével azt is megoldhatja a problémát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-345">In some cases a month is called April and in others it is abbreviated as Apr. We can solve this problem by trimming the string to 3 characters.</span></span> <span data-ttu-id="7cf1d-346">A kódsort most a következőképpen néznek:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-346">The line of code now looks like the following:</span></span>

    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

<span data-ttu-id="7cf1d-347">Futtassa újra a kísérletet, és tekintse meg a kimeneti naplót.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-347">Rerun the experiment and view the output log.</span></span> <span data-ttu-id="7cf1d-348">A kívánt eredmény elérése érdekében a 10. ábrán láthatók.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-348">The expected results are shown in Figure 10.</span></span>  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="7cf1d-349">*10. ábra. Megfelelő számú tényező szintek dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-349">*Figure 10. Summary of the dataframe with correct number of factor levels.*</span></span>

<span data-ttu-id="7cf1d-350">A tényező változó most már a kívánt 12 szinteket.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-350">Our factor variable now has the desired 12 levels.</span></span>

### <a name="basic-data-frame-filtering"></a><span data-ttu-id="7cf1d-351">Alapvető adatok keret szűrése</span><span class="sxs-lookup"><span data-stu-id="7cf1d-351">Basic data frame filtering</span></span>
<span data-ttu-id="7cf1d-352">R dataframes hatékony szűrési lehetőségeket nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-352">R dataframes support powerful filtering capabilities.</span></span> <span data-ttu-id="7cf1d-353">Adatkészletek sorok vagy oszlopok logikai szűrők használatával lehet részlegesen beágyazott.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-353">Datasets can be subsetted by using logical filters on either rows or columns.</span></span> <span data-ttu-id="7cf1d-354">Sok esetben összetett szűrési feltételeket lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-354">In many cases, complex filter criteria will be required.</span></span> <span data-ttu-id="7cf1d-355">A hivatkozások [B függelék – további információ](#appendixb) dataframes szűrés kiterjedt példái tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-355">The references in [Appendix B - Further reading](#appendixb) contain extensive examples of filtering dataframes.</span></span>  

<span data-ttu-id="7cf1d-356">Van egy bittel szűrése az adathalmaz azt kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-356">There is one bit of filtering we should do on our dataset.</span></span> <span data-ttu-id="7cf1d-357">Ha az oszlop szerepel a cadairydata dataframe tekinti meg, látni fogja a két felesleges oszlopok.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-357">If you look at the columns in the cadairydata dataframe, you will see two unnecessary columns.</span></span> <span data-ttu-id="7cf1d-358">Az első oszlop csak egy sor számát, amely nem nagyon hasznos tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-358">The first column just holds a row number, which is not very useful.</span></span> <span data-ttu-id="7cf1d-359">A második oszlopban Year.Month, redundáns információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-359">The second column, Year.Month, contains redundant information.</span></span> <span data-ttu-id="7cf1d-360">Azt is könnyen használatával zárhatja ki ezeket az oszlopokat a következő R-kód.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-360">We can easily exclude these columns by using the following R code.</span></span>

> [!NOTE]
> <span data-ttu-id="7cf1d-361">Mostantól ebben a szakaszban I csak láthatja a hozzáadott a kód a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-361">From now on in this section, I will just show you the additional code I am adding in the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="7cf1d-362">Minden új sor veszek fel **előtt** a `str()` függvény.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-362">I will add each new line **before** the `str()` function.</span></span> <span data-ttu-id="7cf1d-363">Ez a függvény az eredményeket az Azure Machine Learning Studióban használni.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-363">I use this function to verify my results in Azure Machine Learning Studio.</span></span>
> 
> 

<span data-ttu-id="7cf1d-364">Az R kód adhatok hozzá a következő sort a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-364">I add the following line to my R code in the [Execute R Script][execute-r-script] module.</span></span>

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

<span data-ttu-id="7cf1d-365">Futtassa a következő kódot a kísérletben, és ellenőrizze a kimeneti naplót kapott eredmény.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-365">Run this code in your experiment and check the result from the output log.</span></span> <span data-ttu-id="7cf1d-366">Ezekkel az eredményekkel 11. ábra mutatja be.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-366">These results are shown in Figure 11.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="7cf1d-367">*11. ábra. A két oszlop eltávolítása dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-367">*Figure 11. Summary of the dataframe with two columns removed.*</span></span>

<span data-ttu-id="7cf1d-368">Jó hírünk van!</span><span class="sxs-lookup"><span data-stu-id="7cf1d-368">Good news!</span></span> <span data-ttu-id="7cf1d-369">A várt eredményt azt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-369">We get the expected results.</span></span>

### <a name="add-a-new-column"></a><span data-ttu-id="7cf1d-370">Egy olyan új oszlop hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-370">Add a new column</span></span>
<span data-ttu-id="7cf1d-371">Idő adatsorozat modellek létrehozásához lesz idősorozatban elindítása óta a hónap tartalmazó oszlopa lehet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-371">To create time series models it will be convenient to have a column containing the months since the start of the time series.</span></span> <span data-ttu-id="7cf1d-372">Létrehozunk egy olyan új oszlop "Month.Count".</span><span class="sxs-lookup"><span data-stu-id="7cf1d-372">We will create a new column 'Month.Count'.</span></span>

<span data-ttu-id="7cf1d-373">A kód létre fogunk hozni az első egyszerű függvény rendszerezésének elősegítése érdekében `num.month()`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-373">To help organize the code we will create our first simple function, `num.month()`.</span></span> <span data-ttu-id="7cf1d-374">Ez a függvény egy olyan új oszlop létrehozása a dataframe érvényes azt lesz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-374">We will then apply this function to create a new column in the dataframe.</span></span> <span data-ttu-id="7cf1d-375">Az új kódot a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-375">The new code is as follows.</span></span>

    ## Create a new column with the month count
    ## Function to find the number of months from the first
    ## month of the time series
    num.month <- function(Year, Month) {
      ## Find the starting year
      min.year  <- min(Year)

      ## Compute the number of months from the start of the time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute the new column for the dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

<span data-ttu-id="7cf1d-376">Most futtassa a frissített kísérletet, és a kimeneti naplót használja az eredmények megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-376">Now run the updated experiment and use the output log to view the results.</span></span> <span data-ttu-id="7cf1d-377">Ezekkel az eredményekkel 12. ábra mutatja be.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-377">These results are shown in Figure 12.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="7cf1d-378">*12. ábra. További oszlopok dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-378">*Figure 12. Summary of the dataframe with the additional column.*</span></span>

<span data-ttu-id="7cf1d-379">Úgy tűnik, minden működik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-379">It looks like everything is working.</span></span> <span data-ttu-id="7cf1d-380">A saját dataframe van a várt értékeket az új oszlopot.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-380">We have the new column with the expected values in our dataframe.</span></span>

### <a name="value-transformations"></a><span data-ttu-id="7cf1d-381">Érték átalakítása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-381">Value transformations</span></span>
<span data-ttu-id="7cf1d-382">Ebben a szakaszban végezzük el néhány egyszerű átalakítások értékeinek néhány a dataframe oszlopot.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-382">In this section we will perform some simple transformations on the values in some of the columns of our dataframe.</span></span> <span data-ttu-id="7cf1d-383">Az R nyelv támogatja az átalakítások majdnem tetszőleges érték.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-383">The R language supports nearly arbitrary value transformations.</span></span> <span data-ttu-id="7cf1d-384">A hivatkozások [B függelék – további olvasási](#appendixb) kiterjedt példákat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-384">The references in [Appendix B - Further Reading](#appendixb) contain extensive examples.</span></span>

<span data-ttu-id="7cf1d-385">Ha megnézi a dataframe összegzéseinek szereplő értékeket kell valami páratlan itt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-385">If you look at the values in the summaries of our dataframe you should see something odd here.</span></span> <span data-ttu-id="7cf1d-386">Kaliforniai előállított tej-nál több jégkrémje?</span><span class="sxs-lookup"><span data-stu-id="7cf1d-386">Is more ice cream than milk produced in California?</span></span> <span data-ttu-id="7cf1d-387">Nem, természetesen ez nincs értelme, mivel EV, mint a tény lehet, hogy nem egyes velünk jégkrémje lovers.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-387">No, of course not, as this makes no sense, sad as this fact may be to some of us ice cream lovers.</span></span> <span data-ttu-id="7cf1d-388">Az egységek eltérnek.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-388">The units are different.</span></span> <span data-ttu-id="7cf1d-389">Az ár egységekbe, az font, tej van egységekbe, az 1 millió egység USA font, jégkrémje 1000 egységekbe gallon VELÜNK, pedig túró egységekbe, az USA 1000 font.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-389">The price is in units of US pounds, milk is in units of 1 M US pounds, ice cream is in units of 1,000 US gallons, and cottage cheese is in units of 1,000 US pounds.</span></span> <span data-ttu-id="7cf1d-390">Ha jégkrémje tömege / gallonra készül 6.5 font, tehetünk könnyen ennek a szorzás ezeket az értékeket átalakítani, így azok minden 1000 font egyenlő egységekben.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-390">Assuming ice cream weighs about 6.5 pounds per gallon, we can easily do the multiplication to convert these values so they are all in equal units of 1,000 pounds.</span></span>

<span data-ttu-id="7cf1d-391">Az előrejelzési modell azt használjon tényezőt modellt a trendek és a határozza ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-391">For our forecasting model we use a multiplicative model for trend and seasonal adjustment of this data.</span></span> <span data-ttu-id="7cf1d-392">Napló-átalakítás lehetővé teszi egy lineáris modell, hogy az a folyamat használja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-392">A log transformation allows us to use a linear model, simplifying this process.</span></span> <span data-ttu-id="7cf1d-393">A napló átalakítása ugyanezt a funkciót, ha a többszöröző alkalmazzák a alkalmazhatja azt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-393">We can apply the log transformation in the same function where the multiplier is applied.</span></span>

<span data-ttu-id="7cf1d-394">A következő kódot a I meg egy új funkció `log.transform()`, és alkalmazza azt az numerikus értékeket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-394">In the following code, I define a new function, `log.transform()`, and apply it to the rows containing the numerical values.</span></span> <span data-ttu-id="7cf1d-395">Az R `Map()` függvény alkalmazására szolgál a `log.transform()` működnek, mint a kijelölt oszlopokat a dataframe a.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-395">The R `Map()` function is used to apply the `log.transform()` function to the selected columns of the dataframe.</span></span> <span data-ttu-id="7cf1d-396">`Map()`hasonló `apply()` , de lehetővé teszi, hogy a függvény argumentumainak egynél több listáját.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-396">`Map()` is similar to `apply()` but allows for more than one list of arguments to the function.</span></span> <span data-ttu-id="7cf1d-397">Vegye figyelembe, hogy tényezők listájának biztosított a második argumentumának a `log.transform()` függvény.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-397">Note that a list of multipliers supplies the second argument to the `log.transform()` function.</span></span> <span data-ttu-id="7cf1d-398">A `na.omit()` függvény győződjön meg arról, nem találtunk hiányzik vagy nincs megadva érték a dataframe a karbantartási bit típust használja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-398">The `na.omit()` function is used as a bit of cleanup to ensure we do not have missing or undefined values in the dataframe.</span></span>

    log.transform <- function(invec, multiplier = 1) {
      ## Function for the transformation, which is the log
      ## of the input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments to function log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier to funcition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check the input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap the multiplication in tryCatch
      ## If there is an exception, print the warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply the transformation function to the 4 columns
    ## of the dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

<span data-ttu-id="7cf1d-399">Nincs elég egy bit azonban a a `log.transform()` függvény.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-399">There is quite a bit happening in the `log.transform()` function.</span></span> <span data-ttu-id="7cf1d-400">Ez a kód a legtöbb ellenőrzi az argumentumok potenciális problémákat vagy kivételek, továbbra is a számítások során felmerülő foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-400">Most of this code is checking for potential problems with the arguments or dealing with exceptions, which can still arise during the computations.</span></span> <span data-ttu-id="7cf1d-401">Ez a kód csak néhány sornyi ténylegesen tegye a számításokat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-401">Only a few lines of this code actually do the computations.</span></span>

<span data-ttu-id="7cf1d-402">A defenzív programozás célja egy funkcióval, amely megakadályozza a folyamatos feldolgozási hibája megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-402">The goal of the defensive programming is to prevent the failure of a single function that prevents processing from continuing.</span></span> <span data-ttu-id="7cf1d-403">Lehet, hogy a hosszan futó elemzési hirtelen hibát igen kellemetlen, a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-403">An abrupt failure of a long-running analysis can be quite frustrating for users.</span></span> <span data-ttu-id="7cf1d-404">Ez a helyzet elkerüléséhez alapértelmezett visszatérési értékek ki kell választani, amely korlátozza az alárendelt feldolgozási való megosztása kárt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-404">To avoid this situation, default return values must be chosen that will limit damage to downstream processing.</span></span> <span data-ttu-id="7cf1d-405">Egy üzenet is elő, hogy valami állapotba került rossz riasztási felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-405">A message is also produced to alert users that something has gone wrong.</span></span>

<span data-ttu-id="7cf1d-406">Ha nincsenek használatban lévő R defenzív programozás, ez a kód egy kicsit túlságosan tűnhet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-406">If you are not used to defensive programming in R, all this code may seem a bit overwhelming.</span></span> <span data-ttu-id="7cf1d-407">I végigvezeti a fő lépést:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-407">I will walk you through the major steps:</span></span>

1. <span data-ttu-id="7cf1d-408">Négy üzenetek vektor van definiálva.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-408">A vector of four messages is defined.</span></span> <span data-ttu-id="7cf1d-409">Ezek az üzenetek segítségével kommunikálnak a lehetséges hibákat és kivételeket, amelyek akkor fordulhat elő, ezzel a kóddal kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-409">These messages are used to communicate information about some of the possible errors and exceptions that can occur with this code.</span></span>
2. <span data-ttu-id="7cf1d-410">NA érték minden egyes esetben vissza</span><span class="sxs-lookup"><span data-stu-id="7cf1d-410">I return a value of NA for each case.</span></span> <span data-ttu-id="7cf1d-411">Nincsenek sok egyéb lehetőségeket, amelyekkel kevesebb hatásai lehetnek.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-411">There are many other possibilities that might have fewer side effects.</span></span> <span data-ttu-id="7cf1d-412">I visszaadhatja nullából vektor, vagy az eredeti bemeneti vektort, például.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-412">I could return a vector of zeroes, or the original input vector, for example.</span></span>
3. <span data-ttu-id="7cf1d-413">A függvény ellenőrzéseket futtatja a argumentumai.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-413">Checks are run on the arguments to the function.</span></span> <span data-ttu-id="7cf1d-414">Minden esetben a rendszer hibát észlel, ha ki egy alapértelmezett értéket adja vissza, és üzenet hozzák a `warning()` függvény.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-414">In each case, if an error is detected, a default value is returned and a message is produced by the `warning()` function.</span></span> <span data-ttu-id="7cf1d-415">Használok `warning()` helyett `stop()` , ez utóbbi megszünteti végrehajtási, pontosan milyen próbálok elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-415">I am using `warning()` rather than `stop()` as the latter will terminate execution, exactly what I am trying to avoid.</span></span> <span data-ttu-id="7cf1d-416">Vegye figyelembe, hogy I írt ezt a kódot eljárási Style, ahogy a gyorsítás esetében a rendszer valószínűleg összetett és homályos működési megközelítést.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-416">Note that I have written this code in a procedural style, as in this case a functional approach seemed complex and obscure.</span></span>
4. <span data-ttu-id="7cf1d-417">A napló számítások csomagolni vannak `tryCatch()` , hogy a kivételek nem okoz egy hirtelen halt feldolgozás.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-417">The log computations are wrapped in `tryCatch()` so that exceptions will not cause an abrupt halt to processing.</span></span> <span data-ttu-id="7cf1d-418">Nélkül `tryCatch()` R funkciók eredményt stopjelzést, viszont csak, amelyek miatt a legtöbb hiba.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-418">Without `tryCatch()` most errors raised by R functions result in a stop signal, which does just that.</span></span>

<span data-ttu-id="7cf1d-419">Az R-kód végrehajtása a kísérletben, és nyomtatott kimenete egy pillantást a a kimenetét fájlt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-419">Execute this R code in your experiment and have a look at the printed output in the output.log file.</span></span> <span data-ttu-id="7cf1d-420">Ekkor az átalakítani kívánt értékeket a négy oszlopot a naplóban is, ahogy az 13. ábra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-420">You will now see the transformed values of the four columns in the log, as shown in Figure 13.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="7cf1d-421">*13. ábra. A dataframe átalakított értékeinek összegzése.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-421">*Figure 13. Summary of the transformed values in the dataframe.*</span></span>

<span data-ttu-id="7cf1d-422">Az érték átalakítása látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-422">We see the values have been transformed.</span></span> <span data-ttu-id="7cf1d-423">Most tejtermelés nagy mértékben meghaladja minden más tejtermék éles, hogy azt most nézi a Logaritmikus skála tárolóról.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-423">Milk production now greatly exceeds all other dairy product production, recalling that we are now looking at a log scale.</span></span>

<span data-ttu-id="7cf1d-424">Ezen a ponton az adatok törlődnek, és készen áll az egyes modellezési azt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-424">At this point our data is cleaned up and we are ready for some modeling.</span></span> <span data-ttu-id="7cf1d-425">A képi megjelenítés eredménye Dataset kimenete összegzése megnézi a [R-parancsfájl végrehajtása] [ execute-r-script] modul, látni fogja a "Month" oszlop: "Categorical" 12 egyedi értékekkel újra, ugyanúgy, mint akartunk.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-425">Looking at the visualization summary for the Result Dataset output of our [Execute R Script][execute-r-script] module, you will see the 'Month' column is 'Categorical' with 12 unique values, again, just as we wanted.</span></span>

## <span data-ttu-id="7cf1d-426"><a id="timeseries"></a>Idő adatsorozat objektumok és a korrelációs elemzés</span><span class="sxs-lookup"><span data-stu-id="7cf1d-426"><a id="timeseries"></a>Time series objects and correlation analysis</span></span>
<span data-ttu-id="7cf1d-427">Ebben a szakaszban rendszer R idő néhány alapvető adatsorozat objektumok felfedezése és kapcsolatos néhány közötti összefüggések elemzése.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-427">In this section we will explore a few basic R time series objects and analyze the correlations between some of the variables.</span></span> <span data-ttu-id="7cf1d-428">A cél, hogy egy több késedelmes jelentések a páros korrelációs információkat tartalmazó dataframe kimeneti.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-428">Our goal is to output a dataframe containing the pairwise correlation information at several lags.</span></span>

<span data-ttu-id="7cf1d-429">A teljes R-kód az ebben a szakaszban korábban letöltött zip-fájlban van.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-429">The complete R code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="time-series-objects-in-r"></a><span data-ttu-id="7cf1d-430">Az R idő adatsorozat objektumok</span><span class="sxs-lookup"><span data-stu-id="7cf1d-430">Time series objects in R</span></span>
<span data-ttu-id="7cf1d-431">Már említettük, az idő a adatsorokat idő indexelik adatértékek sorozata.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-431">As already mentioned, time series are a series of data values indexed by time.</span></span> <span data-ttu-id="7cf1d-432">R idő adatsorozat objektumok létrehozása és kezelése az idő index szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-432">R time series objects are used to create and manage the time index.</span></span> <span data-ttu-id="7cf1d-433">Nincsenek idő adatsorozat objektumok használata számos előnnyel jár.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-433">There are several advantages to using time series objects.</span></span> <span data-ttu-id="7cf1d-434">Idő adatsorozat objektumok szabad a sok az adatsorozat index időértékek az objektum ágyazott kezelése adatait.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-434">Time series objects free you from the many details of managing the time series index values that are encapsulated in the object.</span></span> <span data-ttu-id="7cf1d-435">Ezenkívül idő adatsorozat objektumok lehetővé teszik az módszerekkel sok időt adatsorozat ábrázolásához, nyomtatási, modellezési stb.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-435">In addition, time series objects allow you to use the many time series methods for plotting, printing, modeling, etc.</span></span>

<span data-ttu-id="7cf1d-436">A POSIXct idő adatsorozat osztály általában arra használják, és viszonylag egyszerű.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-436">The POSIXct time series class is commonly used and is relatively simple.</span></span> <span data-ttu-id="7cf1d-437">Ez a time series intézkedések idő epoch, 1970. január 1. a kezdetektől osztály.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-437">This time series class measures time from the start of the epoch, January 1, 1970.</span></span> <span data-ttu-id="7cf1d-438">Ebben a példában használjuk POSIXct idő adatsorozat objektumok.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-438">We will use POSIXct time series objects in this example.</span></span> <span data-ttu-id="7cf1d-439">Más széles körben használt R idő adatsorozat objektum osztályok itt és xts, bővíthető idősor tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-439">Other widely used R time series object classes include zoo and xts, extensible time series.</span></span>
<!-- Additional information on R time series objects is provided in the references in Section 5.7. [commenting because this section doesn't exist, even in the original] -->

### <a name="time-series-object-example"></a><span data-ttu-id="7cf1d-440">Idő adatsorozat objektum – példa</span><span class="sxs-lookup"><span data-stu-id="7cf1d-440">Time series object example</span></span>
<span data-ttu-id="7cf1d-441">Lássunk neki a fenti példában.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-441">Let's get started with our example.</span></span> <span data-ttu-id="7cf1d-442">Húzza a **új** [R-parancsfájl végrehajtása] [ execute-r-script] modult a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-442">Drag and drop a **new** [Execute R Script][execute-r-script] module into your experiment.</span></span> <span data-ttu-id="7cf1d-443">Kapcsolódás a meglévő az eredmény Dataset1 kimeneti portjára [R-parancsfájl végrehajtása] [ execute-r-script] a Dataset1 modul bemeneti port az új [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-443">Connect the Result Dataset1 output port of the existing [Execute R Script][execute-r-script] module to the Dataset1 input port of the new [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="7cf1d-444">Ahogyan az első példák mint azt végighalad a példa, bizonyos időpontokban I csak a növekményes további sor megjelenik az R-kód egyes lépésnél.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-444">As I did for the first examples, as we progress through the example, at some points I will show only the incremental additional lines of R code at each step.</span></span>  

#### <a name="reading-the-dataframe"></a><span data-ttu-id="7cf1d-445">A dataframe olvasása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-445">Reading the dataframe</span></span>
<span data-ttu-id="7cf1d-446">Első lépésként tegyük egy dataframe olvasása, és győződjön meg arról, hogy a várt eredményt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-446">As a first step, let's read in a dataframe and make sure we get the expected results.</span></span> <span data-ttu-id="7cf1d-447">A következő kódot kell tennie a feladatot.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-447">The following code should do the job.</span></span>

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check the results

<span data-ttu-id="7cf1d-448">Most futtassa a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-448">Now, run the experiment.</span></span> <span data-ttu-id="7cf1d-449">A napló az új R-parancsfájl végrehajtása alakzat 14. ábra hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-449">The log of the new Execute R Script shape should look like Figure 14.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

<span data-ttu-id="7cf1d-450">*14. ábra. Az R-parancsfájl végrehajtása modul dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-450">*Figure 14. Summary of the dataframe in the Execute R Script module.*</span></span>

<span data-ttu-id="7cf1d-451">Ezek az adatok nem a várt típusok és formátumát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-451">This data is of the expected types and format.</span></span> <span data-ttu-id="7cf1d-452">Vegye figyelembe, hogy a "Month" oszlop típusa tényező és szintek várt száma.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-452">Note that the 'Month' column is of type factor and has the expected number of levels.</span></span>

#### <a name="creating-a-time-series-object"></a><span data-ttu-id="7cf1d-453">Idő adatsorozat objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-453">Creating a time series object</span></span>
<span data-ttu-id="7cf1d-454">Egy alkalommal adatsorozat objektum hozzáadása az dataframe kell.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-454">We need to add a time series object to our dataframe.</span></span> <span data-ttu-id="7cf1d-455">Az aktuális kód cserélje le a következő, amely egy olyan új oszlop osztály POSIXct hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-455">Replace the current code with the following, which adds a new column of class POSIXct.</span></span>

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check the results

<span data-ttu-id="7cf1d-456">Most tekintse meg a naplót.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-456">Now, check the log.</span></span> <span data-ttu-id="7cf1d-457">15. ábra kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-457">It should look like Figure 15.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

<span data-ttu-id="7cf1d-458">*15. ábra. Idő adatsorozat objektum dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-458">*Figure 15. Summary of the dataframe with a time series object.*</span></span>

<span data-ttu-id="7cf1d-459">Az összefoglalást adunk, amely az új oszlop ténylegesen osztály POSIXct láthatja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-459">We can see from the summary that the new column is in fact of class POSIXct.</span></span>

### <a name="exploring-and-transforming-the-data"></a><span data-ttu-id="7cf1d-460">Fel és az adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-460">Exploring and transforming the data</span></span>
<span data-ttu-id="7cf1d-461">Most felfedezheti a változók a DataSet adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-461">Let's explore some of the variables in this dataset.</span></span> <span data-ttu-id="7cf1d-462">Egy scatterplot mátrix jó módja a gyorsan át létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-462">A scatterplot matrix is a good way to produce a quick look.</span></span> <span data-ttu-id="7cf1d-463">I vagyok cseréje a `str()` függvény a következő sorral az előző R kódban.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-463">I am replacing the `str()` function in the previous R code with the following line.</span></span>

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

<span data-ttu-id="7cf1d-464">Futtassa a következő kódot, és tekintse meg, mi történik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-464">Run this code and see what happens.</span></span> <span data-ttu-id="7cf1d-465">Az előállított, az R eszköz port rajzot 16. ábra hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-465">The plot produced at the R Device port should look like Figure 16.</span></span>

![A kijelölt változók Scatterplot mátrix][17]

<span data-ttu-id="7cf1d-467">*16. ábra. A kijelölt változók mátrix Scatterplot.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-467">*Figure 16. Scatterplot matrix of selected variables.*</span></span>

<span data-ttu-id="7cf1d-468">Van néhány odd-looking struktúra kapcsolatok változókhoz között.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-468">There is some odd-looking structure in the relationships between these variables.</span></span> <span data-ttu-id="7cf1d-469">Lehet, hogy ez akkor merül fel az adatok a trendek és a tényt, hogy azt nem rendelkezik szabványosított a változókat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-469">Perhaps this arises from trends in the data and from the fact that we have not standardized the variables.</span></span>

### <a name="correlation-analysis"></a><span data-ttu-id="7cf1d-470">Korrelációs elemzés</span><span class="sxs-lookup"><span data-stu-id="7cf1d-470">Correlation analysis</span></span>
<span data-ttu-id="7cf1d-471">Korrelációs elemzés végrehajtásához kell deszerializálni trend és a változók szabványosítására is.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-471">To perform correlation analysis we need to both de-trend and standardize the variables.</span></span> <span data-ttu-id="7cf1d-472">Sikerült egyszerűen használjuk az R `scale()` függvénynek, amely az adatközpontok, mind az méretezi a változókat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-472">We could simply use the R `scale()` function, which both centers and scales variables.</span></span> <span data-ttu-id="7cf1d-473">Ez a funkció is előfordulhat, hogy gyorsabban futnak.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-473">This function might well run faster.</span></span> <span data-ttu-id="7cf1d-474">Azonban kívánt defenzív programing példa látható R.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-474">However, I want to show you an example of defensive programing in R.</span></span>

<span data-ttu-id="7cf1d-475">A `ts.detrend()` függvény alább látható mindkét ezeket a műveleteket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-475">The `ts.detrend()` function shown below performs both of these operations.</span></span> <span data-ttu-id="7cf1d-476">A következő két sornyi kód deszerializálni trend az adatokat, és majd szabványosítására az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-476">The following two lines of code de-trend the data and then standardize the values.</span></span>

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function to de-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time to have the same length',
                    'ERROR: ts.detrend requires argument ts to be of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros to return as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # The input arguments are not of the same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If the ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If the ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that the Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend the time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply the detrend.ts function to the variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot the results to look at the relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

<span data-ttu-id="7cf1d-477">Nincs elég egy bit azonban a a `ts.detrend()` függvény.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-477">There is quite a bit happening in the `ts.detrend()` function.</span></span> <span data-ttu-id="7cf1d-478">Ez a kód a legtöbb ellenőrzi az argumentumok potenciális problémákat vagy kivételek, továbbra is a számítások során felmerülő foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-478">Most of this code is checking for potential problems with the arguments or dealing with exceptions, which can still arise during the computations.</span></span> <span data-ttu-id="7cf1d-479">Ez a kód csak néhány sornyi ténylegesen tegye a számításokat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-479">Only a few lines of this code actually do the computations.</span></span>

<span data-ttu-id="7cf1d-480">Rendelkezik már tárgyalt defenzív programozás a példát [átalakítások érték](#valuetransformations).</span><span class="sxs-lookup"><span data-stu-id="7cf1d-480">We have already discussed an example of defensive programming in [Value transformations](#valuetransformations).</span></span> <span data-ttu-id="7cf1d-481">Mindkét számítási blokkok csomagolni vannak `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-481">Both computation blocks are wrapped in `tryCatch()`.</span></span> <span data-ttu-id="7cf1d-482">A hibák érdemes térjen vissza az eredeti bemeneti vektort, és más esetekben nullák vektor visszatérés.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-482">For some errors it makes sense to return the original input vector, and in other cases, I return a vector of zeros.</span></span>  

<span data-ttu-id="7cf1d-483">Vegye figyelembe, hogy a használt deszerializálni trendek lineáris regressziós egy idő adatsorozat regressziós.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-483">Note that the linear regression used for de-trending is a time series regression.</span></span> <span data-ttu-id="7cf1d-484">A előrejelzőjének változó egy idő adatsorozat objektum.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-484">The predictor variable is a time series object.</span></span>  

<span data-ttu-id="7cf1d-485">Egyszer `ts.detrend()` definiált azt alkalmazhatja azt a dataframe érdeklődik a változókat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-485">Once `ts.detrend()` is defined we apply it to the variables of interest in our dataframe.</span></span> <span data-ttu-id="7cf1d-486">A megjelenő listában által létrehozott igazolnia kell kényszerítési `lapply()` adatok dataframe használatával történő `as.data.frame()`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-486">We must coerce the resulting list created by `lapply()` to data dataframe by using `as.data.frame()`.</span></span> <span data-ttu-id="7cf1d-487">Defenzív aspektusainak miatt `ts.detrend()`, nem sikerült feldolgozni a változók egyikét nem akadályozza meg a helyes feldolgozását a többi.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-487">Because of defensive aspects of `ts.detrend()`, failure to process one of the variables will not prevent correct processing of the others.</span></span>  

<span data-ttu-id="7cf1d-488">A végső kódsort páros scatterplot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-488">The final line of code creates a pairwise scatterplot.</span></span> <span data-ttu-id="7cf1d-489">Miután az R-kód, a scatterplot eredményei láthatók 17. ábra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-489">After running the R code, the results of the scatterplot are shown in Figure 17.</span></span>

![Vonja trendszerű és szabványos idősorozat páros scatterplot][18]

<span data-ttu-id="7cf1d-491">*17. ábra. Páros scatterplot deszerializálni trendszerű és szabványos idősorozat.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-491">*Figure 17. Pairwise scatterplot of de-trended and standardized time series.*</span></span>

<span data-ttu-id="7cf1d-492">Összehasonlíthatja a 16 feltüntetett ezekkel az eredményekkel.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-492">You can compare these results to those shown in Figure 16.</span></span> <span data-ttu-id="7cf1d-493">A trend eltávolítása és a szabványos változók sokkal kevesebb szerkezetben vannak ezek a változók kapcsolatai látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-493">With the trend removed and the variables standardized, we see a lot less structure in the relationships between these variables.</span></span>

<span data-ttu-id="7cf1d-494">Az R FTB objektumként korrelációk számítási kódot a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-494">The code to compute the correlations as R ccf objects is as follows.</span></span>

    ## A function to compute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of the pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute the list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

<span data-ttu-id="7cf1d-495">Ezt a kódot futtató hoz létre a naplófájl látható a 18.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-495">Running this code produces the log shown in Figure 18.</span></span>

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

<span data-ttu-id="7cf1d-496">*18. ábra. FTB listája a páros korrelációs elemzés a következő helyről objektumokat.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-496">*Figure 18. List of ccf objects from the pairwise correlation analysis.*</span></span>

<span data-ttu-id="7cf1d-497">Minden egyes lag korrelációs értéke van.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-497">There is a correlation value for each lag.</span></span> <span data-ttu-id="7cf1d-498">A következő korrelációs értékek nincs elég nagy ahhoz, hogy jelentős.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-498">None of these correlation values is large enough to be significant.</span></span> <span data-ttu-id="7cf1d-499">Azt ezért be, hogy azt az egyes minta egymástól függetlenül is.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-499">We can therefore conclude that we can model each variable independently.</span></span>

### <a name="output-a-dataframe"></a><span data-ttu-id="7cf1d-500">Egy dataframe kimeneti</span><span class="sxs-lookup"><span data-stu-id="7cf1d-500">Output a dataframe</span></span>
<span data-ttu-id="7cf1d-501">A páros korrelációk számított azt rendelkezik, az R FTB objektumok listájának.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-501">We have computed the pairwise correlations as a list of R ccf objects.</span></span> <span data-ttu-id="7cf1d-502">Ez megadja bit típust a probléma, az eredmény adathalmaz kimeneti portjával valóban egy dataframe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-502">This presents a bit of a problem as the Result Dataset output port really requires a dataframe.</span></span> <span data-ttu-id="7cf1d-503">További a FTB objektum pedig egy lista, és azt szeretné, hogy csak az értékeket a lista első elemében, a különböző késedelmes jelentések korrelációk.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-503">Further, the ccf object is itself a list and we want only the values in the first element of this list, the correlations at the various lags.</span></span>

<span data-ttu-id="7cf1d-504">A következő kódot a késés értékek kiolvassa a FTB objektumok, amelyek maguk is listák listáját.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-504">The following code extracts the lag values from the list of ccf objects, which are themselves lists.</span></span>

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with the row names column and the
    ## correlation data frame and assign the column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## The following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

<span data-ttu-id="7cf1d-505">Az első kódsort kicsit legbonyolultabb, és rövid segítenek megérteni azt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-505">The first line of code is a bit tricky, and some explanation may help you understand it.</span></span> <span data-ttu-id="7cf1d-506">Belülről kifelé működik vezetünk be a következőket:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-506">Working from the inside out we have the following:</span></span>

1. <span data-ttu-id="7cf1d-507">A "**[[**"argumentum operator"**1**" választja ki, a késedelmes jelentések korrelációk vektorát a FTB objektum-lista első eleme.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-507">The '**[[**' operator with the argument '**1**' selects the vector of correlations at the lags from the first element of the ccf object list.</span></span>
2. <span data-ttu-id="7cf1d-508">A `do.call()` függvényt alkalmazza a `rbind()` keresztül a lista elemeinek függvény által `lapply()`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-508">The `do.call()` function applies the `rbind()` function over the elements of the list returns by `lapply()`.</span></span>
3. <span data-ttu-id="7cf1d-509">A `data.frame()` függvény típusú értékké konvertál, az eredmény által előállított `do.call()` egy dataframe számára.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-509">The `data.frame()` function coerces the result produced by `do.call()` to a dataframe.</span></span>

<span data-ttu-id="7cf1d-510">Vegye figyelembe, hogy a sor nevek a dataframe oszlopában.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-510">Note that the row names are in a column of the dataframe.</span></span> <span data-ttu-id="7cf1d-511">Ez így megtartja a sor neve, amikor azok kimenetét a [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="7cf1d-511">Doing so preserves the row names when they are output from the [Execute R Script][execute-r-script].</span></span>

<span data-ttu-id="7cf1d-512">A kód futtatásával hozza létre a 19. ábrán látható kimeneti amikor I **Visualize** az eredmény Dataset port a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-512">Running the code produces the output shown in Figure 19 when I **Visualize** the output at the Result Dataset port.</span></span> <span data-ttu-id="7cf1d-513">A sor nevek szerepelnek az első oszlop rendeltetésszerű.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-513">The row names are in the first column, as intended.</span></span>

![A korrelációs elemzési eredmények kimenete][20]

<span data-ttu-id="7cf1d-515">*19. ábra. Az eredmények a korrelációs elemzés kimenetét.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-515">*Figure 19. Results output from the correlation analysis.*</span></span>

## <span data-ttu-id="7cf1d-516"><a id="seasonalforecasting"></a>Idő adatsorozat példa: határozza előrejelzés</span><span class="sxs-lookup"><span data-stu-id="7cf1d-516"><a id="seasonalforecasting"></a>Time series example: seasonal forecasting</span></span>
<span data-ttu-id="7cf1d-517">Az adatok most elemzés alkalmas űrlapon, és azt észlelte a változók között nincs jelentős korrelációk van.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-517">Our data is now in a form suitable for analysis, and we have determined there are no significant correlations between the variables.</span></span> <span data-ttu-id="7cf1d-518">Most lépés, és hozzon létre egy idősorozat-modell előrejelzési.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-518">Let's move on and create a time series forecasting model.</span></span> <span data-ttu-id="7cf1d-519">A modell segítségével azt fogja előrejelzési kaliforniai tejtermelés 2013 12 hónapig.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-519">Using this model we will forecast California milk production for the 12 months of 2013.</span></span>

<span data-ttu-id="7cf1d-520">Az előrejelzési modell két összetevőt, a trend összetevőt és határozza lesz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-520">Our forecasting model will have two components, a trend component and a seasonal component.</span></span> <span data-ttu-id="7cf1d-521">A teljes előrejelzés a két összetevő szorzatát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-521">The complete forecast is the product of these two components.</span></span> <span data-ttu-id="7cf1d-522">Az ilyen típusú modell tényezőt modell néven ismert.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-522">This type of model is known as a multiplicative model.</span></span> <span data-ttu-id="7cf1d-523">Ez esetben egy additívak modellt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-523">The alternative is an additive model.</span></span> <span data-ttu-id="7cf1d-524">A változók iránt, így tractable az elemzés azt már alkalmazott napló átalakítás.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-524">We have already applied a log transformation to the variables of interest, which makes this analysis tractable.</span></span>

<span data-ttu-id="7cf1d-525">A teljes R-kód az ebben a szakaszban korábban letöltött zip-fájlban van.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-525">The complete R code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="creating-the-dataframe-for-analysis"></a><span data-ttu-id="7cf1d-526">Elemzés dataframe létrehozása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-526">Creating the dataframe for analysis</span></span>
<span data-ttu-id="7cf1d-527">Először vegyen fel egy **új** [R-parancsfájl végrehajtása] [ execute-r-script] modult a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-527">Start by adding a **new** [Execute R Script][execute-r-script] module to your experiment.</span></span> <span data-ttu-id="7cf1d-528">Csatlakozás a **eredmény Dataset** kimenetét a meglévő [R-parancsfájl végrehajtása] [ execute-r-script] modult a **Dataset1** új modul bemeneti.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-528">Connect the **Result Dataset** output of the existing [Execute R Script][execute-r-script] module to the **Dataset1** input of the new module.</span></span> <span data-ttu-id="7cf1d-529">Az eredmény hasonlóan kell kinéznie. ábra 20.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-529">The result should look something like Figure 20.</span></span>

![A hozzáadott új R-parancsfájl végrehajtása modul a kísérletben][21]

<span data-ttu-id="7cf1d-531">*20. ábra. A hozzáadott új R-parancsfájl végrehajtása modul a kísérletben.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-531">*Figure 20. The experiment with the new Execute R Script module added.*</span></span>

<span data-ttu-id="7cf1d-532">Mint a korrelációs elemzése, azt csak befejeződött, a kell adatsorozat objektum POSIXct idő oszlop hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-532">As with the correlation analysis we just completed, we need to add a column with a POSIXct time series object.</span></span> <span data-ttu-id="7cf1d-533">A következő kódot fog ehhez csak.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-533">The following code will do just this.</span></span>

    # If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

<span data-ttu-id="7cf1d-534">Futtassa a következő kódot, és nézze meg a naplót.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-534">Run this code and look at the log.</span></span> <span data-ttu-id="7cf1d-535">Az eredmény 21. ábrát hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-535">The result should look like Figure 21.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

<span data-ttu-id="7cf1d-536">*21. ábra. A dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-536">*Figure 21. A summary of the dataframe.*</span></span>

<span data-ttu-id="7cf1d-537">Ez az eredmény a azt az analysis készen áll.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-537">With this result, we are ready to start our analysis.</span></span>

### <a name="create-a-training-dataset"></a><span data-ttu-id="7cf1d-538">Hozzon létre egy képzési adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7cf1d-538">Create a training dataset</span></span>
<span data-ttu-id="7cf1d-539">Az összeállított dataframe a igazolnia kell létrehozni egy képzési adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-539">With the dataframe constructed we need to create a training dataset.</span></span> <span data-ttu-id="7cf1d-540">Ezeket az adatokat tartalmazza az összes az év 2013, kivéve az utolsó 12 észrevételeit Ez az a tesztelési adatkészletnél.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-540">This data will include all of the observations except the last 12, of the year 2013, which is our test dataset.</span></span> <span data-ttu-id="7cf1d-541">A következő code alkészletek a dataframe, és létrehozza a tejelő éles üzemi pontjának és ár változók előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-541">The following code subsets the dataframe and creates plots of the dairy production and price variables.</span></span> <span data-ttu-id="7cf1d-542">Majd a négy termelési előkészítésére létrehozhatók és változók ár.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-542">I then create plots of the four production and price variables.</span></span> <span data-ttu-id="7cf1d-543">Egy névtelen függvény segítségével határozza meg, néhány szabályozva az információhasználat a rajzolási, és a többi két argumentumot listájának majd ismétlés `Map()`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-543">An anonymous function is used to define some augments for plot, and then iterate over the list of the other two arguments with `Map()`.</span></span> <span data-ttu-id="7cf1d-544">Ha vannak-e végezni, amely egy a hurok kellene rendelkeznie működött itt, hogy helyesek.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-544">If you are thinking that a for loop would have worked fine here, you are correct.</span></span> <span data-ttu-id="7cf1d-545">Azonban mivel R I vagyok jeleníti meg a működési megközelítés működési nyelvet.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-545">But, since R is a functional language I am showing you a functional approach.</span></span>

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

<span data-ttu-id="7cf1d-546">A kód futtatásával hozza létre az adatsorozat series tevékenységtérkép az R eszköz kimenetből látható 22. ábra idő.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-546">Running the code produces the series of time series plots from the R Device output shown in Figure 22.</span></span> <span data-ttu-id="7cf1d-547">Vegye figyelembe, hogy a időtengelye a dátumok, az adatsorozat megrajzolásához metódus időt töltött előnyt egységekben.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-547">Note that the time axis is in units of dates, a nice benefit of the time series plot method.</span></span>

![Első idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Második idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Az idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok külső](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Az idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok negyedik](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

<span data-ttu-id="7cf1d-552">*22. ábra. Idő adatsorozat előkészítésére kaliforniai tejtermelésre és adatokat.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-552">*Figure 22. Time series plots of California dairy production and price data.*</span></span>

### <a name="a-trend-model"></a><span data-ttu-id="7cf1d-553">A trend modell</span><span class="sxs-lookup"><span data-stu-id="7cf1d-553">A trend model</span></span>
<span data-ttu-id="7cf1d-554">Egy alkalommal adatsorozat objektum és volna az adatokat egy pillantást létrehozása kezdjük a kaliforniai tej termelési adatok trend modell összeállításához.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-554">Having created a time series object and having had a look at the data, let's start to construct a trend model for the California milk production data.</span></span> <span data-ttu-id="7cf1d-555">Azt a idő adatsorozat regressziós végezheti el.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-555">We can do this with a time series regression.</span></span> <span data-ttu-id="7cf1d-556">Azt azonban nem egyértelmű, hogy a rendszer több mint egy görbét kell és a megfigyelt változása a betanítási adatok pontosan modellezheti intercept a rajzolási a.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-556">However, it is clear from the plot that we will need more than a slope and intercept to accurately model the observed trend in the training data.</span></span>

<span data-ttu-id="7cf1d-557">Az adatok kis léptékű megadott, I lesz a trend Rstudióból a modell létrehozása Kivágás és illessze be az eredményül kapott modell Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-557">Given the small scale of the data, I will build the model for trend in RStudio and then cut and paste the resulting model into Azure Machine Learning.</span></span> <span data-ttu-id="7cf1d-558">Az ilyen típusú interaktív elemzések elvégzéséhez interaktív környezetet biztosít az Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-558">RStudio provides an interactive environment for this type of interactive analysis.</span></span>

<span data-ttu-id="7cf1d-559">Első kísérlet, mint a polinom regressziós rendelkező legfeljebb 3 megpróbálom.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-559">As a first attempt, I will try a polynomial regression with powers up to 3.</span></span> <span data-ttu-id="7cf1d-560">Nincs túlzott illeszkedő modellek az ilyen típusú valós veszélye.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-560">There is a real danger of over-fitting these kinds of models.</span></span> <span data-ttu-id="7cf1d-561">Emiatt érdemes magas rendelés feltételeit elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-561">Therefore, it is best to avoid high order terms.</span></span> <span data-ttu-id="7cf1d-562">A `I()` funkció meggátolja a tartalmának értelmezése rövid nevének (értelmezi a tartalma ","), és lehetővé teszi a egy regressziós egyenlet egy szó értelmezett függvény írása.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-562">The `I()` function inhibits interpretation of the contents (interprets the contents 'as is') and allows you to write a literally interpreted function in a regression equation.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

<span data-ttu-id="7cf1d-563">Ezt követően a következő.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-563">This generates the following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

<span data-ttu-id="7cf1d-564">P értékek (Pr (> |} t |})) szöveget a kimenetben, láthatja, hogy a squared kifejezés nem mindig jelentős.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-564">From P values (Pr(>|t|)) in this output, we can see that the squared term may not be significant.</span></span> <span data-ttu-id="7cf1d-565">Használom a `update()` működnek, mint ez a modell módosításához squared kifejezés ejtésével.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-565">I will use the `update()` function to modify this model by dropping the squared term.</span></span>

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

<span data-ttu-id="7cf1d-566">Ezt követően a következő.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-566">This generates the following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

<span data-ttu-id="7cf1d-567">Ez a következőhöz jobb.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-567">This looks better.</span></span> <span data-ttu-id="7cf1d-568">A feltételek mindegyikének szükség.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-568">All of the terms are significant.</span></span> <span data-ttu-id="7cf1d-569">Azonban a 2e-16 érték az alapértelmezett érték, és nem kell venni súlyosan túl.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-569">However, the 2e-16 value is a default value, and should not be taken too seriously.</span></span>  

<span data-ttu-id="7cf1d-570">Megerősítést tesztként most Meggyőződünk ki kaliforniai tejtermelésre adatok idő adatsorozat rajzot a trend görbe látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-570">As a sanity test, let's make a time series plot of the California dairy production data with the trend curve shown.</span></span> <span data-ttu-id="7cf1d-571">A következő kódot az Azure Machine Learning hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] modell (nem Rstudióból), hogy rajzot, és a modell létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-571">I have added the following code in the Azure Machine Learning [Execute R Script][execute-r-script] model (not RStudio) to create the model and make a plot.</span></span> <span data-ttu-id="7cf1d-572">Az eredmény 23. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-572">The result is shown in Figure 23.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Kaliforniai tej termelési adatok trend modellel látható](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

<span data-ttu-id="7cf1d-574">*23. ábra. Kaliforniai tej termelési adatok trend modellel látható.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-574">*Figure 23. California milk production data with trend model shown.*</span></span>

<span data-ttu-id="7cf1d-575">Úgy tűnik, a trend modell viszonylag jól illeszkedik az adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-575">It looks like the trend model fits the data fairly well.</span></span> <span data-ttu-id="7cf1d-576">További úgy nem tűnik, hogy túlzott méretezés lennie, mint például a modell görbén wiggles páratlan.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-576">Further, there does not seem to be evidence of over-fitting, such as odd wiggles in the model curve.</span></span>  

### <a name="seasonal-model"></a><span data-ttu-id="7cf1d-577">Határozza modell</span><span class="sxs-lookup"><span data-stu-id="7cf1d-577">Seasonal model</span></span>
<span data-ttu-id="7cf1d-578">Az aktuális trend modell igazolnia kell leküldéses és a határozza hatások közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-578">With a trend model in hand, we need to push on and include the seasonal effects.</span></span> <span data-ttu-id="7cf1d-579">Az év hónapját használjuk a lineáris modell dummy változóként havonta készülő hatásának rögzítése.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-579">We will use the month of the year as a dummy variable in the linear model to capture the month-by-month effect.</span></span> <span data-ttu-id="7cf1d-580">Vegye figyelembe, hogy a modell tényező változók bevezetéséhez, ha a webhelynek nem kell számolni.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-580">Note that when you introduce factor variables into a model, the intercept must not be computed.</span></span> <span data-ttu-id="7cf1d-581">Ha ezt nem teszi meg, a képlet osztana meg van adva, de R fog dobja el az egyik a kívánt tényezőket, de megőrizni annak a webhelynek kifejezés.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-581">If you do not do this, the formula is over-specified and R will drop one of the desired factors but keep the intercept term.</span></span>

<span data-ttu-id="7cf1d-582">Mivel kielégítő trend modell tudunk is használhatók a `update()` működnek, mint az új feltételeket hozzáadása a meglévő modellt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-582">Since we have a satisfactory trend model we can use the `update()` function to add the new terms to the existing model.</span></span> <span data-ttu-id="7cf1d-583">A -1, a frissítés képletben intercept kifejezés esik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-583">The -1 in the update formula drops the intercept term.</span></span> <span data-ttu-id="7cf1d-584">Egyelőre folytatja, a Rstudióból:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-584">Continuing in RStudio for the moment:</span></span>

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

<span data-ttu-id="7cf1d-585">Ezt követően a következő.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-585">This generates the following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

<span data-ttu-id="7cf1d-586">Látható, hogy a modell már nem rendelkezik egy intercept kifejezés és 12 hónap jelentős tényező rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-586">We see that the model no longer has an intercept term and has 12 significant month factors.</span></span> <span data-ttu-id="7cf1d-587">Ennek az az pontosan mit akartunk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-587">This is exactly what we wanted to see.</span></span>

<span data-ttu-id="7cf1d-588">Ellenőrizze egy másik idő adatsorozat rajz kaliforniai tejtermelésre adatok milyen jól működik-e a határozza modell.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-588">Let's make another time series plot of the California dairy production data to see how well the seasonal model is working.</span></span> <span data-ttu-id="7cf1d-589">A következő kódot az Azure Machine Learning hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] hogy rajzot, és a modell létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-589">I have added the following code in the Azure Machine Learning [Execute R Script][execute-r-script] to create the model and make a plot.</span></span>

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

<span data-ttu-id="7cf1d-590">Ez a kód futtatása az Azure Machine Learning hoz létre a rajzolási 24. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-590">Running this code in Azure Machine Learning produces the plot shown in Figure 24.</span></span>

![Kaliforniai tejtermelés határozza hatások beleértve modell](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

<span data-ttu-id="7cf1d-592">*24. ábra. Kaliforniai tejtermelés határozza hatások beleértve modellt.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-592">*Figure 24. California milk production with model including seasonal effects.*</span></span>

<span data-ttu-id="7cf1d-593">24. ábrán látható adatok kitöltése ahelyett, hogy ezzel.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-593">The fit to the data shown in Figure 24 is rather encouraging.</span></span> <span data-ttu-id="7cf1d-594">A trendek és a hatása (havi változat) határozza meg a ésszerű.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-594">Both the trend and the seasonal effect (monthly variation) look reasonable.</span></span>

<span data-ttu-id="7cf1d-595">A modell ellenőrzése, mint most rá egy pillantást a például.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-595">As another check on our model, let's have a look at the residuals.</span></span> <span data-ttu-id="7cf1d-596">Kiszámítja a két modell az előre jelzett értékek, a például a határozza modell kiszámítja és majd tevékenységtérkép ezeket a betanítási adatok például a következő kódot.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-596">The following code computes the predicted values from our two models, computes the residuals for the seasonal model, and then plots these residuals for the training data.</span></span>

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot the residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

<span data-ttu-id="7cf1d-597">A maradék rajzot 25. ábra jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-597">The residual plot is shown in Figure 25.</span></span>

![Például a határozza modell a tanítási adatokat](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

<span data-ttu-id="7cf1d-599">*25. ábra. Például a határozza modell a tanítási adatokat.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-599">*Figure 25. Residuals of the seasonal model for the training data.*</span></span>

<span data-ttu-id="7cf1d-600">Ezeket például hely ésszerű.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-600">These residuals look reasonable.</span></span> <span data-ttu-id="7cf1d-601">Nem adott struktúra, kivéve a hatását, hogy a 2008-2009 recesszió, amely a modell nem derül ki van különösen jól.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-601">There is no particular structure, except the effect of the 2008-2009 recession, which our model does not account for particularly well.</span></span>

<span data-ttu-id="7cf1d-602">A megjelenő ábra 25 rajzot bármely időfüggő minták észlelésére a például a.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-602">The plot shown in Figure 25 is useful for detecting any time-dependent patterns in the residuals.</span></span> <span data-ttu-id="7cf1d-603">Az explicit módszert is, amely a használt például számítási és a például a rajzolási sorrendben idő helyezi.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-603">The explicit approach of computing and plotting the residuals I used places the residuals in time order on the plot.</span></span> <span data-ttu-id="7cf1d-604">Ha, másrészt I kellett ábrázolt `milk.lm$residuals`, a rajzolási nem volna időrendjét.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-604">If, on the other hand, I had plotted `milk.lm$residuals`, the plot would not have been in time order.</span></span>

<span data-ttu-id="7cf1d-605">Is `plot.lm()` diagnosztikai előkészítésére sorozata létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-605">You can also use `plot.lm()` to produce a series of diagnostic plots.</span></span>

    ## Show the diagnostic plots for the model
    plot(milk.lm2, ask = FALSE)

<span data-ttu-id="7cf1d-606">Ez a kód 26. ábrán látható diagnosztikai előkészítésére sorozata eredményez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-606">This code produces a series of diagnostic plots shown in Figure 26.</span></span>

![Diagnosztikai előkészítésére határozza modell elsején](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Diagnosztikai előkészítésére határozza modell második](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![A diagnosztikai előkészítésére határozza modell harmadik](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![A diagnosztikai előkészítésére határozza modell negyedik](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

<span data-ttu-id="7cf1d-611">*26. ábra. Diagnosztikai tevékenységtérkép határozza modell.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-611">*Figure 26. Diagnostic plots for the seasonal model.*</span></span>

<span data-ttu-id="7cf1d-612">Nincsenek ezek előkészítésére, de nem végezhető el a kiváló aggodalomra azonosított néhány magas segíti elő pontok.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-612">There are a few highly influential points identified in these plots, but nothing to cause great concern.</span></span> <span data-ttu-id="7cf1d-613">További láthatja a normál Q-Q rajzot az, hogy a például a Bezárás gombra a szokásos módon elosztott, egy fontos feltételezésen lineáris modellek-e.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-613">Further, we can see from the Normal Q-Q plot that the residuals are close to normally distributed, an important assumption for linear models.</span></span>

### <a name="forecasting-and-model-evaluation"></a><span data-ttu-id="7cf1d-614">Előrejelzési és modell kiértékelése</span><span class="sxs-lookup"><span data-stu-id="7cf1d-614">Forecasting and model evaluation</span></span>
<span data-ttu-id="7cf1d-615">Nincs elegendő egyetlen dolog példánkban befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-615">There is just one more thing to do to complete our example.</span></span> <span data-ttu-id="7cf1d-616">Igazolnia kell a számítási előrejelzések, és mérheti a hiba a tényleges adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-616">We need to compute forecasts and measure the error against the actual data.</span></span> <span data-ttu-id="7cf1d-617">Az előrejelzés 2013 12 hónapig lesz.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-617">Our forecast will be for the 12 months of 2013.</span></span> <span data-ttu-id="7cf1d-618">Ez az előrejelzés a tényleges adatok, amely nem része az képzési adatkészletet hiba intézkedésként számíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-618">We can compute an error measure for this forecast to the actual data that is not part of our training dataset.</span></span> <span data-ttu-id="7cf1d-619">Emellett azt is vetik össze a 18 évnyi vizsgálati adatok számított 12 hónapon tanítási adatokat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-619">Additionally, we can compare performance on the 18 years of training data to the 12 months of test data.</span></span>  

<span data-ttu-id="7cf1d-620">A metrikák számú idő adatsorozat modellek teljesítményének méréséhez szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-620">A number of metrics are used to measure the performance of time series models.</span></span> <span data-ttu-id="7cf1d-621">A mi esetünkben használjuk a négyzetes (RMS) által jelzett hibát.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-621">In our case we will use the root mean square (RMS) error.</span></span> <span data-ttu-id="7cf1d-622">A következő függvény kiszámítja a két sorozat közötti RMS hiba.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-622">The following function computes the RMS error between two series.</span></span>  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function to compute the RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments to function RMS.error of wrong type encountered",
                    "ERROR: Input vector to function RMS.error is too short",
                    "ERROR: Input vectors to function RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check the arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate the values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute the RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

<span data-ttu-id="7cf1d-623">Például a `log.transform()` "Érték átalakítások" szakaszban tárgyalt funkció nincs elég nagy mennyiségű ellenőrzése és a kivétel helyreállítási hibakód ebben a függvényben.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-623">As with the `log.transform()` function we discussed in the "Value transformations" section, there is quite a lot of error checking and exception recovery code in this function.</span></span> <span data-ttu-id="7cf1d-624">Ezeket az alapelveket alkalmazott azonosak.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-624">The principles employed are the same.</span></span> <span data-ttu-id="7cf1d-625">A munkát csomagolni két helyen `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-625">The work is done in two places wrapped in `tryCatch()`.</span></span> <span data-ttu-id="7cf1d-626">Először idő adatsorokat exponentiated, mivel azt dolgozott az értékek a naplókat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-626">First, the time series are exponentiated, since we have been working with the logs of the values.</span></span> <span data-ttu-id="7cf1d-627">Második a tényleges RMS hiba számított.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-627">Second, the actual RMS error is computed.</span></span>  

<span data-ttu-id="7cf1d-628">Most egy olyan függvényt felszerelt méréséhez az RMS-hiba, hozza létre és egy dataframe az RMS a hibákat tartalmazó kimeneti.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-628">Equipped with a function to measure the RMS error, let's build and output a dataframe containing the RMS errors.</span></span> <span data-ttu-id="7cf1d-629">Kizárólag trend modell feltételek és a teljes modell határozza tényezők is.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-629">We will include terms for the trend model alone and the complete model with seasonal factors.</span></span> <span data-ttu-id="7cf1d-630">A következő kódot a feladat használatával hajtja végre a két lineáris modell, igazolnia kell kialakítani.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-630">The following code does the job by using the two linear models we have constructed.</span></span>

    ## Compute the RMS error in a dataframe
    ## Include the row names in the first column so they will
    ## appear in the output of the Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

<span data-ttu-id="7cf1d-631">Ezt a kódot futtató hoz létre, az eredmény adathalmaz kimeneti portjával. ábra 27 kimenettel.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-631">Running this code produces the output shown in Figure 27 at the Result Dataset output port.</span></span>

![RMS hibák modellek összehasonlítása][26]

<span data-ttu-id="7cf1d-633">*27. ábra. RMS hibák modellek összehasonlítása.*</span><span class="sxs-lookup"><span data-stu-id="7cf1d-633">*Figure 27. Comparison of RMS errors for the models.*</span></span>

<span data-ttu-id="7cf1d-634">Ezekkel az eredményekkel, az látható, hogy a modell ad hozzá a határozza tényezők csökkenti az RMS hiba jelentősen.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-634">From these results, we see that adding the seasonal factors to the model reduces the RMS error significantly.</span></span> <span data-ttu-id="7cf1d-635">Nem túl beállítás a képzési adatok RMS hiba: jelző bit kisebb, mint az előrejelzéshez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-635">Not too surprisingly, the RMS error for the training data is a bit less than for the forecast.</span></span>

## <span data-ttu-id="7cf1d-636"><a id="appendixa"></a>FÜGGELÉK: Útmutató Rstudióból</span><span class="sxs-lookup"><span data-stu-id="7cf1d-636"><a id="appendixa"></a>APPENDIX A: Guide to RStudio</span></span>
<span data-ttu-id="7cf1d-637">Rstudióból meglehetősen megfelelően legyen dokumentálva, így a függelék I fogja adni az egyes hivatkozásokon a kulcs az első lépésekhez Rstudióból dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-637">RStudio is quite well documented, so in this appendix I will provide some links to the key sections of the RStudio documentation to get you started.</span></span>

1. <span data-ttu-id="7cf1d-638">Projektek létrehozása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-638">Creating projects</span></span>
   
   <span data-ttu-id="7cf1d-639">Rendezheti és az R-kód a projektek Rstudióból használatával kezelni.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-639">You can organize and manage your R code into projects by using RStudio.</span></span> <span data-ttu-id="7cf1d-640">A dokumentáció projektek használó https://support.rstudio.com/hc/articles/200526207-Using-Projects helyen találhatók meg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-640">The documentation that uses projects can be found at https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span></span>
   
   <span data-ttu-id="7cf1d-641">Kövesse az alábbi utasításokat, és az R-Kódminták projekt létrehozása ebben a dokumentumban javasolt.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-641">I recommend that you follow these directions and create a project for the R code examples in this document.</span></span>  
2. <span data-ttu-id="7cf1d-642">Szerkesztés és az R-kód végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-642">Editing and executing R code</span></span>
   
   <span data-ttu-id="7cf1d-643">A szerkesztési és R-kód végrehajtása integrált környezetet biztosít az Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-643">RStudio provides an integrated environment for editing and executing R code.</span></span> <span data-ttu-id="7cf1d-644">Https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code dokumentációjában találhatók.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-644">Documentation can be found at https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span></span>
3. <span data-ttu-id="7cf1d-645">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="7cf1d-645">Debugging</span></span>
   
   <span data-ttu-id="7cf1d-646">Rstudióból hatékony hibakeresési képességeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-646">RStudio includes powerful debugging capabilities.</span></span> <span data-ttu-id="7cf1d-647">Ezek a szolgáltatások dokumentációja https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio jelenleg.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-647">Documentation for these features is at https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span></span>
   
   <span data-ttu-id="7cf1d-648">A töréspont hibaelhárítási lehetőségek: https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-648">The breakpoint troubleshooting features are documented at https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span></span>

## <span data-ttu-id="7cf1d-649"><a id="appendixb"></a>B függelék: További információ</span><span class="sxs-lookup"><span data-stu-id="7cf1d-649"><a id="appendixb"></a>APPENDIX B: Further reading</span></span>
<span data-ttu-id="7cf1d-650">Az R programozási oktatóanyag az Azure Machine Learning Studio az R nyelv használatával kell alapjait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-650">This R programming tutorial covers the basics of what you need to use the R language with Azure Machine Learning Studio.</span></span> <span data-ttu-id="7cf1d-651">Ha nem ismeri a R, a CRAN két bevezetés érhetők el:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-651">If you are not familiar with R, two introductions are available on CRAN:</span></span>

* <span data-ttu-id="7cf1d-652">R Emmanuel Paradis által kezdőknek remek kezdőpont http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-652">R for Beginners by Emmanuel Paradis is a good place to start at http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span></span>  
* <span data-ttu-id="7cf1d-653">R w n bemutatása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-653">An Introduction to R by W. N.</span></span> <span data-ttu-id="7cf1d-654">Venables et.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-654">Venables et.</span></span> <span data-ttu-id="7cf1d-655">al.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-655">al.</span></span> <span data-ttu-id="7cf1d-656">egy kicsit nagyobb mélysége, http://cran.r-project.org/doc/manuals/R-intro.html állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-656">goes into a bit more depth, at http://cran.r-project.org/doc/manuals/R-intro.html.</span></span>

<span data-ttu-id="7cf1d-657">Nincs olyan sok könyvek R, amelyik segíthet a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-657">There are many books on R that can help you get started.</span></span> <span data-ttu-id="7cf1d-658">Íme néhány hasznos található:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-658">Here are a few I find useful:</span></span>

* <span data-ttu-id="7cf1d-659">A kép R programozás: A bemutató a statisztikai szoftver kialakítása által Norman Matloff esetén programozásba a R. kiváló bemutatása</span><span class="sxs-lookup"><span data-stu-id="7cf1d-659">The Art of R Programming: A Tour of Statistical Software Design by Norman Matloff is an excellent introduction to programming in R.</span></span>  
* <span data-ttu-id="7cf1d-660">R Cookbook által Paul Teetor R. használatával probléma és a megoldás megközelítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-660">R Cookbook by Paul Teetor provides a problem and solution approach to using R.</span></span>  
* <span data-ttu-id="7cf1d-661">A művelet által Robert Kabacoff R egy másik hasznos bevezető címjegyzék-alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-661">R in Action by Robert Kabacoff is another useful introductory book.</span></span> <span data-ttu-id="7cf1d-662">A kiegészítő gyors R webhely http://www.statmethods.net/ egy hasznos erőforrás.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-662">The companion Quick R website is a useful resource at http://www.statmethods.net/.</span></span>
* <span data-ttu-id="7cf1d-663">R Inferno által Patrick égési, kezelje a legbonyolultabb és nehezen méretezhetővé foglalkozó témakörök, amelyek is lehet észlelt, amikor az r programozási számos érdekes vicces könyv A címjegyzék szabad http://www.burns-stat.com/documents/books/the-r-inferno/ érhető el.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-663">R Inferno by Patrick Burns is a surprisingly humorous book that deals with a number of tricky and difficult topics that can be encountered when programming in R. The book is available for free at http://www.burns-stat.com/documents/books/the-r-inferno/.</span></span>
* <span data-ttu-id="7cf1d-664">A részletes bemutatója a speciális témakörei R, van szükség egy pillantást a könyv speciális R Hadley Wickham által.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-664">If you want a deep dive into advanced topics in R, have a look at the book Advanced R by Hadley Wickham.</span></span> <span data-ttu-id="7cf1d-665">A könyv online verziója nem szabad http://adv-r.had.co.nz/ érhető el.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-665">The online version of this book is available for free at http://adv-r.had.co.nz/.</span></span>

<span data-ttu-id="7cf1d-666">A katalógus R idő adatsorozat csomagok adatsorozat elemzés CRAN feladat nézetben található: http://cran.r-project.org/web/views/TimeSeries.html.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-666">A catalogue of R time series packages can be found in the CRAN Task View for time series analysis: http://cran.r-project.org/web/views/TimeSeries.html.</span></span> <span data-ttu-id="7cf1d-667">Adatsorozat objektum csomagok adott időben információt, hogy a csomag dokumentációjában kell hivatkozunk.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-667">For information on specific time series object packages, you should refer to the documentation for that package.</span></span>

<span data-ttu-id="7cf1d-668">A címjegyzék bevezető Time Series Paul Cowpertwait és Andrew Metcalfe r bevezetője, amelyek adatsorozat elemzés R használja.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-668">The book Introductory Time Series with R by Paul Cowpertwait and Andrew Metcalfe provides an introduction to using R for time series analysis.</span></span> <span data-ttu-id="7cf1d-669">Számos további elméleti szövegek R példákat.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-669">Many more theoretical texts provide R examples.</span></span>

<span data-ttu-id="7cf1d-670">Néhány nagy internetes erőforrásokhoz:</span><span class="sxs-lookup"><span data-stu-id="7cf1d-670">Some great internet resources:</span></span>

* <span data-ttu-id="7cf1d-671">DataCamp: DataCamp útmutatást ad az R a videó megszerzett és kódolási gyakorlatok a böngésző kényelmét.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-671">DataCamp: DataCamp teaches R in the comfort of your browser with video lessons and coding exercises.</span></span> <span data-ttu-id="7cf1d-672">A legújabb R technikák és csomagok interaktív oktatóanyagok van.</span><span class="sxs-lookup"><span data-stu-id="7cf1d-672">There are interactive tutorials on the latest R techniques and packages.</span></span> <span data-ttu-id="7cf1d-673">A szabad interaktív oktatóanyag R veszik a https://www.datacamp.com/courses/introduction-to-r</span><span class="sxs-lookup"><span data-stu-id="7cf1d-673">Take the free interactive R tutorial at https://www.datacamp.com/courses/introduction-to-r</span></span>
* <span data-ttu-id="7cf1d-674">Útmutató, kezdeti lépések a R Programiz https://www.programiz.com/r-programming a</span><span class="sxs-lookup"><span data-stu-id="7cf1d-674">A guide on Getting started with R from Programiz https://www.programiz.com/r-programming</span></span>
* <span data-ttu-id="7cf1d-675">A gyors R oktatóprogram által Tibor fekete Clarkson egyetemi http://www.cyclismo.org/tutorial/R/</span><span class="sxs-lookup"><span data-stu-id="7cf1d-675">A quick R tutorial by Kelly Black from Clarkson University http://www.cyclismo.org/tutorial/R/</span></span>
* <span data-ttu-id="7cf1d-676">60 + R erőforrások megtalálható a http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span><span class="sxs-lookup"><span data-stu-id="7cf1d-676">60+ R resources listed at http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span></span>

<!--Image references-->
[1]: ./media/machine-learning-r-quickstart/fig1.png
[2]: ./media/machine-learning-r-quickstart/fig2.png
[3]: ./media/machine-learning-r-quickstart/fig3.png
[4]: ./media/machine-learning-r-quickstart/fig4.png
[5]: ./media/machine-learning-r-quickstart/fig5.png
[6]: ./media/machine-learning-r-quickstart/fig6.png
[7]: ./media/machine-learning-r-quickstart/fig7.png
[8]: ./media/machine-learning-r-quickstart/fig8.png
[9]: ./media/machine-learning-r-quickstart/fig9.png
[10]: ./media/machine-learning-r-quickstart/fig10.png
[11]: ./media/machine-learning-r-quickstart/fig11.png
[12]: ./media/machine-learning-r-quickstart/fig12.png
[13]: ./media/machine-learning-r-quickstart/fig13.png
[14]: ./media/machine-learning-r-quickstart/fig14.png
[15]: ./media/machine-learning-r-quickstart/fig15.png
[16]: ./media/machine-learning-r-quickstart/fig16.png
[17]: ./media/machine-learning-r-quickstart/fig17.png
[18]: ./media/machine-learning-r-quickstart/fig18.png
[19]: ./media/machine-learning-r-quickstart/fig19.png
[20]: ./media/machine-learning-r-quickstart/fig20.png
[21]: ./media/machine-learning-r-quickstart/fig21.png
[22]: ./media/machine-learning-r-quickstart/fig22.png

[26]: ./media/machine-learning-r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
