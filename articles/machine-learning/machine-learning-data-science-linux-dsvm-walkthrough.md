---
title: "a Linux adatok tudományos virtuális gép hello aaaData tudományos |} Microsoft Docs"
description: "Hogyan tooperform több közös adattudomány a feladatok a hello Linux adatok tudományos virtuális gép."
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
ms.openlocfilehash: 78764825f2e834fa4ddb7fdc2f59418dbe736e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-on-hello-linux-data-science-virtual-machine"></a><span data-ttu-id="6d6f8-103">A Linux adatok tudományos virtuális gép hello adattudomány</span><span class="sxs-lookup"><span data-stu-id="6d6f8-103">Data science on hello Linux Data Science Virtual Machine</span></span>
<span data-ttu-id="6d6f8-104">Ez a forgatókönyv bemutatja, hogyan tooperform több közös adattudomány feladatokat a hello Linux adatok tudományos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-104">This walkthrough shows you how tooperform several common data science tasks with hello Linux Data Science VM.</span></span> <span data-ttu-id="6d6f8-105">hello Linux adatok tudományos virtuális gép (DSVM) érhető el, amely adatelemzés és a gépi tanulás általánosan használt eszközöket együtt telepített Azure virtuálisgép-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-105">hello Linux Data Science Virtual Machine (DSVM) is a virtual machine image available on Azure that is pre-installed with a collection of tools commonly used for data analytics and machine learning.</span></span> <span data-ttu-id="6d6f8-106">hello kulcs szoftverösszetevőket hello van felsorolva [Linux adatok tudományos virtuális gép kiépítése hello](machine-learning-data-science-linux-dsvm-intro.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-106">hello key software components are itemized in hello [Provision hello Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md) topic.</span></span> <span data-ttu-id="6d6f8-107">hello Virtuálisgép-lemezkép segítségével könnyen tooget történt az adatok tudományos (percben), anélkül, hogy tooinstall elindult, és külön-külön konfigurálása mindegyik hello eszközök.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-107">hello VM image makes it easy tooget started doing data science in minutes, without having tooinstall and configure each of hello tools individually.</span></span> <span data-ttu-id="6d6f8-108">Könnyedén növelheti hello virtuális gép, ha szükséges, és állítsa le, ha nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-108">You can easily scale up hello VM, if needed, and stop it when not in use.</span></span> <span data-ttu-id="6d6f8-109">Ehhez az erőforráshoz, mind a rugalmas és költséghatékony.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-109">So this resource is both elastic and cost-efficient.</span></span>

<span data-ttu-id="6d6f8-110">hello tudományos feladatok ebben a forgatókönyvben bemutatott lépések hello hello leírt [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-110">hello data science tasks demonstrated in this walkthrough follow hello steps outlined in hello [Team Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span></span> <span data-ttu-id="6d6f8-111">Ez a folyamat egy rendszeres megközelítés toodata tudományos tooeffectively hello életciklusát intelligens alkalmazások keresztül együttműködve megosszanak adatszakértőkön csapatoknak nyújt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-111">This process provides a systematic approach toodata science that enables teams of data scientists tooeffectively collaborate over hello lifecycle of building intelligent applications.</span></span> <span data-ttu-id="6d6f8-112">hello adatok tudományos folyamat egy iteratív keretet is biztosít, amely egy adott követhetnek adattudomány.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-112">hello data science process also provides an iterative framework for data science that can be followed by an individual.</span></span>

<span data-ttu-id="6d6f8-113">A Microsoft hello elemzése [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-113">We analyze hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset in this walkthrough.</span></span> <span data-ttu-id="6d6f8-114">Ez egy olyan van megjelölve, vagy a levélszemét, vagy a sonka (ami azt jelenti, amelyek nincsenek levélszemét), e-maileket, és néhány statisztikai hello e-mailek tartalma hello is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-114">This is a set of emails that are marked as either spam or ham (meaning they are not spam), and also contains some statistics on hello content of hello emails.</span></span> <span data-ttu-id="6d6f8-115">hello statisztika része ismerteti hello mellett azonban több szakaszt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-115">hello statistics included are discussed in hello next but one section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d6f8-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6d6f8-116">Prerequisites</span></span>
<span data-ttu-id="6d6f8-117">Adatok tudományos Linux virtuális gépek használata előtt hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-117">Before you can use a Linux Data Science Virtual Machine, you must have hello following:</span></span>

* <span data-ttu-id="6d6f8-118">Egy **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-118">An **Azure subscription**.</span></span> <span data-ttu-id="6d6f8-119">Ha még nem rendelkezik egy, lásd: [ma létrehozása az ingyenes Azure-fiókjával](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-119">If you do not already have one, see [Create your free Azure account today](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="6d6f8-120">A [ **adattudomány Linux virtuális gép**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-120">A [**Linux data science VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span></span> <span data-ttu-id="6d6f8-121">A virtuális gép további információkért lásd: [Linux adatok tudományos virtuális gép kiépítése hello](machine-learning-data-science-linux-dsvm-intro.md).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-121">For information on provisioning this VM, see [Provision hello Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md).</span></span>
* <span data-ttu-id="6d6f8-122">[X2Go](http://wiki.x2go.org/doku.php) telepítve a számítógépre, és egy XFCE munkamenet megnyitása.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-122">[X2Go](http://wiki.x2go.org/doku.php) installed on your computer and opened an XFCE session.</span></span> <span data-ttu-id="6d6f8-123">Telepítésével és konfigurálásával kapcsolatos egy **X2Go ügyfél**, lásd: [telepítése és konfigurálása X2Go ügyfél](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-123">For information on installing and configuring an **X2Go client**, see [Installing and configuring X2Go client](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span></span> 
* <span data-ttu-id="6d6f8-124">Egy **AzureML fiók**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-124">An **AzureML account**.</span></span> <span data-ttu-id="6d6f8-125">Ha még nem rendelkezik egy újat: hello regisztráljon [AzureML kezdőlap](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-125">If you don't already have one, sign up for new one at hello [AzureML homepage](https://studio.azureml.net/).</span></span> <span data-ttu-id="6d6f8-126">Van egy ingyenes használati réteg toohelp használatának első lépéseit.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-126">There is a free usage tier toohelp you get started.</span></span>

## <a name="download-hello-spambase-dataset"></a><span data-ttu-id="6d6f8-127">Töltse le a hello spambase adatkészlet</span><span class="sxs-lookup"><span data-stu-id="6d6f8-127">Download hello spambase dataset</span></span>
<span data-ttu-id="6d6f8-128">Hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) adatkészlet olyan viszonylag kicsi, amely csak 4601 példákat tartalmaz adatokat.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-128">hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset is a relatively small set of data that contains only 4601 examples.</span></span> <span data-ttu-id="6d6f8-129">Ez esetén egy kényelmes mérete toouse bemutatásához, hogy néhány hello funkciói hello adatok tudományos virtuális gép, mert a tartja hello erőforrásigények mérsékelt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-129">This is a convenient size toouse when demonstrating that some of hello key features of hello Data Science VM as it keeps hello resource requirements modest.</span></span>

> [!NOTE]
> <span data-ttu-id="6d6f8-130">Ez a forgatókönyv a D2 v2 méretű Linux adatok tudományos virtuális gépek hozták létre.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-130">This walkthrough was created on a D2 v2-sized Linux Data Science Virtual Machine.</span></span> <span data-ttu-id="6d6f8-131">Ez a méret DSVM ebben a bemutatóban hello eljárások kezelésére képes.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-131">This size DSVM is capable of handling hello procedures in this walkthrough.</span></span>
>
>

<span data-ttu-id="6d6f8-132">Ha több tárhelyet igényel, további lemezeket hozhat létre, és csatolja őket tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-132">If you need more storage space, you can create additional disks and attach them tooyour VM.</span></span> <span data-ttu-id="6d6f8-133">Ezeknek a lemezeknek állandó Azure-tárolót, használ, így a adataikat esetén is megőrződik még kiszolgáló hello van újra kiépíteni esedékes tooresizing vagy le van állítva.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-133">These disks use persistent Azure storage, so their data is preserved even when hello server is reprovisioned due tooresizing or is shut down.</span></span> <span data-ttu-id="6d6f8-134">a lemez tooadd és mellékelje tooyour VM, hello utasításait követve [adja hozzá a lemezt tooa Linux virtuális gép](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-134">tooadd a disk and attach it tooyour VM, follow hello instructions in [Add a disk tooa Linux VM](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="6d6f8-135">Ezeket a lépéseket hello Azure parancssori felület (Azure CLI), amelyen már telepítve van a hello DSVM használja.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-135">These steps use hello Azure Command-Line Interface (Azure CLI), which is already installed on hello DSVM.</span></span> <span data-ttu-id="6d6f8-136">Ezért teljes mértékben a virtuális gépért hello ezekkel az eljárásokkal végezhető el.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-136">So these procedures can be done entirely from hello VM itself.</span></span> <span data-ttu-id="6d6f8-137">Egy másik lehetőség tooincrease tároló toouse [az Azure files](../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-137">Another option tooincrease storage is toouse [Azure files](../storage/files/storage-how-to-use-files-linux.md).</span></span>

<span data-ttu-id="6d6f8-138">toodownload hello adatok, nyisson meg egy terminálablakot, és futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-138">toodownload hello data, open a terminal window and run this command:</span></span>

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

<span data-ttu-id="6d6f8-139">hello letöltött fájl nem rendelkezik a fejlécsor, így hozzon létre egy másik fájlba, amelyen fejlécet.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-139">hello downloaded file does not have a header row, so let's create another file that does have a header.</span></span> <span data-ttu-id="6d6f8-140">Futtassa a fájlt a parancs toocreate hello megfelelő fejlécek:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-140">Run this command toocreate a file with hello appropriate headers:</span></span>

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

<span data-ttu-id="6d6f8-141">Majd összefűzésére hello két fájlt hello parancs együtt:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-141">Then concatenate hello two files together with hello command:</span></span>

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

<span data-ttu-id="6d6f8-142">hello adatkészlet egyes e-mail számos különböző típusú statisztikák rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-142">hello dataset has several types of statistics on each email:</span></span>

* <span data-ttu-id="6d6f8-143">Oszlopok, például ***word\_freq\_WORD*** hello százaléka hello e-mailben a szavakat, amelyek jelzik *WORD*.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-143">Columns like ***word\_freq\_WORD*** indicate hello percentage of words in hello email that match *WORD*.</span></span> <span data-ttu-id="6d6f8-144">Például ha *word\_freq\_ellenőrizze* értéke 1 és 1 % hello e-mailben szereplő minden szó volt *ellenőrizze*.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-144">For example, if *word\_freq\_make* is 1, then 1% of all words in hello email were *make*.</span></span>
* <span data-ttu-id="6d6f8-145">Oszlopok, például ***char\_freq\_CHAR*** hello százalékos karaktereit hello e-mailben, amelyek jelzik *CHAR*.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-145">Columns like ***char\_freq\_CHAR*** indicate hello percentage of all characters in hello email that were *CHAR*.</span></span>
* <span data-ttu-id="6d6f8-146">***beruházási\_futtatása\_hossza\_leghosszabb*** hello leghosszabb hossza nagybetűvel sorozata.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-146">***capital\_run\_length\_longest*** is hello longest length of a sequence of capital letters.</span></span>
* <span data-ttu-id="6d6f8-147">***beruházási\_futtatása\_hossza\_átlagos*** nagybetűvel összes sorozatát hello átlagos hossza.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-147">***capital\_run\_length\_average*** is hello average length of all sequences of capital letters.</span></span>
* <span data-ttu-id="6d6f8-148">***beruházási\_futtatása\_hossza\_teljes*** nagybetűvel összes sorozatát hello teljes hossza.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-148">***capital\_run\_length\_total*** is hello total length of all sequences of capital letters.</span></span>
* <span data-ttu-id="6d6f8-149">***Levélszemét*** azt jelzi, hogy hello e-mail tekintették levélszemét, vagy nem (1 = levélszemét, 0 = nem levélszemét).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-149">***spam*** indicates whether hello email was considered spam or not (1 = spam, 0 = not spam).</span></span>

## <a name="explore-hello-dataset-with-microsoft-r-open"></a><span data-ttu-id="6d6f8-150">Microsoft R nyitott hello adatkészlet felfedezés</span><span class="sxs-lookup"><span data-stu-id="6d6f8-150">Explore hello dataset with Microsoft R Open</span></span>
<span data-ttu-id="6d6f8-151">Most hello megtekintésével, és hajtsa végre a mellékelt adatok tudományos VM R. hello néhány alapvető gépi tanulás [Microsoft R nyitott](https://mran.revolutionanalytics.com/open/) előre telepítve.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-151">Let's examine hello data and do some basic machine learning with R. hello Data Science VM comes with [Microsoft R Open](https://mran.revolutionanalytics.com/open/) pre-installed.</span></span> <span data-ttu-id="6d6f8-152">hello többszálas matematikai szalagtárak R ezen verziója jobb teljesítményt nyújtanak egyszálas különböző verzióiban.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-152">hello multithreaded math libraries in this version of R offer better performance than various single-threaded versions.</span></span> <span data-ttu-id="6d6f8-153">Microsoft R nyitott is biztosít reprodukálhatósági használatával hello CRAN csomag tárház pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-153">Microsoft R Open also provides reproducibility by using a snapshot of hello CRAN package repository.</span></span>

<span data-ttu-id="6d6f8-154">Ebben a bemutatóban Klónozás hello használt hello tooget példánya Kódminták **Azure-Machine-tanulás-adatok-tudományos** segítségével git, amely előre telepítve van a virtuális gép hello tárházba.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-154">tooget copies of hello code samples used in this walkthrough, clone hello **Azure-Machine-Learning-Data-Science** repository using git, which is pre-installed on hello VM.</span></span> <span data-ttu-id="6d6f8-155">Futtassa parancssorból hello git:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-155">From hello git command line, run:</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="6d6f8-156">Nyisson meg egy terminálablakot, és indítson el egy új R munkamenetet hello R interaktív konzolt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-156">Open a terminal window and start a new R session with hello R interactive console.</span></span>

> [!NOTE]
> <span data-ttu-id="6d6f8-157">Az alábbi eljárásokat hello Rstudióból is használható.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-157">You can also use RStudio for hello following procedures.</span></span> <span data-ttu-id="6d6f8-158">tooinstall Rstudióból, az adott parancs végrehajtásához:`./Desktop/DSVM\ tools/installRStudio.sh`</span><span class="sxs-lookup"><span data-stu-id="6d6f8-158">tooinstall RStudio, execute this command at a terminal: `./Desktop/DSVM\ tools/installRStudio.sh`</span></span>
>
>

<span data-ttu-id="6d6f8-159">tooimport hello adatokat, és állítsa be hello környezetet, futtassa:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-159">tooimport hello data and set up hello environment, run:</span></span>

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

<span data-ttu-id="6d6f8-160">egyes oszlopok statisztikája összefoglaló toosee:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-160">toosee summary statistics about each column:</span></span>

    summary(data)

<span data-ttu-id="6d6f8-161">Másik nézet hello adatok:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-161">For a different view of hello data:</span></span>

    str(data)

<span data-ttu-id="6d6f8-162">Ez azt jelenti, hogy az egyes típusú hello és hello először hello adatkészlet kevés érték.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-162">This shows you hello type of each variable and hello first few values in hello dataset.</span></span>

<span data-ttu-id="6d6f8-163">Hello *levélszemét* oszlop beolvasott érték, de ténylegesen egy kategorikus változó (vagy tényező).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-163">hello *spam* column was read as an integer, but it's actually a categorical variable (or factor).</span></span> <span data-ttu-id="6d6f8-164">tooset típusa:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-164">tooset its type:</span></span>

    data$spam <- as.factor(data$spam)

<span data-ttu-id="6d6f8-165">toodo néhány felderítő elemzése, használjon hello [ggplot2](http://ggplot2.org/) csomag, az R, amely már telepítve van a virtuális gép hello népszerű nyújtó grafikus könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-165">toodo some exploratory analysis, use hello [ggplot2](http://ggplot2.org/) package, a popular graphing library for R that is already installed on hello VM.</span></span> <span data-ttu-id="6d6f8-166">Megjegyzés: hello összefoglalókat jelenik meg a korábban, a statisztikák tudunk hello felkiáltójel karakter hello gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-166">Note, from hello summary data displayed earlier, that we have summary statistics on hello frequency of hello exclamation mark character.</span></span> <span data-ttu-id="6d6f8-167">Most megrajzolásához e gyakoriságot Itt a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-167">Let's plot those frequencies here with hello following commands:</span></span>

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="6d6f8-168">Hello nulla sáv hello rajzot van döntés, mivel most selejtezni azt:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-168">Since hello zero bar is skewing hello plot, let's get rid of it:</span></span>

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="6d6f8-169">1 kereső érdekes fent nem triviális sűrűségű van.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-169">There is a non-trivial density above 1 that looks interesting.</span></span> <span data-ttu-id="6d6f8-170">Most, hogy az adatok vizsgáljuk meg:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-170">Let's look at just that data:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="6d6f8-171">Ezután ossza levélszemét vs sonka szerint:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-171">Then split it by spam vs ham:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

<span data-ttu-id="6d6f8-172">Ezek a példák kell lehetővé teszik toomake hasonló ábrái hello más oszlopok tooexplore hello a bennük található adatok.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-172">These examples should enable you toomake similar plots of hello other columns tooexplore hello data contained in them.</span></span>

## <a name="train-and-test-an-ml-model"></a><span data-ttu-id="6d6f8-173">Betanítása és tesztelni az ML-modell</span><span class="sxs-lookup"><span data-stu-id="6d6f8-173">Train and test an ML model</span></span>
<span data-ttu-id="6d6f8-174">Most tegyük a vonat gépi tanulás néhány modellek hello adatkészlet tooclassify hello e-mailek vagy tartalmazóként span vagy sonka.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-174">Now let's train a couple of machine learning models tooclassify hello emails in hello dataset as containing either span or ham.</span></span> <span data-ttu-id="6d6f8-175">Azt a döntési fa modellek és ebben a szakaszban egy véletlenszerű erdő modell betanítását, és tesztelje a azok az előrejelzés pontosságát.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-175">We train a decision tree model and a random forest model in this section and then test their accuracy of their predictions.</span></span>

> [!NOTE]
> <span data-ttu-id="6d6f8-176">hello rpart (rekurzív particionálás és regressziós fák) csomag, amelyet a következő kód hello hello adatok tudományos VM már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-176">hello rpart (Recursive Partitioning and Regression Trees) package used in hello following code is already installed on hello Data Science VM.</span></span>
>
>

<span data-ttu-id="6d6f8-177">Először hozzuk hello dataset felosztása tanítási és tesztelési beállítása:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-177">First, let's split hello dataset into training and test sets:</span></span>

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

<span data-ttu-id="6d6f8-178">Majd hozza létre a döntési fa tooclassify hello e-maileket.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-178">And then create a decision tree tooclassify hello emails.</span></span>

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

<span data-ttu-id="6d6f8-179">Hello eredménye a következő:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-179">Here is hello result:</span></span>

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

<span data-ttu-id="6d6f8-181">milyen mértékben hello képzési hajt végre toodetermine megadásához használja a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-181">toodetermine how well it performs on hello training set, use hello following code:</span></span>

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="6d6f8-182">toodetermine mennyire azt hello TesztKészlet végez:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-182">toodetermine how well it performs on hello test set:</span></span>

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="6d6f8-183">Próbáljuk meg is véletlenszerű erdőmodell.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-183">Let's also try a random forest model.</span></span> <span data-ttu-id="6d6f8-184">Véletlenszerű erdők számos döntési fák betanítása, és kimeneti egy osztály, amely az összes hello egyedi döntési fák hello besorolások hello mód.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-184">Random forests train a multitude of decision trees and output a class that is hello mode of hello classifications from all of hello individual decision trees.</span></span> <span data-ttu-id="6d6f8-185">Nagyobb teljesítményű machine learning-módszer, azok küszöbölje ki a döntési fa modell toooverfit képzési dataset hello hogy biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-185">They provide a more powerful machine learning approach as they correct for hello tendency of a decision tree model toooverfit a training dataset.</span></span>

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-tooazure-ml"></a><span data-ttu-id="6d6f8-186">A modell tooAzure ML telepítése</span><span class="sxs-lookup"><span data-stu-id="6d6f8-186">Deploy a model tooAzure ML</span></span>
<span data-ttu-id="6d6f8-187">[Az Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) egy felhőszolgáltatás, amely lehetővé teszi könnyen toobuild és központi telepítése a prediktív elemzési modellek.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) is a cloud service that makes it easy toobuild and deploy predictive analytics models.</span></span> <span data-ttu-id="6d6f8-188">Hello töltött funkcióit AzureML egyike annak lehetősége toopublish bármely R webszolgáltatásként működik.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-188">One of hello nice features of AzureML is its ability toopublish any R function as a web service.</span></span> <span data-ttu-id="6d6f8-189">hello AzureML R csomagot teszi hello DSVM közvetlenül az R munkamenetet a központi telepítés könnyen toodo.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-189">hello AzureML R package makes deployment easy toodo right from our R session on hello DSVM.</span></span>

<span data-ttu-id="6d6f8-190">toodeploy hello döntési fa kód hello korábbi szakaszában, a Machine Learning Studio tooAzure toosign kell.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-190">toodeploy hello decision tree code from hello previous section, you need toosign in tooAzure Machine Learning Studio.</span></span> <span data-ttu-id="6d6f8-191">A munkaterület azonosítója és a egy engedélyezési jogkivonat toosign van szükség.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-191">You need your workspace ID and an authorization token toosign in.</span></span> <span data-ttu-id="6d6f8-192">toofind alábbi értékekkel és inicializálási hello AzureML-változók őket:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-192">toofind these values and initialize hello AzureML variables with them:</span></span>

<span data-ttu-id="6d6f8-193">Válassza ki **beállítások** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-193">Select **Settings** on hello left-hand menu.</span></span> <span data-ttu-id="6d6f8-194">Megjegyzés: a **MUNKATERÜLET azonosítója**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-194">Note your **WORKSPACE ID**.</span></span> <span data-ttu-id="6d6f8-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span><span class="sxs-lookup"><span data-stu-id="6d6f8-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span></span>

<span data-ttu-id="6d6f8-196">Válassza ki **engedélyezési jogkivonatok** hello általános menüből, és vegye figyelembe a **elsődleges engedélyezési jogkivonat**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span><span class="sxs-lookup"><span data-stu-id="6d6f8-196">Select **Authorization Tokens** from hello overhead menu and note your **Primary Authorization Token**.![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span></span>

<span data-ttu-id="6d6f8-197">Betöltési hello **AzureML** csomag és utána állítsa be a jogkivonatot, és a munkaterület Azonosítóját a hello változók értékeit az R-munkamenetben a hello DSVM:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-197">Load hello **AzureML** package and then set values of hello variables with your token and workspace ID in your R session on hello DSVM:</span></span>

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


<span data-ttu-id="6d6f8-198">Ez a bemutató könnyebb tooimplement most leegyszerűsíti a hello modell toomake.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-198">Let's simplify hello model toomake this demonstration easier tooimplement.</span></span> <span data-ttu-id="6d6f8-199">Hello döntési fa legközelebbi toohello legfelső szintű három változók hello kiválasztják, és csak ezen három változók használata új fa létrehozása:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-199">Pick hello three variables in hello decision tree closest toohello root and build a new tree using just those three variables:</span></span>

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

<span data-ttu-id="6d6f8-200">Igazolnia kell, hogy egy előrejelző függvényben, amely hello szolgáltatások bemenetként, és vissza hello előre jelzett értékek:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-200">We need a prediction function that takes hello features as an input and returns hello predicted values:</span></span>

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

<span data-ttu-id="6d6f8-201">Hello predictSpam függvény tooAzureML hello segítségével közzététele **publishWebService** függvény:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-201">Publish hello predictSpam function tooAzureML using hello **publishWebService** function:</span></span>

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

<span data-ttu-id="6d6f8-202">Ez a függvény argumentuma hello **predictSpam** működik, létrehoz egy nevű webszolgáltatás-bővítmény **spamWebService** a meghatározni a bemenetekhez és kimenetekhez, és új végpont hello információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-202">This function takes hello **predictSpam** function, creates a web service named **spamWebService** with defined inputs and outputs, and returns information about hello new endpoint.</span></span>

<span data-ttu-id="6d6f8-203">Hello részleteinek megtekintése a közzétett webes szolgáltatás, beleértve az API-végpont és a hozzáférés a hello paranccsal kulcsokat:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-203">View details of hello published web service, including its API endpoint and access keys with hello command:</span></span>

    spamWebService[[2]]

<span data-ttu-id="6d6f8-204">tootry azt a hello hello TesztKészlet az első 10 sor:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-204">tootry it out on hello first 10 rows of hello test set:</span></span>

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a><span data-ttu-id="6d6f8-205">Rendelkezésre álló eszközök</span><span class="sxs-lookup"><span data-stu-id="6d6f8-205">Use other tools available</span></span>
<span data-ttu-id="6d6f8-206">hello fennmaradó szakaszok bemutatják, hogyan néhány hello eszközök telepíteni toouse hello Linux adatok tudományos virtuális Gépet. Eszközök tárgyalt hello listája itt található:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-206">hello remaining sections show how toouse some of hello tools installed on hello Linux Data Science VM.Here is hello list of tools discussed:</span></span>

* <span data-ttu-id="6d6f8-207">XGBoost</span><span class="sxs-lookup"><span data-stu-id="6d6f8-207">XGBoost</span></span>
* <span data-ttu-id="6d6f8-208">Python</span><span class="sxs-lookup"><span data-stu-id="6d6f8-208">Python</span></span>
* <span data-ttu-id="6d6f8-209">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="6d6f8-209">Jupyterhub</span></span>
* <span data-ttu-id="6d6f8-210">Rattle</span><span class="sxs-lookup"><span data-stu-id="6d6f8-210">Rattle</span></span>
* <span data-ttu-id="6d6f8-211">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="6d6f8-211">PostgreSQL & Squirrel SQL</span></span>
* <span data-ttu-id="6d6f8-212">SQL Server-adatraktár</span><span class="sxs-lookup"><span data-stu-id="6d6f8-212">SQL Server Data Warehouse</span></span>

## <a name="xgboost"></a><span data-ttu-id="6d6f8-213">XGBoost</span><span class="sxs-lookup"><span data-stu-id="6d6f8-213">XGBoost</span></span>
<span data-ttu-id="6d6f8-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) olyan eszköz, amely gyors és pontos súlyozott fa valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) is a tool that provides a fast and accurate boosted tree implementation.</span></span>

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

<span data-ttu-id="6d6f8-215">XGBoost is meghívhatja a python vagy a parancssorból végzi.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-215">XGBoost can also call from python or a command line.</span></span>

## <a name="python"></a><span data-ttu-id="6d6f8-216">Python</span><span class="sxs-lookup"><span data-stu-id="6d6f8-216">Python</span></span>
<span data-ttu-id="6d6f8-217">A fejlesztési pythonos környezetekben a hello Anaconda Python terjesztéseket 2.7 és 3.5-ös hello DSVM sikeresen telepítette.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-217">For development using Python, hello Anaconda Python distributions 2.7 and 3.5 have been installed in hello DSVM.</span></span>

> [!NOTE]
> <span data-ttu-id="6d6f8-218">hello Anaconda terjesztési tartalmaz [Condas](http://conda.pydata.org/docs/index.html), amely különböző verziói és/vagy a bennük foglalt telepített csomagok rendelkező Python használt toocreate egyéni környezetek lehet.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-218">hello Anaconda distribution includes [Condas](http://conda.pydata.org/docs/index.html), which can be used toocreate custom environments for Python that have different versions and/or packages installed in them.</span></span>
>
>

<span data-ttu-id="6d6f8-219">Most hello spambase adatkészlet egyes olvasása és hello e-mailek besorolására scikit a támogatási vektoros gépekkel – ismerje meg:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-219">Let's read in some of hello spambase dataset and classify hello emails with support vector machines in scikit-learn:</span></span>

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="6d6f8-220">toomake előrejelzéseket:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-220">toomake predictions:</span></span>

    clf.predict(X.ix[0:20, :])

<span data-ttu-id="6d6f8-221">tooshow hogyan toopublish AzureML végpont, most egy egyszerűbb modell ellenőrizze hello három változó azt korábban közzétett Microsoft hello R-modell hasonló.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-221">tooshow how toopublish an AzureML endpoint, let's make a simpler model hello three variables as we did when we published hello R model previously.</span></span>

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="6d6f8-222">toopublish hello modell tooAzureML:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-222">toopublish hello model tooAzureML:</span></span>

    # Publish hello model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about hello resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call hello model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> <span data-ttu-id="6d6f8-223">Ez a lehetőség csak a python 2.7, és még nem támogatott a 3.5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-223">This is only available for python 2.7 and is not yet supported on 3.5.</span></span> <span data-ttu-id="6d6f8-224">Futtassa a **/anaconda/bin/python2.7**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-224">Run with **/anaconda/bin/python2.7**.</span></span>
>
>

## <a name="jupyterhub"></a><span data-ttu-id="6d6f8-225">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="6d6f8-225">Jupyterhub</span></span>
<span data-ttu-id="6d6f8-226">hello Anaconda terjesztési a hello DSVM tartalmaz Jupyter notebook, a többplatformos környezetben tooshare Python, R vagy Ágnes kódot és elemzését.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-226">hello Anaconda distribution in hello DSVM comes with a Jupyter notebook, a cross-platform environment tooshare Python, R, or Julia code and analysis.</span></span> <span data-ttu-id="6d6f8-227">hello Jupyter notebook JupyterHub keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-227">hello Jupyter notebook is accessed through JupyterHub.</span></span> <span data-ttu-id="6d6f8-228">A helyi Linux-felhasználónév és jelszó használatával bejelentkezik ***https://\<VM DNS-nevét vagy IP-cím\>: 8000 /***.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-228">You sign in using your local Linux user name and password at ***https://\<VM DNS name or IP Address\>:8000/***.</span></span> <span data-ttu-id="6d6f8-229">Minden konfigurációs fájlt a JupyterHub könyvtárban találhatók **/etc/jupyterhub**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-229">All configuration files for JupyterHub are found in directory **/etc/jupyterhub**.</span></span>

<span data-ttu-id="6d6f8-230">Több minta jegyzetfüzetet már telepítve van a virtuális gép hello:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-230">Several sample notebooks are already installed on hello VM:</span></span>

* <span data-ttu-id="6d6f8-231">Lásd: hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) egy minta Python jegyzetfüzet.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-231">See hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) for a sample Python notebook.</span></span>
* <span data-ttu-id="6d6f8-232">Lásd: [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) egy minta **R** notebookot.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-232">See [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) for a sample **R** notebook.</span></span>
* <span data-ttu-id="6d6f8-233">Lásd: hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) egy másik minta **Python** notebookot.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-233">See hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) for another sample **Python** notebook.</span></span>

> [!NOTE]
> <span data-ttu-id="6d6f8-234">Ágnes nyelvi hello parancssorából hello hello Linux adatok tudományos VM is érhető el.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-234">hello Julia language is also available from hello command line on hello Linux Data Science VM.</span></span>
>
>

## <a name="rattle"></a><span data-ttu-id="6d6f8-235">Rattle</span><span class="sxs-lookup"><span data-stu-id="6d6f8-235">Rattle</span></span>
<span data-ttu-id="6d6f8-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (R analitikai eszköz tooLearn hello könnyen) egy grafikus R eszköz adatbányászathoz.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (hello R Analytical Tool tooLearn Easily) is a graphical R tool for data mining.</span></span> <span data-ttu-id="6d6f8-237">Egy egyszerűen elsajátítható felülete, így könnyen tooload, megismeréséhez és adatok átalakítása és build és modellek kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-237">It has an intuitive interface that makes it easy tooload, explore, and transform data and build and evaluate models.</span></span>  <span data-ttu-id="6d6f8-238">hello cikk [Rattle: A Data Mining GUI az R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) biztosít a forgatókönyv azt mutatja be, a szolgáltatásai.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-238">hello article [Rattle: A Data Mining GUI for R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) provides a walkthrough that demonstrates its features.</span></span>

<span data-ttu-id="6d6f8-239">Telepítse, és indítsa el a következő parancsok hello Rattle:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-239">Install and start Rattle with hello following commands:</span></span>

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> <span data-ttu-id="6d6f8-240">Telepítési hello DSVM nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-240">Installation is not required on hello DSVM.</span></span> <span data-ttu-id="6d6f8-241">De Rattle késztethetik tooinstall további csomagokat betöltéskor.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-241">But Rattle may prompt you tooinstall additional packages when it loads.</span></span>
>
>

<span data-ttu-id="6d6f8-242">Rattle egy lapon-alapú felületet használja.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-242">Rattle uses a tab-based interface.</span></span> <span data-ttu-id="6d6f8-243">Hello lapok többsége felel meg a hello toosteps [adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), például az adatok betöltése vagy felfedezését.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-243">Most of hello tabs correspond toosteps in hello [Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), like loading data or exploring it.</span></span> <span data-ttu-id="6d6f8-244">hello tudományos folyamata a bal oldali tooright hello lapok keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-244">hello data science process flows from left tooright through hello tabs.</span></span> <span data-ttu-id="6d6f8-245">Azonban hello utolsó lap tartalmaz hello R parancsok futtatásához Rattle naplóját.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-245">But hello last tab contains a log of hello R commands run by Rattle.</span></span>

<span data-ttu-id="6d6f8-246">tooload és adatkészlet hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-246">tooload and configure hello dataset:</span></span>

* <span data-ttu-id="6d6f8-247">tooload hello fájlt, jelölje be hello **adatok** lapra, majd</span><span class="sxs-lookup"><span data-stu-id="6d6f8-247">tooload hello file, select hello **Data** tab, then</span></span>
* <span data-ttu-id="6d6f8-248">Válasszon hello választó mellett túl**Fájlnév** válassza **spambaseHeaders.data**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-248">Choose hello selector next too**Filename** and choose **spambaseHeaders.data**.</span></span>
* <span data-ttu-id="6d6f8-249">tooload hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-249">tooload hello file.</span></span> <span data-ttu-id="6d6f8-250">Válassza ki **Execute** gombok hello legfelső sorában.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-250">select **Execute** in hello top row of buttons.</span></span> <span data-ttu-id="6d6f8-251">Megtekintheti az egyes oszlopok, beleértve az azonosított adattípushoz, hogy bemeneti, cél vagy más típusú változó, és egyedi értékek számának hello összegzését.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-251">You should see a summary of each column, including its identified data type, whether it's an input, a target, or other type of variable, and hello number of unique values.</span></span>
* <span data-ttu-id="6d6f8-252">Rattle helyesen észlelt hello **levélszemét** oszlop hello célként.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-252">Rattle has correctly identified hello **spam** column as hello target.</span></span> <span data-ttu-id="6d6f8-253">Jelölje be hello levélszemét oszlop, majd a készlet hello **cél adattípus** túl**Categoric**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-253">Select hello spam column, then set hello **Target Data Type** too**Categoric**.</span></span>

<span data-ttu-id="6d6f8-254">tooexplore hello adatokat:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-254">tooexplore hello data:</span></span>

* <span data-ttu-id="6d6f8-255">Jelölje be hello **böngészés** fülre.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-255">Select hello **Explore** tab.</span></span>
* <span data-ttu-id="6d6f8-256">Kattintson a **összefoglaló**, majd **Execute**, toosee hello változó típusok és néhány statisztikák kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-256">Click **Summary**, then **Execute**, toosee some information about hello variable types and some summary statistics.</span></span>
* <span data-ttu-id="6d6f8-257">tooview más típusú statisztikák változókról, válassza ki az egyéb beállítások, például a **adja** vagy **alapjai**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-257">tooview other types of statistics about each variable, select other options like **Describe** or **Basics**.</span></span>

<span data-ttu-id="6d6f8-258">Hello **böngészés** lapon is lehetővé teszi számos osztályon tevékenységtérkép toogenerate.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-258">hello **Explore** tab also allows you toogenerate many insightful plots.</span></span> <span data-ttu-id="6d6f8-259">a hisztogram hello adatok tooplot:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-259">tooplot a histogram of hello data:</span></span>

* <span data-ttu-id="6d6f8-260">Válassza ki **Terjesztéseket**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-260">Select **Distributions**.</span></span>
* <span data-ttu-id="6d6f8-261">Ellenőrizze **hisztogram** a **word_freq_remove** és **word_freq_you**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-261">Check **Histogram** for **word_freq_remove** and **word_freq_you**.</span></span>
* <span data-ttu-id="6d6f8-262">Válassza ki **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-262">Select **Execute**.</span></span> <span data-ttu-id="6d6f8-263">Meg kell jelennie egy grafikonon ablakban, amennyiben törölje a jelet tevékenységtérkép mindkét sűrűség adott hello word "Ön" gyakran sokkal megjelenik az e-mailek, mint az "Eltávolítás".</span><span class="sxs-lookup"><span data-stu-id="6d6f8-263">You should see both density plots in a single graph window, where it is clear that hello word "you" appears much more frequently in emails than "remove".</span></span>

<span data-ttu-id="6d6f8-264">hello korrelációs előkészítésére érdekes egyaránt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-264">hello Correlation plots are also interesting.</span></span> <span data-ttu-id="6d6f8-265">egy toocreate:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-265">toocreate one:</span></span>

* <span data-ttu-id="6d6f8-266">Válasszon **korrelációs** , hello **típus**, majd</span><span class="sxs-lookup"><span data-stu-id="6d6f8-266">Choose **Correlation** as hello **Type**, then</span></span>
* <span data-ttu-id="6d6f8-267">Válassza ki **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-267">Select **Execute**.</span></span>
* <span data-ttu-id="6d6f8-268">Rattle figyelmezteti, hogy legfeljebb 40 változók javasol.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-268">Rattle warns you that it recommends a maximum of 40 variables.</span></span> <span data-ttu-id="6d6f8-269">Válassza ki **Igen** tooview hello rajzot.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-269">Select **Yes** tooview hello plot.</span></span>

<span data-ttu-id="6d6f8-270">Van néhány érdekes közti korrelációk elérni: "technológia" erősen korrelált túl "HP" és "labs", például.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-270">There are some interesting correlations that come up: "technology" is strongly correlated too"HP" and "labs", for example.</span></span> <span data-ttu-id="6d6f8-271">Azt is erősen visszamenőleges korrelációban állnak túl "650", mert a hello dataset adók hello körzetszám 650.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-271">It is also strongly correlated too"650", because hello area code of hello dataset donors is 650.</span></span>

<span data-ttu-id="6d6f8-272">hello számértékek hello korrelációk szavakat közötti hello Intéző ablakában érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-272">hello numeric values for hello correlations between words are available in hello Explore window.</span></span> <span data-ttu-id="6d6f8-273">Fontos, hogy érdekes toonote, például, hogy "technológia" negatívan tartozzanak "a" és "pénzt".</span><span class="sxs-lookup"><span data-stu-id="6d6f8-273">It is interesting toonote, for example, that "technology" is negatively correlated with "your" and "money".</span></span>

<span data-ttu-id="6d6f8-274">Rattle hello dataset toohandle alakíthatja át néhány gyakori problémákat.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-274">Rattle can transform hello dataset toohandle some common issues.</span></span> <span data-ttu-id="6d6f8-275">Például lehetővé teszi toorescale funkciókat, imputálására a hiányzó értékeket, kiugró kezelni és változók vagy megfigyelések hiányzó adatok eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-275">For example, it allows you toorescale features, impute missing values, handle outliers, and remove variables or observations with missing data.</span></span> <span data-ttu-id="6d6f8-276">Rattle is azonosíthatja a társítási szabályok megfigyelések és/vagy változók között.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-276">Rattle can also identify association rules between observations and/or variables.</span></span> <span data-ttu-id="6d6f8-277">A lapok kívül esnek a hatókörön bevezető forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-277">These tabs are out of scope for this introductory walkthrough.</span></span>

<span data-ttu-id="6d6f8-278">Rattle fürt analysis is végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-278">Rattle can also perform cluster analysis.</span></span> <span data-ttu-id="6d6f8-279">Bizonyos funkciók toomake hello kimeneti könnyebb tooread most kizárása.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-279">Let's exclude some features toomake hello output easier tooread.</span></span> <span data-ttu-id="6d6f8-280">A hello **adatok** lapra, majd **figyelmen kívül hagyása** következő tooeach hello változók tíz elemekhez kivételével:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-280">On hello **Data** tab, choose **Ignore** next tooeach of hello variables except these ten items:</span></span>

* <span data-ttu-id="6d6f8-281">word_freq_hp</span><span class="sxs-lookup"><span data-stu-id="6d6f8-281">word_freq_hp</span></span>
* <span data-ttu-id="6d6f8-282">word_freq_technology</span><span class="sxs-lookup"><span data-stu-id="6d6f8-282">word_freq_technology</span></span>
* <span data-ttu-id="6d6f8-283">word_freq_george</span><span class="sxs-lookup"><span data-stu-id="6d6f8-283">word_freq_george</span></span>
* <span data-ttu-id="6d6f8-284">word_freq_remove</span><span class="sxs-lookup"><span data-stu-id="6d6f8-284">word_freq_remove</span></span>
* <span data-ttu-id="6d6f8-285">word_freq_your</span><span class="sxs-lookup"><span data-stu-id="6d6f8-285">word_freq_your</span></span>
* <span data-ttu-id="6d6f8-286">word_freq_dollar</span><span class="sxs-lookup"><span data-stu-id="6d6f8-286">word_freq_dollar</span></span>
* <span data-ttu-id="6d6f8-287">word_freq_money</span><span class="sxs-lookup"><span data-stu-id="6d6f8-287">word_freq_money</span></span>
* <span data-ttu-id="6d6f8-288">capital_run_length_longest</span><span class="sxs-lookup"><span data-stu-id="6d6f8-288">capital_run_length_longest</span></span>
* <span data-ttu-id="6d6f8-289">word_freq_business</span><span class="sxs-lookup"><span data-stu-id="6d6f8-289">word_freq_business</span></span>
* <span data-ttu-id="6d6f8-290">Levélszemét</span><span class="sxs-lookup"><span data-stu-id="6d6f8-290">spam</span></span>

<span data-ttu-id="6d6f8-291">Ezután térjen vissza toohello **fürt** lapra, majd **KMeans**, és a set hello *számát* too4.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-291">Then go back toohello **Cluster** tab, choose **KMeans**, and set hello *Number of clusters* too4.</span></span> <span data-ttu-id="6d6f8-292">Majd **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-292">Then **Execute**.</span></span> <span data-ttu-id="6d6f8-293">hello eredmények hello kimeneti ablakban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-293">hello results are displayed in hello output window.</span></span> <span data-ttu-id="6d6f8-294">Egy fürt nagy gyakoriságú "hp" és "nagy szóval" rendelkezik, és valószínűleg egy valós üzleti e-mailt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-294">One cluster has high frequency of "george" and "hp" and is probably a legitimate business email.</span></span>

<span data-ttu-id="6d6f8-295">egy egyszerű döntési fa gépi tanulási modell toobuild:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-295">toobuild a simple decision tree machine learning model:</span></span>

* <span data-ttu-id="6d6f8-296">Jelölje be hello **modell** lap</span><span class="sxs-lookup"><span data-stu-id="6d6f8-296">Select hello **Model** tab,</span></span>
* <span data-ttu-id="6d6f8-297">Válasszon **fa** , hello **típus**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-297">Choose **Tree** as hello **Type**.</span></span>
* <span data-ttu-id="6d6f8-298">Válassza ki **Execute** toodisplay hello fa hello szöveg formájában a kimeneti ablakban.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-298">Select **Execute** toodisplay hello tree in text form in hello output window.</span></span>
* <span data-ttu-id="6d6f8-299">Jelölje be hello **megrajzolásához** gomb tooview egy grafikus verziót.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-299">Select hello **Draw** button tooview a graphical version.</span></span> <span data-ttu-id="6d6f8-300">Ez azt a korábban beszerzett nagyon hasonló toohello fa keres *rpart*.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-300">This looks quite similar toohello tree we obtained earlier using *rpart*.</span></span>

<span data-ttu-id="6d6f8-301">Hello Rattle töltött szolgáltatásai közül a képes toorun több gépi tanulás módszerek, és gyorsan értékeli őket.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-301">One of hello nice features of Rattle is its ability toorun several machine learning methods and quickly evaluate them.</span></span> <span data-ttu-id="6d6f8-302">Hello folyamat során a rendszer:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-302">Here is hello procedure:</span></span>

* <span data-ttu-id="6d6f8-303">Válasszon **összes** a hello **típus**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-303">Choose **All** for hello **Type**.</span></span>
* <span data-ttu-id="6d6f8-304">Válassza ki **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-304">Select **Execute**.</span></span>
* <span data-ttu-id="6d6f8-305">Művelet befejeződése után kattintson a bármely egyetlen **típus**, például **SVM**, és az hello eredmények megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-305">After it finishes you can click any single **Type**, like **SVM**, and view hello results.</span></span>
* <span data-ttu-id="6d6f8-306">Összehasonlíthatja hello teljesítmény hello modellek segítségével hello hello ellenőrzés **Evaluate** fülre. Például hello **hiba mátrix** kijelölés megjeleníti a hello zavart mátrix, általános hiba, és az egyes átlagolt osztály hiba hello érvényesítési beállítása a.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-306">You can also compare hello performance of hello models on hello validation set using hello **Evaluate** tab. For example, hello **Error Matrix** selection shows you hello confusion matrix, overall error, and averaged class error for each model on hello validation set.</span></span>
* <span data-ttu-id="6d6f8-307">ROC görbék megrajzolásához, érzékenységi elemzést, és hajtsa végre a modell értékelések más típusú.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-307">You can also plot ROC curves, perform sensitivity analysis, and do other types of model evaluations.</span></span>

<span data-ttu-id="6d6f8-308">Miután elkészült, létrehozási modelleket, válassza ki a hello **napló** tooview hello R kód futtatásához a munkamenet során Rattle fülre.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-308">Once you're finished building models, select hello **Log** tab tooview hello R code run by Rattle during your session.</span></span> <span data-ttu-id="6d6f8-309">Kiválaszthatja a hello **exportálása** gomb toosave azt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-309">You can select hello **Export** button toosave it.</span></span>

> [!NOTE]
> <span data-ttu-id="6d6f8-310">Hogy programhiba van Rattle jelenlegi kiadásában.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-310">There is a bug in current release of Rattle.</span></span> <span data-ttu-id="6d6f8-311">toomodify parancsfájl hello, vagy használatra, toorepeat a lépéseket később, a # karaktert elé kell beilleszteni * exportálása... Ez a napló * hello napló hello szövegben.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-311">toomodify hello script or use it toorepeat your steps later, you must insert a # character in front of *Export this log ... * in hello text of hello log.</span></span>
>
>

## <a name="postgresql--squirrel-sql"></a><span data-ttu-id="6d6f8-312">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="6d6f8-312">PostgreSQL & Squirrel SQL</span></span>
<span data-ttu-id="6d6f8-313">hello DSVM PostgreSQL telepítve rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-313">hello DSVM comes with PostgreSQL installed.</span></span> <span data-ttu-id="6d6f8-314">PostgreSQL egy olyan kifinomult, nyílt forráskódú relációs adatbázis.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-314">PostgreSQL is a sophisticated, open-source relational database.</span></span> <span data-ttu-id="6d6f8-315">Ez a szakasz bemutatja, hogyan tooload a levélszemét-adatkészlet PostgreSQL be, és majd lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-315">This section shows how tooload our spam dataset into PostgreSQL and then query it.</span></span>

<span data-ttu-id="6d6f8-316">Hello adatok betöltése előtt kell hello localhost tooallow jelszó-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-316">Before you can load hello data, you need tooallow password authentication from hello localhost.</span></span> <span data-ttu-id="6d6f8-317">A parancssorba:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-317">At a command prompt:</span></span>

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

<span data-ttu-id="6d6f8-318">Hello alsó hello konfigurációs fájl közel vannak, amelyek kapcsolatok engedélyezett hello több sorba:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-318">Near hello bottom of hello config file are several lines that detail hello allowed connections:</span></span>

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

<span data-ttu-id="6d6f8-319">Módosítása hello "IPv4-alapú helyi kapcsolatok" sor toouse md5 helyett ident, így azt is bejelentkezhetnek a felhasználónévvel és jelszóval:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-319">Change hello "IPv4 local connections" line toouse md5 instead of ident, so we can log in using a username and password:</span></span>

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

<span data-ttu-id="6d6f8-320">És hello postgres szolgáltatás újraindításához:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-320">And restart hello postgres service:</span></span>

    sudo systemctl restart postgresql

<span data-ttu-id="6d6f8-321">toolaunch psql, egy interaktív terminálon PostgreSQL hello beépített postgres felhasználóként, futtassa a következő parancsot a parancssorba hello:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-321">toolaunch psql, an interactive terminal for PostgreSQL, as hello built-in postgres user, run hello following command from a prompt:</span></span>

    sudo -u postgres psql

<span data-ttu-id="6d6f8-322">Hozzon létre egy új felhasználói fiókot, használatával hello azonos felhasználónév szerint hello Linux fiók jelenleg jelentkezett be, és adjon neki egy jelszó:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-322">Create a new user account, using hello same username as hello Linux account you're currently logged in as, and give it a password:</span></span>

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

<span data-ttu-id="6d6f8-323">A felhasználó nevében, majd jelentkezzen be toopsql:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-323">Then log in toopsql as your user:</span></span>

    psql

<span data-ttu-id="6d6f8-324">És hello adatok importálása egy új adatbázist:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-324">And import hello data into a new database:</span></span>

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

<span data-ttu-id="6d6f8-325">Most tegyük hello adatokba, és egyes lekérdezések futtatása használatával **Squirrel SQL**, egy grafikus eszközt, amely lehetővé teszi az adatbázisok JDBC-illesztőt keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-325">Now, let's explore hello data and run some queries using **Squirrel SQL**, a graphical tool that lets you interact with databases via a JDBC driver.</span></span>

<span data-ttu-id="6d6f8-326">tooget elindult, indítsa el a Squirrel SQL hello alkalmazások menüből.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-326">tooget started, launch Squirrel SQL from hello Applications menu.</span></span> <span data-ttu-id="6d6f8-327">hello illesztőprogramot tooset:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-327">tooset up hello driver:</span></span>

* <span data-ttu-id="6d6f8-328">Válassza ki **Windows**, majd **illesztőprogram megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-328">Select **Windows**, then **View Drivers**.</span></span>
* <span data-ttu-id="6d6f8-329">Kattintson a jobb gombbal a **PostgreSQL** válassza **módosítása illesztőprogram**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-329">Right-click on **PostgreSQL** and select **Modify Driver**.</span></span>
* <span data-ttu-id="6d6f8-330">Válassza ki **Extra osztály az elérési út**, majd **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-330">Select **Extra Class Path**, then **Add**.</span></span>
* <span data-ttu-id="6d6f8-331">Adja meg ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** a hello **Fájlnév** és</span><span class="sxs-lookup"><span data-stu-id="6d6f8-331">Enter ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** for hello **File Name** and</span></span>
* <span data-ttu-id="6d6f8-332">Válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-332">Select **Open**.</span></span>
* <span data-ttu-id="6d6f8-333">Válassza ki a lista illesztőprogramokat, majd válassza ki **org.postgresql.Driver** a **osztálynév**, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-333">Choose List Drivers, then select **org.postgresql.Driver** in **Class Name**, and select **OK**.</span></span>

<span data-ttu-id="6d6f8-334">hello kapcsolat toohello helyi kiszolgáló tooset:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-334">tooset up hello connection toohello local server:</span></span>

* <span data-ttu-id="6d6f8-335">Válassza ki **Windows**, majd **aliasok megjelenítése.**</span><span class="sxs-lookup"><span data-stu-id="6d6f8-335">Select **Windows**, then **View Aliases.**</span></span>
* <span data-ttu-id="6d6f8-336">Válassza ki a hello  **+**  gomb toomake új aliast.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-336">Choose hello **+** button toomake a new alias.</span></span>
* <span data-ttu-id="6d6f8-337">Nevezze el *levélszemét adatbázis*, válassza a **PostgreSQL** a hello **illesztőprogram** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-337">Name it *Spam database*, choose **PostgreSQL** in hello **Driver** drop-down.</span></span>
* <span data-ttu-id="6d6f8-338">Hello URL-Címének beállítása túl*jdbc:postgresql://localhost/spam*.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-338">Set hello URL too*jdbc:postgresql://localhost/spam*.</span></span>
* <span data-ttu-id="6d6f8-339">Adja meg a *felhasználónév* és *jelszó*.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-339">Enter your *username* and *password*.</span></span>
* <span data-ttu-id="6d6f8-340">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-340">Click **OK**.</span></span>
* <span data-ttu-id="6d6f8-341">tooopen hello **kapcsolat** ablakban kattintson duplán a hello ***levélszemét adatbázis*** alias.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-341">tooopen hello **Connection** window, double-click hello ***Spam database*** alias.</span></span>
* <span data-ttu-id="6d6f8-342">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-342">Select **Connect**.</span></span>

<span data-ttu-id="6d6f8-343">toorun néhány lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-343">toorun some queries:</span></span>

* <span data-ttu-id="6d6f8-344">Jelölje be hello **SQL** fülre.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-344">Select hello **SQL** tab.</span></span>
* <span data-ttu-id="6d6f8-345">Adja meg például egy egyszerű lekérdezést `SELECT * from data;` hello lekérdezés szövegmezőben hello SQL lapon hello tetején.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-345">Enter a simple query such as `SELECT * from data;` in hello query textbox at hello top of hello SQL tab.</span></span>
* <span data-ttu-id="6d6f8-346">Nyomja le az **Ctrl-adja meg a** toorun azt.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-346">Press **Ctrl-Enter** toorun it.</span></span> <span data-ttu-id="6d6f8-347">Alapértelmezés szerint visszaadja a hello Squirrel SQL első 100 sor a lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-347">By default Squirrel SQL returns hello first 100 rows from your query.</span></span>

<span data-ttu-id="6d6f8-348">Nincsenek további lekérdezések futtatása is tooexplore ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-348">There are many more queries you could run tooexplore this data.</span></span> <span data-ttu-id="6d6f8-349">Például hogyan működik a hello hello word gyakorisága *ellenőrizze* eltérnek a levélszemét és sonka?</span><span class="sxs-lookup"><span data-stu-id="6d6f8-349">For example, how does hello frequency of hello word *make* differ between spam and ham?</span></span>

    SELECT avg(word_freq_make), spam from data group by spam;

<span data-ttu-id="6d6f8-350">Mik azok a hello jellemzőit az e-mailek gyakran tartalmazó vagy *3d*?</span><span class="sxs-lookup"><span data-stu-id="6d6f8-350">Or what are hello characteristics of email that frequently contain *3d*?</span></span>

    SELECT * from data order by word_freq_3d desc;

<span data-ttu-id="6d6f8-351">A legtöbb e-mailek, amelyek magas előfordulása *3d* vannak látszólag levélszemét, így egy prediktív modellt tooclassify hello e-mailek készítéséhez egy hasznos funkció lehet.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-351">Most emails that have a high occurrence of *3d* are apparently spam, so it could be a useful feature for building a predictive model tooclassify hello emails.</span></span>

<span data-ttu-id="6d6f8-352">Ha egy PostgreSQL-adatbázisban tárolt adatok tooperform gépi tanulás, érdemes lehet [MADlib](http://madlib.incubator.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-352">If you wanted tooperform machine learning with data stored in a PostgreSQL database, consider using [MADlib](http://madlib.incubator.apache.org/).</span></span>

## <a name="sql-server-data-warehouse"></a><span data-ttu-id="6d6f8-353">SQL Server-adatraktár</span><span class="sxs-lookup"><span data-stu-id="6d6f8-353">SQL Server Data Warehouse</span></span>
<span data-ttu-id="6d6f8-354">Az Azure SQL Data Warehouse egy felhőalapú, horizontálisan felskálázható adatbázis, amely nagy mennyiségű relációs és nem relációs adatot képes feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-354">Azure SQL Data Warehouse is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span> <span data-ttu-id="6d6f8-355">További információkért lásd: [Mi az Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="6d6f8-355">For more information, see [What is Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span></span>

<span data-ttu-id="6d6f8-356">tooconnect toohello adatraktárát, és hozzon létre hello táblát, futtatási hello következő parancsot a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-356">tooconnect toohello data warehouse and create hello table, run hello following command from a command prompt:</span></span>

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

<span data-ttu-id="6d6f8-357">Ezután a hello sqlcmd parancssorból:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-357">Then at hello sqlcmd prompt:</span></span>

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

<span data-ttu-id="6d6f8-358">Másolja az adatokat a BCP-vel:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-358">Copy data with bcp:</span></span>

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> <span data-ttu-id="6d6f8-359">hello sorvégződések hello letöltött fájlt a Windows-stílusú, de bcp UNIX stílusú vár, ezért ellenőriznünk kell, hogy a hello - r jelző tootell bcp.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-359">hello line endings in hello downloaded file are Windows-style, but bcp expects UNIX-style, so we need tootell bcp that with hello -r flag.</span></span>
>
>

<span data-ttu-id="6d6f8-360">És lekérdezés az Sqlcmd használatával:</span><span class="sxs-lookup"><span data-stu-id="6d6f8-360">And query with sqlcmd:</span></span>

    select top 10 spam, char_freq_dollar from spam;
    GO

<span data-ttu-id="6d6f8-361">Az Squirrel SQL is lekérdezhet.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-361">You could also query with Squirrel SQL.</span></span> <span data-ttu-id="6d6f8-362">Hasonló lépésekkel PostgreSQL használatával hello Microsoft MSSQL Server JDBC-illesztőt, amely itt található: a ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-362">Follow similar steps for PostgreSQL, using hello Microsoft MSSQL Server JDBC Driver, which can be found in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d6f8-363">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d6f8-363">Next steps</span></span>
<span data-ttu-id="6d6f8-364">Témakörök, amelyek végigvezetik Önt az Azure-ban hello Adattudomány folyamat alkotó hello feladatok áttekintéséhez lásd: [Team adatok tudományos folyamat](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-364">For an overview of topics that walk you through hello tasks that comprise hello Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="6d6f8-365">Más végpont forgatókönyvek, amelyek hello Team adatok tudományos folyamat az adott forgatókönyveket hello lépéseit bemutatása ismertetését lásd: [Team adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md).</span><span class="sxs-lookup"><span data-stu-id="6d6f8-365">For a description of other end-to-end walkthroughs that demonstrate hello steps in hello Team Data Science Process for specific scenarios, see [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md).</span></span> <span data-ttu-id="6d6f8-366">hello forgatókönyvek is bemutatják, hogyan toocombine felhőalapú és helyszíni eszközök és szolgáltatások a munkafolyamatok és folyamat toocreate intelligens kérelmet.</span><span class="sxs-lookup"><span data-stu-id="6d6f8-366">hello walkthroughs also illustrate how toocombine cloud and on-premises tools and services into a workflow or pipeline toocreate an intelligent application.</span></span>
