---
title: "A Linux adatok tudományos virtuálisgép adattudomány |} Microsoft Docs"
description: "Hogyan hajthat végre több közös adatok tudományos feladatokat a a Linux adatok tudományos virtuális Géphez."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev;paulsh
ms.openlocfilehash: 6da9a8e3f9f8ac851c2a8deb861ac1d0b3ec5874
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="data-science-on-the-linux-data-science-virtual-machine"></a><span data-ttu-id="211ae-103">Adatelemzés a Linux Data Science virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="211ae-103">Data science on the Linux Data Science Virtual Machine</span></span>
<span data-ttu-id="211ae-104">Ez a forgatókönyv bemutatja, hogyan végezhető több közös adatok tudományos a Linux adatok tudományos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="211ae-104">This walkthrough shows you how to perform several common data science tasks with the Linux Data Science VM.</span></span> <span data-ttu-id="211ae-105">A Linux adatok tudományos virtuális gép (DSVM) érhető el, amely adatelemzés és a gépi tanulás általánosan használt eszközöket együtt telepített Azure virtuálisgép-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="211ae-105">The Linux Data Science Virtual Machine (DSVM) is a virtual machine image available on Azure that is pre-installed with a collection of tools commonly used for data analytics and machine learning.</span></span> <span data-ttu-id="211ae-106">A kulcs szoftverösszetevőket van felsorolva a [a Linux adatok tudományos virtuális gép kiépítéséhez](machine-learning-data-science-linux-dsvm-intro.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="211ae-106">The key software components are itemized in the [Provision the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md) topic.</span></span> <span data-ttu-id="211ae-107">A Virtuálisgép-lemezkép megkönnyíti az első lépések során adattudomány (percben), anélkül, hogy telepítse és konfigurálja az egyes eszközökről külön-külön kellene.</span><span class="sxs-lookup"><span data-stu-id="211ae-107">The VM image makes it easy to get started doing data science in minutes, without having to install and configure each of the tools individually.</span></span> <span data-ttu-id="211ae-108">Könnyedén növelheti a virtuális gép, ha szükséges, és állítsa le, ha nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="211ae-108">You can easily scale up the VM, if needed, and stop it when not in use.</span></span> <span data-ttu-id="211ae-109">Ehhez az erőforráshoz, mind a rugalmas és költséghatékony.</span><span class="sxs-lookup"><span data-stu-id="211ae-109">So this resource is both elastic and cost-efficient.</span></span>

<span data-ttu-id="211ae-110">Az ebben a forgatókönyvben bemutatott tudományos feladatok kövesse a a [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span><span class="sxs-lookup"><span data-stu-id="211ae-110">The data science tasks demonstrated in this walkthrough follow the steps outlined in the [Team Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span></span> <span data-ttu-id="211ae-111">Ez a folyamat, amely lehetővé teszi az adatszakértőkön át a hatékony az intelligens alkalmazások teljes életciklusát keresztül együttműködve megosszanak csoportjai adattudomány rendszeres megközelítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="211ae-111">This process provides a systematic approach to data science that enables teams of data scientists to effectively collaborate over the lifecycle of building intelligent applications.</span></span> <span data-ttu-id="211ae-112">Az tudományos folyamat egy iteratív keretet is biztosít, amely egy adott követhetnek adattudomány.</span><span class="sxs-lookup"><span data-stu-id="211ae-112">The data science process also provides an iterative framework for data science that can be followed by an individual.</span></span>

<span data-ttu-id="211ae-113">Azt a elemzése a [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="211ae-113">We analyze the [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset in this walkthrough.</span></span> <span data-ttu-id="211ae-114">Ez egy olyan van megjelölve, vagy a levélszemét, vagy a sonka (ami azt jelenti, amelyek nincsenek levélszemét), e-mailek és az e-mailek tartalmának néhány statisztikai is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="211ae-114">This is a set of emails that are marked as either spam or ham (meaning they are not spam), and also contains some statistics on the content of the emails.</span></span> <span data-ttu-id="211ae-115">A statisztikai adatokat tartalmaz a következő, de egy szakasz tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="211ae-115">The statistics included are discussed in the next but one section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="211ae-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="211ae-116">Prerequisites</span></span>
<span data-ttu-id="211ae-117">Adatok tudományos Linux virtuális gépek használata előtt az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="211ae-117">Before you can use a Linux Data Science Virtual Machine, you must have the following:</span></span>

* <span data-ttu-id="211ae-118">Egy **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="211ae-118">An **Azure subscription**.</span></span> <span data-ttu-id="211ae-119">Ha még nem rendelkezik egy, lásd: [ma létrehozása az ingyenes Azure-fiókjával](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="211ae-119">If you do not already have one, see [Create your free Azure account today](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="211ae-120">A [ **adattudomány Linux virtuális gép**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="211ae-120">A [**Linux data science VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span></span> <span data-ttu-id="211ae-121">A virtuális gép további információkért lásd: [a Linux adatok tudományos virtuális gép kiépítéséhez](machine-learning-data-science-linux-dsvm-intro.md).</span><span class="sxs-lookup"><span data-stu-id="211ae-121">For information on provisioning this VM, see [Provision the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md).</span></span>
* <span data-ttu-id="211ae-122">[X2Go](http://wiki.x2go.org/doku.php) telepítve a számítógépre, és egy XFCE munkamenet megnyitása.</span><span class="sxs-lookup"><span data-stu-id="211ae-122">[X2Go](http://wiki.x2go.org/doku.php) installed on your computer and opened an XFCE session.</span></span> <span data-ttu-id="211ae-123">Telepítésével és konfigurálásával kapcsolatos egy **X2Go ügyfél**, lásd: [telepítése és konfigurálása X2Go ügyfél](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span><span class="sxs-lookup"><span data-stu-id="211ae-123">For information on installing and configuring an **X2Go client**, see [Installing and configuring X2Go client](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span></span> 
* <span data-ttu-id="211ae-124">Egy **AzureML fiók**.</span><span class="sxs-lookup"><span data-stu-id="211ae-124">An **AzureML account**.</span></span> <span data-ttu-id="211ae-125">Ha még nem rendelkezik egy újat, Regisztráljon a [AzureML kezdőlap](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="211ae-125">If you don't already have one, sign up for new one at the [AzureML homepage](https://studio.azureml.net/).</span></span> <span data-ttu-id="211ae-126">Van egy ingyenes használati módosítása a következőre segítségére lesz.</span><span class="sxs-lookup"><span data-stu-id="211ae-126">There is a free usage tier to help you get started.</span></span>

## <a name="download-the-spambase-dataset"></a><span data-ttu-id="211ae-127">Töltse le a spambase adatkészlet</span><span class="sxs-lookup"><span data-stu-id="211ae-127">Download the spambase dataset</span></span>
<span data-ttu-id="211ae-128">A [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) adatkészlet olyan viszonylag kicsi, amely csak 4601 példákat tartalmaz adatokat.</span><span class="sxs-lookup"><span data-stu-id="211ae-128">The [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset is a relatively small set of data that contains only 4601 examples.</span></span> <span data-ttu-id="211ae-129">Ez akkor használja, ha a bemutatásához, hogy az adatok tudományos virtuális gép, mert a kulcsfontosságú szolgáltatásokat egy részénél tartja az erőforrás-követelmények mérsékelt tetszés szerinti méretét.</span><span class="sxs-lookup"><span data-stu-id="211ae-129">This is a convenient size to use when demonstrating that some of the key features of the Data Science VM as it keeps the resource requirements modest.</span></span>

> [!NOTE]
> <span data-ttu-id="211ae-130">Ez a forgatókönyv a D2 v2 méretű Linux adatok tudományos virtuális gépek hozták létre.</span><span class="sxs-lookup"><span data-stu-id="211ae-130">This walkthrough was created on a D2 v2-sized Linux Data Science Virtual Machine.</span></span> <span data-ttu-id="211ae-131">A DSVM mérete a Kalauzban leírt eljárások kezelésére képes.</span><span class="sxs-lookup"><span data-stu-id="211ae-131">This size DSVM is capable of handling the procedures in this walkthrough.</span></span>
>
>

<span data-ttu-id="211ae-132">Ha több tárhelyet igényel, további lemezeket hozhat létre, és csatolja azokat a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="211ae-132">If you need more storage space, you can create additional disks and attach them to your VM.</span></span> <span data-ttu-id="211ae-133">Ezeknek a lemezeknek állandó Azure tárhelyet használja, így az adatok megmaradjanak, akkor is, ha a kiszolgáló átméretezése miatt újra kell kiépíteni, vagy le van állítva.</span><span class="sxs-lookup"><span data-stu-id="211ae-133">These disks use persistent Azure storage, so their data is preserved even when the server is reprovisioned due to resizing or is shut down.</span></span> <span data-ttu-id="211ae-134">Adjon hozzá egy lemezt, és csatlakoztassa a virtuális Gépet, kövesse az utasításokat a [vehet fel lemezt a Linux virtuális gép](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="211ae-134">To add a disk and attach it to your VM, follow the instructions in [Add a disk to a Linux VM](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="211ae-135">Ezeket a lépéseket az Azure parancssori felület (Azure CLI), amelyen már telepítve van a DSVM használja.</span><span class="sxs-lookup"><span data-stu-id="211ae-135">These steps use the Azure Command-Line Interface (Azure CLI), which is already installed on the DSVM.</span></span> <span data-ttu-id="211ae-136">Így teljesen át a virtuális gépért ezekkel az eljárásokkal végezhető el.</span><span class="sxs-lookup"><span data-stu-id="211ae-136">So these procedures can be done entirely from the VM itself.</span></span> <span data-ttu-id="211ae-137">Egy másik lehetőség az megnövelni a tárhely [az Azure files](../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="211ae-137">Another option to increase storage is to use [Azure files](../storage/files/storage-how-to-use-files-linux.md).</span></span>

<span data-ttu-id="211ae-138">Az adatok letöltése, nyisson meg egy terminálablakot, és futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="211ae-138">To download the data, open a terminal window and run this command:</span></span>

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

<span data-ttu-id="211ae-139">Most hozzon létre egy másik fájlba, amelyen fejlécet a letöltött fájl nem rendelkezik a fejlécsor.</span><span class="sxs-lookup"><span data-stu-id="211ae-139">The downloaded file does not have a header row, so let's create another file that does have a header.</span></span> <span data-ttu-id="211ae-140">Futtassa ezt a parancsot a megfelelő fejlécekkel-fájl létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="211ae-140">Run this command to create a file with the appropriate headers:</span></span>

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

<span data-ttu-id="211ae-141">Majd összefűzésére együtt a parancs két fájlt:</span><span class="sxs-lookup"><span data-stu-id="211ae-141">Then concatenate the two files together with the command:</span></span>

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

<span data-ttu-id="211ae-142">Az adatkészlet egyes e-mail számos különböző típusú statisztikák rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="211ae-142">The dataset has several types of statistics on each email:</span></span>

* <span data-ttu-id="211ae-143">Oszlopok, például ***word\_freq\_WORD*** jelzi az e-mailben a szavakat, amelyek *WORD*.</span><span class="sxs-lookup"><span data-stu-id="211ae-143">Columns like ***word\_freq\_WORD*** indicate the percentage of words in the email that match *WORD*.</span></span> <span data-ttu-id="211ae-144">Például ha *word\_freq\_ellenőrizze* értéke 1 és 1 %-át az e-mailben szereplő minden szó volt *ellenőrizze*.</span><span class="sxs-lookup"><span data-stu-id="211ae-144">For example, if *word\_freq\_make* is 1, then 1% of all words in the email were *make*.</span></span>
* <span data-ttu-id="211ae-145">Oszlopok, például ***char\_freq\_CHAR*** , melyeket az e-mailben szereplő összes karakter jelzi *CHAR*.</span><span class="sxs-lookup"><span data-stu-id="211ae-145">Columns like ***char\_freq\_CHAR*** indicate the percentage of all characters in the email that were *CHAR*.</span></span>
* <span data-ttu-id="211ae-146">***beruházási\_futtatása\_hossza\_leghosszabb*** nagybetűvel sorozatát leghosszabb hossza.</span><span class="sxs-lookup"><span data-stu-id="211ae-146">***capital\_run\_length\_longest*** is the longest length of a sequence of capital letters.</span></span>
* <span data-ttu-id="211ae-147">***beruházási\_futtatása\_hossza\_átlagos*** nagybetűvel összes sorozatát átlagos hossza.</span><span class="sxs-lookup"><span data-stu-id="211ae-147">***capital\_run\_length\_average*** is the average length of all sequences of capital letters.</span></span>
* <span data-ttu-id="211ae-148">***beruházási\_futtatása\_hossza\_teljes*** nagybetűvel összes sorozatát teljes hossza.</span><span class="sxs-lookup"><span data-stu-id="211ae-148">***capital\_run\_length\_total*** is the total length of all sequences of capital letters.</span></span>
* <span data-ttu-id="211ae-149">***Levélszemét*** azt jelzi, hogy az e-mailt tekintették levélszemét, vagy nem (1 = levélszemét, 0 = nem levélszemét).</span><span class="sxs-lookup"><span data-stu-id="211ae-149">***spam*** indicates whether the email was considered spam or not (1 = spam, 0 = not spam).</span></span>

## <a name="explore-the-dataset-with-microsoft-r-open"></a><span data-ttu-id="211ae-150">A Microsoft R nyissa meg az adatkészlet felfedezés</span><span class="sxs-lookup"><span data-stu-id="211ae-150">Explore the dataset with Microsoft R Open</span></span>
<span data-ttu-id="211ae-151">Most az adatok vizsgálatát, és hajtsa végre az r tanulási néhány alapvető gép Az adatok tudományos VM mellékelt [Microsoft R nyitott](https://mran.revolutionanalytics.com/open/) előre telepítve.</span><span class="sxs-lookup"><span data-stu-id="211ae-151">Let's examine the data and do some basic machine learning with R. The Data Science VM comes with [Microsoft R Open](https://mran.revolutionanalytics.com/open/) pre-installed.</span></span> <span data-ttu-id="211ae-152">Az R jelen verziójában a többszálas matematikai könyvtárak egyszálas verziók jobb teljesítményt nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="211ae-152">The multithreaded math libraries in this version of R offer better performance than various single-threaded versions.</span></span> <span data-ttu-id="211ae-153">Microsoft R nyitott is reprodukálhatósági segítségével biztosítja a CRAN csomag tárház pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="211ae-153">Microsoft R Open also provides reproducibility by using a snapshot of the CRAN package repository.</span></span>

<span data-ttu-id="211ae-154">Ahhoz, hogy ebben a forgatókönyvben használt mintakódok példányait, klónozza a **Azure-Machine-tanulás-adatok-tudományos** segítségével git, amely előre telepítve van a virtuális Gépet a tárházban.</span><span class="sxs-lookup"><span data-stu-id="211ae-154">To get copies of the code samples used in this walkthrough, clone the **Azure-Machine-Learning-Data-Science** repository using git, which is pre-installed on the VM.</span></span> <span data-ttu-id="211ae-155">A git parancssorból futtassa:</span><span class="sxs-lookup"><span data-stu-id="211ae-155">From the git command line, run:</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="211ae-156">Nyisson meg egy terminálablakot, és indítson el egy új R munkamenetet az R-interaktív konzol.</span><span class="sxs-lookup"><span data-stu-id="211ae-156">Open a terminal window and start a new R session with the R interactive console.</span></span>

> [!NOTE]
> <span data-ttu-id="211ae-157">Az alábbi eljárásokat használhatja Rstudióból is.</span><span class="sxs-lookup"><span data-stu-id="211ae-157">You can also use RStudio for the following procedures.</span></span> <span data-ttu-id="211ae-158">Rstudióból telepítéséhez az adott parancs végrehajtásához:`./Desktop/DSVM\ tools/installRStudio.sh`</span><span class="sxs-lookup"><span data-stu-id="211ae-158">To install RStudio, execute this command at a terminal: `./Desktop/DSVM\ tools/installRStudio.sh`</span></span>
>
>

<span data-ttu-id="211ae-159">Importálhatja az adatokat, és állítsa be a környezetet, futtassa:</span><span class="sxs-lookup"><span data-stu-id="211ae-159">To import the data and set up the environment, run:</span></span>

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

<span data-ttu-id="211ae-160">Egyes oszlopok összefoglaló statisztikája megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="211ae-160">To see summary statistics about each column:</span></span>

    summary(data)

<span data-ttu-id="211ae-161">Az adatok különböző nézet:</span><span class="sxs-lookup"><span data-stu-id="211ae-161">For a different view of the data:</span></span>

    str(data)

<span data-ttu-id="211ae-162">Megjelenik az egyes és az első néhány értékeket az adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="211ae-162">This shows you the type of each variable and the first few values in the dataset.</span></span>

<span data-ttu-id="211ae-163">A *levélszemét* oszlop beolvasott érték, de ténylegesen egy kategorikus változó (vagy tényező).</span><span class="sxs-lookup"><span data-stu-id="211ae-163">The *spam* column was read as an integer, but it's actually a categorical variable (or factor).</span></span> <span data-ttu-id="211ae-164">A típus megadása:</span><span class="sxs-lookup"><span data-stu-id="211ae-164">To set its type:</span></span>

    data$spam <- as.factor(data$spam)

<span data-ttu-id="211ae-165">Néhány felderítő elemzés teheti a [ggplot2](http://ggplot2.org/) csomag, az R, amely már telepítve van a virtuális gép egy népszerű nyújtó grafikus könyvtár.</span><span class="sxs-lookup"><span data-stu-id="211ae-165">To do some exploratory analysis, use the [ggplot2](http://ggplot2.org/) package, a popular graphing library for R that is already installed on the VM.</span></span> <span data-ttu-id="211ae-166">Vegye figyelembe, jelenik meg a korábban, az összefoglaló adatokból statisztikák tudunk felkiáltójel karakter gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="211ae-166">Note, from the summary data displayed earlier, that we have summary statistics on the frequency of the exclamation mark character.</span></span> <span data-ttu-id="211ae-167">Most ábrázolhatók ezek gyakoriságot itt az alábbi parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="211ae-167">Let's plot those frequencies here with the following commands:</span></span>

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="211ae-168">A nulla sávon a rajzolási van döntés, mivel most selejtezni azt:</span><span class="sxs-lookup"><span data-stu-id="211ae-168">Since the zero bar is skewing the plot, let's get rid of it:</span></span>

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="211ae-169">1 kereső érdekes fent nem triviális sűrűségű van.</span><span class="sxs-lookup"><span data-stu-id="211ae-169">There is a non-trivial density above 1 that looks interesting.</span></span> <span data-ttu-id="211ae-170">Most, hogy az adatok vizsgáljuk meg:</span><span class="sxs-lookup"><span data-stu-id="211ae-170">Let's look at just that data:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="211ae-171">Ezután ossza levélszemét vs sonka szerint:</span><span class="sxs-lookup"><span data-stu-id="211ae-171">Then split it by spam vs ham:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

<span data-ttu-id="211ae-172">Ezekben a példákban lehetővé kell tennie, hogy más oszlop alapján megismerheti a bennük található adatok hasonló előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="211ae-172">These examples should enable you to make similar plots of the other columns to explore the data contained in them.</span></span>

## <a name="train-and-test-an-ml-model"></a><span data-ttu-id="211ae-173">Betanítása és tesztelni az ML-modell</span><span class="sxs-lookup"><span data-stu-id="211ae-173">Train and test an ML model</span></span>
<span data-ttu-id="211ae-174">Most tegyük a machine learning modellek az adatkészletben, span vagy sonka tartalmazzon, az e-mailek besorolására néhány betanításához.</span><span class="sxs-lookup"><span data-stu-id="211ae-174">Now let's train a couple of machine learning models to classify the emails in the dataset as containing either span or ham.</span></span> <span data-ttu-id="211ae-175">Azt a döntési fa modellek és ebben a szakaszban egy véletlenszerű erdő modell betanítását, és tesztelje a azok az előrejelzés pontosságát.</span><span class="sxs-lookup"><span data-stu-id="211ae-175">We train a decision tree model and a random forest model in this section and then test their accuracy of their predictions.</span></span>

> [!NOTE]
> <span data-ttu-id="211ae-176">Az alábbi kódban használt rpart (rekurzív particionálás és regressziós fák) csomag már telepítve van az adatok tudományos virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="211ae-176">The rpart (Recursive Partitioning and Regression Trees) package used in the following code is already installed on the Data Science VM.</span></span>
>
>

<span data-ttu-id="211ae-177">Első, az adatkészlet most felosztása tanítási és tesztelési beállítása:</span><span class="sxs-lookup"><span data-stu-id="211ae-177">First, let's split the dataset into training and test sets:</span></span>

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

<span data-ttu-id="211ae-178">És az e-mailek besorolására a döntési fában.</span><span class="sxs-lookup"><span data-stu-id="211ae-178">And then create a decision tree to classify the emails.</span></span>

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

<span data-ttu-id="211ae-179">Az eredmény a következő:</span><span class="sxs-lookup"><span data-stu-id="211ae-179">Here is the result:</span></span>

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

<span data-ttu-id="211ae-181">Határozza meg, milyen mértékben a gyakorlókészlethez elvégez, használja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="211ae-181">To determine how well it performs on the training set, use the following code:</span></span>

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="211ae-182">Annak meghatározásához, hogy milyen mértékben a TesztKészlet a hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="211ae-182">To determine how well it performs on the test set:</span></span>

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="211ae-183">Próbáljuk meg is véletlenszerű erdőmodell.</span><span class="sxs-lookup"><span data-stu-id="211ae-183">Let's also try a random forest model.</span></span> <span data-ttu-id="211ae-184">Véletlenszerű erdők számos döntési fák betanítása, és kimeneti egy osztály, amely az összes az egyes döntési fák besorolások mód.</span><span class="sxs-lookup"><span data-stu-id="211ae-184">Random forests train a multitude of decision trees and output a class that is the mode of the classifications from all of the individual decision trees.</span></span> <span data-ttu-id="211ae-185">Nagyobb teljesítményű machine learning-módszer, azok küszöbölje ki a döntési fa modell hajlamos képzési dataset overfit biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="211ae-185">They provide a more powerful machine learning approach as they correct for the tendency of a decision tree model to overfit a training dataset.</span></span>

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a><span data-ttu-id="211ae-186">A modell rendszerbe állítása Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="211ae-186">Deploy a model to Azure ML</span></span>
<span data-ttu-id="211ae-187">[Az Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) egy felhőszolgáltatás, amely segítségével egyszerűen és üzembe prediktív elemzési modellek.</span><span class="sxs-lookup"><span data-stu-id="211ae-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) is a cloud service that makes it easy to build and deploy predictive analytics models.</span></span> <span data-ttu-id="211ae-188">Az AzureML töltött funkcióit egyik közzététele az R funkció webszolgáltatásként való képességét.</span><span class="sxs-lookup"><span data-stu-id="211ae-188">One of the nice features of AzureML is its ability to publish any R function as a web service.</span></span> <span data-ttu-id="211ae-189">Az AzureML R csomagot közvetlenül a az R munkamenetet a DSVM elvégzéséhez egyszerűen központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="211ae-189">The AzureML R package makes deployment easy to do right from our R session on the DSVM.</span></span>

<span data-ttu-id="211ae-190">A döntési fa kódot a fenti szakaszban leírt központi telepítéséhez kell bejelentkezni az Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="211ae-190">To deploy the decision tree code from the previous section, you need to sign in to Azure Machine Learning Studio.</span></span> <span data-ttu-id="211ae-191">A munkaterület azonosítója és egy engedélyezési jogkivonatot való bejelentkezéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="211ae-191">You need your workspace ID and an authorization token to sign in.</span></span> <span data-ttu-id="211ae-192">Ezek az értékek található, és a velük AzureML változók inicializálása:</span><span class="sxs-lookup"><span data-stu-id="211ae-192">To find these values and initialize the AzureML variables with them:</span></span>

<span data-ttu-id="211ae-193">Válassza ki **beállítások** a bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="211ae-193">Select **Settings** on the left-hand menu.</span></span> <span data-ttu-id="211ae-194">Megjegyzés: a **MUNKATERÜLET azonosítója**.</span><span class="sxs-lookup"><span data-stu-id="211ae-194">Note your **WORKSPACE ID**.</span></span> <span data-ttu-id="211ae-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span><span class="sxs-lookup"><span data-stu-id="211ae-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span></span>

<span data-ttu-id="211ae-196">Válassza ki **engedélyezési jogkivonatok** az általános menüből, és vegye figyelembe a **elsődleges engedélyezési jogkivonat**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span><span class="sxs-lookup"><span data-stu-id="211ae-196">Select **Authorization Tokens** from the overhead menu and note your **Primary Authorization Token**.![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span></span>

<span data-ttu-id="211ae-197">Betöltési a **AzureML** csomag és utána állítsa be a jogkivonatot, és a munkaterület Azonosítóját a változók értékeit az R-munkamenetben a DSVM meg:</span><span class="sxs-lookup"><span data-stu-id="211ae-197">Load the **AzureML** package and then set values of the variables with your token and workspace ID in your R session on the DSVM:</span></span>

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


<span data-ttu-id="211ae-198">Most egyszerűsítése ebben a bemutatóban egyszerűbb, hogy a modell.</span><span class="sxs-lookup"><span data-stu-id="211ae-198">Let's simplify the model to make this demonstration easier to implement.</span></span> <span data-ttu-id="211ae-199">Válassza ki a három változók a döntési fa gyökere legközelebbi, és csak a megadott három változók használata új fa létrehozása:</span><span class="sxs-lookup"><span data-stu-id="211ae-199">Pick the three variables in the decision tree closest to the root and build a new tree using just those three variables:</span></span>

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

<span data-ttu-id="211ae-200">Igazolnia kell a előrejelző függvényben a szolgáltatások veszi bemenetként, és visszaadja az előre jelzett értékek:</span><span class="sxs-lookup"><span data-stu-id="211ae-200">We need a prediction function that takes the features as an input and returns the predicted values:</span></span>

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

<span data-ttu-id="211ae-201">A predictSpam függvény közzétételére az AzureML használatával a **publishWebService** függvény:</span><span class="sxs-lookup"><span data-stu-id="211ae-201">Publish the predictSpam function to AzureML using the **publishWebService** function:</span></span>

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

<span data-ttu-id="211ae-202">Ez a függvény a **predictSpam** működik, létrehoz egy nevű webszolgáltatás-bővítmény **spamWebService** a meghatározni a bemenetekhez és kimenetekhez, és az új végpont információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="211ae-202">This function takes the **predictSpam** function, creates a web service named **spamWebService** with defined inputs and outputs, and returns information about the new endpoint.</span></span>

<span data-ttu-id="211ae-203">A közzétett webes szolgáltatás, így az API-végpont részleteinek megtekintése, és hozzáférési kulcsokkal paranccsal:</span><span class="sxs-lookup"><span data-stu-id="211ae-203">View details of the published web service, including its API endpoint and access keys with the command:</span></span>

    spamWebService[[2]]

<span data-ttu-id="211ae-204">Próbálja ki az első 10 sor a vizsgálat beállítása:</span><span class="sxs-lookup"><span data-stu-id="211ae-204">To try it out on the first 10 rows of the test set:</span></span>

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a><span data-ttu-id="211ae-205">Rendelkezésre álló eszközök</span><span class="sxs-lookup"><span data-stu-id="211ae-205">Use other tools available</span></span>
<span data-ttu-id="211ae-206">A fennmaradó részei bemutatják, hogyan használhatja néhány a Linux adatok tudományos virtuális Gépre telepített eszközök. A tárgyalt eszközök listája itt található:</span><span class="sxs-lookup"><span data-stu-id="211ae-206">The remaining sections show how to use some of the tools installed on the Linux Data Science VM.Here is the list of tools discussed:</span></span>

* <span data-ttu-id="211ae-207">XGBoost</span><span class="sxs-lookup"><span data-stu-id="211ae-207">XGBoost</span></span>
* <span data-ttu-id="211ae-208">Python</span><span class="sxs-lookup"><span data-stu-id="211ae-208">Python</span></span>
* <span data-ttu-id="211ae-209">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="211ae-209">Jupyterhub</span></span>
* <span data-ttu-id="211ae-210">Rattle</span><span class="sxs-lookup"><span data-stu-id="211ae-210">Rattle</span></span>
* <span data-ttu-id="211ae-211">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="211ae-211">PostgreSQL & Squirrel SQL</span></span>
* <span data-ttu-id="211ae-212">SQL Server-adatraktár</span><span class="sxs-lookup"><span data-stu-id="211ae-212">SQL Server Data Warehouse</span></span>

## <a name="xgboost"></a><span data-ttu-id="211ae-213">XGBoost</span><span class="sxs-lookup"><span data-stu-id="211ae-213">XGBoost</span></span>
<span data-ttu-id="211ae-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) olyan eszköz, amely gyors és pontos súlyozott fa valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="211ae-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) is a tool that provides a fast and accurate boosted tree implementation.</span></span>

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

<span data-ttu-id="211ae-215">XGBoost is meghívhatja a python vagy a parancssorból végzi.</span><span class="sxs-lookup"><span data-stu-id="211ae-215">XGBoost can also call from python or a command line.</span></span>

## <a name="python"></a><span data-ttu-id="211ae-216">Python</span><span class="sxs-lookup"><span data-stu-id="211ae-216">Python</span></span>
<span data-ttu-id="211ae-217">A fejlesztési pythonos környezetekben az Anaconda Python azokat a terjesztéseket 2.7 és 3.5-ös telepítve vannak a DSVM.</span><span class="sxs-lookup"><span data-stu-id="211ae-217">For development using Python, the Anaconda Python distributions 2.7 and 3.5 have been installed in the DSVM.</span></span>

> [!NOTE]
> <span data-ttu-id="211ae-218">A Anaconda terjesztési tartalmaz [Condas](http://conda.pydata.org/docs/index.html), amelyek segítségével hozzon létre egyéni környezeteket, amelyek különböző verziói és/vagy a bennük foglalt telepített csomagok Python.</span><span class="sxs-lookup"><span data-stu-id="211ae-218">The Anaconda distribution includes [Condas](http://conda.pydata.org/docs/index.html), which can be used to create custom environments for Python that have different versions and/or packages installed in them.</span></span>
>
>

<span data-ttu-id="211ae-219">Most a spambase adatkészlet egyes olvasása és az e-mailek besorolására scikit a támogatási vektoros gépekkel – ismerje meg:</span><span class="sxs-lookup"><span data-stu-id="211ae-219">Let's read in some of the spambase dataset and classify the emails with support vector machines in scikit-learn:</span></span>

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="211ae-220">A előrejelzéseket készítsen:</span><span class="sxs-lookup"><span data-stu-id="211ae-220">To make predictions:</span></span>

    clf.predict(X.ix[0:20, :])

<span data-ttu-id="211ae-221">Megjelenítése a közzététele AzureML végpont, most Meggyőződünk egy egyszerűbb modell a három változók, ahogy azt korábban nyitva jelenleg az R-modell.</span><span class="sxs-lookup"><span data-stu-id="211ae-221">To show how to publish an AzureML endpoint, let's make a simpler model the three variables as we did when we published the R model previously.</span></span>

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="211ae-222">A modell közzétételére AzureML:</span><span class="sxs-lookup"><span data-stu-id="211ae-222">To publish the model to AzureML:</span></span>

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> <span data-ttu-id="211ae-223">Ez a lehetőség csak a python 2.7, és még nem támogatott a 3.5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="211ae-223">This is only available for python 2.7 and is not yet supported on 3.5.</span></span> <span data-ttu-id="211ae-224">Futtassa a **/anaconda/bin/python2.7**.</span><span class="sxs-lookup"><span data-stu-id="211ae-224">Run with **/anaconda/bin/python2.7**.</span></span>
>
>

## <a name="jupyterhub"></a><span data-ttu-id="211ae-225">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="211ae-225">Jupyterhub</span></span>
<span data-ttu-id="211ae-226">A Anaconda terjesztési a DSVM a Jupyter notebook, olyan többplatformos környezetben, a Python, R vagy Ágnes kóddal és elemzési tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="211ae-226">The Anaconda distribution in the DSVM comes with a Jupyter notebook, a cross-platform environment to share Python, R, or Julia code and analysis.</span></span> <span data-ttu-id="211ae-227">A Jupyter notebook JupyterHub keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="211ae-227">The Jupyter notebook is accessed through JupyterHub.</span></span> <span data-ttu-id="211ae-228">A helyi Linux-felhasználónév és jelszó használatával bejelentkezik ***https://\<VM DNS-nevét vagy IP-cím\>: 8000 /***.</span><span class="sxs-lookup"><span data-stu-id="211ae-228">You sign in using your local Linux user name and password at ***https://\<VM DNS name or IP Address\>:8000/***.</span></span> <span data-ttu-id="211ae-229">Minden konfigurációs fájlt a JupyterHub könyvtárban találhatók **/etc/jupyterhub**.</span><span class="sxs-lookup"><span data-stu-id="211ae-229">All configuration files for JupyterHub are found in directory **/etc/jupyterhub**.</span></span>

<span data-ttu-id="211ae-230">Több minta jegyzetfüzetet már telepítve van a virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="211ae-230">Several sample notebooks are already installed on the VM:</span></span>

* <span data-ttu-id="211ae-231">Tekintse meg a [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) egy minta Python jegyzetfüzet.</span><span class="sxs-lookup"><span data-stu-id="211ae-231">See the [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) for a sample Python notebook.</span></span>
* <span data-ttu-id="211ae-232">Lásd: [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) egy minta **R** notebookot.</span><span class="sxs-lookup"><span data-stu-id="211ae-232">See [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) for a sample **R** notebook.</span></span>
* <span data-ttu-id="211ae-233">Tekintse meg a [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) egy másik minta **Python** notebookot.</span><span class="sxs-lookup"><span data-stu-id="211ae-233">See the [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) for another sample **Python** notebook.</span></span>

> [!NOTE]
> <span data-ttu-id="211ae-234">Ágnes nyelvét a Linux adatok tudományos virtuális Gépen a parancssorból is érhető el.</span><span class="sxs-lookup"><span data-stu-id="211ae-234">The Julia language is also available from the command line on the Linux Data Science VM.</span></span>
>
>

## <a name="rattle"></a><span data-ttu-id="211ae-235">Rattle</span><span class="sxs-lookup"><span data-stu-id="211ae-235">Rattle</span></span>
<span data-ttu-id="211ae-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (az R analitikai eszköz a további könnyen) az adatbányászathoz grafikus R eszközzel.</span><span class="sxs-lookup"><span data-stu-id="211ae-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (the R Analytical Tool To Learn Easily) is a graphical R tool for data mining.</span></span> <span data-ttu-id="211ae-237">Rendelkezik egy intuitív felület, amellyel könnyedén betölteni, vizsgálatát, és adatok átalakítása és build és modellek értékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="211ae-237">It has an intuitive interface that makes it easy to load, explore, and transform data and build and evaluate models.</span></span>  <span data-ttu-id="211ae-238">A cikk [Rattle: A Data Mining GUI az R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) biztosít a forgatókönyv azt mutatja be, a szolgáltatásai.</span><span class="sxs-lookup"><span data-stu-id="211ae-238">The article [Rattle: A Data Mining GUI for R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) provides a walkthrough that demonstrates its features.</span></span>

<span data-ttu-id="211ae-239">Telepítse, és indítsa el a Rattle az alábbi parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="211ae-239">Install and start Rattle with the following commands:</span></span>

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> <span data-ttu-id="211ae-240">Telepítés nem szükséges a DSVM.</span><span class="sxs-lookup"><span data-stu-id="211ae-240">Installation is not required on the DSVM.</span></span> <span data-ttu-id="211ae-241">De Rattle késztethetik további csomagok telepítése, ha betölti a.</span><span class="sxs-lookup"><span data-stu-id="211ae-241">But Rattle may prompt you to install additional packages when it loads.</span></span>
>
>

<span data-ttu-id="211ae-242">Rattle egy lapon-alapú felületet használja.</span><span class="sxs-lookup"><span data-stu-id="211ae-242">Rattle uses a tab-based interface.</span></span> <span data-ttu-id="211ae-243">A lapok többsége felel meg a lépéseket a [adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), például az adatok betöltése vagy felfedezését.</span><span class="sxs-lookup"><span data-stu-id="211ae-243">Most of the tabs correspond to steps in the [Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), like loading data or exploring it.</span></span> <span data-ttu-id="211ae-244">Az tudományos folyamat zajlik balról jobbra a lapfülekre.</span><span class="sxs-lookup"><span data-stu-id="211ae-244">The data science process flows from left to right through the tabs.</span></span> <span data-ttu-id="211ae-245">De az utolsó lap tartalmazza az R-parancsok Rattle futtatásához.</span><span class="sxs-lookup"><span data-stu-id="211ae-245">But the last tab contains a log of the R commands run by Rattle.</span></span>

<span data-ttu-id="211ae-246">Betölteni, és konfigurálja az adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="211ae-246">To load and configure the dataset:</span></span>

* <span data-ttu-id="211ae-247">Töltse be a fájlt, jelölje be a **adatok** lapra, majd</span><span class="sxs-lookup"><span data-stu-id="211ae-247">To load the file, select the **Data** tab, then</span></span>
* <span data-ttu-id="211ae-248">Válassza ki a választó melletti **Fájlnév** válassza **spambaseHeaders.data**.</span><span class="sxs-lookup"><span data-stu-id="211ae-248">Choose the selector next to **Filename** and choose **spambaseHeaders.data**.</span></span>
* <span data-ttu-id="211ae-249">A fájl betöltése.</span><span class="sxs-lookup"><span data-stu-id="211ae-249">To load the file.</span></span> <span data-ttu-id="211ae-250">Válassza ki **Execute** gombok legfelső sorában.</span><span class="sxs-lookup"><span data-stu-id="211ae-250">select **Execute** in the top row of buttons.</span></span> <span data-ttu-id="211ae-251">Megtekintheti az egyes oszlopok, beleértve az azonosított adattípushoz, hogy bemeneti, cél vagy más típusú változó, és egyedi értékek számának összegzését.</span><span class="sxs-lookup"><span data-stu-id="211ae-251">You should see a summary of each column, including its identified data type, whether it's an input, a target, or other type of variable, and the number of unique values.</span></span>
* <span data-ttu-id="211ae-252">Rattle helyesen észlelt a **levélszemét** oszlop céljaként.</span><span class="sxs-lookup"><span data-stu-id="211ae-252">Rattle has correctly identified the **spam** column as the target.</span></span> <span data-ttu-id="211ae-253">Válassza ki a levélszemét oszlopot, majd állítsa be a **cél adattípus** való **Categoric**.</span><span class="sxs-lookup"><span data-stu-id="211ae-253">Select the spam column, then set the **Target Data Type** to **Categoric**.</span></span>

<span data-ttu-id="211ae-254">Ásna az adatokban:</span><span class="sxs-lookup"><span data-stu-id="211ae-254">To explore the data:</span></span>

* <span data-ttu-id="211ae-255">Válassza ki a **böngészés** fülre.</span><span class="sxs-lookup"><span data-stu-id="211ae-255">Select the **Explore** tab.</span></span>
* <span data-ttu-id="211ae-256">Kattintson a **összefoglaló**, majd **Execute**, a változó típusok kapcsolatos információkat, és néhány statisztikák.</span><span class="sxs-lookup"><span data-stu-id="211ae-256">Click **Summary**, then **Execute**, to see some information about the variable types and some summary statistics.</span></span>
* <span data-ttu-id="211ae-257">Válassza ki, ha az egyes statisztikája más típusú egyéb beállítások, például a **adja** vagy **alapjai**.</span><span class="sxs-lookup"><span data-stu-id="211ae-257">To view other types of statistics about each variable, select other options like **Describe** or **Basics**.</span></span>

<span data-ttu-id="211ae-258">A **böngészés** lapon is lehetővé teszi számos osztályon előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="211ae-258">The **Explore** tab also allows you to generate many insightful plots.</span></span> <span data-ttu-id="211ae-259">Az adatok hisztogram megrajzolásához:</span><span class="sxs-lookup"><span data-stu-id="211ae-259">To plot a histogram of the data:</span></span>

* <span data-ttu-id="211ae-260">Válassza ki **Terjesztéseket**.</span><span class="sxs-lookup"><span data-stu-id="211ae-260">Select **Distributions**.</span></span>
* <span data-ttu-id="211ae-261">Ellenőrizze **hisztogram** a **word_freq_remove** és **word_freq_you**.</span><span class="sxs-lookup"><span data-stu-id="211ae-261">Check **Histogram** for **word_freq_remove** and **word_freq_you**.</span></span>
* <span data-ttu-id="211ae-262">Válassza ki **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="211ae-262">Select **Execute**.</span></span> <span data-ttu-id="211ae-263">Meg kell jelennie egy grafikonon ablakban, amennyiben egyértelmű, hogy a word "Ön" sokkal gyakran megjelenik az e-mailek, mint az "Eltávolítás" mindkét sűrűség előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="211ae-263">You should see both density plots in a single graph window, where it is clear that the word "you" appears much more frequently in emails than "remove".</span></span>

<span data-ttu-id="211ae-264">A korrelációs előkészítésére érdekes egyaránt.</span><span class="sxs-lookup"><span data-stu-id="211ae-264">The Correlation plots are also interesting.</span></span> <span data-ttu-id="211ae-265">A létrehozáshoz:</span><span class="sxs-lookup"><span data-stu-id="211ae-265">To create one:</span></span>

* <span data-ttu-id="211ae-266">Válasszon **korrelációs** , a **típus**, majd</span><span class="sxs-lookup"><span data-stu-id="211ae-266">Choose **Correlation** as the **Type**, then</span></span>
* <span data-ttu-id="211ae-267">Válassza ki **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="211ae-267">Select **Execute**.</span></span>
* <span data-ttu-id="211ae-268">Rattle figyelmezteti, hogy legfeljebb 40 változók javasol.</span><span class="sxs-lookup"><span data-stu-id="211ae-268">Rattle warns you that it recommends a maximum of 40 variables.</span></span> <span data-ttu-id="211ae-269">Válassza ki **Igen** a rajzolási megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="211ae-269">Select **Yes** to view the plot.</span></span>

<span data-ttu-id="211ae-270">Van néhány érdekes közti korrelációk elérni: "technológia" erősen visszamenőleges korrelációban állnak "HP" és "labs", például.</span><span class="sxs-lookup"><span data-stu-id="211ae-270">There are some interesting correlations that come up: "technology" is strongly correlated to "HP" and "labs", for example.</span></span> <span data-ttu-id="211ae-271">Azt is erősen visszamenőleges korrelációban állnak, a "650", mert az adatkészlet adók, a körzetszámot 650.</span><span class="sxs-lookup"><span data-stu-id="211ae-271">It is also strongly correlated to "650", because the area code of the dataset donors is 650.</span></span>

<span data-ttu-id="211ae-272">A numerikus érték, a szavak közötti összefüggések az Intéző ablakában érhetők el.</span><span class="sxs-lookup"><span data-stu-id="211ae-272">The numeric values for the correlations between words are available in the Explore window.</span></span> <span data-ttu-id="211ae-273">Érdemes megjegyzés, például, hogy "technológia" negatívan tartozzanak "a" és "pénzt".</span><span class="sxs-lookup"><span data-stu-id="211ae-273">It is interesting to note, for example, that "technology" is negatively correlated with "your" and "money".</span></span>

<span data-ttu-id="211ae-274">Rattle alakíthatja át a dataset gyakori problémákat kezelésére.</span><span class="sxs-lookup"><span data-stu-id="211ae-274">Rattle can transform the dataset to handle some common issues.</span></span> <span data-ttu-id="211ae-275">Például lehetővé teszi szolgáltatások átméretezése, imputálására a hiányzó értékeket, kiugró kezelni és változók vagy megfigyelések hiányzó adatok eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="211ae-275">For example, it allows you to rescale features, impute missing values, handle outliers, and remove variables or observations with missing data.</span></span> <span data-ttu-id="211ae-276">Rattle is azonosíthatja a társítási szabályok megfigyelések és/vagy változók között.</span><span class="sxs-lookup"><span data-stu-id="211ae-276">Rattle can also identify association rules between observations and/or variables.</span></span> <span data-ttu-id="211ae-277">A lapok kívül esnek a hatókörön bevezető forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="211ae-277">These tabs are out of scope for this introductory walkthrough.</span></span>

<span data-ttu-id="211ae-278">Rattle fürt analysis is végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="211ae-278">Rattle can also perform cluster analysis.</span></span> <span data-ttu-id="211ae-279">Most kizárása egyes funkciók a kimeneti könnyebb átláthatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="211ae-279">Let's exclude some features to make the output easier to read.</span></span> <span data-ttu-id="211ae-280">Az a **adatok** lapra, majd **figyelmen kívül hagyása** mellett minden egyes tíz elemekhez kivételével a változók:</span><span class="sxs-lookup"><span data-stu-id="211ae-280">On the **Data** tab, choose **Ignore** next to each of the variables except these ten items:</span></span>

* <span data-ttu-id="211ae-281">word_freq_hp</span><span class="sxs-lookup"><span data-stu-id="211ae-281">word_freq_hp</span></span>
* <span data-ttu-id="211ae-282">word_freq_technology</span><span class="sxs-lookup"><span data-stu-id="211ae-282">word_freq_technology</span></span>
* <span data-ttu-id="211ae-283">word_freq_george</span><span class="sxs-lookup"><span data-stu-id="211ae-283">word_freq_george</span></span>
* <span data-ttu-id="211ae-284">word_freq_remove</span><span class="sxs-lookup"><span data-stu-id="211ae-284">word_freq_remove</span></span>
* <span data-ttu-id="211ae-285">word_freq_your</span><span class="sxs-lookup"><span data-stu-id="211ae-285">word_freq_your</span></span>
* <span data-ttu-id="211ae-286">word_freq_dollar</span><span class="sxs-lookup"><span data-stu-id="211ae-286">word_freq_dollar</span></span>
* <span data-ttu-id="211ae-287">word_freq_money</span><span class="sxs-lookup"><span data-stu-id="211ae-287">word_freq_money</span></span>
* <span data-ttu-id="211ae-288">capital_run_length_longest</span><span class="sxs-lookup"><span data-stu-id="211ae-288">capital_run_length_longest</span></span>
* <span data-ttu-id="211ae-289">word_freq_business</span><span class="sxs-lookup"><span data-stu-id="211ae-289">word_freq_business</span></span>
* <span data-ttu-id="211ae-290">Levélszemét</span><span class="sxs-lookup"><span data-stu-id="211ae-290">spam</span></span>

<span data-ttu-id="211ae-291">Ezután térjen vissza a **fürt** lapra, majd **KMeans**, és állítsa be a *számát* 4.</span><span class="sxs-lookup"><span data-stu-id="211ae-291">Then go back to the **Cluster** tab, choose **KMeans**, and set the *Number of clusters* to 4.</span></span> <span data-ttu-id="211ae-292">Majd **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="211ae-292">Then **Execute**.</span></span> <span data-ttu-id="211ae-293">Az eredmények a kimeneti ablakban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="211ae-293">The results are displayed in the output window.</span></span> <span data-ttu-id="211ae-294">Egy fürt nagy gyakoriságú "hp" és "nagy szóval" rendelkezik, és valószínűleg egy valós üzleti e-mailt.</span><span class="sxs-lookup"><span data-stu-id="211ae-294">One cluster has high frequency of "george" and "hp" and is probably a legitimate business email.</span></span>

<span data-ttu-id="211ae-295">Egy egyszerű döntési fa gépi tanulási modellek létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="211ae-295">To build a simple decision tree machine learning model:</span></span>

* <span data-ttu-id="211ae-296">Válassza ki a **modell** lap</span><span class="sxs-lookup"><span data-stu-id="211ae-296">Select the **Model** tab,</span></span>
* <span data-ttu-id="211ae-297">Válasszon **fa** , a **típus**.</span><span class="sxs-lookup"><span data-stu-id="211ae-297">Choose **Tree** as the **Type**.</span></span>
* <span data-ttu-id="211ae-298">Válassza ki **Execute** szöveg űrlap a kimeneti ablakban a fa megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="211ae-298">Select **Execute** to display the tree in text form in the output window.</span></span>
* <span data-ttu-id="211ae-299">Válassza ki a **megrajzolásához** gombra kattintva megtekintheti az olyan grafikus verziója.</span><span class="sxs-lookup"><span data-stu-id="211ae-299">Select the **Draw** button to view a graphical version.</span></span> <span data-ttu-id="211ae-300">Ez elég hasonlít a fához azt a korábban beszerzett *rpart*.</span><span class="sxs-lookup"><span data-stu-id="211ae-300">This looks quite similar to the tree we obtained earlier using *rpart*.</span></span>

<span data-ttu-id="211ae-301">A Rattle töltött szolgáltatásait egyik képességét a számos machine learning módszer futtatásához, és gyorsan értékeli őket.</span><span class="sxs-lookup"><span data-stu-id="211ae-301">One of the nice features of Rattle is its ability to run several machine learning methods and quickly evaluate them.</span></span> <span data-ttu-id="211ae-302">A folyamat során a rendszer:</span><span class="sxs-lookup"><span data-stu-id="211ae-302">Here is the procedure:</span></span>

* <span data-ttu-id="211ae-303">Válasszon **összes** a a **típus**.</span><span class="sxs-lookup"><span data-stu-id="211ae-303">Choose **All** for the **Type**.</span></span>
* <span data-ttu-id="211ae-304">Válassza ki **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="211ae-304">Select **Execute**.</span></span>
* <span data-ttu-id="211ae-305">Művelet befejeződése után kattintson a bármely egyetlen **típus**, például **SVM**, és tekintse meg az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="211ae-305">After it finishes you can click any single **Type**, like **SVM**, and view the results.</span></span>
* <span data-ttu-id="211ae-306">Összehasonlíthatja a teljesítmény a modellek segítségével ellenőrzéséről a **Evaluate** fülre. Például a **hiba mátrix** kijelölés megjeleníti a a félreértések mátrix, általános hiba, és az egyes átlagolt osztály hiba az érvényesítési beállítása a.</span><span class="sxs-lookup"><span data-stu-id="211ae-306">You can also compare the performance of the models on the validation set using the **Evaluate** tab. For example, the **Error Matrix** selection shows you the confusion matrix, overall error, and averaged class error for each model on the validation set.</span></span>
* <span data-ttu-id="211ae-307">ROC görbék megrajzolásához, érzékenységi elemzést, és hajtsa végre a modell értékelések más típusú.</span><span class="sxs-lookup"><span data-stu-id="211ae-307">You can also plot ROC curves, perform sensitivity analysis, and do other types of model evaluations.</span></span>

<span data-ttu-id="211ae-308">Miután elkészült, létrehozási modelleket, válassza ki a **napló** fülre kattintva megtekintheti az R-kód Rattle futtatásához a munkamenet során.</span><span class="sxs-lookup"><span data-stu-id="211ae-308">Once you're finished building models, select the **Log** tab to view the R code run by Rattle during your session.</span></span> <span data-ttu-id="211ae-309">Kiválaszthatja a **exportálása** gombra kattintva mentse.</span><span class="sxs-lookup"><span data-stu-id="211ae-309">You can select the **Export** button to save it.</span></span>

> [!NOTE]
> <span data-ttu-id="211ae-310">Hogy programhiba van Rattle jelenlegi kiadásában.</span><span class="sxs-lookup"><span data-stu-id="211ae-310">There is a bug in current release of Rattle.</span></span> <span data-ttu-id="211ae-311">A parancsfájl módosításával, vagy ismételje meg a későbbi segítségével, egy # karaktert elé kell beilleszteni * exportálása... Ez a napló * a szöveges napló.</span><span class="sxs-lookup"><span data-stu-id="211ae-311">To modify the script or use it to repeat your steps later, you must insert a # character in front of *Export this log ... * in the text of the log.</span></span>
>
>

## <a name="postgresql--squirrel-sql"></a><span data-ttu-id="211ae-312">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="211ae-312">PostgreSQL & Squirrel SQL</span></span>
<span data-ttu-id="211ae-313">A DSVM PostgreSQL telepítve rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="211ae-313">The DSVM comes with PostgreSQL installed.</span></span> <span data-ttu-id="211ae-314">PostgreSQL egy olyan kifinomult, nyílt forráskódú relációs adatbázis.</span><span class="sxs-lookup"><span data-stu-id="211ae-314">PostgreSQL is a sophisticated, open-source relational database.</span></span> <span data-ttu-id="211ae-315">Ez a szakasz bemutatja, hogyan a levélszemét adatkészlet betöltése a PostgreSQL és majd lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="211ae-315">This section shows how to load our spam dataset into PostgreSQL and then query it.</span></span>

<span data-ttu-id="211ae-316">Az adatok betöltése előtt szeretné engedélyezni a localhost jelszó-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="211ae-316">Before you can load the data, you need to allow password authentication from the localhost.</span></span> <span data-ttu-id="211ae-317">A parancssorba:</span><span class="sxs-lookup"><span data-stu-id="211ae-317">At a command prompt:</span></span>

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

<span data-ttu-id="211ae-318">A konfigurációs fájl alján, amelyek az engedélyezett kapcsolatok több sorba a következők:</span><span class="sxs-lookup"><span data-stu-id="211ae-318">Near the bottom of the config file are several lines that detail the allowed connections:</span></span>

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

<span data-ttu-id="211ae-319">Módosítsa a "Helyi kapcsolatok IPv4" sort használatát md5 helyett ident, azt is bejelentkezhetnek a felhasználónévvel és jelszóval:</span><span class="sxs-lookup"><span data-stu-id="211ae-319">Change the "IPv4 local connections" line to use md5 instead of ident, so we can log in using a username and password:</span></span>

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

<span data-ttu-id="211ae-320">Majd indítsa újra a postgres szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="211ae-320">And restart the postgres service:</span></span>

    sudo systemctl restart postgresql

<span data-ttu-id="211ae-321">Az interaktív terminál PostgreSQL, a beépített postgres felhasználóként a psql elindításához futtassa a következő parancsot a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="211ae-321">To launch psql, an interactive terminal for PostgreSQL, as the built-in postgres user, run the following command from a prompt:</span></span>

    sudo -u postgres psql

<span data-ttu-id="211ae-322">Hozzon létre egy új felhasználói fiókot, amely a azonos felhasználónév használata a Linux fiókként van jelenleg a következőként bejelentkezve, és adjon neki egy jelszó:</span><span class="sxs-lookup"><span data-stu-id="211ae-322">Create a new user account, using the same username as the Linux account you're currently logged in as, and give it a password:</span></span>

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

<span data-ttu-id="211ae-323">Majd jelentkezzen be psql a felhasználó:</span><span class="sxs-lookup"><span data-stu-id="211ae-323">Then log in to psql as your user:</span></span>

    psql

<span data-ttu-id="211ae-324">És az adatok importálása egy új adatbázist:</span><span class="sxs-lookup"><span data-stu-id="211ae-324">And import the data into a new database:</span></span>

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

<span data-ttu-id="211ae-325">Most tegyük feltérképezheti az adatokat, és egyes lekérdezések futtatása használatával **Squirrel SQL**, egy grafikus eszközt, amely lehetővé teszi az adatbázisok JDBC-illesztőt keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="211ae-325">Now, let's explore the data and run some queries using **Squirrel SQL**, a graphical tool that lets you interact with databases via a JDBC driver.</span></span>

<span data-ttu-id="211ae-326">A kezdéshez indítsa el az alkalmazás menüből Squirrel SQL.</span><span class="sxs-lookup"><span data-stu-id="211ae-326">To get started, launch Squirrel SQL from the Applications menu.</span></span> <span data-ttu-id="211ae-327">Az illesztőprogram telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="211ae-327">To set up the driver:</span></span>

* <span data-ttu-id="211ae-328">Válassza ki **Windows**, majd **illesztőprogram megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="211ae-328">Select **Windows**, then **View Drivers**.</span></span>
* <span data-ttu-id="211ae-329">Kattintson a jobb gombbal a **PostgreSQL** válassza **módosítása illesztőprogram**.</span><span class="sxs-lookup"><span data-stu-id="211ae-329">Right-click on **PostgreSQL** and select **Modify Driver**.</span></span>
* <span data-ttu-id="211ae-330">Válassza ki **Extra osztály az elérési út**, majd **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="211ae-330">Select **Extra Class Path**, then **Add**.</span></span>
* <span data-ttu-id="211ae-331">Adja meg ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** a a **Fájlnév** és</span><span class="sxs-lookup"><span data-stu-id="211ae-331">Enter ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** for the **File Name** and</span></span>
* <span data-ttu-id="211ae-332">Válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="211ae-332">Select **Open**.</span></span>
* <span data-ttu-id="211ae-333">Válassza ki a lista illesztőprogramokat, majd válassza ki **org.postgresql.Driver** a **osztálynév**, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="211ae-333">Choose List Drivers, then select **org.postgresql.Driver** in **Class Name**, and select **OK**.</span></span>

<span data-ttu-id="211ae-334">A kapcsolat a helyi kiszolgáló beállítása:</span><span class="sxs-lookup"><span data-stu-id="211ae-334">To set up the connection to the local server:</span></span>

* <span data-ttu-id="211ae-335">Válassza ki **Windows**, majd **aliasok megjelenítése.**</span><span class="sxs-lookup"><span data-stu-id="211ae-335">Select **Windows**, then **View Aliases.**</span></span>
* <span data-ttu-id="211ae-336">Válassza ki a  **+**  gomb új aliast.</span><span class="sxs-lookup"><span data-stu-id="211ae-336">Choose the **+** button to make a new alias.</span></span>
* <span data-ttu-id="211ae-337">Nevezze el *levélszemét adatbázis*, válassza a **PostgreSQL** a a **illesztőprogram** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="211ae-337">Name it *Spam database*, choose **PostgreSQL** in the **Driver** drop-down.</span></span>
* <span data-ttu-id="211ae-338">Az URL-cím beállítása *jdbc:postgresql://localhost/spam*.</span><span class="sxs-lookup"><span data-stu-id="211ae-338">Set the URL to *jdbc:postgresql://localhost/spam*.</span></span>
* <span data-ttu-id="211ae-339">Adja meg a *felhasználónév* és *jelszó*.</span><span class="sxs-lookup"><span data-stu-id="211ae-339">Enter your *username* and *password*.</span></span>
* <span data-ttu-id="211ae-340">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="211ae-340">Click **OK**.</span></span>
* <span data-ttu-id="211ae-341">Lehetőségre a **kapcsolat** ablakban kattintson duplán a ***levélszemét adatbázis*** alias.</span><span class="sxs-lookup"><span data-stu-id="211ae-341">To open the **Connection** window, double-click the ***Spam database*** alias.</span></span>
* <span data-ttu-id="211ae-342">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="211ae-342">Select **Connect**.</span></span>

<span data-ttu-id="211ae-343">Egyes lekérdezések futtatása:</span><span class="sxs-lookup"><span data-stu-id="211ae-343">To run some queries:</span></span>

* <span data-ttu-id="211ae-344">Válassza ki a **SQL** fülre.</span><span class="sxs-lookup"><span data-stu-id="211ae-344">Select the **SQL** tab.</span></span>
* <span data-ttu-id="211ae-345">Adja meg például egy egyszerű lekérdezést `SELECT * from data;` a lekérdezés szövegmezőjének az SQL lap tetején.</span><span class="sxs-lookup"><span data-stu-id="211ae-345">Enter a simple query such as `SELECT * from data;` in the query textbox at the top of the SQL tab.</span></span>
* <span data-ttu-id="211ae-346">Nyomja le az **Ctrl-adja meg a** futtatni.</span><span class="sxs-lookup"><span data-stu-id="211ae-346">Press **Ctrl-Enter** to run it.</span></span> <span data-ttu-id="211ae-347">Alapértelmezés szerint Squirrel SQL sorát adja vissza az első 100 a lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="211ae-347">By default Squirrel SQL returns the first 100 rows from your query.</span></span>

<span data-ttu-id="211ae-348">Nincsenek további lekérdezések futtathatja az adatokba.</span><span class="sxs-lookup"><span data-stu-id="211ae-348">There are many more queries you could run to explore this data.</span></span> <span data-ttu-id="211ae-349">Például hogyan működik a word gyakoriságát *ellenőrizze* eltérnek a levélszemét és sonka?</span><span class="sxs-lookup"><span data-stu-id="211ae-349">For example, how does the frequency of the word *make* differ between spam and ham?</span></span>

    SELECT avg(word_freq_make), spam from data group by spam;

<span data-ttu-id="211ae-350">Mik a tulajdonságokkal rendelkeznek az e-mailek gyakran tartalmaznak, vagy *3d*?</span><span class="sxs-lookup"><span data-stu-id="211ae-350">Or what are the characteristics of email that frequently contain *3d*?</span></span>

    SELECT * from data order by word_freq_3d desc;

<span data-ttu-id="211ae-351">A legtöbb e-mailek, amelyek magas előfordulása *3d* vannak látszólag levélszemét, így az e-mailek besorolására a prediktív modell létrehozásához egy hasznos funkció lehet.</span><span class="sxs-lookup"><span data-stu-id="211ae-351">Most emails that have a high occurrence of *3d* are apparently spam, so it could be a useful feature for building a predictive model to classify the emails.</span></span>

<span data-ttu-id="211ae-352">Ha egy PostgreSQL-adatbázisban tárolt adatok gépi tanulás elvégzésére, érdemes lehet [MADlib](http://madlib.incubator.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="211ae-352">If you wanted to perform machine learning with data stored in a PostgreSQL database, consider using [MADlib](http://madlib.incubator.apache.org/).</span></span>

## <a name="sql-server-data-warehouse"></a><span data-ttu-id="211ae-353">SQL Server-adatraktár</span><span class="sxs-lookup"><span data-stu-id="211ae-353">SQL Server Data Warehouse</span></span>
<span data-ttu-id="211ae-354">Az Azure SQL Data Warehouse egy felhőalapú, horizontálisan felskálázható adatbázis, amely nagy mennyiségű relációs és nem relációs adatot képes feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="211ae-354">Azure SQL Data Warehouse is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span> <span data-ttu-id="211ae-355">További információkért lásd: [Mi az Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="211ae-355">For more information, see [What is Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span></span>

<span data-ttu-id="211ae-356">Csatlakozás az adatraktárhoz, és a tábla létrehozásához futtassa a következő parancsot a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="211ae-356">To connect to the data warehouse and create the table, run the following command from a command prompt:</span></span>

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

<span data-ttu-id="211ae-357">Majd az sqlcmd parancssorból:</span><span class="sxs-lookup"><span data-stu-id="211ae-357">Then at the sqlcmd prompt:</span></span>

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

<span data-ttu-id="211ae-358">Másolja az adatokat a BCP-vel:</span><span class="sxs-lookup"><span data-stu-id="211ae-358">Copy data with bcp:</span></span>

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> <span data-ttu-id="211ae-359">A sorvégződések a letöltött fájlt a Windows-stílusú, de bcp UNIX stílusú vár, ezért ellenőriznünk kell, hogy bcp, amely az - r jelzővel.</span><span class="sxs-lookup"><span data-stu-id="211ae-359">The line endings in the downloaded file are Windows-style, but bcp expects UNIX-style, so we need to tell bcp that with the -r flag.</span></span>
>
>

<span data-ttu-id="211ae-360">És lekérdezés az Sqlcmd használatával:</span><span class="sxs-lookup"><span data-stu-id="211ae-360">And query with sqlcmd:</span></span>

    select top 10 spam, char_freq_dollar from spam;
    GO

<span data-ttu-id="211ae-361">Az Squirrel SQL is lekérdezhet.</span><span class="sxs-lookup"><span data-stu-id="211ae-361">You could also query with Squirrel SQL.</span></span> <span data-ttu-id="211ae-362">Hasonló lépésekkel PostgreSQL, használja a Microsoft MSSQL Server JDBC-illesztőt, amely itt található: a ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span><span class="sxs-lookup"><span data-stu-id="211ae-362">Follow similar steps for PostgreSQL, using the Microsoft MSSQL Server JDBC Driver, which can be found in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span></span>

## <a name="next-steps"></a><span data-ttu-id="211ae-363">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="211ae-363">Next steps</span></span>
<span data-ttu-id="211ae-364">Témakörök, amelyek végigvezetik a feladatokat az Azure-ban a Adattudomány folyamat alkotó áttekintését lásd: [Team adatok tudományos folyamat](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="211ae-364">For an overview of topics that walk you through the tasks that comprise the Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="211ae-365">Egy másik végpont forgatókönyvek, amelyek bemutatják, meghatározott forgatókönyvek esetén az Team tudományos folyamat lépéseit ismertetését lásd: [Team adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md).</span><span class="sxs-lookup"><span data-stu-id="211ae-365">For a description of other end-to-end walkthroughs that demonstrate the steps in the Team Data Science Process for specific scenarios, see [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md).</span></span> <span data-ttu-id="211ae-366">A forgatókönyvek is bemutatják, hogyan lehet a felhő- és a helyszíni eszközök és szolgáltatások egyesítése munkafolyamat vagy csővezeték intelligens alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="211ae-366">The walkthroughs also illustrate how to combine cloud and on-premises tools and services into a workflow or pipeline to create an intelligent application.</span></span>
