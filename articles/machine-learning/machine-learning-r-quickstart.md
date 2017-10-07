---
title: "az R nyelv a Machine Learning aaaQuickstart oktatóanyag |} Microsoft Docs"
description: "Az R programozási útmutató tooget használatának gyors hello R nyelv az Azure Machine Learning Studio toocreate előrejelzési megoldást használni."
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
ms.openlocfilehash: 9995f8728f4d7bf9a5c15412015e4cf769cdac96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-tutorial-for-hello-r-programming-language-for-azure-machine-learning"></a><span data-ttu-id="c1170-104">Az hello R programozási nyelv az Azure Machine Learning gyors üzembe helyezési útmutató</span><span class="sxs-lookup"><span data-stu-id="c1170-104">Quickstart tutorial for hello R programming language for Azure Machine Learning</span></span>

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a><span data-ttu-id="c1170-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c1170-105">Introduction</span></span>
<span data-ttu-id="c1170-106">Gyors üzembe helyezési oktatóanyag segítségével gyorsan az Azure Machine Learning kiterjesztése az hello R programozási nyelv használatával indíthat el.</span><span class="sxs-lookup"><span data-stu-id="c1170-106">This quickstart tutorial helps you quickly start extending Azure Machine Learning by using hello R programming language.</span></span> <span data-ttu-id="c1170-107">Kövesse a programozási útmutató toocreate R, tesztelése és R kódban az Azure Machine Learning segítségével hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="c1170-107">Follow this R programming tutorial toocreate, test and execute R code within Azure Machine Learning.</span></span> <span data-ttu-id="c1170-108">Oktatóanyag munka, előrejelzési teljes körű megoldást hoz létre az Azure Machine Learning hello R nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="c1170-108">As you work through tutorial, you will create a complete forecasting solution by using hello R language in Azure Machine Learning.</span></span>  

<span data-ttu-id="c1170-109">Számos hatékony gépi tanulási és adatok adatkezelési modulok Microsoft Azure Machine Learning tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-109">Microsoft Azure Machine Learning contains many powerful machine learning and data manipulation modules.</span></span> <span data-ttu-id="c1170-110">hello hatékony R nyelv szerint hello nyelv franca Analytics leírása.</span><span class="sxs-lookup"><span data-stu-id="c1170-110">hello powerful R language has been described as hello lingua franca of analytics.</span></span> <span data-ttu-id="c1170-111">Elemzés és az adatok kezelése az Azure Machine Learning boldogan, bővíthető R. használatával Ez a kombináció biztosít hello méretezhetőséget és a központi telepítés az Azure Machine Learning könnyű hello rugalmasságot és az r mély elemzés</span><span class="sxs-lookup"><span data-stu-id="c1170-111">Happily, analytics and data manipulation in Azure Machine Learning can be extended by using R. This combination provides hello scalability and ease of deployment of Azure Machine Learning with hello flexibility and deep analytics of R.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-hello-dataset"></a><span data-ttu-id="c1170-112">Az előrejelzés és hello adatkészlet</span><span class="sxs-lookup"><span data-stu-id="c1170-112">Forecasting and hello dataset</span></span>
<span data-ttu-id="c1170-113">Az előrejelzés a széles körben munkavállalók és elég hasznos lehet analitikai módszer.</span><span class="sxs-lookup"><span data-stu-id="c1170-113">Forecasting is a widely employed and quite useful analytical method.</span></span> <span data-ttu-id="c1170-114">Közös közé határozza cikkek optimális készlet szintek, toopredicting makrogazdasági változók meghatározása előrejelzésére használja.</span><span class="sxs-lookup"><span data-stu-id="c1170-114">Common uses range from predicting sales of seasonal items, determining optimal inventory levels, toopredicting macroeconomic variables.</span></span> <span data-ttu-id="c1170-115">Az előrejelzés idő adatsorozat modellekkel való általában történik.</span><span class="sxs-lookup"><span data-stu-id="c1170-115">Forecasting is typically done with time series models.</span></span>

<span data-ttu-id="c1170-116">Adatsorozat időadatok az adatai, amelyben hello értékek rendelkezik idő indexszel.</span><span class="sxs-lookup"><span data-stu-id="c1170-116">Time series data is data in which hello values have a time index.</span></span> <span data-ttu-id="c1170-117">hello idő index rendszeres, lehet, például minden hónap vagy percben, vagy szabálytalan.</span><span class="sxs-lookup"><span data-stu-id="c1170-117">hello time index can be regular, e.g. every month or every minute, or irregular.</span></span> <span data-ttu-id="c1170-118">Egy idősorozat-modellbe idő adatsorozat adatokon alapul.</span><span class="sxs-lookup"><span data-stu-id="c1170-118">A time series model is based on time series data.</span></span> <span data-ttu-id="c1170-119">hello R programozási nyelv egy rugalmas keretrendszer és a széles körű elemzés idő adatsorozat adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-119">hello R programming language contains a flexible framework and extensive analytics for time series data.</span></span>

<span data-ttu-id="c1170-120">Az első lépések útmutató rendszer kell kaliforniai tejtermelésre használata és díjszabási adatok.</span><span class="sxs-lookup"><span data-stu-id="c1170-120">In this quickstart guide we will be working with California dairy production and pricing data.</span></span> <span data-ttu-id="c1170-121">Ezek az adatok több tejtermékek hello éles és hello ár tejzsír, egy teljesítményteszt hagyományos havi információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1170-121">This data includes monthly information on hello production of several dairy products and hello price of milk fat, a benchmark commodity.</span></span>

<span data-ttu-id="c1170-122">Ebben a cikkben, és az R parancsfájlokat használt hello adatok [itt letöltött][download].</span><span class="sxs-lookup"><span data-stu-id="c1170-122">hello data used in this article, along with R scripts, can be [downloaded here][download].</span></span> <span data-ttu-id="c1170-123">Ezeket az adatokat eredetileg történt synthesized származó információk érhetők el a következő http://future.aae.wisc.edu/tab/production.html egyetemi a Wisconsin hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-123">This data was originally synthesized from information available from hello University of Wisconsin at http://future.aae.wisc.edu/tab/production.html.</span></span>

### <a name="organization"></a><span data-ttu-id="c1170-124">Szervezet</span><span class="sxs-lookup"><span data-stu-id="c1170-124">Organization</span></span>
<span data-ttu-id="c1170-125">Megtudhatja, hogyan toocreate, tesztelése és elemzés és az adatok R adatkezelési kód végrehajtása hello Azure Machine Learning környezetben, azt fogja előrehaladás számos lépésen.</span><span class="sxs-lookup"><span data-stu-id="c1170-125">We will progress through several steps as you learn how toocreate, test and execute analytics and data manipulation R code in hello Azure Machine Learning environment.</span></span>  

* <span data-ttu-id="c1170-126">Először néhány hello R nyelv használatával hello Azure Machine Learning Studio környezetben hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="c1170-126">First we will explore hello basics of using hello R language in hello Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="c1170-127">Ezután azt előrehaladás toodiscussing i/o adatok, az R-kód és a grafikus hello Azure Machine Learning-környezetben különböző jellemzőinek megtekintését.</span><span class="sxs-lookup"><span data-stu-id="c1170-127">Then we progress toodiscussing various aspects of I/O for data, R code and graphics in hello Azure Machine Learning environment.</span></span>
* <span data-ttu-id="c1170-128">A Microsoft fog majd összeállítani hello első megoldás részét képező az előrejelzési adatok tisztítása és átalakítása kódjának létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="c1170-128">We will then construct hello first part of our forecasting solution by creating code for data cleaning and transformation.</span></span>
* <span data-ttu-id="c1170-129">Az előkészített adataink végezzük el hello korrelációk között az adathalmaz hello változók számos elemzését.</span><span class="sxs-lookup"><span data-stu-id="c1170-129">With our data prepared we will perform an analysis of hello correlations between several of hello variables in our dataset.</span></span>
* <span data-ttu-id="c1170-130">Végül létre fogunk hozni egy határozza idő series előrejelzési modell tejtermelésre.</span><span class="sxs-lookup"><span data-stu-id="c1170-130">Finally, we will create a seasonal time series forecasting model for milk production.</span></span>

## <span data-ttu-id="c1170-131"><a id="mlstudio"></a>Együttműködhet az R nyelv a Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="c1170-131"><a id="mlstudio"></a>Interact with R language in Machine Learning Studio</span></span>
<span data-ttu-id="c1170-132">Ez a szakasz végigvezeti néhány hello R programozási nyelv hello Machine Learning Studio környezetben való interakció alapjait.</span><span class="sxs-lookup"><span data-stu-id="c1170-132">This section takes you through some basics of interacting with hello R programming language in hello Machine Learning Studio environment.</span></span> <span data-ttu-id="c1170-133">hello R nyelv biztosít egy hatékony eszköz testre szabott toocreate elemzés és az adatok adatkezelési modulok hello Azure Machine Learning környezeten belül.</span><span class="sxs-lookup"><span data-stu-id="c1170-133">hello R language provides a powerful tool toocreate customized analytics and data manipulation modules within hello Azure Machine Learning environment.</span></span>

<span data-ttu-id="c1170-134">Kis méretű Rstudióból toodevelop, tesztelése és hibakeresési R-kód fogja használni.</span><span class="sxs-lookup"><span data-stu-id="c1170-134">I will use RStudio toodevelop, test and debug R code on a small scale.</span></span> <span data-ttu-id="c1170-135">Ez a kód vágjon és a beillesztési műveleteket egy [R-parancsfájl végrehajtása] [ execute-r-script] a Machine Learning Studio készen toorun modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-135">This code is then cut and paste into an [Execute R Script][execute-r-script] module in Machine Learning Studio ready toorun.</span></span>  

### <a name="hello-execute-r-script-module"></a><span data-ttu-id="c1170-136">hello R-parancsfájl végrehajtása modul</span><span class="sxs-lookup"><span data-stu-id="c1170-136">hello Execute R Script module</span></span>
<span data-ttu-id="c1170-137">A Machine Learning Studio belül R parancsfájlok belül hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-137">Within Machine Learning Studio, R scripts are run within hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-138">Példa hello [R-parancsfájl végrehajtása] [ execute-r-script] modul a Machine Learning Studióban az 1. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-138">An example of hello [Execute R Script][execute-r-script] module in Machine Learning Studio is shown in Figure 1.</span></span>

 ![R programozási nyelv: hello R-parancsfájl végrehajtása modul kiválasztva a Machine Learning Studióban][1]

<span data-ttu-id="c1170-140">*1. ábra. hello Machine Learning Studio környezet megjelenítő hello R-parancsfájl végrehajtása modul kiválasztva.*</span><span class="sxs-lookup"><span data-stu-id="c1170-140">*Figure 1. hello Machine Learning Studio environment showing hello Execute R Script module selected.*</span></span>

<span data-ttu-id="c1170-141">TooFigure 1 utaló, vizsgáljuk meg néhány hello hello használata a Machine Learning Studio környezet legfontosabb részeit hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-141">Referring tooFigure 1, let's look at some of hello key parts of hello Machine Learning Studio environment for working with hello [Execute R Script][execute-r-script] module.</span></span>

