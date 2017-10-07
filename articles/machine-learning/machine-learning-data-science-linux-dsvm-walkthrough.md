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
# <a name="data-science-on-hello-linux-data-science-virtual-machine"></a>A Linux adatok tudományos virtuális gép hello adattudomány
Ez a forgatókönyv bemutatja, hogyan tooperform több közös adattudomány feladatokat a hello Linux adatok tudományos virtuális gép. hello Linux adatok tudományos virtuális gép (DSVM) érhető el, amely adatelemzés és a gépi tanulás általánosan használt eszközöket együtt telepített Azure virtuálisgép-lemezkép. hello kulcs szoftverösszetevőket hello van felsorolva [Linux adatok tudományos virtuális gép kiépítése hello](machine-learning-data-science-linux-dsvm-intro.md) témakör. hello Virtuálisgép-lemezkép segítségével könnyen tooget történt az adatok tudományos (percben), anélkül, hogy tooinstall elindult, és külön-külön konfigurálása mindegyik hello eszközök. Könnyedén növelheti hello virtuális gép, ha szükséges, és állítsa le, ha nincsenek használatban. Ehhez az erőforráshoz, mind a rugalmas és költséghatékony.

hello tudományos feladatok ebben a forgatókönyvben bemutatott lépések hello hello leírt [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Ez a folyamat egy rendszeres megközelítés toodata tudományos tooeffectively hello életciklusát intelligens alkalmazások keresztül együttműködve megosszanak adatszakértőkön csapatoknak nyújt. hello adatok tudományos folyamat egy iteratív keretet is biztosít, amely egy adott követhetnek adattudomány.

A Microsoft hello elemzése [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset ebben a forgatókönyvben. Ez egy olyan van megjelölve, vagy a levélszemét, vagy a sonka (ami azt jelenti, amelyek nincsenek levélszemét), e-maileket, és néhány statisztikai hello e-mailek tartalma hello is tartalmaz. hello statisztika része ismerteti hello mellett azonban több szakaszt.

## <a name="prerequisites"></a>Előfeltételek
Adatok tudományos Linux virtuális gépek használata előtt hello következő kell rendelkeznie:

* Egy **Azure-előfizetés**. Ha még nem rendelkezik egy, lásd: [ma létrehozása az ingyenes Azure-fiókjával](https://azure.microsoft.com/free/).
* A [ **adattudomány Linux virtuális gép**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). A virtuális gép további információkért lásd: [Linux adatok tudományos virtuális gép kiépítése hello](machine-learning-data-science-linux-dsvm-intro.md).
* [X2Go](http://wiki.x2go.org/doku.php) telepítve a számítógépre, és egy XFCE munkamenet megnyitása. Telepítésével és konfigurálásával kapcsolatos egy **X2Go ügyfél**, lásd: [telepítése és konfigurálása X2Go ügyfél](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client). 
* Egy **AzureML fiók**. Ha még nem rendelkezik egy újat: hello regisztráljon [AzureML kezdőlap](https://studio.azureml.net/). Van egy ingyenes használati réteg toohelp használatának első lépéseit.

## <a name="download-hello-spambase-dataset"></a>Töltse le a hello spambase adatkészlet
Hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) adatkészlet olyan viszonylag kicsi, amely csak 4601 példákat tartalmaz adatokat. Ez esetén egy kényelmes mérete toouse bemutatásához, hogy néhány hello funkciói hello adatok tudományos virtuális gép, mert a tartja hello erőforrásigények mérsékelt.

> [!NOTE]
> Ez a forgatókönyv a D2 v2 méretű Linux adatok tudományos virtuális gépek hozták létre. Ez a méret DSVM ebben a bemutatóban hello eljárások kezelésére képes.
>
>

Ha több tárhelyet igényel, további lemezeket hozhat létre, és csatolja őket tooyour virtuális gép. Ezeknek a lemezeknek állandó Azure-tárolót, használ, így a adataikat esetén is megőrződik még kiszolgáló hello van újra kiépíteni esedékes tooresizing vagy le van állítva. a lemez tooadd és mellékelje tooyour VM, hello utasításait követve [adja hozzá a lemezt tooa Linux virtuális gép](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ezeket a lépéseket hello Azure parancssori felület (Azure CLI), amelyen már telepítve van a hello DSVM használja. Ezért teljes mértékben a virtuális gépért hello ezekkel az eljárásokkal végezhető el. Egy másik lehetőség tooincrease tároló toouse [az Azure files](../storage/files/storage-how-to-use-files-linux.md).

toodownload hello adatok, nyisson meg egy terminálablakot, és futtassa a parancsot:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

hello letöltött fájl nem rendelkezik a fejlécsor, így hozzon létre egy másik fájlba, amelyen fejlécet. Futtassa a fájlt a parancs toocreate hello megfelelő fejlécek:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Majd összefűzésére hello két fájlt hello parancs együtt:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

hello adatkészlet egyes e-mail számos különböző típusú statisztikák rendelkezik:

* Oszlopok, például ***word\_freq\_WORD*** hello százaléka hello e-mailben a szavakat, amelyek jelzik *WORD*. Például ha *word\_freq\_ellenőrizze* értéke 1 és 1 % hello e-mailben szereplő minden szó volt *ellenőrizze*.
* Oszlopok, például ***char\_freq\_CHAR*** hello százalékos karaktereit hello e-mailben, amelyek jelzik *CHAR*.
* ***beruházási\_futtatása\_hossza\_leghosszabb*** hello leghosszabb hossza nagybetűvel sorozata.
* ***beruházási\_futtatása\_hossza\_átlagos*** nagybetűvel összes sorozatát hello átlagos hossza.
* ***beruházási\_futtatása\_hossza\_teljes*** nagybetűvel összes sorozatát hello teljes hossza.
* ***Levélszemét*** azt jelzi, hogy hello e-mail tekintették levélszemét, vagy nem (1 = levélszemét, 0 = nem levélszemét).

## <a name="explore-hello-dataset-with-microsoft-r-open"></a>Microsoft R nyitott hello adatkészlet felfedezés
Most hello megtekintésével, és hajtsa végre a mellékelt adatok tudományos VM R. hello néhány alapvető gépi tanulás [Microsoft R nyitott](https://mran.revolutionanalytics.com/open/) előre telepítve. hello többszálas matematikai szalagtárak R ezen verziója jobb teljesítményt nyújtanak egyszálas különböző verzióiban. Microsoft R nyitott is biztosít reprodukálhatósági használatával hello CRAN csomag tárház pillanatképet.

Ebben a bemutatóban Klónozás hello használt hello tooget példánya Kódminták **Azure-Machine-tanulás-adatok-tudományos** segítségével git, amely előre telepítve van a virtuális gép hello tárházba. Futtassa parancssorból hello git:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Nyisson meg egy terminálablakot, és indítson el egy új R munkamenetet hello R interaktív konzolt.

> [!NOTE]
> Az alábbi eljárásokat hello Rstudióból is használható. tooinstall Rstudióból, az adott parancs végrehajtásához:`./Desktop/DSVM\ tools/installRStudio.sh`
>
>

tooimport hello adatokat, és állítsa be hello környezetet, futtassa:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

egyes oszlopok statisztikája összefoglaló toosee:

    summary(data)

Másik nézet hello adatok:

    str(data)

Ez azt jelenti, hogy az egyes típusú hello és hello először hello adatkészlet kevés érték.

Hello *levélszemét* oszlop beolvasott érték, de ténylegesen egy kategorikus változó (vagy tényező). tooset típusa:

    data$spam <- as.factor(data$spam)

toodo néhány felderítő elemzése, használjon hello [ggplot2](http://ggplot2.org/) csomag, az R, amely már telepítve van a virtuális gép hello népszerű nyújtó grafikus könyvtár. Megjegyzés: hello összefoglalókat jelenik meg a korábban, a statisztikák tudunk hello felkiáltójel karakter hello gyakoriságát. Most megrajzolásához e gyakoriságot Itt a következő parancsok hello:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Hello nulla sáv hello rajzot van döntés, mivel most selejtezni azt:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

1 kereső érdekes fent nem triviális sűrűségű van. Most, hogy az adatok vizsgáljuk meg:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Ezután ossza levélszemét vs sonka szerint:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Ezek a példák kell lehetővé teszik toomake hasonló ábrái hello más oszlopok tooexplore hello a bennük található adatok.

## <a name="train-and-test-an-ml-model"></a>Betanítása és tesztelni az ML-modell
Most tegyük a vonat gépi tanulás néhány modellek hello adatkészlet tooclassify hello e-mailek vagy tartalmazóként span vagy sonka. Azt a döntési fa modellek és ebben a szakaszban egy véletlenszerű erdő modell betanítását, és tesztelje a azok az előrejelzés pontosságát.

> [!NOTE]
> hello rpart (rekurzív particionálás és regressziós fák) csomag, amelyet a következő kód hello hello adatok tudományos VM már telepítve van.
>
>

Először hozzuk hello dataset felosztása tanítási és tesztelési beállítása:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

Majd hozza létre a döntési fa tooclassify hello e-maileket.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Hello eredménye a következő:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

milyen mértékben hello képzési hajt végre toodetermine megadásához használja a következő kód hello:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

toodetermine mennyire azt hello TesztKészlet végez:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Próbáljuk meg is véletlenszerű erdőmodell. Véletlenszerű erdők számos döntési fák betanítása, és kimeneti egy osztály, amely az összes hello egyedi döntési fák hello besorolások hello mód. Nagyobb teljesítményű machine learning-módszer, azok küszöbölje ki a döntési fa modell toooverfit képzési dataset hello hogy biztosítanak.

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-tooazure-ml"></a>A modell tooAzure ML telepítése
[Az Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) egy felhőszolgáltatás, amely lehetővé teszi könnyen toobuild és központi telepítése a prediktív elemzési modellek. Hello töltött funkcióit AzureML egyike annak lehetősége toopublish bármely R webszolgáltatásként működik. hello AzureML R csomagot teszi hello DSVM közvetlenül az R munkamenetet a központi telepítés könnyen toodo.

toodeploy hello döntési fa kód hello korábbi szakaszában, a Machine Learning Studio tooAzure toosign kell. A munkaterület azonosítója és a egy engedélyezési jogkivonat toosign van szükség. toofind alábbi értékekkel és inicializálási hello AzureML-változók őket:

Válassza ki **beállítások** hello bal oldali menüben. Megjegyzés: a **MUNKATERÜLET azonosítója**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Válassza ki **engedélyezési jogkivonatok** hello általános menüből, és vegye figyelembe a **elsődleges engedélyezési jogkivonat**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Betöltési hello **AzureML** csomag és utána állítsa be a jogkivonatot, és a munkaterület Azonosítóját a hello változók értékeit az R-munkamenetben a hello DSVM:

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Ez a bemutató könnyebb tooimplement most leegyszerűsíti a hello modell toomake. Hello döntési fa legközelebbi toohello legfelső szintű három változók hello kiválasztják, és csak ezen három változók használata új fa létrehozása:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Igazolnia kell, hogy egy előrejelző függvényben, amely hello szolgáltatások bemenetként, és vissza hello előre jelzett értékek:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Hello predictSpam függvény tooAzureML hello segítségével közzététele **publishWebService** függvény:

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Ez a függvény argumentuma hello **predictSpam** működik, létrehoz egy nevű webszolgáltatás-bővítmény **spamWebService** a meghatározni a bemenetekhez és kimenetekhez, és új végpont hello információt ad vissza.

Hello részleteinek megtekintése a közzétett webes szolgáltatás, beleértve az API-végpont és a hozzáférés a hello paranccsal kulcsokat:

    spamWebService[[2]]

tootry azt a hello hello TesztKészlet az első 10 sor:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Rendelkezésre álló eszközök
hello fennmaradó szakaszok bemutatják, hogyan néhány hello eszközök telepíteni toouse hello Linux adatok tudományos virtuális Gépet. Eszközök tárgyalt hello listája itt található:

* XGBoost
* Python
* Jupyterhub
* Rattle
* PostgreSQL & Squirrel SQL
* SQL Server-adatraktár

## <a name="xgboost"></a>XGBoost
[XGBoost](https://xgboost.readthedocs.org/en/latest/) olyan eszköz, amely gyors és pontos súlyozott fa valósítja meg.

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

XGBoost is meghívhatja a python vagy a parancssorból végzi.

## <a name="python"></a>Python
A fejlesztési pythonos környezetekben a hello Anaconda Python terjesztéseket 2.7 és 3.5-ös hello DSVM sikeresen telepítette.

> [!NOTE]
> hello Anaconda terjesztési tartalmaz [Condas](http://conda.pydata.org/docs/index.html), amely különböző verziói és/vagy a bennük foglalt telepített csomagok rendelkező Python használt toocreate egyéni környezetek lehet.
>
>

Most hello spambase adatkészlet egyes olvasása és hello e-mailek besorolására scikit a támogatási vektoros gépekkel – ismerje meg:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

toomake előrejelzéseket:

    clf.predict(X.ix[0:20, :])

tooshow hogyan toopublish AzureML végpont, most egy egyszerűbb modell ellenőrizze hello három változó azt korábban közzétett Microsoft hello R-modell hasonló.

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

toopublish hello modell tooAzureML:

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
> Ez a lehetőség csak a python 2.7, és még nem támogatott a 3.5-ös verzióját. Futtassa a **/anaconda/bin/python2.7**.
>
>

## <a name="jupyterhub"></a>Jupyterhub
hello Anaconda terjesztési a hello DSVM tartalmaz Jupyter notebook, a többplatformos környezetben tooshare Python, R vagy Ágnes kódot és elemzését. hello Jupyter notebook JupyterHub keresztül érhető el. A helyi Linux-felhasználónév és jelszó használatával bejelentkezik ***https://\<VM DNS-nevét vagy IP-cím\>: 8000 /***. Minden konfigurációs fájlt a JupyterHub könyvtárban találhatók **/etc/jupyterhub**.

Több minta jegyzetfüzetet már telepítve van a virtuális gép hello:

* Lásd: hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) egy minta Python jegyzetfüzet.
* Lásd: [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) egy minta **R** notebookot.
* Lásd: hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) egy másik minta **Python** notebookot.

> [!NOTE]
> Ágnes nyelvi hello parancssorából hello hello Linux adatok tudományos VM is érhető el.
>
>

## <a name="rattle"></a>Rattle
[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (R analitikai eszköz tooLearn hello könnyen) egy grafikus R eszköz adatbányászathoz. Egy egyszerűen elsajátítható felülete, így könnyen tooload, megismeréséhez és adatok átalakítása és build és modellek kiértékeléséhez.  hello cikk [Rattle: A Data Mining GUI az R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) biztosít a forgatókönyv azt mutatja be, a szolgáltatásai.

Telepítse, és indítsa el a következő parancsok hello Rattle:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> Telepítési hello DSVM nem szükséges. De Rattle késztethetik tooinstall további csomagokat betöltéskor.
>
>

Rattle egy lapon-alapú felületet használja. Hello lapok többsége felel meg a hello toosteps [adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), például az adatok betöltése vagy felfedezését. hello tudományos folyamata a bal oldali tooright hello lapok keresztül zajlik. Azonban hello utolsó lap tartalmaz hello R parancsok futtatásához Rattle naplóját.

tooload és adatkészlet hello konfigurálása:

* tooload hello fájlt, jelölje be hello **adatok** lapra, majd
* Válasszon hello választó mellett túl**Fájlnév** válassza **spambaseHeaders.data**.
* tooload hello fájlt. Válassza ki **Execute** gombok hello legfelső sorában. Megtekintheti az egyes oszlopok, beleértve az azonosított adattípushoz, hogy bemeneti, cél vagy más típusú változó, és egyedi értékek számának hello összegzését.
* Rattle helyesen észlelt hello **levélszemét** oszlop hello célként. Jelölje be hello levélszemét oszlop, majd a készlet hello **cél adattípus** túl**Categoric**.

tooexplore hello adatokat:

* Jelölje be hello **böngészés** fülre.
* Kattintson a **összefoglaló**, majd **Execute**, toosee hello változó típusok és néhány statisztikák kapcsolatos információkat.
* tooview más típusú statisztikák változókról, válassza ki az egyéb beállítások, például a **adja** vagy **alapjai**.

Hello **böngészés** lapon is lehetővé teszi számos osztályon tevékenységtérkép toogenerate. a hisztogram hello adatok tooplot:

* Válassza ki **Terjesztéseket**.
* Ellenőrizze **hisztogram** a **word_freq_remove** és **word_freq_you**.
* Válassza ki **hajtható végre**. Meg kell jelennie egy grafikonon ablakban, amennyiben törölje a jelet tevékenységtérkép mindkét sűrűség adott hello word "Ön" gyakran sokkal megjelenik az e-mailek, mint az "Eltávolítás".

hello korrelációs előkészítésére érdekes egyaránt. egy toocreate:

* Válasszon **korrelációs** , hello **típus**, majd
* Válassza ki **hajtható végre**.
* Rattle figyelmezteti, hogy legfeljebb 40 változók javasol. Válassza ki **Igen** tooview hello rajzot.

Van néhány érdekes közti korrelációk elérni: "technológia" erősen korrelált túl "HP" és "labs", például. Azt is erősen visszamenőleges korrelációban állnak túl "650", mert a hello dataset adók hello körzetszám 650.

hello számértékek hello korrelációk szavakat közötti hello Intéző ablakában érhetők el. Fontos, hogy érdekes toonote, például, hogy "technológia" negatívan tartozzanak "a" és "pénzt".

Rattle hello dataset toohandle alakíthatja át néhány gyakori problémákat. Például lehetővé teszi toorescale funkciókat, imputálására a hiányzó értékeket, kiugró kezelni és változók vagy megfigyelések hiányzó adatok eltávolítása. Rattle is azonosíthatja a társítási szabályok megfigyelések és/vagy változók között. A lapok kívül esnek a hatókörön bevezető forgatókönyvhöz.

Rattle fürt analysis is végrehajtható. Bizonyos funkciók toomake hello kimeneti könnyebb tooread most kizárása. A hello **adatok** lapra, majd **figyelmen kívül hagyása** következő tooeach hello változók tíz elemekhez kivételével:

* word_freq_hp
* word_freq_technology
* word_freq_george
* word_freq_remove
* word_freq_your
* word_freq_dollar
* word_freq_money
* capital_run_length_longest
* word_freq_business
* Levélszemét

Ezután térjen vissza toohello **fürt** lapra, majd **KMeans**, és a set hello *számát* too4. Majd **hajtható végre**. hello eredmények hello kimeneti ablakban jelennek meg. Egy fürt nagy gyakoriságú "hp" és "nagy szóval" rendelkezik, és valószínűleg egy valós üzleti e-mailt.

egy egyszerű döntési fa gépi tanulási modell toobuild:

* Jelölje be hello **modell** lap
* Válasszon **fa** , hello **típus**.
* Válassza ki **Execute** toodisplay hello fa hello szöveg formájában a kimeneti ablakban.
* Jelölje be hello **megrajzolásához** gomb tooview egy grafikus verziót. Ez azt a korábban beszerzett nagyon hasonló toohello fa keres *rpart*.

Hello Rattle töltött szolgáltatásai közül a képes toorun több gépi tanulás módszerek, és gyorsan értékeli őket. Hello folyamat során a rendszer:

* Válasszon **összes** a hello **típus**.
* Válassza ki **hajtható végre**.
* Művelet befejeződése után kattintson a bármely egyetlen **típus**, például **SVM**, és az hello eredmények megtekintéséhez.
* Összehasonlíthatja hello teljesítmény hello modellek segítségével hello hello ellenőrzés **Evaluate** fülre. Például hello **hiba mátrix** kijelölés megjeleníti a hello zavart mátrix, általános hiba, és az egyes átlagolt osztály hiba hello érvényesítési beállítása a.
* ROC görbék megrajzolásához, érzékenységi elemzést, és hajtsa végre a modell értékelések más típusú.

Miután elkészült, létrehozási modelleket, válassza ki a hello **napló** tooview hello R kód futtatásához a munkamenet során Rattle fülre. Kiválaszthatja a hello **exportálása** gomb toosave azt.

> [!NOTE]
> Hogy programhiba van Rattle jelenlegi kiadásában. toomodify parancsfájl hello, vagy használatra, toorepeat a lépéseket később, a # karaktert elé kell beilleszteni * exportálása... Ez a napló * hello napló hello szövegben.
>
>

## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL
hello DSVM PostgreSQL telepítve rendelkezik. PostgreSQL egy olyan kifinomult, nyílt forráskódú relációs adatbázis. Ez a szakasz bemutatja, hogyan tooload a levélszemét-adatkészlet PostgreSQL be, és majd lekérdezése.

Hello adatok betöltése előtt kell hello localhost tooallow jelszó-hitelesítést. A parancssorba:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Hello alsó hello konfigurációs fájl közel vannak, amelyek kapcsolatok engedélyezett hello több sorba:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Módosítása hello "IPv4-alapú helyi kapcsolatok" sor toouse md5 helyett ident, így azt is bejelentkezhetnek a felhasználónévvel és jelszóval:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

És hello postgres szolgáltatás újraindításához:

    sudo systemctl restart postgresql

toolaunch psql, egy interaktív terminálon PostgreSQL hello beépített postgres felhasználóként, futtassa a következő parancsot a parancssorba hello:

    sudo -u postgres psql

Hozzon létre egy új felhasználói fiókot, használatával hello azonos felhasználónév szerint hello Linux fiók jelenleg jelentkezett be, és adjon neki egy jelszó:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

A felhasználó nevében, majd jelentkezzen be toopsql:

    psql

És hello adatok importálása egy új adatbázist:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Most tegyük hello adatokba, és egyes lekérdezések futtatása használatával **Squirrel SQL**, egy grafikus eszközt, amely lehetővé teszi az adatbázisok JDBC-illesztőt keresztül kommunikál.

tooget elindult, indítsa el a Squirrel SQL hello alkalmazások menüből. hello illesztőprogramot tooset:

* Válassza ki **Windows**, majd **illesztőprogram megjelenítése**.
* Kattintson a jobb gombbal a **PostgreSQL** válassza **módosítása illesztőprogram**.
* Válassza ki **Extra osztály az elérési út**, majd **hozzáadása**.
* Adja meg ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** a hello **Fájlnév** és
* Válassza ki **nyitott**.
* Válassza ki a lista illesztőprogramokat, majd válassza ki **org.postgresql.Driver** a **osztálynév**, és válassza ki **OK**.

hello kapcsolat toohello helyi kiszolgáló tooset:

* Válassza ki **Windows**, majd **aliasok megjelenítése.**
* Válassza ki a hello  **+**  gomb toomake új aliast.
* Nevezze el *levélszemét adatbázis*, válassza a **PostgreSQL** a hello **illesztőprogram** legördülő listán.
* Hello URL-Címének beállítása túl*jdbc:postgresql://localhost/spam*.
* Adja meg a *felhasználónév* és *jelszó*.
* Kattintson az **OK** gombra.
* tooopen hello **kapcsolat** ablakban kattintson duplán a hello ***levélszemét adatbázis*** alias.
* Kattintson a **Csatlakozás** gombra.

toorun néhány lekérdezést:

* Jelölje be hello **SQL** fülre.
* Adja meg például egy egyszerű lekérdezést `SELECT * from data;` hello lekérdezés szövegmezőben hello SQL lapon hello tetején.
* Nyomja le az **Ctrl-adja meg a** toorun azt. Alapértelmezés szerint visszaadja a hello Squirrel SQL első 100 sor a lekérdezésből.

Nincsenek további lekérdezések futtatása is tooexplore ezeket az adatokat. Például hogyan működik a hello hello word gyakorisága *ellenőrizze* eltérnek a levélszemét és sonka?

    SELECT avg(word_freq_make), spam from data group by spam;

Mik azok a hello jellemzőit az e-mailek gyakran tartalmazó vagy *3d*?

    SELECT * from data order by word_freq_3d desc;

A legtöbb e-mailek, amelyek magas előfordulása *3d* vannak látszólag levélszemét, így egy prediktív modellt tooclassify hello e-mailek készítéséhez egy hasznos funkció lehet.

Ha egy PostgreSQL-adatbázisban tárolt adatok tooperform gépi tanulás, érdemes lehet [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server-adatraktár
Az Azure SQL Data Warehouse egy felhőalapú, horizontálisan felskálázható adatbázis, amely nagy mennyiségű relációs és nem relációs adatot képes feldolgozni. További információkért lásd: [Mi az Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

tooconnect toohello adatraktárát, és hozzon létre hello táblát, futtatási hello következő parancsot a parancssorba:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Ezután a hello sqlcmd parancssorból:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Másolja az adatokat a BCP-vel:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> hello sorvégződések hello letöltött fájlt a Windows-stílusú, de bcp UNIX stílusú vár, ezért ellenőriznünk kell, hogy a hello - r jelző tootell bcp.
>
>

És lekérdezés az Sqlcmd használatával:

    select top 10 spam, char_freq_dollar from spam;
    GO

Az Squirrel SQL is lekérdezhet. Hasonló lépésekkel PostgreSQL használatával hello Microsoft MSSQL Server JDBC-illesztőt, amely itt található: a ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Következő lépések
Témakörök, amelyek végigvezetik Önt az Azure-ban hello Adattudomány folyamat alkotó hello feladatok áttekintéséhez lásd: [Team adatok tudományos folyamat](http://aka.ms/datascienceprocess).

Más végpont forgatókönyvek, amelyek hello Team adatok tudományos folyamat az adott forgatókönyveket hello lépéseit bemutatása ismertetését lásd: [Team adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md). hello forgatókönyvek is bemutatják, hogyan toocombine felhőalapú és helyszíni eszközök és szolgáltatások a munkafolyamatok és folyamat toocreate intelligens kérelmet.