* <span data-ttu-id="c1170-142">hello modulok hello kísérletben hello középső ablaktáblán jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-142">hello modules in hello experiment are shown in hello center pane.</span></span>
* <span data-ttu-id="c1170-143">hello jobb oldali ablaktábla felső részén hello egy ablak tooview tartalmazza, és az R parancsfájlok szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="c1170-143">hello upper part of hello right pane contains a window tooview and edit your R scripts.</span></span>  
* <span data-ttu-id="c1170-144">jobb oldali hello alsó részén látható hello néhány tulajdonságát [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="c1170-144">hello lower part of right pane shows some properties of hello [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="c1170-145">Hello hiba és a kimeneti naplók hello megfelelő tesztüzeméhez, ha a panel kattintva tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-145">You can view hello error and output logs by clicking on hello appropriate spots of this pane.</span></span>

<span data-ttu-id="c1170-146">Azt fogja, természetesen kell megvitatása hello [R-parancsfájl végrehajtása] [ execute-r-script] Ez a dokumentum többi hello nagyobb részletességgel.</span><span class="sxs-lookup"><span data-stu-id="c1170-146">We will, of course, be discussing hello [Execute R Script][execute-r-script] in greater detail in hello rest of this document.</span></span>

<span data-ttu-id="c1170-147">Az összetett R funkciók használatakor javasolt, hogy szerkesztése, tesztelése és hibakeresése a Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="c1170-147">When working with complex R functions, I recommend that you edit, test and debug in RStudio.</span></span> <span data-ttu-id="c1170-148">Csakúgy, mint bármely szoftverfejlesztői Növekményesen kiterjesztése a kódot, és kis egyszerű vizsgálati eseteknél az tesztelik azt.</span><span class="sxs-lookup"><span data-stu-id="c1170-148">As with any software development, extend your code incrementally and test it on small simple test cases.</span></span> <span data-ttu-id="c1170-149">Majd Kivágás, és illessze be a funkciók hello R parancsfájl ablakának hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-149">Then cut and paste your functions into hello R script window of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-150">Ez a megközelítés lehetővé teszi tooharness egyaránt hello Rstudióból integrált fejlesztési környezeti (IDE) és hatványra emelésének Azure Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-150">This approach allows you tooharness both hello RStudio integrated development environment (IDE) and hello power of Azure Machine Learning.</span></span>  

#### <a name="execute-r-code"></a><span data-ttu-id="c1170-151">R-kód végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c1170-151">Execute R code</span></span>
<span data-ttu-id="c1170-152">Hello bármely R-kód [R-parancsfájl végrehajtása] [ execute-r-script] modul végrehajtja a hello kísérlet hello kattintva futtatásakor **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1170-152">Any R code in hello [Execute R Script][execute-r-script] module will execute when you run hello experiment by clicking on hello **Run** button.</span></span> <span data-ttu-id="c1170-153">Végrehajtás befejezése után megjelenik a négyzet hello [R-parancsfájl végrehajtása] [ execute-r-script] ikonra.</span><span class="sxs-lookup"><span data-stu-id="c1170-153">When execution has completed, a check mark will appear on hello [Execute R Script][execute-r-script] icon.</span></span>

#### <a name="defensive-r-coding-for-azure-machine-learning"></a><span data-ttu-id="c1170-154">Defenzív R kódolási Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c1170-154">Defensive R coding for Azure Machine Learning</span></span>
<span data-ttu-id="c1170-155">Tegyük fel például, egy webszolgáltatás-bővítmény az R-kód fejlesztői Azure Machine Learning segítségével, meg kell mindenképpen terveznie módját a kód foglalkozni fog váratlan adatok bemeneti és a kivételek.</span><span class="sxs-lookup"><span data-stu-id="c1170-155">If you are developing R code for, say, a web service by using Azure Machine Learning, you should definitely plan how your code will deal with an unexpected data input and exceptions.</span></span> <span data-ttu-id="c1170-156">toomaintain átláthatóság érdekében I nem szereplő nagy ellenőrzése vagy a legtöbb látható hello kódpéldák kivételkezelő hello módon.</span><span class="sxs-lookup"><span data-stu-id="c1170-156">toomaintain clarity, I have not included much in hello way of checking or exception handling in most of hello code examples shown.</span></span> <span data-ttu-id="c1170-157">Azonban, a Folytatás I fogja diktálni funkciók számos példát R-hez tartozó kivételkezelő funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="c1170-157">However, as we proceed I will give you several examples of functions by using R's exception handling capability.</span></span>  

<span data-ttu-id="c1170-158">Ha egy R kivételkezelést teljesebb kezelésére van szüksége, I javasoljuk, hogy olvassa el a megfelelő szakaszait hello hello könyv felsorolt Wickham által [B függelék – további olvasási](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="c1170-158">If you need a more complete treatment of R exception handling, I recommend you read hello applicable sections of hello book by Wickham listed in [Appendix B - Further Reading](#appendixb).</span></span>

#### <a name="debug-and-test-r-in-machine-learning-studio"></a><span data-ttu-id="c1170-159">Hibakeresését és tesztelését R a Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="c1170-159">Debug and test R in Machine Learning Studio</span></span>
<span data-ttu-id="c1170-160">tooreiterate, javasolt tesztelése és hibakeresése az R-kód az Rstudióból kis méretű.</span><span class="sxs-lookup"><span data-stu-id="c1170-160">tooreiterate, I recommend you test and debug your R code on a small scale in RStudio.</span></span> <span data-ttu-id="c1170-161">Azonban, vannak esetek, ahol tootrack R kódproblémáknak az hello le kell [R-parancsfájl végrehajtása] [ execute-r-script] magát.</span><span class="sxs-lookup"><span data-stu-id="c1170-161">However, there are cases where you will need tootrack down R code problems in hello [Execute R Script][execute-r-script] itself.</span></span> <span data-ttu-id="c1170-162">Ezenkívül azt is célszerű toocheck az eredményeket a Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="c1170-162">In addition, it is good practice toocheck your results in Machine Learning Studio.</span></span>

<span data-ttu-id="c1170-163">Az R-kód és hello Azure Machine Learning platformon hello végrehajtási eredményének elsősorban megtalálható kimenetét.</span><span class="sxs-lookup"><span data-stu-id="c1170-163">Output from hello execution of your R code and on hello Azure Machine Learning platform is found primarily in output.log.</span></span> <span data-ttu-id="c1170-164">További információk a error.log fogja látni.</span><span class="sxs-lookup"><span data-stu-id="c1170-164">Some additional information will be seen in error.log.</span></span>  

<span data-ttu-id="c1170-165">Ha a hiba akkor fordul elő, a Machine Learning Studióban az R-kód futtatása során, az első lépések: error.log toolook kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c1170-165">If an error occurs in Machine Learning Studio while running your R code, your first course of action should be toolook at error.log.</span></span> <span data-ttu-id="c1170-166">Ez a fájl hasznos hiba üzenetek toohelp tudomásul veszi, és javítsa ki a hibát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-166">This file can contain useful error messages toohelp you understand and correct your error.</span></span> <span data-ttu-id="c1170-167">tooview error.log, kattintson a **nézet hibanapló** a hello **tulajdonságok panelen** a hello [R-parancsfájl végrehajtása] [ execute-r-script] hello hibát tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="c1170-167">tooview error.log, click on **View error log** on hello **properties pane** for hello [Execute R Script][execute-r-script] containing hello error.</span></span>

<span data-ttu-id="c1170-168">Például az R-kód, nem definiált változó y, a következő hello futtattam egy [R-parancsfájl végrehajtása] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="c1170-168">For example, I ran hello following R code, with an undefined variable y, in an [Execute R Script][execute-r-script] module:</span></span>

    x <- 1.0
    z <- x + y

<span data-ttu-id="c1170-169">Ez a kód nem sikerül tooexecute hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="c1170-169">This code fails tooexecute, resulting in an error condition.</span></span> <span data-ttu-id="c1170-170">Kattintson a **nézet hibanapló** a hello **tulajdonságok panelen** előállított hello megjelenítési 2. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-170">Clicking on **View error log** on hello **properties pane** produces hello display shown in Figure 2.</span></span>

  ![Felugró hibaüzenet][2]

<span data-ttu-id="c1170-172">*2. ábra. Előugró hibaüzenet.*</span><span class="sxs-lookup"><span data-stu-id="c1170-172">*Figure 2. Error message pop-up.*</span></span>

<span data-ttu-id="c1170-173">Úgy tűnik, igazolnia kell a kimenetét toosee hello R hibaüzenet toolook.</span><span class="sxs-lookup"><span data-stu-id="c1170-173">It looks like we need toolook in output.log toosee hello R error message.</span></span> <span data-ttu-id="c1170-174">Kattintson a hello [R-parancsfájl végrehajtása] [ execute-r-script] majd kattintson a hello **kimenetét megtekintése** hello cikk **tulajdonságok panelen** toohello jobbra.</span><span class="sxs-lookup"><span data-stu-id="c1170-174">Click on hello [Execute R Script][execute-r-script] and then click on hello **View output.log** item on hello **properties pane** toohello right.</span></span> <span data-ttu-id="c1170-175">Egy új böngészőablakban nyílik meg, és a következő hello meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-175">A new browser window opens, and I see hello following.</span></span>

    [Critical]     Error: Error 0063: hello following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

<span data-ttu-id="c1170-176">Ez a hibaüzenet nem meglepetések számát tartalmazza, és egyértelműen azonosítják a hello probléma.</span><span class="sxs-lookup"><span data-stu-id="c1170-176">This error message contains no surprises and clearly identifies hello problem.</span></span>

<span data-ttu-id="c1170-177">az R bármely objektum tooinspect hello értéket, nyomtathatja ki ezen értékek toohello kimenetét fájlt.</span><span class="sxs-lookup"><span data-stu-id="c1170-177">tooinspect hello value of any object in R, you can print these values toohello output.log file.</span></span> <span data-ttu-id="c1170-178">hello szabályok objektumot vizsgáló értékek a következők hello ugyanaz, mint egy interaktív R munkamenet.</span><span class="sxs-lookup"><span data-stu-id="c1170-178">hello rules for examining object values are essentially hello same as in an interactive R session.</span></span> <span data-ttu-id="c1170-179">Például sorban adjon meg egy változónevet, hello objektum hello értéket nyomtatott toohello kimenetét fájl lesz.</span><span class="sxs-lookup"><span data-stu-id="c1170-179">For example, if you type a variable name on a line, hello value of hello object will be printed toohello output.log file.</span></span>  

#### <a name="packages-in-machine-learning-studio"></a><span data-ttu-id="c1170-180">A Machine Learning Studióban csomagok</span><span class="sxs-lookup"><span data-stu-id="c1170-180">Packages in Machine Learning Studio</span></span>
<span data-ttu-id="c1170-181">Az Azure Machine Learning több mint 350 előre telepített R nyelvi csomagokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1170-181">Azure Machine Learning comes with over 350 preinstalled R language packages.</span></span> <span data-ttu-id="c1170-182">Használhatja a következő kódot a hello hello [R-parancsfájl végrehajtása] [ execute-r-script] modul tooretrieve hello listája előre telepített csomagokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-182">You can use hello following code in hello [Execute R Script][execute-r-script] module tooretrieve a list of hello preinstalled packages.</span></span>

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

<span data-ttu-id="c1170-183">Ez a kód utolsó sora hello hello pillanatban nem megismerése, olvassa el a.</span><span class="sxs-lookup"><span data-stu-id="c1170-183">If you don't understand hello last line of this code at hello moment, read on.</span></span> <span data-ttu-id="c1170-184">Ez a dokumentum többi hello nagymértékben ismertetik hello Azure Machine Learning környezet az R nyelv használatát.</span><span class="sxs-lookup"><span data-stu-id="c1170-184">In hello rest of this document we will extensively discuss using R in hello Azure Machine Learning environment.</span></span>

### <a name="introduction-toorstudio"></a><span data-ttu-id="c1170-185">Bevezetés tooRStudio</span><span class="sxs-lookup"><span data-stu-id="c1170-185">Introduction tooRStudio</span></span>
<span data-ttu-id="c1170-186">Rstudióból egy széles körben használt IDE az R. A Szerkesztés, a tesztelés és a hibakereséshez hello R-kód gyors üzembe helyezési útmutatóban használt egyes használom Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="c1170-186">RStudio is a widely used IDE for R. I will use RStudio for editing, testing and debugging some of hello R code used in this quick start guide.</span></span> <span data-ttu-id="c1170-187">Ha tesztelt, és készen áll, az R-kód egyszerűen kimásolni, majd a hello Rstudióból szerkesztő illessze be a Machine Learning Studio [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-187">Once R code is tested and ready, you simply cut and paste from hello RStudio editor into a Machine Learning Studio [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="c1170-188">Ha nem rendelkezik hello R programozási nyelv a asztali gépen telepítve van, akkor tegye meg most javasolt.</span><span class="sxs-lookup"><span data-stu-id="c1170-188">If you do not have hello R programming language installed on your desktop machine, I recommend you do so now.</span></span> <span data-ttu-id="c1170-189">Ingyenes letöltést nyílt forráskódú R nyelv hello átfogó R archív hálózati (CRAN) webhelyen érhetők el: [http://www.r-project.org/](http://www.r-project.org/).</span><span class="sxs-lookup"><span data-stu-id="c1170-189">Free downloads of open source R language are available at hello Comprehensive R Archive Network (CRAN) at [http://www.r-project.org/](http://www.r-project.org/).</span></span> <span data-ttu-id="c1170-190">Nincsenek elérhető a Windows, Mac OS és Linux/UNIX letöltések.</span><span class="sxs-lookup"><span data-stu-id="c1170-190">There are downloads available for Windows, Mac OS, and Linux/UNIX.</span></span> <span data-ttu-id="c1170-191">Válassza ki a közeli tükrözött, és kövesse hello letöltési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-191">Choose a nearby mirror and follow hello download directions.</span></span> <span data-ttu-id="c1170-192">Ezenkívül a CRAN számos hasznos elemzés és az adatok adatkezelési csomagok olyan tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-192">In addition, CRAN contains a wealth of useful analytics and data manipulation packages.</span></span>

<span data-ttu-id="c1170-193">Ha új tooRStudio, töltse le és telepítse hello asztali verzióját.</span><span class="sxs-lookup"><span data-stu-id="c1170-193">If you are new tooRStudio, you should download and install hello desktop version.</span></span> <span data-ttu-id="c1170-194">Rstudióból tölti le a Windows, Mac OS és Linux/UNIX http://www.rstudio.com/products/RStudio/: hello található.</span><span class="sxs-lookup"><span data-stu-id="c1170-194">You can find hello RStudio downloads for Windows, Mac OS, and Linux/UNIX at http://www.rstudio.com/products/RStudio/.</span></span> <span data-ttu-id="c1170-195">Kövesse az asztali gépen tooinstall Rstudióból megadott hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-195">Follow hello directions provided tooinstall RStudio on your desktop machine.</span></span>  

<span data-ttu-id="c1170-196">Egy bevezető oktatóanyag tooRStudio https://support.rstudio.com/hc/sections/200107586-Using-RStudio érhető el.</span><span class="sxs-lookup"><span data-stu-id="c1170-196">A tutorial introduction tooRStudio is available at https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span></span>

<span data-ttu-id="c1170-197">I néhány kiegészítő információt nyújt az Rstudióból használatával [függelék][appendixa].</span><span class="sxs-lookup"><span data-stu-id="c1170-197">I provide some additional information on using RStudio in [Appendix A][appendixa].</span></span>  

## <span data-ttu-id="c1170-198"><a id="scriptmodule"></a>Mindkét hello R-parancsfájl végrehajtása modul beolvasása</span><span class="sxs-lookup"><span data-stu-id="c1170-198"><a id="scriptmodule"></a>Get data in and out of hello Execute R Script module</span></span>
<span data-ttu-id="c1170-199">Ebben a szakaszban ismertetjük, hogyan elérhetővé adatok esetében bejövő és kimenő hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-199">In this section we will discuss how you get data into and out of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-200">Hogyan toohandle különféle adatok olvasása-hello fog tanulmányozzák [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-200">We will review how toohandle various data types read into and out of hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="c1170-201">hello teljes kód látható az ebben a szakaszban korábban letöltött zip-fájl hello van.</span><span class="sxs-lookup"><span data-stu-id="c1170-201">hello complete code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="load-and-check-data-in-machine-learning-studio"></a><span data-ttu-id="c1170-202">Betölteni, és ellenőrizze az adatokat a Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="c1170-202">Load and check data in Machine Learning Studio</span></span>
#### <span data-ttu-id="c1170-203"><a id="loading"></a>Hello adatkészlet betöltése</span><span class="sxs-lookup"><span data-stu-id="c1170-203"><a id="loading"></a>Load hello dataset</span></span>
<span data-ttu-id="c1170-204">Először hello betöltésével **csdairydata.csv** Azure Machine Learning Studio fájlból.</span><span class="sxs-lookup"><span data-stu-id="c1170-204">We will start by loading hello **csdairydata.csv** file into Azure Machine Learning Studio.</span></span>

* <span data-ttu-id="c1170-205">Indítsa el az Azure Machine Learning Studio környezetet.</span><span class="sxs-lookup"><span data-stu-id="c1170-205">Start your Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="c1170-206">Kattintson a **+ új** hello, a képernyő, és válassza ki a bal alsó **Dataset**.</span><span class="sxs-lookup"><span data-stu-id="c1170-206">Click on **+ NEW** at hello lower left of your screen and select **Dataset**.</span></span>
* <span data-ttu-id="c1170-207">Válassza ki **helyi fájlból**, majd **Tallózás** tooselect hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="c1170-207">Select **From Local File**, and then **Browse** tooselect hello file.</span></span>
* <span data-ttu-id="c1170-208">Győződjön meg arról, hogy a kijelölt **általános CSV-fájl (.csv) fejlécű** hello adatkészlet hello típusként.</span><span class="sxs-lookup"><span data-stu-id="c1170-208">Make sure you have selected **Generic CSV file with header (.csv)** as hello type for hello dataset.</span></span>
* <span data-ttu-id="c1170-209">Kattintson a hello pipára.</span><span class="sxs-lookup"><span data-stu-id="c1170-209">Click hello check mark.</span></span>
* <span data-ttu-id="c1170-210">Hello dataset feltöltése után megtekintheti az új adatkészlet hello hello kattintva **adatkészletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="c1170-210">After hello dataset has been uploaded, you should see hello new dataset by clicking on hello **Datasets** tab.</span></span>  

#### <a name="create-an-experiment"></a><span data-ttu-id="c1170-211">A kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1170-211">Create an experiment</span></span>
<span data-ttu-id="c1170-212">Most, hogy néhány adat a Machine Learning Studióban, igazolnia kell a toocreate kísérlet toodo hello elemzését.</span><span class="sxs-lookup"><span data-stu-id="c1170-212">Now that we have some data in Machine Learning Studio, we need toocreate an experiment toodo hello analysis.</span></span>  

* <span data-ttu-id="c1170-213">Kattintson a **+ új** hello a bal alsó, és válassza ki **kísérlet**, majd **üres kísérlet**.</span><span class="sxs-lookup"><span data-stu-id="c1170-213">Click on **+ NEW** at hello lower left and select **Experiment**, then **Blank Experiment**.</span></span>
* <span data-ttu-id="c1170-214">Meg kísérletbe kiválasztásával, és módosítja, hello **kísérlet létre...**  cím hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c1170-214">You can name your experiment by selecting, and modifying, hello **Experiment created on ...** title at hello top of hello page.</span></span> <span data-ttu-id="c1170-215">Például megváltoztathatja túl**hitelesítésszolgáltató tejtermék Analysis**.</span><span class="sxs-lookup"><span data-stu-id="c1170-215">For example, changing it too**CA Dairy Analysis**.</span></span>
* <span data-ttu-id="c1170-216">Hello hello kísérlet lap bal oldali, bontsa ki a **mentett adatkészletek**, majd **saját adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="c1170-216">On hello left of hello experiment page, expand **Saved Datasets**, and then **My Datasets**.</span></span> <span data-ttu-id="c1170-217">Megtekintheti az hello **cadairydata.csv** korábban feltöltött.</span><span class="sxs-lookup"><span data-stu-id="c1170-217">You should see hello **cadairydata.csv** that you uploaded earlier.</span></span>
* <span data-ttu-id="c1170-218">A fogd és vidd hello **csdairydata.csv dataset** alakzatot hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="c1170-218">Drag and drop hello **csdairydata.csv dataset** onto hello experiment.</span></span>
* <span data-ttu-id="c1170-219">A hello **keresési kísérletezhet elemek** hello felső részén hello bal oldali ablaktáblán típus mezőjében [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="c1170-219">In hello **Search experiment items** box on hello top of hello left pane, type [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="c1170-220">Látni fogja hello modul hello keresési listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-220">You will see hello module appear in hello search list.</span></span>
* <span data-ttu-id="c1170-221">A fogd és vidd hello [R-parancsfájl végrehajtása] [ execute-r-script] modul alakzatot a sorát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-221">Drag and drop hello [Execute R Script][execute-r-script] module onto your pallet.</span></span>  
* <span data-ttu-id="c1170-222">Csatlakozás hello hello kimenete **csdairydata.csv adatkészlet** toohello bal szélső bemeneti (**Dataset1**) a hello [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="c1170-222">Connect hello output of hello **csdairydata.csv dataset** toohello leftmost input (**Dataset1**) of hello [Execute R Script][execute-r-script].</span></span>
* <span data-ttu-id="c1170-223">**Ne felejtse el a "Mentés" tooclick!**</span><span class="sxs-lookup"><span data-stu-id="c1170-223">**Don't forget tooclick on 'Save'!**</span></span>  

<span data-ttu-id="c1170-224">Ezen a ponton a kísérletben hasonlóan kell kinéznie 3. ábra.</span><span class="sxs-lookup"><span data-stu-id="c1170-224">At this point your experiment should look something like Figure 3.</span></span>

![a DataSet adatkészlet és R-parancsfájl végrehajtása modul kísérletezhet hello hitelesítésszolgáltató tejtermék elemzés][3]

<span data-ttu-id="c1170-226">*3. ábra. Hitelesítésszolgáltató tejtermék elemzés hello dataset és R-parancsfájl végrehajtása modul kísérletezhet.*</span><span class="sxs-lookup"><span data-stu-id="c1170-226">*Figure 3. hello CA Dairy Analysis experiment with dataset and Execute R Script module.*</span></span>

#### <a name="check-on-hello-data"></a><span data-ttu-id="c1170-227">Hello adatok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c1170-227">Check on hello data</span></span>
<span data-ttu-id="c1170-228">Most rá egy pillantást hello adatok azt töltött be a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="c1170-228">Let's have a look at hello data we have loaded into our experiment.</span></span> <span data-ttu-id="c1170-229">Hello kísérletben, kattintson a hello hello kimenete **cadairydata.csv dataset** válassza **megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="c1170-229">In hello experiment, click on hello output of hello **cadairydata.csv dataset** and select **visualize**.</span></span> <span data-ttu-id="c1170-230">4. ábra hasonlót meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="c1170-230">You should see something like Figure 4.</span></span>  

![Hello cadairydata.csv dataset összegzése][4]

<span data-ttu-id="c1170-232">*4. ábra. Hello cadairydata.csv adatkészlet összegzését.*</span><span class="sxs-lookup"><span data-stu-id="c1170-232">*Figure 4. Summary of hello cadairydata.csv dataset.*</span></span>

<span data-ttu-id="c1170-233">Ebben a nézetben látható nagy mennyiségű hasznos információkat.</span><span class="sxs-lookup"><span data-stu-id="c1170-233">In this view we see a lot of useful information.</span></span> <span data-ttu-id="c1170-234">Láthatja, hogy a dataset első több sornyi hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-234">We can see hello first several rows of that dataset.</span></span> <span data-ttu-id="c1170-235">Ha olyan oszlopot válasszon ki, hogy a hello statisztika szakasz hello oszlop további információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-235">If we select a column, hello Statistics section shows more information about hello column.</span></span> <span data-ttu-id="c1170-236">Például hello szolgáltatástípus sor látható az Azure Machine Learning Studio hozzárendelt toohello oszlop milyen adatokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-236">For example, hello Feature Type row shows us what data types Azure Machine Learning Studio assigned toohello column.</span></span> <span data-ttu-id="c1170-237">Ehhez hasonló gyorsan át, akkor az a jó megerősítést ellenőrzés előtt súlyos munka toodo először.</span><span class="sxs-lookup"><span data-stu-id="c1170-237">Having a quick look like this is a good sanity check before we start toodo any serious work.</span></span>

### <a name="first-r-script"></a><span data-ttu-id="c1170-238">Első R-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c1170-238">First R script</span></span>
<span data-ttu-id="c1170-239">Az Azure Machine Learning Studióban hozzon létre egy egyszerű első R parancsfájl tooexperiment együtt.</span><span class="sxs-lookup"><span data-stu-id="c1170-239">Let's create a simple first R script tooexperiment with in Azure Machine Learning Studio.</span></span> <span data-ttu-id="c1170-240">Létrehozta és tesztelte hello Rstudióból-parancsfájl a következő.</span><span class="sxs-lookup"><span data-stu-id="c1170-240">I have created and tested hello following script in RStudio.</span></span>  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="c1170-241">Most kell tootransfer a parancsfájl tooAzure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="c1170-241">Now I need tootransfer this script tooAzure Machine Learning Studio.</span></span> <span data-ttu-id="c1170-242">Sikerült egyszerűen Kivágás, majd illessze be.</span><span class="sxs-lookup"><span data-stu-id="c1170-242">I could simply cut and paste.</span></span> <span data-ttu-id="c1170-243">Azonban ebben az esetben fogja átvinni az R-parancsfájl egy zip-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="c1170-243">However, in this case, I will transfer my R script via a zip file.</span></span>

### <a name="data-input-toohello-execute-r-script-module"></a><span data-ttu-id="c1170-244">Adatok bemeneti toohello R-parancsfájl végrehajtása modul</span><span class="sxs-lookup"><span data-stu-id="c1170-244">Data input toohello Execute R Script module</span></span>
<span data-ttu-id="c1170-245">Most rendelkezik egy pillantást hello bemenetek toohello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-245">Let's have a look at hello inputs toohello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-246">Ebben a példában a Microsoft hello kaliforniai tejelő adatokat olvassa be hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-246">In this example we will read hello California dairy data into hello [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="c1170-247">Nincsenek hello három lehetséges bemeneteit [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-247">There are three possible inputs for hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-248">Bármely egyikét vagy mindegyikét a bemenetek, attól függően, hogy az alkalmazás használhat.</span><span class="sxs-lookup"><span data-stu-id="c1170-248">You may use any one or all of these inputs, depending on your application.</span></span> <span data-ttu-id="c1170-249">Akkor is, amelyek egyáltalán nem bemenetből fogad adatokat tökéletesen ésszerű toouse az R-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c1170-249">It is also perfectly reasonable toouse an R script that takes no input at all.</span></span>  

<span data-ttu-id="c1170-250">Vizsgáljuk meg mindegyik származó, a bal oldali tooright is.</span><span class="sxs-lookup"><span data-stu-id="c1170-250">Let's look at each of these inputs, going from left tooright.</span></span> <span data-ttu-id="c1170-251">Az egyes hello bemenetek hello nevek helyezi el a kurzor fölé hello bemeneti és hello elemleírás tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-251">You can see hello names of each of hello inputs by placing your cursor over hello input and reading hello tooltip.</span></span>  

#### <a name="script-bundle"></a><span data-ttu-id="c1170-252">Parancsfájl-csomag</span><span class="sxs-lookup"><span data-stu-id="c1170-252">Script Bundle</span></span>
<span data-ttu-id="c1170-253">hello parancsfájl köteg bemeneti lehetővé teszi a toopass hello tartalmát egy zip-fájlba történő [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-253">hello Script Bundle input allows you toopass hello contents of a zip file into [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-254">Hello parancsok tooread hello hello zip-fájl tartalmát az R-kód a következő egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="c1170-254">You can use one of hello following commands tooread hello contents of hello zip file into your R code.</span></span>

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> <span data-ttu-id="c1170-255">Az Azure Machine Learning kezeli a hello zip fájlt, ha azok hello src / könyvtár, ezért meg kell, hogy a fájl nevét, a könyvtárnév tooprefix.</span><span class="sxs-lookup"><span data-stu-id="c1170-255">Azure Machine Learning treats files in hello zip as if they are in hello src/ directory, so you need tooprefix your file names with this directory name.</span></span> <span data-ttu-id="c1170-256">Például ha hello zip hello fájlokat tartalmaz `yourfile.R` és `yourData.rdata` hello legfelső szintű hello zip, meg kellene cím ezek `src/yourfile.R` és `src/yourData.rdata` használatakor `source` és `load`.</span><span class="sxs-lookup"><span data-stu-id="c1170-256">For example, if hello zip contains hello files `yourfile.R` and `yourData.rdata` in hello root of hello zip, you would address these as `src/yourfile.R` and `src/yourData.rdata` when using `source` and `load`.</span></span>
> 
> 

<span data-ttu-id="c1170-257">Ismertettük már adathalmaz betöltése [hello adatkészlet betöltése](#loading).</span><span class="sxs-lookup"><span data-stu-id="c1170-257">We already discussed loading datasets in [Loading hello dataset](#loading).</span></span> <span data-ttu-id="c1170-258">Miután létrehozta és tesztelte hello előző részben hello R-parancsfájl, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="c1170-258">Once you have created and tested hello R script shown in hello previous section, do hello following:</span></span>

1. <span data-ttu-id="c1170-259">Mentse a hello R parancsfájlt a. R-fájl.</span><span class="sxs-lookup"><span data-stu-id="c1170-259">Save hello R script into a .R file.</span></span> <span data-ttu-id="c1170-260">A parancsfájl "simpleplot jelentkezem. R".</span><span class="sxs-lookup"><span data-stu-id="c1170-260">I call my script file "simpleplot.R".</span></span> <span data-ttu-id="c1170-261">Az alábbiakban hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="c1170-261">Here's hello contents.</span></span>
   
        ## Only one of hello following two lines should be used
        ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
        ## If in RStudio, use hello second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## hello following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. <span data-ttu-id="c1170-262">Hozzon létre egy zip fájlt, és másolja a parancsfájlt a zip-fájlba.</span><span class="sxs-lookup"><span data-stu-id="c1170-262">Create a zip file and copy your script into this zip file.</span></span> <span data-ttu-id="c1170-263">A Windows hello fájl gombbal és válassza **küldése**, majd **tömörített mappa**.</span><span class="sxs-lookup"><span data-stu-id="c1170-263">On Windows, you can right-click on hello file and select **Send to**, and then **Compressed folder**.</span></span> <span data-ttu-id="c1170-264">Ezzel létrehoz egy új zip-fájlt tartalmazó hello "simpleplot. R"fájl.</span><span class="sxs-lookup"><span data-stu-id="c1170-264">This will create a new zip file containing hello "simpleplot.R" file.</span></span>
3. <span data-ttu-id="c1170-265">Adja hozzá a fájl toohello **adatkészletek** a Machine Learning Studióban hello típus megadása **zip**.</span><span class="sxs-lookup"><span data-stu-id="c1170-265">Add your file toohello **datasets** in Machine Learning Studio, specifying hello type as **zip**.</span></span> <span data-ttu-id="c1170-266">Ekkor megjelenik az adatkészletek hello zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="c1170-266">You should now see hello zip file in your datasets.</span></span>
4. <span data-ttu-id="c1170-267">A fogd és vidd hello zip-fájl a **adatkészletek** alakzatot hello **ML Studio vászonra**.</span><span class="sxs-lookup"><span data-stu-id="c1170-267">Drag and drop hello zip file from **datasets** onto hello **ML Studio canvas**.</span></span>
5. <span data-ttu-id="c1170-268">Csatlakozás hello hello kimenete **zip-adatok** ikon toohello **parancsfájl köteg** hello a bemeneti [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-268">Connect hello output of hello **zip data** icon toohello **Script Bundle** input of hello [Execute R Script][execute-r-script] module.</span></span>
6. <span data-ttu-id="c1170-269">Típus hello `source()` függvény a zip fájlt hello kód ablakba a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-269">Type hello `source()` function with your zip file name into hello code window for hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-270">Abban az esetben, ha a beírt `source("src/simpleplot.R")`.</span><span class="sxs-lookup"><span data-stu-id="c1170-270">In my case I typed `source("src/simpleplot.R")`.</span></span>  
7. <span data-ttu-id="c1170-271">Ellenőrizze, hogy **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c1170-271">Make sure you click **Save**.</span></span>

<span data-ttu-id="c1170-272">Ha ezeket a lépéseket befejeződött, hello [R-parancsfájl végrehajtása] [ execute-r-script] modul végrehajtja a hello R-parancsfájl hello zip-fájlban szereplő hello kísérlet futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="c1170-272">Once these steps are complete, hello [Execute R Script][execute-r-script] module will execute hello R script in hello zip file when hello experiment is run.</span></span> <span data-ttu-id="c1170-273">Ezen a ponton a kísérletben hasonlóan kell kinéznie 5. ábra.</span><span class="sxs-lookup"><span data-stu-id="c1170-273">At this point your experiment should look something like Figure 5.</span></span>

![Kísérlet zip R-parancsfájl használatával][6]

<span data-ttu-id="c1170-275">*5. ábra. A kísérletben zip R-parancsfájl használatával.*</span><span class="sxs-lookup"><span data-stu-id="c1170-275">*Figure 5. Experiment using zipped R script.*</span></span>

#### <a name="dataset1"></a><span data-ttu-id="c1170-276">Dataset1</span><span class="sxs-lookup"><span data-stu-id="c1170-276">Dataset1</span></span>
<span data-ttu-id="c1170-277">Adatok tooyour R kód téglalap alakú táblázatát átadhatók hello Dataset1 bemeneti használatával.</span><span class="sxs-lookup"><span data-stu-id="c1170-277">You can pass a rectangular table of data tooyour R code by using hello Dataset1 input.</span></span> <span data-ttu-id="c1170-278">Az egyszerű parancsprogram hello a `maml.mapInputPort(1)` függvény hello adatokat olvas %1 porton.</span><span class="sxs-lookup"><span data-stu-id="c1170-278">In our simple script hello `maml.mapInputPort(1)` function reads hello data from port 1.</span></span> <span data-ttu-id="c1170-279">Ezeket az adatokat, majd hozzá van rendelve a kódban tooa dataframe változónév.</span><span class="sxs-lookup"><span data-stu-id="c1170-279">This data is then assigned tooa dataframe variable name in your code.</span></span> <span data-ttu-id="c1170-280">Az egyszerű parancsfájlban hello első kódsort hello hozzárendelés hajt végre.</span><span class="sxs-lookup"><span data-stu-id="c1170-280">In our simple script hello first line of code performs hello assignment.</span></span>

    cadairydata <- maml.mapInputPort(1)

<span data-ttu-id="c1170-281">Hajtsa végre a kísérlet hello kattintva **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1170-281">Execute your experiment by clicking on hello **Run** button.</span></span> <span data-ttu-id="c1170-282">Hello végrehajtásának befejezése után kattintson a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, és kattintson **kimeneti napló megtekintése** hello tulajdonságai panelen.</span><span class="sxs-lookup"><span data-stu-id="c1170-282">When hello execution finishes, click on hello [Execute R Script][execute-r-script] module and then click **View output log** on hello properties pane.</span></span> <span data-ttu-id="c1170-283">Egy új lapot ábrázoló hello hello kimenetét fájl tartalmát a böngésző meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="c1170-283">A new page should appear in your browser showing hello contents of hello output.log file.</span></span> <span data-ttu-id="c1170-284">Amikor görgessen lefelé megtekintheti az alábbihoz hasonló hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-284">When you scroll down you should see something like hello following.</span></span>

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

<span data-ttu-id="c1170-285">Lefelé hello távolabb a lap hello oszlopokat, amelyek a következőhöz hello következő hasonlóan fog kinézni a további tájékoztatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c1170-285">Farther down hello page is more detailed information on hello columns, which will look something like hello following.</span></span>

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

<span data-ttu-id="c1170-286">Ezekkel az eredményekkel többnyire 228 megfigyelések és hello dataframe 9 oszlopai várt módon vannak.</span><span class="sxs-lookup"><span data-stu-id="c1170-286">These results are mostly as expected, with 228 observations and 9 columns in hello dataframe.</span></span> <span data-ttu-id="c1170-287">Hello oszlopnevek, hello R adattípus és egy minta oszlopok láthatja.</span><span class="sxs-lookup"><span data-stu-id="c1170-287">We can see hello column names, hello R data type and a sample of each column.</span></span>

> [!NOTE]
> <span data-ttu-id="c1170-288">Az azonos nyomtatásra érhető kényelmesen hello hello R eszköz kimenete [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-288">This same printed output is conveniently available from hello R Device output of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-289">Hello hello kimenetének ismertetik [R-parancsfájl végrehajtása] [ execute-r-script] modul hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="c1170-289">We will discuss hello outputs of hello [Execute R Script][execute-r-script] module in hello next section.</span></span>  
> 
> 

#### <a name="dataset2"></a><span data-ttu-id="c1170-290">Dataset2</span><span class="sxs-lookup"><span data-stu-id="c1170-290">Dataset2</span></span>
<span data-ttu-id="c1170-291">hello hello Dataset2 bemeneti működése azonos toothat a Dataset1.</span><span class="sxs-lookup"><span data-stu-id="c1170-291">hello behavior of hello Dataset2 input is identical toothat of Dataset1.</span></span> <span data-ttu-id="c1170-292">Használja a bemeneti átviheti az adatok egy második téglalap alakú tábla az R-kódba.</span><span class="sxs-lookup"><span data-stu-id="c1170-292">Using this input you can pass a second rectangular table of data into your R code.</span></span> <span data-ttu-id="c1170-293">hello függvény `maml.mapInputPort(2)`, 2 hello argumentummal van használt toopass ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-293">hello function `maml.mapInputPort(2)`, with hello argument 2, is used toopass this data.</span></span>  

### <a name="execute-r-script-outputs"></a><span data-ttu-id="c1170-294">R-parancsfájl kimenetek végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c1170-294">Execute R Script outputs</span></span>
#### <a name="output-a-dataframe"></a><span data-ttu-id="c1170-295">Egy dataframe kimeneti</span><span class="sxs-lookup"><span data-stu-id="c1170-295">Output a dataframe</span></span>
<span data-ttu-id="c1170-296">A kimenetre küldheti hello tartalmát egy R dataframe hello eredmény Dataset1 porton keresztül téglalap alakú táblázatként hello segítségével `maml.mapOutputPort()` függvény.</span><span class="sxs-lookup"><span data-stu-id="c1170-296">You can output hello contents of an R dataframe as a rectangular table through hello Result Dataset1 port by using hello `maml.mapOutputPort()` function.</span></span> <span data-ttu-id="c1170-297">Az egyszerű R-parancsfájl ez hajtható végre a következő sor hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-297">In our simple R script this is performed by hello following line.</span></span>

    maml.mapOutputPort('cadairydata')

<span data-ttu-id="c1170-298">Futó hello kísérletet követően kattintson a hello eredmény Dataset1 kimeneti portra, és kattintson a **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="c1170-298">After running hello experiment, click on hello Result Dataset1 output port and then click on **Visualize**.</span></span> <span data-ttu-id="c1170-299">6. ábra hasonlót meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="c1170-299">You should see something like Figure 6.</span></span>

![hello képi megjelenítés hello kibocsátásának hello kaliforniai tejelő adatok][7]

<span data-ttu-id="c1170-301">*6. ábra. hello képi megjelenítés hello kaliforniai tejelő adatok hello kimenetét.*</span><span class="sxs-lookup"><span data-stu-id="c1170-301">*Figure 6. hello visualization of hello output of hello California dairy data.*</span></span>

<span data-ttu-id="c1170-302">A kimeneti azonos toohello bemeneti keres, pontosan úgy várt.</span><span class="sxs-lookup"><span data-stu-id="c1170-302">This output looks identical toohello input, exactly as we expected.</span></span>  

### <a name="r-device-output"></a><span data-ttu-id="c1170-303">R eszköz kimeneti</span><span class="sxs-lookup"><span data-stu-id="c1170-303">R Device output</span></span>
<span data-ttu-id="c1170-304">Eszköz kimenete hello hello [R-parancsfájl végrehajtása] [ execute-r-script] modul üzenetek és grafikus kimeneti tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-304">hello Device output of hello [Execute R Script][execute-r-script] module contains messages and graphics output.</span></span> <span data-ttu-id="c1170-305">Mindkét standard kimenet és a standard hiba R üzenetei toohello R eszköz kimeneti portjával.</span><span class="sxs-lookup"><span data-stu-id="c1170-305">Both standard output and standard error messages from R are sent toohello R Device output port.</span></span>  

<span data-ttu-id="c1170-306">tooview hello R eszköz kimeneti, kattintson a hello porton, majd a **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="c1170-306">tooview hello R Device output, click on hello port and then on **Visualize**.</span></span> <span data-ttu-id="c1170-307">Hello standard kimenet és a standard hiba a hello R-parancsfájl a 7. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-307">We see hello standard output and standard error from hello R script in Figure 7.</span></span>

![Standard kimenet és a standard hiba a hello R eszköz port][8]

<span data-ttu-id="c1170-309">*7. ábra. Standard kimenet és a standard hiba a hello R eszköz port.*</span><span class="sxs-lookup"><span data-stu-id="c1170-309">*Figure 7. Standard output and standard error from hello R Device port.*</span></span>

<span data-ttu-id="c1170-310">Azt lefelé görgetéshez használható tekintse meg a 8. ábrán az R-parancsfájl kimenete hello grafikus.</span><span class="sxs-lookup"><span data-stu-id="c1170-310">Scrolling down we see hello graphics output from our R script in Figure 8.</span></span>  

![Hello R eszköz port grafikus kimenete][9]

<span data-ttu-id="c1170-312">*8. ábra. A grafikus hello R eszköz port kimenetét.*</span><span class="sxs-lookup"><span data-stu-id="c1170-312">*Figure 8. Graphics output from hello R Device port.*</span></span>  

## <span data-ttu-id="c1170-313"><a id="filtering"></a>Adatok szűrése és átalakítás</span><span class="sxs-lookup"><span data-stu-id="c1170-313"><a id="filtering"></a>Data filtering and transformation</span></span>
<span data-ttu-id="c1170-314">Ebben a szakaszban végezzük el a néhány alapvető adatok szűrése és a hello kaliforniai tejelő adatok átalakítási műveletek.</span><span class="sxs-lookup"><span data-stu-id="c1170-314">In this section we will perform some basic data filtering and transformation operations on hello California dairy data.</span></span> <span data-ttu-id="c1170-315">Ez a szakasz hello végéig azt van adatok formátuma nem megfelelő az elemzési modell készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c1170-315">By hello end of this section we will have data in a format suitable for building an analytic model.</span></span>  

<span data-ttu-id="c1170-316">Pontosabban, ez a szakasz azt feladatokat kell elvégezni több közös adatok tisztítása és átalakítás: Adja meg a szűrést dataframes, hozzáadása új számított oszlop, az átalakítási és átalakítások értéket.</span><span class="sxs-lookup"><span data-stu-id="c1170-316">More specifically, in this section we will perform several common data cleaning and transformation tasks: type transformation, filtering on dataframes, adding new computed columns, and value transformations.</span></span> <span data-ttu-id="c1170-317">A háttérben segítséget valós problémákat észlelt számos változata hello foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="c1170-317">This background should help you deal with hello many variations encountered in real-world problems.</span></span>

<span data-ttu-id="c1170-318">Ez a szakasz hello teljes R kód hello korábban letöltött zip-fájlban szereplő érhető el.</span><span class="sxs-lookup"><span data-stu-id="c1170-318">hello complete R code for this section is available in hello zip file you downloaded earlier.</span></span>

### <a name="type-transformations"></a><span data-ttu-id="c1170-319">Típus átalakítások</span><span class="sxs-lookup"><span data-stu-id="c1170-319">Type transformations</span></span>
<span data-ttu-id="c1170-320">Most, hogy a Microsoft hello kaliforniai tejelő adatok elolvashatják a hello R kódra a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, igazolnia kell, hogy hello adatok hello oszlopban rendelkezik szánt hello típusát és formátumát tooensure.</span><span class="sxs-lookup"><span data-stu-id="c1170-320">Now that we can read hello California dairy data into hello R code in hello [Execute R Script][execute-r-script] module, we need tooensure that hello data in hello columns has hello intended type and format.</span></span>  

<span data-ttu-id="c1170-321">R dinamikusan gépelt nyelven, ami azt jelenti, hogy az adattípusokat egy tooanother szükség szerint a rendszer kényszerített.</span><span class="sxs-lookup"><span data-stu-id="c1170-321">R is a dynamically typed language, which means that data types are coerced from one tooanother as required.</span></span> <span data-ttu-id="c1170-322">atomi adattípusok hello R numerikus, logikai és karaktert tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1170-322">hello atomic data types in R include numeric, logical and character.</span></span> <span data-ttu-id="c1170-323">hello tényező típus használt toocompactly kategorikus adatok tárolása.</span><span class="sxs-lookup"><span data-stu-id="c1170-323">hello factor type is used toocompactly store categorical data.</span></span> <span data-ttu-id="c1170-324">Adattípusok sokkal további információk találhatók hello hivatkozások [B függelék – további információ](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="c1170-324">You can find much more information on data types in hello references in [Appendix B - Further reading](#appendixb).</span></span>

<span data-ttu-id="c1170-325">Táblázatos adatolvasás a külső forrásból származó R, esetén mindig egy jó ötlet toocheck hello származó típusok hello oszlopok.</span><span class="sxs-lookup"><span data-stu-id="c1170-325">When tabular data is read into R from an external source, it is always a good idea toocheck hello resulting types in hello columns.</span></span> <span data-ttu-id="c1170-326">Érdemes lehet egy oszlophoz Típusjelző karakter, de sok esetben ez fog megjelenni tényezőként vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="c1170-326">You may want a column of type character, but in many cases this will show up as factor or vice versa.</span></span> <span data-ttu-id="c1170-327">Más esetekben egy oszlopot úgy gondolja, hogy legyen numerikus által képviselt karakter adatok, például az 1,23, lebegőpontos helyett a "1,23" pont számát.</span><span class="sxs-lookup"><span data-stu-id="c1170-327">In other cases a column you think should be numeric is represented by character data, e.g. '1.23' rather than 1.23 as a floating point number.</span></span>  

<span data-ttu-id="c1170-328">Szerencsére is könnyen tooconvert egy típus tooanother, mindaddig, amíg lesz lehetséges.</span><span class="sxs-lookup"><span data-stu-id="c1170-328">Fortunately, it is easy tooconvert one type tooanother, as long as mapping is possible.</span></span> <span data-ttu-id="c1170-329">Például "Nevada" konvertálása nem egy numerikus érték, de akkor átalakíthatja tooa tényező (kategorikus változó).</span><span class="sxs-lookup"><span data-stu-id="c1170-329">For example, you cannot convert 'Nevada' into a numeric value, but you can convert it tooa factor (categorical variable).</span></span> <span data-ttu-id="c1170-330">Másik példaként egy numerikus 1 átalakíthatja a következő karaktert: "1" vagy egy tényező.</span><span class="sxs-lookup"><span data-stu-id="c1170-330">As another example, you can convert a numeric 1 into a character '1' or a factor.</span></span>  

<span data-ttu-id="c1170-331">hello ezek az átalakítások bármelyikét szintaxisa a következő egyszerű: `as.datatype()`.</span><span class="sxs-lookup"><span data-stu-id="c1170-331">hello syntax for any of these conversions is simple: `as.datatype()`.</span></span> <span data-ttu-id="c1170-332">A típuskonverziós hello következők.</span><span class="sxs-lookup"><span data-stu-id="c1170-332">These type conversion functions include hello following.</span></span>

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

<span data-ttu-id="c1170-333">Azt az előző szakaszban hello bemeneti hello oszlopok adattípusai hello megnézi: az összes oszlop sem típusú numerikus, "Month", amelynek a típuskarakter feliratú hello oszlopok kivételével.</span><span class="sxs-lookup"><span data-stu-id="c1170-333">Looking at hello data types of hello columns we input in hello previous section: all columns are of type numeric, except for hello column labeled 'Month', which is of type character.</span></span> <span data-ttu-id="c1170-334">Most a tooa tényező átalakítani, és hello teszteredmények.</span><span class="sxs-lookup"><span data-stu-id="c1170-334">Let's convert this tooa factor and test hello results.</span></span>  

<span data-ttu-id="c1170-335">Hello scatterplot mátrix létrehozza és hozzáadja egy sor hello "Month" oszlop tooa tényező konvertálása hello sor rendelkezik törölve</span><span class="sxs-lookup"><span data-stu-id="c1170-335">I have deleted hello line that created hello scatterplot matrix and added a line converting hello 'Month' column tooa factor.</span></span> <span data-ttu-id="c1170-336">A kísérletemben I fog csak kivágása és hello R-kód beillesztése hello kód ablakának hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-336">In my experiment I will just cut and paste hello R code into hello code window of hello [Execute R Script][execute-r-script] Module.</span></span> <span data-ttu-id="c1170-337">Is képes hello zip-fájl frissítése, és töltse fel a Machine Learning Studio tooAzure, de ez számos lépést időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c1170-337">You could also update hello zip file and upload it tooAzure Machine Learning Studio, but this takes several steps.</span></span>  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check hello result
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="c1170-338">Most futtassa ezt a kódot, és tekintse meg az R-parancsfájl hello hello kimeneti naplót.</span><span class="sxs-lookup"><span data-stu-id="c1170-338">Let's execute this code and look at hello output log for hello R script.</span></span> <span data-ttu-id="c1170-339">hello vonatkozó adatokat hello naplóból 9. ábra jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-339">hello relevant data from hello log is shown in Figure 9.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="c1170-340">*9. ábra. Hello dataframe tényező változóhoz összegzését.*</span><span class="sxs-lookup"><span data-stu-id="c1170-340">*Figure 9. Summary of hello dataframe with a factor variable.*</span></span>

<span data-ttu-id="c1170-341">hello típus hónaphoz most üzenetnek kell megjelennie "**keresztüli 14 szintek rendelkező tényező**".</span><span class="sxs-lookup"><span data-stu-id="c1170-341">hello type for Month should now say '**Factor w/ 14 levels**'.</span></span> <span data-ttu-id="c1170-342">Ez az probléma, mivel nincsenek hello évben csak 12 hónapig.</span><span class="sxs-lookup"><span data-stu-id="c1170-342">This is a problem since there are only 12 months in hello year.</span></span> <span data-ttu-id="c1170-343">Ellenőrizheti, hogy a típus hello toosee a **Visualize** hello eredmény Dataset port az "**Categorical**".</span><span class="sxs-lookup"><span data-stu-id="c1170-343">You can also check toosee that hello type in **Visualize** of hello Result Dataset port is '**Categorical**'.</span></span>

<span data-ttu-id="c1170-344">hello probléma az, hogy hello "Month" oszlop nem lett a kódolt rendszeresen.</span><span class="sxs-lookup"><span data-stu-id="c1170-344">hello problem is that hello 'Month' column has not been coded systematically.</span></span> <span data-ttu-id="c1170-345">Bizonyos esetekben egy hónap. április nevezik, és más, ápr rövidítéseket tartalmaz. A Microsoft hello karakterlánc too3 karakterek segítségével megoldhatja a problémát.</span><span class="sxs-lookup"><span data-stu-id="c1170-345">In some cases a month is called April and in others it is abbreviated as Apr. We can solve this problem by trimming hello string too3 characters.</span></span> <span data-ttu-id="c1170-346">hello kódsort most következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="c1170-346">hello line of code now looks like hello following:</span></span>

    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

<span data-ttu-id="c1170-347">Futtassa újra a hello kísérletet, és tekintse meg hello kimeneti naplót.</span><span class="sxs-lookup"><span data-stu-id="c1170-347">Rerun hello experiment and view hello output log.</span></span> <span data-ttu-id="c1170-348">hello várt eredmény 10. ábrán láthatók.</span><span class="sxs-lookup"><span data-stu-id="c1170-348">hello expected results are shown in Figure 10.</span></span>  

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="c1170-349">*10. ábra. Megfelelő számú tényező szintek hello dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="c1170-349">*Figure 10. Summary of hello dataframe with correct number of factor levels.*</span></span>

<span data-ttu-id="c1170-350">A tényező változó most már rendelkezik a szükséges hello 12 szintek.</span><span class="sxs-lookup"><span data-stu-id="c1170-350">Our factor variable now has hello desired 12 levels.</span></span>

### <a name="basic-data-frame-filtering"></a><span data-ttu-id="c1170-351">Alapvető adatok keret szűrése</span><span class="sxs-lookup"><span data-stu-id="c1170-351">Basic data frame filtering</span></span>
<span data-ttu-id="c1170-352">R dataframes hatékony szűrési lehetőségeket nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="c1170-352">R dataframes support powerful filtering capabilities.</span></span> <span data-ttu-id="c1170-353">Adatkészletek sorok vagy oszlopok logikai szűrők használatával lehet részlegesen beágyazott.</span><span class="sxs-lookup"><span data-stu-id="c1170-353">Datasets can be subsetted by using logical filters on either rows or columns.</span></span> <span data-ttu-id="c1170-354">Sok esetben összetett szűrési feltételeket lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="c1170-354">In many cases, complex filter criteria will be required.</span></span> <span data-ttu-id="c1170-355">hello sorszámra [B függelék – további információ](#appendixb) dataframes szűrés kiterjedt példái tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1170-355">hello references in [Appendix B - Further reading](#appendixb) contain extensive examples of filtering dataframes.</span></span>  

<span data-ttu-id="c1170-356">Van egy bittel szűrése az adathalmaz azt kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="c1170-356">There is one bit of filtering we should do on our dataset.</span></span> <span data-ttu-id="c1170-357">Hello cadairydata dataframe hello oszlopai tekinti meg, két felesleges oszlopok látják.</span><span class="sxs-lookup"><span data-stu-id="c1170-357">If you look at hello columns in hello cadairydata dataframe, you will see two unnecessary columns.</span></span> <span data-ttu-id="c1170-358">hello első oszlop csak egy sor számát, amely nem nagyon hasznos tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-358">hello first column just holds a row number, which is not very useful.</span></span> <span data-ttu-id="c1170-359">hello második oszlopban Year.Month, redundáns információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1170-359">hello second column, Year.Month, contains redundant information.</span></span> <span data-ttu-id="c1170-360">Azt is könnyen használatával zárhatja ki ezeket az oszlopokat a következő R-kód hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-360">We can easily exclude these columns by using hello following R code.</span></span>

> [!NOTE]
> <span data-ttu-id="c1170-361">A most az ebben a szakaszban I ugyanúgy jelennek meg hello hello a hozzáadott kód [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-361">From now on in this section, I will just show you hello additional code I am adding in hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="c1170-362">Minden új sor veszek fel **előtt** hello `str()` függvény.</span><span class="sxs-lookup"><span data-stu-id="c1170-362">I will add each new line **before** hello `str()` function.</span></span> <span data-ttu-id="c1170-363">Használhatom a függvény tooverify az eredmények az Azure Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="c1170-363">I use this function tooverify my results in Azure Machine Learning Studio.</span></span>
> 
> 

<span data-ttu-id="c1170-364">A következő sor toomy R kód hello hello hozzáadok [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-364">I add hello following line toomy R code in hello [Execute R Script][execute-r-script] module.</span></span>

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

<span data-ttu-id="c1170-365">Futtassa a következő kódot a kísérletben, és ellenőrizze a hello kimeneti naplóból hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="c1170-365">Run this code in your experiment and check hello result from hello output log.</span></span> <span data-ttu-id="c1170-366">Ezekkel az eredményekkel 11. ábra mutatja be.</span><span class="sxs-lookup"><span data-stu-id="c1170-366">These results are shown in Figure 11.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="c1170-367">*11. ábra. A két oszlop hello dataframe összefoglalása eltávolítani.*</span><span class="sxs-lookup"><span data-stu-id="c1170-367">*Figure 11. Summary of hello dataframe with two columns removed.*</span></span>

<span data-ttu-id="c1170-368">Jó hírünk van!</span><span class="sxs-lookup"><span data-stu-id="c1170-368">Good news!</span></span> <span data-ttu-id="c1170-369">A Microsoft hello várt eredmény érhető el.</span><span class="sxs-lookup"><span data-stu-id="c1170-369">We get hello expected results.</span></span>

### <a name="add-a-new-column"></a><span data-ttu-id="c1170-370">Egy olyan új oszlop hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c1170-370">Add a new column</span></span>
<span data-ttu-id="c1170-371">toocreate idő adatsorozat modellek lesz kényelmes toohave egy oszlopot tartalmazó hello hónap hello idősorozat hello indítása óta.</span><span class="sxs-lookup"><span data-stu-id="c1170-371">toocreate time series models it will be convenient toohave a column containing hello months since hello start of hello time series.</span></span> <span data-ttu-id="c1170-372">Létrehozunk egy olyan új oszlop "Month.Count".</span><span class="sxs-lookup"><span data-stu-id="c1170-372">We will create a new column 'Month.Count'.</span></span>

<span data-ttu-id="c1170-373">toohelp rendszerezése létre fogunk hozni az első egyszerű függvény hello kód `num.month()`.</span><span class="sxs-lookup"><span data-stu-id="c1170-373">toohelp organize hello code we will create our first simple function, `num.month()`.</span></span> <span data-ttu-id="c1170-374">Fog majd érvénybe lépni a függvény toocreate hello dataframe új oszlopát.</span><span class="sxs-lookup"><span data-stu-id="c1170-374">We will then apply this function toocreate a new column in hello dataframe.</span></span> <span data-ttu-id="c1170-375">hello új kódot a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="c1170-375">hello new code is as follows.</span></span>

    ## Create a new column with hello month count
    ## Function toofind hello number of months from hello first
    ## month of hello time series
    num.month <- function(Year, Month) {
      ## Find hello starting year
      min.year  <- min(Year)

      ## Compute hello number of months from hello start of hello time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute hello new column for hello dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

<span data-ttu-id="c1170-376">Most frissíteni hello kísérlet futtatása, és hello kimeneti napló tooview hello eredmények használja.</span><span class="sxs-lookup"><span data-stu-id="c1170-376">Now run hello updated experiment and use hello output log tooview hello results.</span></span> <span data-ttu-id="c1170-377">Ezekkel az eredményekkel 12. ábra mutatja be.</span><span class="sxs-lookup"><span data-stu-id="c1170-377">These results are shown in Figure 12.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="c1170-378">*12. ábra. Hello dataframe hello további oszlop összegzése.*</span><span class="sxs-lookup"><span data-stu-id="c1170-378">*Figure 12. Summary of hello dataframe with hello additional column.*</span></span>

<span data-ttu-id="c1170-379">Úgy tűnik, minden működik.</span><span class="sxs-lookup"><span data-stu-id="c1170-379">It looks like everything is working.</span></span> <span data-ttu-id="c1170-380">Új oszlop hello hello várt értékei a dataframe a van.</span><span class="sxs-lookup"><span data-stu-id="c1170-380">We have hello new column with hello expected values in our dataframe.</span></span>

### <a name="value-transformations"></a><span data-ttu-id="c1170-381">Érték átalakítása</span><span class="sxs-lookup"><span data-stu-id="c1170-381">Value transformations</span></span>
<span data-ttu-id="c1170-382">Ebben a szakaszban végezzük el néhány egyszerű átalakítások hello értékek egyes a dataframe hello oszlopait.</span><span class="sxs-lookup"><span data-stu-id="c1170-382">In this section we will perform some simple transformations on hello values in some of hello columns of our dataframe.</span></span> <span data-ttu-id="c1170-383">hello R nyelv majdnem tetszőleges érték átalakítások támogatja.</span><span class="sxs-lookup"><span data-stu-id="c1170-383">hello R language supports nearly arbitrary value transformations.</span></span> <span data-ttu-id="c1170-384">hello sorszámra [B függelék – további olvasási](#appendixb) kiterjedt példákat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1170-384">hello references in [Appendix B - Further Reading](#appendixb) contain extensive examples.</span></span>

<span data-ttu-id="c1170-385">Ha megnézi a dataframe hello összegzéseinek hello értékeket kell valami páratlan itt.</span><span class="sxs-lookup"><span data-stu-id="c1170-385">If you look at hello values in hello summaries of our dataframe you should see something odd here.</span></span> <span data-ttu-id="c1170-386">Kaliforniai előállított tej-nál több jégkrémje?</span><span class="sxs-lookup"><span data-stu-id="c1170-386">Is more ice cream than milk produced in California?</span></span> <span data-ttu-id="c1170-387">Nem, természetesen ez nincs értelme, mivel EV, mint a tény lehet, hogy nem mindannyian toosome jégkrémje lovers.</span><span class="sxs-lookup"><span data-stu-id="c1170-387">No, of course not, as this makes no sense, sad as this fact may be toosome of us ice cream lovers.</span></span> <span data-ttu-id="c1170-388">hello egységek eltérnek.</span><span class="sxs-lookup"><span data-stu-id="c1170-388">hello units are different.</span></span> <span data-ttu-id="c1170-389">hello ár egységekbe, az font, tej van egységekbe, az 1 millió egység USA font, jégkrémje 1000 egységekbe gallon VELÜNK, pedig túró egységekbe, az USA 1000 font.</span><span class="sxs-lookup"><span data-stu-id="c1170-389">hello price is in units of US pounds, milk is in units of 1 M US pounds, ice cream is in units of 1,000 US gallons, and cottage cheese is in units of 1,000 US pounds.</span></span> <span data-ttu-id="c1170-390">Ha jégkrémje tömege / gallonra készül 6.5 font, azt is könnyen hello szorzás tooconvert ezeket az értékeket úgy, hogy azok mind 1000 font egyenlő egységekben.</span><span class="sxs-lookup"><span data-stu-id="c1170-390">Assuming ice cream weighs about 6.5 pounds per gallon, we can easily do hello multiplication tooconvert these values so they are all in equal units of 1,000 pounds.</span></span>

<span data-ttu-id="c1170-391">Az előrejelzési modell azt használjon tényezőt modellt a trendek és a határozza ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-391">For our forecasting model we use a multiplicative model for trend and seasonal adjustment of this data.</span></span> <span data-ttu-id="c1170-392">Napló-átalakítás lehetővé teszi egy lineáris modell, hogy az a folyamat toouse.</span><span class="sxs-lookup"><span data-stu-id="c1170-392">A log transformation allows us toouse a linear model, simplifying this process.</span></span> <span data-ttu-id="c1170-393">Azt is érvényesek lesznek hello napló átalakítása a hello olyan funkciókat, amikor a hello szorzója van érvényben.</span><span class="sxs-lookup"><span data-stu-id="c1170-393">We can apply hello log transformation in hello same function where hello multiplier is applied.</span></span>

<span data-ttu-id="c1170-394">A következő kód hello, I határozza meg egy új funkció `log.transform()`, és alkalmazza azt hello numerikus értékeket tartalmazó toohello sorok.</span><span class="sxs-lookup"><span data-stu-id="c1170-394">In hello following code, I define a new function, `log.transform()`, and apply it toohello rows containing hello numerical values.</span></span> <span data-ttu-id="c1170-395">hello R `Map()` függvény használt tooapply hello `log.transform()` függvény toohello kijelölt hello dataframe oszlopait.</span><span class="sxs-lookup"><span data-stu-id="c1170-395">hello R `Map()` function is used tooapply hello `log.transform()` function toohello selected columns of hello dataframe.</span></span> <span data-ttu-id="c1170-396">`Map()`túl hasonlít`apply()` , de lehetővé teszi, hogy a argumentumok toohello függvény egynél több listáját.</span><span class="sxs-lookup"><span data-stu-id="c1170-396">`Map()` is similar too`apply()` but allows for more than one list of arguments toohello function.</span></span> <span data-ttu-id="c1170-397">Vegye figyelembe, hogy tényezők listájának megadja az hello második argumentum toohello `log.transform()` függvény.</span><span class="sxs-lookup"><span data-stu-id="c1170-397">Note that a list of multipliers supplies hello second argument toohello `log.transform()` function.</span></span> <span data-ttu-id="c1170-398">Hello `na.omit()` kicsit tisztítás tooensure, nem találtunk hiányzik vagy nem definiált értékek hello dataframe függvény használatos.</span><span class="sxs-lookup"><span data-stu-id="c1170-398">hello `na.omit()` function is used as a bit of cleanup tooensure we do not have missing or undefined values in hello dataframe.</span></span>

    log.transform <- function(invec, multiplier = 1) {
      ## Function for hello transformation, which is hello log
      ## of hello input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments toofunction log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier toofuncition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check hello input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap hello multiplication in tryCatch
      ## If there is an exception, print hello warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply hello transformation function toohello 4 columns
    ## of hello dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

<span data-ttu-id="c1170-399">Nincs elég egy bit azonban a hello `log.transform()` függvény.</span><span class="sxs-lookup"><span data-stu-id="c1170-399">There is quite a bit happening in hello `log.transform()` function.</span></span> <span data-ttu-id="c1170-400">Ez a kód a legtöbb hello argumentumok kapcsolatos esetleges problémák keresése vagy kivételek, továbbra is hello számítások során felmerülő foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="c1170-400">Most of this code is checking for potential problems with hello arguments or dealing with exceptions, which can still arise during hello computations.</span></span> <span data-ttu-id="c1170-401">Ez a kód csak néhány sornyi ténylegesen hello számításokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-401">Only a few lines of this code actually do hello computations.</span></span>

<span data-ttu-id="c1170-402">hello hello defenzív programozás célja tooprevent hello sikertelen volt-e, amely megakadályozza a folyamatos feldolgozási egy funkcióval.</span><span class="sxs-lookup"><span data-stu-id="c1170-402">hello goal of hello defensive programming is tooprevent hello failure of a single function that prevents processing from continuing.</span></span> <span data-ttu-id="c1170-403">Lehet, hogy a hosszan futó elemzési hirtelen hibát igen kellemetlen, a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="c1170-403">An abrupt failure of a long-running analysis can be quite frustrating for users.</span></span> <span data-ttu-id="c1170-404">Ebben az esetben alapértelmezett visszatérési értékek ki kell választani, amely korlátozza az tooavoid toodownstream feldolgozási sérülést.</span><span class="sxs-lookup"><span data-stu-id="c1170-404">tooavoid this situation, default return values must be chosen that will limit damage toodownstream processing.</span></span> <span data-ttu-id="c1170-405">Egy üzenet egyben előállított tooalert felhasználók, amelynek valami hibás.</span><span class="sxs-lookup"><span data-stu-id="c1170-405">A message is also produced tooalert users that something has gone wrong.</span></span>

<span data-ttu-id="c1170-406">Ha még nem használt toodefensive programozásba a R, ez a kód egy kicsit túlságosan tűnhet.</span><span class="sxs-lookup"><span data-stu-id="c1170-406">If you are not used toodefensive programming in R, all this code may seem a bit overwhelming.</span></span> <span data-ttu-id="c1170-407">I haladhat végig hello fő lépést:</span><span class="sxs-lookup"><span data-stu-id="c1170-407">I will walk you through hello major steps:</span></span>

1. <span data-ttu-id="c1170-408">Négy üzenetek vektor van definiálva.</span><span class="sxs-lookup"><span data-stu-id="c1170-408">A vector of four messages is defined.</span></span> <span data-ttu-id="c1170-409">Ezek az üzenetek nem használt toocommunicate tájékoztatásra hello lehetséges hibákat és kivételeket, amik akkor léphetnek fel ezzel a kóddal.</span><span class="sxs-lookup"><span data-stu-id="c1170-409">These messages are used toocommunicate information about some of hello possible errors and exceptions that can occur with this code.</span></span>
2. <span data-ttu-id="c1170-410">NA érték minden egyes esetben vissza</span><span class="sxs-lookup"><span data-stu-id="c1170-410">I return a value of NA for each case.</span></span> <span data-ttu-id="c1170-411">Nincsenek sok egyéb lehetőségeket, amelyekkel kevesebb hatásai lehetnek.</span><span class="sxs-lookup"><span data-stu-id="c1170-411">There are many other possibilities that might have fewer side effects.</span></span> <span data-ttu-id="c1170-412">I visszaadhatja a vektoros nullából álló, illetve ha hello eredeti bemeneti vektor, például.</span><span class="sxs-lookup"><span data-stu-id="c1170-412">I could return a vector of zeroes, or hello original input vector, for example.</span></span>
3. <span data-ttu-id="c1170-413">A hello argumentumok toohello függvény ellenőrzéseket futtatja.</span><span class="sxs-lookup"><span data-stu-id="c1170-413">Checks are run on hello arguments toohello function.</span></span> <span data-ttu-id="c1170-414">Minden esetben a rendszer hibát észlel, ha ki egy alapértelmezett értéket adja vissza, és üzenet hello hozzák `warning()` függvény.</span><span class="sxs-lookup"><span data-stu-id="c1170-414">In each case, if an error is detected, a default value is returned and a message is produced by hello `warning()` function.</span></span> <span data-ttu-id="c1170-415">Használok `warning()` helyett `stop()` , ez utóbbi hello megszünteti a végrehajtási, pontosan milyen próbálok tooavoid.</span><span class="sxs-lookup"><span data-stu-id="c1170-415">I am using `warning()` rather than `stop()` as hello latter will terminate execution, exactly what I am trying tooavoid.</span></span> <span data-ttu-id="c1170-416">Vegye figyelembe, hogy I írt ezt a kódot eljárási Style, ahogy a gyorsítás esetében a rendszer valószínűleg összetett és homályos működési megközelítést.</span><span class="sxs-lookup"><span data-stu-id="c1170-416">Note that I have written this code in a procedural style, as in this case a functional approach seemed complex and obscure.</span></span>
4. <span data-ttu-id="c1170-417">hello napló számítások csomagolni vannak `tryCatch()` , hogy a kivételek nem okoz egy hirtelen halt tooprocessing.</span><span class="sxs-lookup"><span data-stu-id="c1170-417">hello log computations are wrapped in `tryCatch()` so that exceptions will not cause an abrupt halt tooprocessing.</span></span> <span data-ttu-id="c1170-418">Nélkül `tryCatch()` R funkciók eredményt stopjelzést, viszont csak, amelyek miatt a legtöbb hiba.</span><span class="sxs-lookup"><span data-stu-id="c1170-418">Without `tryCatch()` most errors raised by R functions result in a stop signal, which does just that.</span></span>

<span data-ttu-id="c1170-419">Az R-kód végrehajtása a kísérletben, és kinyomtatott hello egy pillantást kimeneti hello kimenetét fájlban.</span><span class="sxs-lookup"><span data-stu-id="c1170-419">Execute this R code in your experiment and have a look at hello printed output in hello output.log file.</span></span> <span data-ttu-id="c1170-420">Ekkor hello négy oszlopai naplózásához hello hello át legyenek-e értékeket is, ahogy az 13. ábra.</span><span class="sxs-lookup"><span data-stu-id="c1170-420">You will now see hello transformed values of hello four columns in hello log, as shown in Figure 13.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="c1170-421">*13. ábra. Hello összefoglalása hello dataframe szereplő értékek átalakítva.*</span><span class="sxs-lookup"><span data-stu-id="c1170-421">*Figure 13. Summary of hello transformed values in hello dataframe.*</span></span>

<span data-ttu-id="c1170-422">Hello értékek átalakítása látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-422">We see hello values have been transformed.</span></span> <span data-ttu-id="c1170-423">Most tejtermelés nagy mértékben meghaladja minden más tejtermék éles, hogy azt most nézi a Logaritmikus skála tárolóról.</span><span class="sxs-lookup"><span data-stu-id="c1170-423">Milk production now greatly exceeds all other dairy product production, recalling that we are now looking at a log scale.</span></span>

<span data-ttu-id="c1170-424">Ezen a ponton az adatok törlődnek, és készen áll az egyes modellezési azt.</span><span class="sxs-lookup"><span data-stu-id="c1170-424">At this point our data is cleaned up and we are ready for some modeling.</span></span> <span data-ttu-id="c1170-425">Hello képi megjelenítés eredménye Dataset a kimeneti hello összegzése megnézi a [R-parancsfájl végrehajtása] [ execute-r-script] modul, látni fogja hello "Month" oszlop: "Categorical" 12 egyedi értékekkel újra, ugyanúgy, mint a akartunk .</span><span class="sxs-lookup"><span data-stu-id="c1170-425">Looking at hello visualization summary for hello Result Dataset output of our [Execute R Script][execute-r-script] module, you will see hello 'Month' column is 'Categorical' with 12 unique values, again, just as we wanted.</span></span>

## <span data-ttu-id="c1170-426"><a id="timeseries"></a>Idő adatsorozat objektumok és a korrelációs elemzés</span><span class="sxs-lookup"><span data-stu-id="c1170-426"><a id="timeseries"></a>Time series objects and correlation analysis</span></span>
<span data-ttu-id="c1170-427">Ebben a szakaszban rendszer R idő néhány alapvető adatsorozat objektumok felfedezése és elemezheti hello korrelációk néhány hello változók között.</span><span class="sxs-lookup"><span data-stu-id="c1170-427">In this section we will explore a few basic R time series objects and analyze hello correlations between some of hello variables.</span></span> <span data-ttu-id="c1170-428">Célunk toooutput egy dataframe hello páros korrelációs információkat tartalmazó, több késedelmes jelentések.</span><span class="sxs-lookup"><span data-stu-id="c1170-428">Our goal is toooutput a dataframe containing hello pairwise correlation information at several lags.</span></span>

<span data-ttu-id="c1170-429">Ez a szakasz hello teljes R kód hello korábban letöltött zip-fájl van.</span><span class="sxs-lookup"><span data-stu-id="c1170-429">hello complete R code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="time-series-objects-in-r"></a><span data-ttu-id="c1170-430">Az R idő adatsorozat objektumok</span><span class="sxs-lookup"><span data-stu-id="c1170-430">Time series objects in R</span></span>
<span data-ttu-id="c1170-431">Már említettük, az idő a adatsorokat idő indexelik adatértékek sorozata.</span><span class="sxs-lookup"><span data-stu-id="c1170-431">As already mentioned, time series are a series of data values indexed by time.</span></span> <span data-ttu-id="c1170-432">R idő adatsorozat objektumok használt toocreate és hello idő index kezelése.</span><span class="sxs-lookup"><span data-stu-id="c1170-432">R time series objects are used toocreate and manage hello time index.</span></span> <span data-ttu-id="c1170-433">Nincsenek több előnyeit toousing idő adatsorozat objektumok.</span><span class="sxs-lookup"><span data-stu-id="c1170-433">There are several advantages toousing time series objects.</span></span> <span data-ttu-id="c1170-434">Idő adatsorozat objektumok mentes, hello számos részletével hello idő index sorozatértékek hello objektum ágyazott kezelése.</span><span class="sxs-lookup"><span data-stu-id="c1170-434">Time series objects free you from hello many details of managing hello time series index values that are encapsulated in hello object.</span></span> <span data-ttu-id="c1170-435">Ezenkívül idő adatsorozat objektumok lehetővé teszik a toouse hello sok időt adatsorozat módszerek ábrázolásához, nyomtatási, modellezési stb.</span><span class="sxs-lookup"><span data-stu-id="c1170-435">In addition, time series objects allow you toouse hello many time series methods for plotting, printing, modeling, etc.</span></span>

<span data-ttu-id="c1170-436">hello POSIXct idő adatsorozat osztály általában arra használják, és viszonylag egyszerű.</span><span class="sxs-lookup"><span data-stu-id="c1170-436">hello POSIXct time series class is commonly used and is relatively simple.</span></span> <span data-ttu-id="c1170-437">Az idő adatsorozat osztály hello kezdetét hello epoch, 1970. január 1. a időt méri.</span><span class="sxs-lookup"><span data-stu-id="c1170-437">This time series class measures time from hello start of hello epoch, January 1, 1970.</span></span> <span data-ttu-id="c1170-438">Ebben a példában használjuk POSIXct idő adatsorozat objektumok.</span><span class="sxs-lookup"><span data-stu-id="c1170-438">We will use POSIXct time series objects in this example.</span></span> <span data-ttu-id="c1170-439">Más széles körben használt R idő adatsorozat objektum osztályok itt és xts, bővíthető idősor tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-439">Other widely used R time series object classes include zoo and xts, extensible time series.</span></span>
<!-- Additional information on R time series objects is provided in hello references in Section 5.7. [commenting because this section doesn't exist, even in hello original] -->

### <a name="time-series-object-example"></a><span data-ttu-id="c1170-440">Idő adatsorozat objektum – példa</span><span class="sxs-lookup"><span data-stu-id="c1170-440">Time series object example</span></span>
<span data-ttu-id="c1170-441">Lássunk neki a fenti példában.</span><span class="sxs-lookup"><span data-stu-id="c1170-441">Let's get started with our example.</span></span> <span data-ttu-id="c1170-442">Húzza a **új** [R-parancsfájl végrehajtása] [ execute-r-script] modult a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="c1170-442">Drag and drop a **new** [Execute R Script][execute-r-script] module into your experiment.</span></span> <span data-ttu-id="c1170-443">Csatlakozás hello eredmény Dataset1 kimeneti port hello meglévő [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello Dataset1 adjon meg új hello portja [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c1170-443">Connect hello Result Dataset1 output port of hello existing [Execute R Script][execute-r-script] module toohello Dataset1 input port of hello new [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="c1170-444">Ahogyan hello első példák, mint azt hello például bizonyos időpontokban I megjeleníti a folyamat csak hello növekményes további kódsorokat R lépésről lépésre.</span><span class="sxs-lookup"><span data-stu-id="c1170-444">As I did for hello first examples, as we progress through hello example, at some points I will show only hello incremental additional lines of R code at each step.</span></span>  

#### <a name="reading-hello-dataframe"></a><span data-ttu-id="c1170-445">Hello dataframe olvasása</span><span class="sxs-lookup"><span data-stu-id="c1170-445">Reading hello dataframe</span></span>
<span data-ttu-id="c1170-446">Első lépésként tegyük egy dataframe olvasása, és győződjön meg arról, hogy a Microsoft hello várt eredményekhez juthat.</span><span class="sxs-lookup"><span data-stu-id="c1170-446">As a first step, let's read in a dataframe and make sure we get hello expected results.</span></span> <span data-ttu-id="c1170-447">hello alábbira tegyen hello feladat.</span><span class="sxs-lookup"><span data-stu-id="c1170-447">hello following code should do hello job.</span></span>

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check hello results

<span data-ttu-id="c1170-448">Most futtassa a hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="c1170-448">Now, run hello experiment.</span></span> <span data-ttu-id="c1170-449">hello napló hello új R-parancsfájl végrehajtása alakzat 14. ábra hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="c1170-449">hello log of hello new Execute R Script shape should look like Figure 14.</span></span>

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

<span data-ttu-id="c1170-450">*14. ábra. Hello dataframe hello R-parancsfájl végrehajtása modul összegzését.*</span><span class="sxs-lookup"><span data-stu-id="c1170-450">*Figure 14. Summary of hello dataframe in hello Execute R Script module.*</span></span>

<span data-ttu-id="c1170-451">Ezek az adatok hello várt típusok és formátumban van.</span><span class="sxs-lookup"><span data-stu-id="c1170-451">This data is of hello expected types and format.</span></span> <span data-ttu-id="c1170-452">Vegye figyelembe, hogy hello "Month" oszlop típusa tényező, és rendelkezik hello várt szintek száma.</span><span class="sxs-lookup"><span data-stu-id="c1170-452">Note that hello 'Month' column is of type factor and has hello expected number of levels.</span></span>

#### <a name="creating-a-time-series-object"></a><span data-ttu-id="c1170-453">Idő adatsorozat objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1170-453">Creating a time series object</span></span>
<span data-ttu-id="c1170-454">Egy alkalommal adatsorozat objektum tooour dataframe igazolnia kell tooadd.</span><span class="sxs-lookup"><span data-stu-id="c1170-454">We need tooadd a time series object tooour dataframe.</span></span> <span data-ttu-id="c1170-455">Hello aktuális kód cseréje hello következő, az osztály POSIXct egy új oszlopot ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="c1170-455">Replace hello current code with hello following, which adds a new column of class POSIXct.</span></span>

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check hello results

<span data-ttu-id="c1170-456">Most a naplóban hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-456">Now, check hello log.</span></span> <span data-ttu-id="c1170-457">15. ábra kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="c1170-457">It should look like Figure 15.</span></span>

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

<span data-ttu-id="c1170-458">*15. ábra. Hello dataframe idő adatsorozat objektum összegzését.*</span><span class="sxs-lookup"><span data-stu-id="c1170-458">*Figure 15. Summary of hello dataframe with a time series object.*</span></span>

<span data-ttu-id="c1170-459">Az összefoglaló hello láthatja, hogy hello új oszlop valójában osztály POSIXct.</span><span class="sxs-lookup"><span data-stu-id="c1170-459">We can see from hello summary that hello new column is in fact of class POSIXct.</span></span>

### <a name="exploring-and-transforming-hello-data"></a><span data-ttu-id="c1170-460">Fel és hello adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="c1170-460">Exploring and transforming hello data</span></span>
<span data-ttu-id="c1170-461">Most felfedezheti hello változók DataSet adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="c1170-461">Let's explore some of hello variables in this dataset.</span></span> <span data-ttu-id="c1170-462">Egy scatterplot mátrixban egy jó módszer tooproduce gyorsan át.</span><span class="sxs-lookup"><span data-stu-id="c1170-462">A scatterplot matrix is a good way tooproduce a quick look.</span></span> <span data-ttu-id="c1170-463">Vagyok helyettesíteni a hello `str()` függvényt a következő sor hello hello előző R kódot.</span><span class="sxs-lookup"><span data-stu-id="c1170-463">I am replacing hello `str()` function in hello previous R code with hello following line.</span></span>

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

<span data-ttu-id="c1170-464">Futtassa a következő kódot, és tekintse meg, mi történik.</span><span class="sxs-lookup"><span data-stu-id="c1170-464">Run this code and see what happens.</span></span> <span data-ttu-id="c1170-465">hello R eszköz port bemutatni hello rajzot 16. ábra hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="c1170-465">hello plot produced at hello R Device port should look like Figure 16.</span></span>

![A kijelölt változók Scatterplot mátrix][17]

<span data-ttu-id="c1170-467">*16. ábra. A kijelölt változók mátrix Scatterplot.*</span><span class="sxs-lookup"><span data-stu-id="c1170-467">*Figure 16. Scatterplot matrix of selected variables.*</span></span>

<span data-ttu-id="c1170-468">Van néhány odd-looking struktúra hello kapcsolatok változókhoz között.</span><span class="sxs-lookup"><span data-stu-id="c1170-468">There is some odd-looking structure in hello relationships between these variables.</span></span> <span data-ttu-id="c1170-469">Lehet, hogy ez akkor merül fel hello adatok a trendek és hello tényt, hogy azt nem rendelkezik szabványosított hello változók.</span><span class="sxs-lookup"><span data-stu-id="c1170-469">Perhaps this arises from trends in hello data and from hello fact that we have not standardized hello variables.</span></span>

### <a name="correlation-analysis"></a><span data-ttu-id="c1170-470">Korrelációs elemzés</span><span class="sxs-lookup"><span data-stu-id="c1170-470">Correlation analysis</span></span>
<span data-ttu-id="c1170-471">tooperform korrelációs elemzési tooboth kell deszerializálni trend, és szabványosítására hello változók.</span><span class="sxs-lookup"><span data-stu-id="c1170-471">tooperform correlation analysis we need tooboth de-trend and standardize hello variables.</span></span> <span data-ttu-id="c1170-472">Sikerült egyszerűen használjuk hello R `scale()` függvénynek, amely az adatközpontok, mind az méretezi a változókat.</span><span class="sxs-lookup"><span data-stu-id="c1170-472">We could simply use hello R `scale()` function, which both centers and scales variables.</span></span> <span data-ttu-id="c1170-473">Ez a funkció is előfordulhat, hogy gyorsabban futnak.</span><span class="sxs-lookup"><span data-stu-id="c1170-473">This function might well run faster.</span></span> <span data-ttu-id="c1170-474">Azonban tooshow kívánt, az r defenzív programing példa</span><span class="sxs-lookup"><span data-stu-id="c1170-474">However, I want tooshow you an example of defensive programing in R.</span></span>

<span data-ttu-id="c1170-475">Hello `ts.detrend()` függvény alább látható mindkét ezeket a műveleteket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="c1170-475">hello `ts.detrend()` function shown below performs both of these operations.</span></span> <span data-ttu-id="c1170-476">hello következő két sornyi kód deszerializálni trend hello adatok és majd szabványosítására hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="c1170-476">hello following two lines of code de-trend hello data and then standardize hello values.</span></span>

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function toode-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time toohave hello same length',
                    'ERROR: ts.detrend requires argument ts toobe of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros tooreturn as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # hello input arguments are not of hello same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If hello ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If hello ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that hello Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend hello time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply hello detrend.ts function toohello variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot hello results toolook at hello relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

<span data-ttu-id="c1170-477">Nincs elég egy bit azonban a hello `ts.detrend()` függvény.</span><span class="sxs-lookup"><span data-stu-id="c1170-477">There is quite a bit happening in hello `ts.detrend()` function.</span></span> <span data-ttu-id="c1170-478">Ez a kód a legtöbb hello argumentumok kapcsolatos esetleges problémák keresése vagy kivételek, továbbra is hello számítások során felmerülő foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="c1170-478">Most of this code is checking for potential problems with hello arguments or dealing with exceptions, which can still arise during hello computations.</span></span> <span data-ttu-id="c1170-479">Ez a kód csak néhány sornyi ténylegesen hello számításokat.</span><span class="sxs-lookup"><span data-stu-id="c1170-479">Only a few lines of this code actually do hello computations.</span></span>

<span data-ttu-id="c1170-480">Rendelkezik már tárgyalt defenzív programozás a példát [átalakítások érték](#valuetransformations).</span><span class="sxs-lookup"><span data-stu-id="c1170-480">We have already discussed an example of defensive programming in [Value transformations](#valuetransformations).</span></span> <span data-ttu-id="c1170-481">Mindkét számítási blokkok csomagolni vannak `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="c1170-481">Both computation blocks are wrapped in `tryCatch()`.</span></span> <span data-ttu-id="c1170-482">A hibák logika tooreturn hello eredeti bemeneti vektoros lehetővé teszi, és más esetekben nullák vektor visszatérés.</span><span class="sxs-lookup"><span data-stu-id="c1170-482">For some errors it makes sense tooreturn hello original input vector, and in other cases, I return a vector of zeros.</span></span>  

<span data-ttu-id="c1170-483">Vegye figyelembe, hogy hello deszerializálni trendek használt lineáris regresszió egy idő adatsorozat regressziós.</span><span class="sxs-lookup"><span data-stu-id="c1170-483">Note that hello linear regression used for de-trending is a time series regression.</span></span> <span data-ttu-id="c1170-484">hello előrejelzőjének változó egy idő adatsorozat objektum.</span><span class="sxs-lookup"><span data-stu-id="c1170-484">hello predictor variable is a time series object.</span></span>  

<span data-ttu-id="c1170-485">Egyszer `ts.detrend()` definiált érvénybe lépni, a dataframe érdeklődik toohello változókat.</span><span class="sxs-lookup"><span data-stu-id="c1170-485">Once `ts.detrend()` is defined we apply it toohello variables of interest in our dataframe.</span></span> <span data-ttu-id="c1170-486">Igazolnia kell kényszerítési hello listájában által létrehozott `lapply()` toodata dataframe használatával `as.data.frame()`.</span><span class="sxs-lookup"><span data-stu-id="c1170-486">We must coerce hello resulting list created by `lapply()` toodata dataframe by using `as.data.frame()`.</span></span> <span data-ttu-id="c1170-487">Defenzív aspektusainak miatt `ts.detrend()`, egy hello változók nem akadályozza meg a hiba tooprocess javítsa hello feldolgozása mások számára.</span><span class="sxs-lookup"><span data-stu-id="c1170-487">Because of defensive aspects of `ts.detrend()`, failure tooprocess one of hello variables will not prevent correct processing of hello others.</span></span>  

<span data-ttu-id="c1170-488">hello végső kódsort páros scatterplot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c1170-488">hello final line of code creates a pairwise scatterplot.</span></span> <span data-ttu-id="c1170-489">Miután hello R-kód, hello scatterplot hello eredményei láthatók 17. ábra.</span><span class="sxs-lookup"><span data-stu-id="c1170-489">After running hello R code, hello results of hello scatterplot are shown in Figure 17.</span></span>

![Vonja trendszerű és szabványos idősorozat páros scatterplot][18]

<span data-ttu-id="c1170-491">*17. ábra. Páros scatterplot deszerializálni trendszerű és szabványos idősorozat.*</span><span class="sxs-lookup"><span data-stu-id="c1170-491">*Figure 17. Pairwise scatterplot of de-trended and standardized time series.*</span></span>

<span data-ttu-id="c1170-492">Ezek a 16 megjelenített eredmények toothose összehasonlíthatja.</span><span class="sxs-lookup"><span data-stu-id="c1170-492">You can compare these results toothose shown in Figure 16.</span></span> <span data-ttu-id="c1170-493">Hello trend eltávolítva és szabványosított, változók hello sokkal kevesebb szerkezetben vannak ezek a változók hello kapcsolatai látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-493">With hello trend removed and hello variables standardized, we see a lot less structure in hello relationships between these variables.</span></span>

<span data-ttu-id="c1170-494">hello kód toocompute hello korrelációk R FTB objektumként a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="c1170-494">hello code toocompute hello correlations as R ccf objects is as follows.</span></span>

    ## A function toocompute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of hello pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute hello list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

<span data-ttu-id="c1170-495">Ezt a kódot futtató hoz létre a 18 látható hello napló.</span><span class="sxs-lookup"><span data-stu-id="c1170-495">Running this code produces hello log shown in Figure 18.</span></span>

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

<span data-ttu-id="c1170-496">*18. ábra. FTB listája hello páros korrelációs elemzés a következő helyről objektumokat.*</span><span class="sxs-lookup"><span data-stu-id="c1170-496">*Figure 18. List of ccf objects from hello pairwise correlation analysis.*</span></span>

<span data-ttu-id="c1170-497">Minden egyes lag korrelációs értéke van.</span><span class="sxs-lookup"><span data-stu-id="c1170-497">There is a correlation value for each lag.</span></span> <span data-ttu-id="c1170-498">A következő korrelációs értékek nincs elég nagy toobe jelentős.</span><span class="sxs-lookup"><span data-stu-id="c1170-498">None of these correlation values is large enough toobe significant.</span></span> <span data-ttu-id="c1170-499">Azt ezért be, hogy azt az egyes minta egymástól függetlenül is.</span><span class="sxs-lookup"><span data-stu-id="c1170-499">We can therefore conclude that we can model each variable independently.</span></span>

### <a name="output-a-dataframe"></a><span data-ttu-id="c1170-500">Egy dataframe kimeneti</span><span class="sxs-lookup"><span data-stu-id="c1170-500">Output a dataframe</span></span>
<span data-ttu-id="c1170-501">Az R FTB objektumok listájának azt rendelkezik számított hello páros korrelációk.</span><span class="sxs-lookup"><span data-stu-id="c1170-501">We have computed hello pairwise correlations as a list of R ccf objects.</span></span> <span data-ttu-id="c1170-502">Ez megadja bit típust egy probléma hello eredmény adathalmaz kimeneti portjával valóban egy dataframe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c1170-502">This presents a bit of a problem as hello Result Dataset output port really requires a dataframe.</span></span> <span data-ttu-id="c1170-503">További hello FTB objektum pedig egy lista, és szeretnénk csak hello értékek hello első eleme ebben a listában, a hello hello korrelációk különböző késedelmes jelentések.</span><span class="sxs-lookup"><span data-stu-id="c1170-503">Further, hello ccf object is itself a list and we want only hello values in hello first element of this list, hello correlations at hello various lags.</span></span>

<span data-ttu-id="c1170-504">hello követő kód kivonatok hello lag értékek FTB objektumok, amelyek maguk is listák hello listája.</span><span class="sxs-lookup"><span data-stu-id="c1170-504">hello following code extracts hello lag values from hello list of ccf objects, which are themselves lists.</span></span>

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with hello row names column and the
    ## correlation data frame and assign hello column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## hello following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

<span data-ttu-id="c1170-505">hello első sor a kód egy kicsit legbonyolultabb, és a rövid segítenek megismerni.</span><span class="sxs-lookup"><span data-stu-id="c1170-505">hello first line of code is a bit tricky, and some explanation may help you understand it.</span></span> <span data-ttu-id="c1170-506">Hello ki belül dolgozó tudunk hello következő:</span><span class="sxs-lookup"><span data-stu-id="c1170-506">Working from hello inside out we have hello following:</span></span>

1. <span data-ttu-id="c1170-507">hello "**[[**"hello argumentummal operator"**1**" választja ki a hello késedelmes jelentések hello hello FTB objektum-lista első eleme a korrelációk vektorát hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-507">hello '**[[**' operator with hello argument '**1**' selects hello vector of correlations at hello lags from hello first element of hello ccf object list.</span></span>
2. <span data-ttu-id="c1170-508">Hello `do.call()` függvény vonatkozik hello `rbind()` keresztül hello lista elemeinek hello függvény által `lapply()`.</span><span class="sxs-lookup"><span data-stu-id="c1170-508">hello `do.call()` function applies hello `rbind()` function over hello elements of hello list returns by `lapply()`.</span></span>
3. <span data-ttu-id="c1170-509">Hello `data.frame()` függvény típusú értékké konvertál által előállított hello eredmény `do.call()` tooa dataframe.</span><span class="sxs-lookup"><span data-stu-id="c1170-509">hello `data.frame()` function coerces hello result produced by `do.call()` tooa dataframe.</span></span>

<span data-ttu-id="c1170-510">Vegye figyelembe, hogy hello sor nevek hello dataframe oszlopában.</span><span class="sxs-lookup"><span data-stu-id="c1170-510">Note that hello row names are in a column of hello dataframe.</span></span> <span data-ttu-id="c1170-511">Így megőrzi hello sor neveket, ha azok hello kimenete [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="c1170-511">Doing so preserves hello row names when they are output from hello [Execute R Script][execute-r-script].</span></span>

<span data-ttu-id="c1170-512">Hello kód futtatásával hozza létre a 19. ábra hello kimenettel amikor I **Visualize** hello kimeneti: hello eredmény Dataset port.</span><span class="sxs-lookup"><span data-stu-id="c1170-512">Running hello code produces hello output shown in Figure 19 when I **Visualize** hello output at hello Result Dataset port.</span></span> <span data-ttu-id="c1170-513">hello sor nevek rendeltetésszerű hello első oszlopban szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="c1170-513">hello row names are in hello first column, as intended.</span></span>

![Hello korrelációs elemzési eredmények kimenete][20]

<span data-ttu-id="c1170-515">*19. ábra. Az eredmények hello korrelációs elemzés kimenetét.*</span><span class="sxs-lookup"><span data-stu-id="c1170-515">*Figure 19. Results output from hello correlation analysis.*</span></span>

## <span data-ttu-id="c1170-516"><a id="seasonalforecasting"></a>Idő adatsorozat példa: határozza előrejelzés</span><span class="sxs-lookup"><span data-stu-id="c1170-516"><a id="seasonalforecasting"></a>Time series example: seasonal forecasting</span></span>
<span data-ttu-id="c1170-517">Az adatok mostantól elemzés alkalmas űrlapon, és nincsenek hello változók között nincs jelentős korrelációk azt észlelte.</span><span class="sxs-lookup"><span data-stu-id="c1170-517">Our data is now in a form suitable for analysis, and we have determined there are no significant correlations between hello variables.</span></span> <span data-ttu-id="c1170-518">Most lépés, és hozzon létre egy idősorozat-modell előrejelzési.</span><span class="sxs-lookup"><span data-stu-id="c1170-518">Let's move on and create a time series forecasting model.</span></span> <span data-ttu-id="c1170-519">A modell segítségével azt fogja előrejelzési kaliforniai tejtermelésre hello 2013 számított 12 hónapon.</span><span class="sxs-lookup"><span data-stu-id="c1170-519">Using this model we will forecast California milk production for hello 12 months of 2013.</span></span>

<span data-ttu-id="c1170-520">Az előrejelzési modell két összetevőt, a trend összetevőt és határozza lesz.</span><span class="sxs-lookup"><span data-stu-id="c1170-520">Our forecasting model will have two components, a trend component and a seasonal component.</span></span> <span data-ttu-id="c1170-521">hello teljes előrejelzés a két összetevő hello szorzatát.</span><span class="sxs-lookup"><span data-stu-id="c1170-521">hello complete forecast is hello product of these two components.</span></span> <span data-ttu-id="c1170-522">Az ilyen típusú modell tényezőt modell néven ismert.</span><span class="sxs-lookup"><span data-stu-id="c1170-522">This type of model is known as a multiplicative model.</span></span> <span data-ttu-id="c1170-523">hello alternatíva a következő: egy additívak modellt.</span><span class="sxs-lookup"><span data-stu-id="c1170-523">hello alternative is an additive model.</span></span> <span data-ttu-id="c1170-524">A napló átalakítása toohello változók iránt, így tractable az elemzés azt már telepítették.</span><span class="sxs-lookup"><span data-stu-id="c1170-524">We have already applied a log transformation toohello variables of interest, which makes this analysis tractable.</span></span>

<span data-ttu-id="c1170-525">Ez a szakasz hello teljes R kód hello korábban letöltött zip-fájl van.</span><span class="sxs-lookup"><span data-stu-id="c1170-525">hello complete R code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="creating-hello-dataframe-for-analysis"></a><span data-ttu-id="c1170-526">Hello dataframe elemzés létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1170-526">Creating hello dataframe for analysis</span></span>
<span data-ttu-id="c1170-527">Először vegyen fel egy **új** [R-parancsfájl végrehajtása] [ execute-r-script] modul tooyour kísérlet.</span><span class="sxs-lookup"><span data-stu-id="c1170-527">Start by adding a **new** [Execute R Script][execute-r-script] module tooyour experiment.</span></span> <span data-ttu-id="c1170-528">Csatlakozás hello **eredmény Dataset** hello meglévő kimeneti [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello **Dataset1** hello új modul bemeneti.</span><span class="sxs-lookup"><span data-stu-id="c1170-528">Connect hello **Result Dataset** output of hello existing [Execute R Script][execute-r-script] module toohello **Dataset1** input of hello new module.</span></span> <span data-ttu-id="c1170-529">hello eredmény hasonlóan kell kinéznie. ábra 20.</span><span class="sxs-lookup"><span data-stu-id="c1170-529">hello result should look something like Figure 20.</span></span>

![hello kísérletezhet hello hozzáadott új R-parancsfájl végrehajtása modul][21]

<span data-ttu-id="c1170-531">*20. ábra. hello hello hozzáadott új R-parancsfájl végrehajtása modul kísérletezhet.*</span><span class="sxs-lookup"><span data-stu-id="c1170-531">*Figure 20. hello experiment with hello new Execute R Script module added.*</span></span>

<span data-ttu-id="c1170-532">Mint hello korrelációs elemzése azt imént befejeződött, a szükséges tooadd adatsorozat objektum POSIXct idő oszlop.</span><span class="sxs-lookup"><span data-stu-id="c1170-532">As with hello correlation analysis we just completed, we need tooadd a column with a POSIXct time series object.</span></span> <span data-ttu-id="c1170-533">a következő kód hello fog ehhez csak.</span><span class="sxs-lookup"><span data-stu-id="c1170-533">hello following code will do just this.</span></span>

    # If running in Machine Learning Studio, uncomment hello first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

<span data-ttu-id="c1170-534">Futtassa a következő kódot, és nézze meg hello naplót.</span><span class="sxs-lookup"><span data-stu-id="c1170-534">Run this code and look at hello log.</span></span> <span data-ttu-id="c1170-535">hello eredmény 21. ábrát hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="c1170-535">hello result should look like Figure 21.</span></span>

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

<span data-ttu-id="c1170-536">*21. ábra. Hello dataframe összegzését.*</span><span class="sxs-lookup"><span data-stu-id="c1170-536">*Figure 21. A summary of hello dataframe.*</span></span>

<span data-ttu-id="c1170-537">Az eredményt, vagy folyamatban, készen áll a toostart az elemzés.</span><span class="sxs-lookup"><span data-stu-id="c1170-537">With this result, we are ready toostart our analysis.</span></span>

### <a name="create-a-training-dataset"></a><span data-ttu-id="c1170-538">Hozzon létre egy képzési adatkészlet</span><span class="sxs-lookup"><span data-stu-id="c1170-538">Create a training dataset</span></span>
<span data-ttu-id="c1170-539">Az összeállított hello dataframe kell toocreate képzési dataset.</span><span class="sxs-lookup"><span data-stu-id="c1170-539">With hello dataframe constructed we need toocreate a training dataset.</span></span> <span data-ttu-id="c1170-540">Ezeket az adatokat fog tartalmaznak minden hello megfigyelések kivételével hello utolsó 12 hello év 2013, amelyek a tesztelési adatkészletnél.</span><span class="sxs-lookup"><span data-stu-id="c1170-540">This data will include all of hello observations except hello last 12, of hello year 2013, which is our test dataset.</span></span> <span data-ttu-id="c1170-541">hello következő alkészletek hello dataframe code, és létrehozza a hello tejelő éles üzemi pontjának és ár változók előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="c1170-541">hello following code subsets hello dataframe and creates plots of hello dairy production and price variables.</span></span> <span data-ttu-id="c1170-542">I majd függvényében hello négy éles üzemi pontjának és ár változókat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c1170-542">I then create plots of hello four production and price variables.</span></span> <span data-ttu-id="c1170-543">Egy névtelen funkció használt toodefine néhány szabályozva az információhasználat a rajzolási, és majd ismétlés hello hello listája más két argumentumot `Map()`.</span><span class="sxs-lookup"><span data-stu-id="c1170-543">An anonymous function is used toodefine some augments for plot, and then iterate over hello list of hello other two arguments with `Map()`.</span></span> <span data-ttu-id="c1170-544">Ha vannak-e végezni, amely egy a hurok kellene rendelkeznie működött itt, hogy helyesek.</span><span class="sxs-lookup"><span data-stu-id="c1170-544">If you are thinking that a for loop would have worked fine here, you are correct.</span></span> <span data-ttu-id="c1170-545">Azonban mivel R I vagyok jeleníti meg a működési megközelítés működési nyelvet.</span><span class="sxs-lookup"><span data-stu-id="c1170-545">But, since R is a functional language I am showing you a functional approach.</span></span>

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

<span data-ttu-id="c1170-546">A time series sorozatát tevékenységtérkép 22. ábra látható hello R eszköz kimenetből hello hello kód futtatásával hozza létre.</span><span class="sxs-lookup"><span data-stu-id="c1170-546">Running hello code produces hello series of time series plots from hello R Device output shown in Figure 22.</span></span> <span data-ttu-id="c1170-547">Vegye figyelembe, hogy hello időtengelye egységekbe, dátumok, adatsorozat megrajzolásához metódus hello időt töltött előnyeit.</span><span class="sxs-lookup"><span data-stu-id="c1170-547">Note that hello time axis is in units of dates, a nice benefit of hello time series plot method.</span></span>

![Első idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Második idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Az idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok külső](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Az idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok negyedik](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

<span data-ttu-id="c1170-552">*22. ábra. Idő adatsorozat előkészítésére kaliforniai tejtermelésre és adatokat.*</span><span class="sxs-lookup"><span data-stu-id="c1170-552">*Figure 22. Time series plots of California dairy production and price data.*</span></span>

### <a name="a-trend-model"></a><span data-ttu-id="c1170-553">A trend modell</span><span class="sxs-lookup"><span data-stu-id="c1170-553">A trend model</span></span>
<span data-ttu-id="c1170-554">Egy idő adatsorozat objektum, és hogy megtekintette hello adatok létrehozása tooconstruct hello kaliforniai tej termelési adatok trend modellt kezdjük.</span><span class="sxs-lookup"><span data-stu-id="c1170-554">Having created a time series object and having had a look at hello data, let's start tooconstruct a trend model for hello California milk production data.</span></span> <span data-ttu-id="c1170-555">Azt a idő adatsorozat regressziós végezheti el.</span><span class="sxs-lookup"><span data-stu-id="c1170-555">We can do this with a time series regression.</span></span> <span data-ttu-id="c1170-556">Azonban származik egyértelműen hello rajzot, hogy azt fogja kell több mint egy görbét és intercept tooaccurately modell hello betanítási adatok trendet megfigyelt hello.</span><span class="sxs-lookup"><span data-stu-id="c1170-556">However, it is clear from hello plot that we will need more than a slope and intercept tooaccurately model hello observed trend in hello training data.</span></span>

<span data-ttu-id="c1170-557">Megadott hello kis léptékű hello adatok, I elkészíti az Rstudióból trend hello modell és majd kivágása és illessze be hello eredményül kapott modell Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c1170-557">Given hello small scale of hello data, I will build hello model for trend in RStudio and then cut and paste hello resulting model into Azure Machine Learning.</span></span> <span data-ttu-id="c1170-558">Az ilyen típusú interaktív elemzések elvégzéséhez interaktív környezetet biztosít az Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="c1170-558">RStudio provides an interactive environment for this type of interactive analysis.</span></span>

<span data-ttu-id="c1170-559">Első kísérlet, mint a polinom regressziós rendelkező too3 fel fog meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-559">As a first attempt, I will try a polynomial regression with powers up too3.</span></span> <span data-ttu-id="c1170-560">Nincs túlzott illeszkedő modellek az ilyen típusú valós veszélye.</span><span class="sxs-lookup"><span data-stu-id="c1170-560">There is a real danger of over-fitting these kinds of models.</span></span> <span data-ttu-id="c1170-561">Ezért akkor ajánlott tooavoid magas rendelés feltételeit.</span><span class="sxs-lookup"><span data-stu-id="c1170-561">Therefore, it is best tooavoid high order terms.</span></span> <span data-ttu-id="c1170-562">Hello `I()` hello tartalmának értelmezése funkció meggátolja a rövid nevének (értelmezi hello tartalma ","), és lehetővé teszi a egy regressziós egyenlet toowrite szó értelmezett függvényt.</span><span class="sxs-lookup"><span data-stu-id="c1170-562">hello `I()` function inhibits interpretation of hello contents (interprets hello contents 'as is') and allows you toowrite a literally interpreted function in a regression equation.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

<span data-ttu-id="c1170-563">Ezt követően hello következő.</span><span class="sxs-lookup"><span data-stu-id="c1170-563">This generates hello following.</span></span>

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

<span data-ttu-id="c1170-564">P értékek (Pr (> |} t |})) szöveget a kimenetben, láthatja, hogy hello squared kifejezés nem mindig jelentős.</span><span class="sxs-lookup"><span data-stu-id="c1170-564">From P values (Pr(>|t|)) in this output, we can see that hello squared term may not be significant.</span></span> <span data-ttu-id="c1170-565">Hello használom `update()` Ez a modell által figyelmen kívül hagyása hello négyzet kifejezés toomodify működik.</span><span class="sxs-lookup"><span data-stu-id="c1170-565">I will use hello `update()` function toomodify this model by dropping hello squared term.</span></span>

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

<span data-ttu-id="c1170-566">Ezt követően hello következő.</span><span class="sxs-lookup"><span data-stu-id="c1170-566">This generates hello following.</span></span>

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

<span data-ttu-id="c1170-567">Ez a következőhöz jobb.</span><span class="sxs-lookup"><span data-stu-id="c1170-567">This looks better.</span></span> <span data-ttu-id="c1170-568">Hello feltételek összes jelentős.</span><span class="sxs-lookup"><span data-stu-id="c1170-568">All of hello terms are significant.</span></span> <span data-ttu-id="c1170-569">Azonban a hello 2e-16 érték az alapértelmezett érték, és nem kell venni súlyosan túl.</span><span class="sxs-lookup"><span data-stu-id="c1170-569">However, hello 2e-16 value is a default value, and should not be taken too seriously.</span></span>  

<span data-ttu-id="c1170-570">Megerősítést tesztként most Meggyőződünk hello kaliforniai tejtermelésre adatok ábrázolása idő adatsorozat hello trend görbéjű látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-570">As a sanity test, let's make a time series plot of hello California dairy production data with hello trend curve shown.</span></span> <span data-ttu-id="c1170-571">A következő kódot az Azure Machine Learning hello hello hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] modell (nem Rstudióból) toocreate hello modell, és tegye rajzot.</span><span class="sxs-lookup"><span data-stu-id="c1170-571">I have added hello following code in hello Azure Machine Learning [Execute R Script][execute-r-script] model (not RStudio) toocreate hello model and make a plot.</span></span> <span data-ttu-id="c1170-572">23. ábra hello eredmény jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-572">hello result is shown in Figure 23.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Kaliforniai tej termelési adatok trend modellel látható](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

<span data-ttu-id="c1170-574">*23. ábra. Kaliforniai tej termelési adatok trend modellel látható.*</span><span class="sxs-lookup"><span data-stu-id="c1170-574">*Figure 23. California milk production data with trend model shown.*</span></span>

<span data-ttu-id="c1170-575">Úgy tűnik, hello trend modell hello adatok viszonylag jól illeszkedik.</span><span class="sxs-lookup"><span data-stu-id="c1170-575">It looks like hello trend model fits hello data fairly well.</span></span> <span data-ttu-id="c1170-576">További úgy nem tűnik toobe igazolása túlzott méretezés, például a következő hello modell görbe wiggles páratlan.</span><span class="sxs-lookup"><span data-stu-id="c1170-576">Further, there does not seem toobe evidence of over-fitting, such as odd wiggles in hello model curve.</span></span>  

### <a name="seasonal-model"></a><span data-ttu-id="c1170-577">Határozza modell</span><span class="sxs-lookup"><span data-stu-id="c1170-577">Seasonal model</span></span>
<span data-ttu-id="c1170-578">Az aktuális trend modell azt kell toopush és hello határozza hatások közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="c1170-578">With a trend model in hand, we need toopush on and include hello seasonal effects.</span></span> <span data-ttu-id="c1170-579">Hello év hónapját hello hello lineáris modell toocapture hello havonta készülő érvényben dummy változóként használjuk.</span><span class="sxs-lookup"><span data-stu-id="c1170-579">We will use hello month of hello year as a dummy variable in hello linear model toocapture hello month-by-month effect.</span></span> <span data-ttu-id="c1170-580">Vegye figyelembe, hogy amikor egy modell tényező változók bevezetéséhez, hello intercept nem kell számolni.</span><span class="sxs-lookup"><span data-stu-id="c1170-580">Note that when you introduce factor variables into a model, hello intercept must not be computed.</span></span> <span data-ttu-id="c1170-581">Ha nem ezt teszi, hello képlet túlzott megadott és R eldobja hello egyik tényező szükséges, de tartsa hello intercept kifejezés.</span><span class="sxs-lookup"><span data-stu-id="c1170-581">If you do not do this, hello formula is over-specified and R will drop one of hello desired factors but keep hello intercept term.</span></span>

<span data-ttu-id="c1170-582">Mivel kielégítő trend modell tudunk hello használhatjuk `update()` függvény tooadd hello új toohello meglévő modell feltételeket.</span><span class="sxs-lookup"><span data-stu-id="c1170-582">Since we have a satisfactory trend model we can use hello `update()` function tooadd hello new terms toohello existing model.</span></span> <span data-ttu-id="c1170-583">-1 hello hello frissítés képletben hello intercept kifejezés esik.</span><span class="sxs-lookup"><span data-stu-id="c1170-583">hello -1 in hello update formula drops hello intercept term.</span></span> <span data-ttu-id="c1170-584">Folytatás az Rstudióból hello pillanatra:</span><span class="sxs-lookup"><span data-stu-id="c1170-584">Continuing in RStudio for hello moment:</span></span>

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

<span data-ttu-id="c1170-585">Ezt követően hello következő.</span><span class="sxs-lookup"><span data-stu-id="c1170-585">This generates hello following.</span></span>

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

<span data-ttu-id="c1170-586">Adott hello modell nem tartalmaz egy intercept kifejezés és 12 hónap jelentős tényező látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-586">We see that hello model no longer has an intercept term and has 12 significant month factors.</span></span> <span data-ttu-id="c1170-587">Ez az pontosan mit akartunk toosee.</span><span class="sxs-lookup"><span data-stu-id="c1170-587">This is exactly what we wanted toosee.</span></span>

<span data-ttu-id="c1170-588">Ellenőrizze egy másik idő adatsorozat ábrázolása hello kaliforniai tejtermelésre adatok toosee hello határozza modell mennyire működik.</span><span class="sxs-lookup"><span data-stu-id="c1170-588">Let's make another time series plot of hello California dairy production data toosee how well hello seasonal model is working.</span></span> <span data-ttu-id="c1170-589">A következő kódot az Azure Machine Learning hello hello hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] toocreate hello modell, és tegye rajzot.</span><span class="sxs-lookup"><span data-stu-id="c1170-589">I have added hello following code in hello Azure Machine Learning [Execute R Script][execute-r-script] toocreate hello model and make a plot.</span></span>

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

<span data-ttu-id="c1170-590">Ez a kód futtatása az Azure Machine Learning hoz létre a hello rajzot 24. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-590">Running this code in Azure Machine Learning produces hello plot shown in Figure 24.</span></span>

![Kaliforniai tejtermelés határozza hatások beleértve modell](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

<span data-ttu-id="c1170-592">*24. ábra. Kaliforniai tejtermelés határozza hatások beleértve modellt.*</span><span class="sxs-lookup"><span data-stu-id="c1170-592">*Figure 24. California milk production with model including seasonal effects.*</span></span>

<span data-ttu-id="c1170-593">hello illeszkedő toohello 24. ábrán látható adata ahelyett, hogy ezzel.</span><span class="sxs-lookup"><span data-stu-id="c1170-593">hello fit toohello data shown in Figure 24 is rather encouraging.</span></span> <span data-ttu-id="c1170-594">Hello trendek és a hello határozza hatás (havi változat) ésszerű jelenik meg</span><span class="sxs-lookup"><span data-stu-id="c1170-594">Both hello trend and hello seasonal effect (monthly variation) look reasonable.</span></span>

<span data-ttu-id="c1170-595">A modell ellenőrzése most rendelkeznie hello például egy pillantást.</span><span class="sxs-lookup"><span data-stu-id="c1170-595">As another check on our model, let's have a look at hello residuals.</span></span> <span data-ttu-id="c1170-596">hello következő kód kiszámítja a két modell az előre jelzett értékek hello hello például hello határozza modell kiszámítja és majd tevékenységtérkép ezek például hello betanítási adatok.</span><span class="sxs-lookup"><span data-stu-id="c1170-596">hello following code computes hello predicted values from our two models, computes hello residuals for hello seasonal model, and then plots these residuals for hello training data.</span></span>

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot hello residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

<span data-ttu-id="c1170-597">hello maradék rajzot 25. ábra jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c1170-597">hello residual plot is shown in Figure 25.</span></span>

![Például hello határozza modell hello betanítási adatok](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

<span data-ttu-id="c1170-599">*25. ábra. Például hello határozza modell hello betanítási adatok.*</span><span class="sxs-lookup"><span data-stu-id="c1170-599">*Figure 25. Residuals of hello seasonal model for hello training data.*</span></span>

<span data-ttu-id="c1170-600">Ezeket például hely ésszerű.</span><span class="sxs-lookup"><span data-stu-id="c1170-600">These residuals look reasonable.</span></span> <span data-ttu-id="c1170-601">Nem adott struktúra, kivéve hello 2008-2009 recesszió, amely a modell nem derül ki hello hatása van különösen jól.</span><span class="sxs-lookup"><span data-stu-id="c1170-601">There is no particular structure, except hello effect of hello 2008-2009 recession, which our model does not account for particularly well.</span></span>

<span data-ttu-id="c1170-602">25. ábrán látható hello rajzot bármely időfüggő minták észlelésére hello például a.</span><span class="sxs-lookup"><span data-stu-id="c1170-602">hello plot shown in Figure 25 is useful for detecting any time-dependent patterns in hello residuals.</span></span> <span data-ttu-id="c1170-603">hello explicit módszert is, amely használt hello például számítási és a hello rajzot időrendjét hello például helyezi.</span><span class="sxs-lookup"><span data-stu-id="c1170-603">hello explicit approach of computing and plotting hello residuals I used places hello residuals in time order on hello plot.</span></span> <span data-ttu-id="c1170-604">Ha a hello, ugyanakkor I kellett ábrázolt `milk.lm$residuals`, hello rajzot nem volna időrendjét.</span><span class="sxs-lookup"><span data-stu-id="c1170-604">If, on hello other hand, I had plotted `milk.lm$residuals`, hello plot would not have been in time order.</span></span>

<span data-ttu-id="c1170-605">Is `plot.lm()` tooproduce diagnosztikai előkészítésére sorozata.</span><span class="sxs-lookup"><span data-stu-id="c1170-605">You can also use `plot.lm()` tooproduce a series of diagnostic plots.</span></span>

    ## Show hello diagnostic plots for hello model
    plot(milk.lm2, ask = FALSE)

<span data-ttu-id="c1170-606">Ez a kód 26. ábrán látható diagnosztikai előkészítésére sorozata eredményez.</span><span class="sxs-lookup"><span data-stu-id="c1170-606">This code produces a series of diagnostic plots shown in Figure 26.</span></span>

![Diagnosztikai kialakítása hello határozza modell az első](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Diagnosztikai előkészítésére hello határozza modell második](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![A diagnosztikai előkészítésére hello határozza modell harmadik](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![A diagnosztikai előkészítésére hello határozza modell negyedik](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

<span data-ttu-id="c1170-611">*26. ábra. Diagnosztikai tevékenységtérkép hello határozza modell.*</span><span class="sxs-lookup"><span data-stu-id="c1170-611">*Figure 26. Diagnostic plots for hello seasonal model.*</span></span>

<span data-ttu-id="c1170-612">Ezek előkészítésére, de semmi nem azonosított néhány magas segíti elő pontjai toocause nagy gondot okoz.</span><span class="sxs-lookup"><span data-stu-id="c1170-612">There are a few highly influential points identified in these plots, but nothing toocause great concern.</span></span> <span data-ttu-id="c1170-613">További láthatja a hello normál Q-Q rajzot, hogy hello például-e a Bezárás toonormally elosztott, egy fontos feltételezésen lineáris modellek esetén.</span><span class="sxs-lookup"><span data-stu-id="c1170-613">Further, we can see from hello Normal Q-Q plot that hello residuals are close toonormally distributed, an important assumption for linear models.</span></span>

### <a name="forecasting-and-model-evaluation"></a><span data-ttu-id="c1170-614">Előrejelzési és modell kiértékelése</span><span class="sxs-lookup"><span data-stu-id="c1170-614">Forecasting and model evaluation</span></span>
<span data-ttu-id="c1170-615">Nincs elegendő egyetlen további lépésként toodo toocomplete példában.</span><span class="sxs-lookup"><span data-stu-id="c1170-615">There is just one more thing toodo toocomplete our example.</span></span> <span data-ttu-id="c1170-616">Azt toocompute előrejelzések kell, és mérheti hello hiba hello tényleges adatokkal szemben.</span><span class="sxs-lookup"><span data-stu-id="c1170-616">We need toocompute forecasts and measure hello error against hello actual data.</span></span> <span data-ttu-id="c1170-617">Az előrejelzés lesznek hello 2013 számított 12 hónapon.</span><span class="sxs-lookup"><span data-stu-id="c1170-617">Our forecast will be for hello 12 months of 2013.</span></span> <span data-ttu-id="c1170-618">Hiba az előrejelzési toohello tényleges adatokat, amelyek nem része az képzési adatkészletet intézkedésként számíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="c1170-618">We can compute an error measure for this forecast toohello actual data that is not part of our training dataset.</span></span> <span data-ttu-id="c1170-619">Emellett azt is vetik össze a hello a betanítási adatok toohello 18 évnyi vizsgálati adatok 12 hónapon keresztül.</span><span class="sxs-lookup"><span data-stu-id="c1170-619">Additionally, we can compare performance on hello 18 years of training data toohello 12 months of test data.</span></span>  

<span data-ttu-id="c1170-620">Metrikák számos használt toomeasure hello teljesítményét az idő adatsorozat modellek.</span><span class="sxs-lookup"><span data-stu-id="c1170-620">A number of metrics are used toomeasure hello performance of time series models.</span></span> <span data-ttu-id="c1170-621">A mi esetünkben használjuk hello négyzetes (RMS)-hiba.</span><span class="sxs-lookup"><span data-stu-id="c1170-621">In our case we will use hello root mean square (RMS) error.</span></span> <span data-ttu-id="c1170-622">hello következő függvény kiszámítja hello RMS hiba két sorozat között.</span><span class="sxs-lookup"><span data-stu-id="c1170-622">hello following function computes hello RMS error between two series.</span></span>  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function toocompute hello RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments toofunction RMS.error of wrong type encountered",
                    "ERROR: Input vector toofunction RMS.error is too short",
                    "ERROR: Input vectors toofunction RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check hello arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate hello values, else just copy
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

    ## Compute hello RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

<span data-ttu-id="c1170-623">A hello `log.transform()` a tárgyalt funkció hello "Érték átalakítások" szakaszban, nincs elég nagy mennyiségű ellenőrzése és a kivétel helyreállítási hibakód ebben a függvényben.</span><span class="sxs-lookup"><span data-stu-id="c1170-623">As with hello `log.transform()` function we discussed in hello "Value transformations" section, there is quite a lot of error checking and exception recovery code in this function.</span></span> <span data-ttu-id="c1170-624">hello alapelvek alkalmazott vannak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="c1170-624">hello principles employed are hello same.</span></span> <span data-ttu-id="c1170-625">hello munkát csomagolni két helyen `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="c1170-625">hello work is done in two places wrapped in `tryCatch()`.</span></span> <span data-ttu-id="c1170-626">Első lépésként hello idő adatsorokat exponentiated, mivel azt dolgozott hello naplók hello értékek.</span><span class="sxs-lookup"><span data-stu-id="c1170-626">First, hello time series are exponentiated, since we have been working with hello logs of hello values.</span></span> <span data-ttu-id="c1170-627">Második számított hello tényleges RMS hiba.</span><span class="sxs-lookup"><span data-stu-id="c1170-627">Second, hello actual RMS error is computed.</span></span>  

<span data-ttu-id="c1170-628">Most egy függvény toomeasure hello RMS hiba felszerelve, hozza létre és egy dataframe hello RMS a hibákat tartalmazó kimeneti.</span><span class="sxs-lookup"><span data-stu-id="c1170-628">Equipped with a function toomeasure hello RMS error, let's build and output a dataframe containing hello RMS errors.</span></span> <span data-ttu-id="c1170-629">Hello trend modell önmagában feltételek és a teljes modell hello határozza tényezők is.</span><span class="sxs-lookup"><span data-stu-id="c1170-629">We will include terms for hello trend model alone and hello complete model with seasonal factors.</span></span> <span data-ttu-id="c1170-630">hello alábbira hello feladat használatával hello két lineáris modellek igazolnia kell kialakítani.</span><span class="sxs-lookup"><span data-stu-id="c1170-630">hello following code does hello job by using hello two linear models we have constructed.</span></span>

    ## Compute hello RMS error in a dataframe
    ## Include hello row names in hello first column so they will
    ## appear in hello output of hello Execute R Script
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

    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

<span data-ttu-id="c1170-631">Ezt a kódot futtató hoz létre a hello kimeneti hello eredmény adathalmaz kimeneti portjával 27. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="c1170-631">Running this code produces hello output shown in Figure 27 at hello Result Dataset output port.</span></span>

![RMS hibák hello modellek összehasonlítása][26]

<span data-ttu-id="c1170-633">*27. ábra. RMS hibák hello modellek összehasonlítása.*</span><span class="sxs-lookup"><span data-stu-id="c1170-633">*Figure 27. Comparison of RMS errors for hello models.*</span></span>

<span data-ttu-id="c1170-634">Ezekkel az eredményekkel, az látható, hogy hello határozza tényezők toohello hozzáadása modell hello RMS hiba lényegesen csökkenti.</span><span class="sxs-lookup"><span data-stu-id="c1170-634">From these results, we see that adding hello seasonal factors toohello model reduces hello RMS error significantly.</span></span> <span data-ttu-id="c1170-635">Nem túl érdekes hello RMS hiba a betanítási adatok hello bites kisebb, mint hello előrejelzési.</span><span class="sxs-lookup"><span data-stu-id="c1170-635">Not too surprisingly, hello RMS error for hello training data is a bit less than for hello forecast.</span></span>

## <span data-ttu-id="c1170-636"><a id="appendixa"></a>FÜGGELÉK: Útmutató tooRStudio</span><span class="sxs-lookup"><span data-stu-id="c1170-636"><a id="appendixa"></a>APPENDIX A: Guide tooRStudio</span></span>
<span data-ttu-id="c1170-637">Rstudióból meglehetősen megfelelően legyen dokumentálva, így a függelék I fogja adni az egyes hivatkozások toohello hello Rstudióból dokumentáció tooget használatba kulcsfontosságú részeit.</span><span class="sxs-lookup"><span data-stu-id="c1170-637">RStudio is quite well documented, so in this appendix I will provide some links toohello key sections of hello RStudio documentation tooget you started.</span></span>

1. <span data-ttu-id="c1170-638">Projektek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1170-638">Creating projects</span></span>
   
   <span data-ttu-id="c1170-639">Rendezheti és az R-kód a projektek Rstudióból használatával kezelni.</span><span class="sxs-lookup"><span data-stu-id="c1170-639">You can organize and manage your R code into projects by using RStudio.</span></span> <span data-ttu-id="c1170-640">https://support.rstudio.com/hc/articles/200526207-Using-Projects projektek használó hello dokumentációjában találhatók.</span><span class="sxs-lookup"><span data-stu-id="c1170-640">hello documentation that uses projects can be found at https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span></span>
   
   <span data-ttu-id="c1170-641">Kövesse az alábbi utasításokat, és hello R kódpéldák projekt létrehozása ebben a dokumentumban javasolt.</span><span class="sxs-lookup"><span data-stu-id="c1170-641">I recommend that you follow these directions and create a project for hello R code examples in this document.</span></span>  
2. <span data-ttu-id="c1170-642">Szerkesztés és az R-kód végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c1170-642">Editing and executing R code</span></span>
   
   <span data-ttu-id="c1170-643">A szerkesztési és R-kód végrehajtása integrált környezetet biztosít az Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="c1170-643">RStudio provides an integrated environment for editing and executing R code.</span></span> <span data-ttu-id="c1170-644">Https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code dokumentációjában találhatók.</span><span class="sxs-lookup"><span data-stu-id="c1170-644">Documentation can be found at https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span></span>
3. <span data-ttu-id="c1170-645">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="c1170-645">Debugging</span></span>
   
   <span data-ttu-id="c1170-646">Rstudióból hatékony hibakeresési képességeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1170-646">RStudio includes powerful debugging capabilities.</span></span> <span data-ttu-id="c1170-647">Ezek a szolgáltatások dokumentációja https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio jelenleg.</span><span class="sxs-lookup"><span data-stu-id="c1170-647">Documentation for these features is at https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span></span>
   
   <span data-ttu-id="c1170-648">hello töréspont hibaelhárítási lehetőségek: https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="c1170-648">hello breakpoint troubleshooting features are documented at https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span></span>

## <span data-ttu-id="c1170-649"><a id="appendixb"></a>B függelék: További információ</span><span class="sxs-lookup"><span data-stu-id="c1170-649"><a id="appendixb"></a>APPENDIX B: Further reading</span></span>
<span data-ttu-id="c1170-650">Ez magában foglalja az oktatóanyag hello alapjai programozási R, hogy mit kell toouse hello Azure Machine Learning Studio R nyelven.</span><span class="sxs-lookup"><span data-stu-id="c1170-650">This R programming tutorial covers hello basics of what you need toouse hello R language with Azure Machine Learning Studio.</span></span> <span data-ttu-id="c1170-651">Ha nem ismeri a R, a CRAN két bevezetés érhetők el:</span><span class="sxs-lookup"><span data-stu-id="c1170-651">If you are not familiar with R, two introductions are available on CRAN:</span></span>

* <span data-ttu-id="c1170-652">R Emmanuel Paradis által kezdőknek egy remek toostart: http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span><span class="sxs-lookup"><span data-stu-id="c1170-652">R for Beginners by Emmanuel Paradis is a good place toostart at http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span></span>  
* <span data-ttu-id="c1170-653">Egy bevezető bemutató w-n</span><span class="sxs-lookup"><span data-stu-id="c1170-653">An Introduction tooR by W. N.</span></span> <span data-ttu-id="c1170-654">Venables et.</span><span class="sxs-lookup"><span data-stu-id="c1170-654">Venables et.</span></span> <span data-ttu-id="c1170-655">al.</span><span class="sxs-lookup"><span data-stu-id="c1170-655">al.</span></span> <span data-ttu-id="c1170-656">egy kicsit nagyobb mélysége, http://cran.r-project.org/doc/manuals/R-intro.html állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="c1170-656">goes into a bit more depth, at http://cran.r-project.org/doc/manuals/R-intro.html.</span></span>

<span data-ttu-id="c1170-657">Nincs olyan sok könyvek R, amelyik segíthet a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="c1170-657">There are many books on R that can help you get started.</span></span> <span data-ttu-id="c1170-658">Íme néhány hasznos található:</span><span class="sxs-lookup"><span data-stu-id="c1170-658">Here are a few I find useful:</span></span>

* <span data-ttu-id="c1170-659">Kép az R programozási hello: A bemutató a statisztikai szoftver kialakítása Norman Matloff által esetén egy kiváló bemutatása tooprogramming az R.</span><span class="sxs-lookup"><span data-stu-id="c1170-659">hello Art of R Programming: A Tour of Statistical Software Design by Norman Matloff is an excellent introduction tooprogramming in R.</span></span>  
* <span data-ttu-id="c1170-660">R Cookbook által Paul Teetor nyújt egy probléma és a megoldás megközelítés toousing R.</span><span class="sxs-lookup"><span data-stu-id="c1170-660">R Cookbook by Paul Teetor provides a problem and solution approach toousing R.</span></span>  
* <span data-ttu-id="c1170-661">A művelet által Robert Kabacoff R egy másik hasznos bevezető címjegyzék-alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="c1170-661">R in Action by Robert Kabacoff is another useful introductory book.</span></span> <span data-ttu-id="c1170-662">hello kiegészítő gyors R webhely http://www.statmethods.net/ egy hasznos erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c1170-662">hello companion Quick R website is a useful resource at http://www.statmethods.net/.</span></span>
* <span data-ttu-id="c1170-663">R Inferno Patrick égési által, amely foglalkozik a legbonyolultabb és nehezen méretezhetővé foglalkozó témakörök, amelyek is észlelt, amikor R. hello könyv programozási számos érdekes vicces könyv szabad http://www.burns-stat.com/documents/books/the-r-inferno/ érhető el.</span><span class="sxs-lookup"><span data-stu-id="c1170-663">R Inferno by Patrick Burns is a surprisingly humorous book that deals with a number of tricky and difficult topics that can be encountered when programming in R. hello book is available for free at http://www.burns-stat.com/documents/books/the-r-inferno/.</span></span>
* <span data-ttu-id="c1170-664">A részletes bemutatója a speciális témakörei R, van szükség egy pillantást hello könyv speciális R Hadley Wickham által.</span><span class="sxs-lookup"><span data-stu-id="c1170-664">If you want a deep dive into advanced topics in R, have a look at hello book Advanced R by Hadley Wickham.</span></span> <span data-ttu-id="c1170-665">hello online változata annak könyvben megtalálható szabad http://adv-r.had.co.nz/.</span><span class="sxs-lookup"><span data-stu-id="c1170-665">hello online version of this book is available for free at http://adv-r.had.co.nz/.</span></span>

<span data-ttu-id="c1170-666">A katalógus R idő adatsorozat csomagok hello CRAN feladat megtekintése az adatsorozat elemzés található: http://cran.r-project.org/web/views/TimeSeries.html.</span><span class="sxs-lookup"><span data-stu-id="c1170-666">A catalogue of R time series packages can be found in hello CRAN Task View for time series analysis: http://cran.r-project.org/web/views/TimeSeries.html.</span></span> <span data-ttu-id="c1170-667">Adott időpont adatsorozat objektum csomagok olvashat olvassa el, hogy a csomag toohello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="c1170-667">For information on specific time series object packages, you should refer toohello documentation for that package.</span></span>

<span data-ttu-id="c1170-668">hello könyv bevezető Time Series Paul Cowpertwait és Andrew Metcalfe r biztosít egy bevezető toousing R idő adatsorozat elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="c1170-668">hello book Introductory Time Series with R by Paul Cowpertwait and Andrew Metcalfe provides an introduction toousing R for time series analysis.</span></span> <span data-ttu-id="c1170-669">Számos további elméleti szövegek R példákat.</span><span class="sxs-lookup"><span data-stu-id="c1170-669">Many more theoretical texts provide R examples.</span></span>

<span data-ttu-id="c1170-670">Néhány nagy internetes erőforrásokhoz:</span><span class="sxs-lookup"><span data-stu-id="c1170-670">Some great internet resources:</span></span>

* <span data-ttu-id="c1170-671">DataCamp: DataCamp útmutatást ad az R a videó megszerzett és kódolási gyakorlatok a böngésző hello kényelmét.</span><span class="sxs-lookup"><span data-stu-id="c1170-671">DataCamp: DataCamp teaches R in hello comfort of your browser with video lessons and coding exercises.</span></span> <span data-ttu-id="c1170-672">Nincsenek interaktív oktatóanyagok hello legújabb R technikák és csomagok.</span><span class="sxs-lookup"><span data-stu-id="c1170-672">There are interactive tutorials on hello latest R techniques and packages.</span></span> <span data-ttu-id="c1170-673">Hello szabad interaktív R oktatóanyag veszik a https://www.datacamp.com/courses/introduction-to-r</span><span class="sxs-lookup"><span data-stu-id="c1170-673">Take hello free interactive R tutorial at https://www.datacamp.com/courses/introduction-to-r</span></span>
* <span data-ttu-id="c1170-674">Útmutató, kezdeti lépések a R Programiz https://www.programiz.com/r-programming a</span><span class="sxs-lookup"><span data-stu-id="c1170-674">A guide on Getting started with R from Programiz https://www.programiz.com/r-programming</span></span>
* <span data-ttu-id="c1170-675">A gyors R oktatóprogram által Tibor fekete Clarkson egyetemi http://www.cyclismo.org/tutorial/R/</span><span class="sxs-lookup"><span data-stu-id="c1170-675">A quick R tutorial by Kelly Black from Clarkson University http://www.cyclismo.org/tutorial/R/</span></span>
* <span data-ttu-id="c1170-676">60 + R erőforrások megtalálható a http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span><span class="sxs-lookup"><span data-stu-id="c1170-676">60+ R resources listed at http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span></span>

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
