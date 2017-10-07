---
title: "aaaTen lehetősége van az adatok tudományos virtuális gép hello |} Microsoft Docs"
description: "Hajtsa végre különböző adatok feltárása és modellezési feladat hello adattudomány virtuális gépet."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;weig;bradsev
ms.openlocfilehash: 4dfe22f14f00208c63e26ce44b05123c9ac4b850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ten-things-you-can-do-on-hello-data-science-virtual-machine"></a>Tíz végezhető műveletek hello adatok tudományos virtuális gép
hello Microsoft adatok tudományos virtuális gép (DSVM) olyan hatékony tudományos fejlesztési környezet, amely lehetővé teszi az Ön tooperform különböző adatok feltárása és modellezési feladatokat. hello környezet származik, már beépített csomagolt számos népszerű adatokkal, melyek könnyen tooget elemzőeszközök használatának gyors megkezdése az elemzési helyszíni, felhőalapú vagy hibrid telepítések. hello DSVM szorosan együttműködik olyan sok Azure-szolgáltatást, és képes tooread és a folyamat a Azure-ban az Azure SQL Data Warehouse, az Azure Data Lake, az Azure Storage, vagy az Azure Cosmos Adatbázisba már tárolt adatokat. Azt is kihasználhatják a más analytics eszközök, például az Azure Machine Learning és az Azure Data Factory.

Ebben a cikkben azt végigvezetik Önt toouse a DSVM tooperform különböző adattudomány feladatokat, és más Azure-szolgáltatásokkal kommunikálni. Az alábbiakban néhány hello DSVM elvégezhető hello egyikét:

1. Adatok vizsgálatát, és helyileg az hello segítségével a Microsoft R Server, Python DSVM modellek fejlesztése
2. A Jupyter notebook tooexperiment használ az adatok egy böngésző Python 2, Python 3, Microsoft R méretezhetőség és teljesítmény készült R készen áll a vállalati verziójának használata
3. Azok a modellek használatával készített R és Python az Azure Machine Learning szolgáltatást az ügyfélalkalmazások számára a szolgáltatások egyszerű webes felületen keresztül modellek
4. Felügyelheti az Azure-erőforrások Azure-portálon vagy a Powershell használatával
5. A tárhely kiterjesztése és nagy méretű adatkészletek megosztása / hozzon létre egy Azure File storage a DSVM csatlakoztatható meghajtóként között a teljes csoport kód
6. Kód megoszthatja a Githubon használatával munkatársaival, és hozzáférni a tárházat hello előre telepített Git ügyfelek - Git Bash Git grafikus felhasználói felület használatával.
7. Hozzáférjen a különböző Azure adatok és analitikák szolgáltatásokhoz hasonlóan az Azure blob storage Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & adatbázisok
8. Jelentéseket és hello előtelepített hello DSVM a Power BI Desktop használatával hozhat létre és telepíthet hello felhő
9. A DSVM toomeet a projektet dinamikusan méretezése
10. A virtuális gép további eszközök telepítése   

> [!NOTE]
> További használati díjak hello további adatok tárolási és elemzési szolgáltatások a cikkben szereplő számos vonatkoznak. Tekintse meg a toohello [Azure díjszabása](https://azure.microsoft.com/pricing/) a lapot.
> 
> 

**Előfeltételek**

* Azure-előfizetés szükséges. Regisztrálhat egy ingyenes próbaverzióra [Itt](https://azure.microsoft.com/free/).
* Hello Azure-portálon található adatok tudományos virtuális géphez történő üzembe helyezéséhez utasításokat érhetők el [egy virtuális gép létrehozása](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. Adatok vizsgálatát, és a Microsoft R Server vagy a Python modellek fejlesztése
Használhat olyan nyelveket R és Python toodo hasonlóan a adatelemzés jogosultsággal a hello DSVM.

Az R egy IDE "8.0 fordulat R vállalati" nevű hello start menüből vagy az asztal hello található is használhatja. A Microsoft közzétett további könyvtárak hello nyitott forrás/CRAN-R tooenable méretezhető elemzés és hello képességét tooanalyze adatok párhuzamos darabolt elemzés végrehajtásával engedélyezett hello memória mérete nagyobb felett. A choice hasonló egy R IDE is telepíthet [Rstudióból](https://www.rstudio.com/products/rstudio-desktop/).

A Python például a Visual Studio Community Edition hello Python Tools előre telepített Visual Studio (PTVS) kiterjesztés rendelkező egy IDE használhatják. Alapértelmezés szerint csak egy alapszintű Python 2.7 van konfigurálva a PTVS (nélkül analytics függvénytárat, például a SciKit, Pandas). A sorrend tooenable Anaconda Python 2.7-es és 3.5-ös verzióját toodo hello következőkre lesz szüksége:

* Hozzon létre egyéni környezetek egyes verzióihoz útvonalon túl**eszközök** -> **Python Tools** -> **Python-környezetek** , majd kattintson " **+ Egyéni**"a Visual Studio 2015 Community Edition hello
* Adjon meg leírást, majd állítsa be a hello környezet előtag elérési utakat *c:\anaconda* Anaconda Python 2.7 vagy *c:\anaconda\envs\py35* a Anaconda Python 3.5
* Kattintson a **az automatikus észlelés** , majd **alkalmaz** toosave hello környezetben.

Ez milyen hello egyéni környezet telepítése a Visual Studio tűnik.

![PTVS-telepítő](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

Lásd: hello [PVTS dokumentációban](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) a további részleteket a Python-környezetek toocreate.

Most már elkészült toocreate egy új Python-projektet. Keresse meg a túl**fájl** -> **új** -> **projekt** -> **Python** és hello típusának kiválasztása Python alkalmazást hoz létre. Hello Python-környezetben hello aktuális projekt toohello kívánt verziójával (Anaconda 2.7 vagy 3.5-ös) állíthatja be: kattintson a jobb gombbal hello **Python-környezetben**, jelölje be **hozzáadása Python-környezetek**, és Ezután válassza hello szükséges környezet tooassociate hello projekt esetében. PTVS hello termék kezelésével kapcsolatos további információ található [dokumentáció](https://github.com/Microsoft/PTVS/wiki) lap.

## <a name="2-using-a-jupyter-notebook-tooexplore-and-model-your-data-with-python-or-r"></a>2. A Jupyter Notebook tooexplore és a modell adatait a Python vagy használó R
Jupyter Notebook hello egy hatékony környezetben, amely egy webböngésző-alapú "IDE" biztosít az adatok feltárása és modellezési. Jupyter Notebook Python 2, Python 3 vagy R (nyílt forráskódú és hello Microsoft R Server) használhatja.

Jupyter Notebook toolaunch hello kattintson a hello start menü ikonra / asztali ikon című **Jupyter Notebook**. A hello DSVM is megkeresheti túl "https://localhost:9999 /" tooaccess hello Jupiter Notebookot. Ha azt kéri a jelszót, használja a hello utasításokat ***hogyan toocreate hello Jupyter notebook kiszolgálón egy erős jelszót*** hello szakasza [rendelkezés hello Microsoft adatok tudományos virtuális gép](machine-learning-data-science-provision-vm.md)témakör toocreate erős jelszó tooaccess hello Jupyter notebook. 

Hello notebook megnyitása után meg kell jelennie egy néhány példa notebookok, amelyek a hello DSVM előcsomagolt tartalmazó könyvtár. Most már a következőket teheti:

* Kattintson a hello notebook toosee hello kódot.
* Minden cella végrehajtása billentyűkombináció lenyomásával **a SHIFT + ENTER**.
* hello teljes jegyzetfüzet kattintva elindítja **cella** -> **futtatása**
* Hozzon létre egy új notebook hello Jupyter ikon (bal felső sarokban) parancsra, majd **új** hello megfelelő, és kattintson a hello notebook nyelvi (más néven kernelek) gombra.   

> [!NOTE]
> Jelenleg támogatjuk a Python 2.7, Python 3.5-ös és R. hello R kernel programozási támogatja mind nyílt forráskódú R, valamint hello vállalati méretezhető Microsoft R Server.   
> 
> 

Miután belépett hello notebook az adatokba, hello modell létrehozása, szalagtárak tetszőleges hello modell teszteléséhez.

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. R vagy Python és használatával Operationalize őket az Azure Machine Learning modellek létrehozása
Miután létrehozott és érvényesített hello következő lépésként modell általában toodeploy az éles környezetben. Ez lehetővé teszi az ügyfél alkalmazások tooinvoke hello modell előrejelzéseket egy valós idejű vagy kötegelt módban alapon. Az Azure Machine Learning a beépített R vagy Python modellt biztosít egy mechanizmus toooperationalize.

Azok az az Azure Machine Learning modellje, amikor egy webszolgáltatás-bővítmény, amely lehetővé teszi az ügyfelek toomake REST-hívások adjon át bemeneti paraméterekkel és előrejelzéseket fogadása hello modellből kimenetek van közzétéve.   

> [!NOTE]
> Ha nem még feliratkozott az Azure Machine Learning, ezt úgy szerezheti be egy szabad munkaterület vagy egy szabványos munkaterület hello felkeresésével [Azure Machine Learning Studio](https://studio.azureml.net/) kezdőlapját, majd kattintson az "első lépések".   
> 
> 

### <a name="build-and-operationalize-python-models"></a>Buildelés és azok Python modellek
Íme egy hello SciKit további szalagtárat használó egyszerű modell épülő Python Jupyter Notebook létre kódrészletét.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

hello metódus használt toodeploy a python-modellek tooAzure Machine Learning becsomagolja hello előrejelzését hello modell egy függvénynek, és azt hello előre telepített Azure Machine Learning python kódtár által biztosított attribútumokkal, az Azure Machine jelöl decorates Learning munkaterületének Azonosítóját, az API-kulcs és hello adjon meg, és térjen vissza a paraméterek.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

Egy ügyfél hajtsa végre hívások toohello webes szolgáltatás. Nincsenek kényelmi burkolók, amely a REST API-kérés hello összeállításához. Íme egy minta kód tooconsume hello webszolgáltatás-bővítmény.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> hello Azure Machine Learning könyvtár csak jelenleg támogatott a Python 2.7.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Buildelés és azok R modellek
Hello adatok tudományos virtuális gép vagy máshová helyezik Azure Machine Learning oly módon, amely hasonló toohow Python elkészült a beépített R modellek is telepíthet. Saját hello lépést:

* Hozzon létre egy settings.json fájl tooprovide a munkaterület azonosítója és a hitelesítési token, ahogy az alábbi példakód hello.
* a burkoló írni, ehhez a hello modell előrejelzése függvény.
* hívás ```publishWebService``` a hello Azure Machine Learning könyvtár toopass a hello függvény burkoló.  

Itt található hello eljárás és kód kódtöredékek használt tooset mentése, felépítéséhez, közzététele, és egy modellt használ az Azure Machine Learning webszolgáltatásként.

#### <a name="setup"></a>Beállítás
1. Hello Machine Learning R telepítéséhez írja be a ```install.packages("AzureML")``` fordulat R vállalati 8.0 IDE vagy az R IDE.
2. Töltse le a RTools [Itt](https://cran.r-project.org/bin/windows/Rtools/). Kell hello zip hello elérési útja (és a nevesített zip.exe) segédprogram toooperationalize az R csomag Machine Learning be.
3. Hozzon létre egy settings.json fájlt nevű alatt ```.azureml``` a kezdőkönyvtár alatt, és adjon meg hello paraméterek az Azure Machine Learning-munkaterület:

Settings.JSON fájlstruktúra:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>Az R egy modell létrehozása, és tegye közzé az Azure Machine Learning
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function toopublish based on hello model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-hello-model-deployed-in-azure-machine-learning"></a>Az Azure Machine Learning telepített hello modell felhasználása
hello Azure Machine Learning könyvtár toolook mentése hello használjuk tooconsume hello modell a ügyfélalkalmazás webszolgáltatás közzétevő neve hello segítségével `services` API-hívás toodetermine hello végpont. Ezt követően csak hívja hello `consume` működik, és adja át a hello adatok keret toobe előre jelezni.
a következő kód hello használt tooconsume hello modellt közzé az Azure Machine Learning webszolgáltatásként.

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use hello last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

További információ a hello Azure Machine Learning R könyvtárban található [Itt](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. Felügyelheti az Azure-erőforrások Azure-portálon vagy a Powershell használatával
hello DSVM csak nem teszi lehetővé toobuild a elemzési megoldások helyben hello a virtuális gép, de emellett lehetővé teszi a Microsoft Azure felhőbe tooaccess szolgáltatások. Azure több számítási, tárolási, analytics adatszolgáltatások és egyéb szolgáltatások, amelyek felügyeletéhez, és a DSVM hozzáférést biztosít.

tooadminister az Azure előfizetés és a felhő-erőforrások használhatja a böngésző és a pont toothe [Azure-portálon](https://portal.azure.com). Használhatja az Azure Powershell tooadminister, az Azure-előfizetés és az erőforrások egy parancsfájl segítségével.
Azure Powershell parancsikon hello asztali környezetben futnak, vagy hello a start menü "Microsoft Azure Powershell" című témakört. Tekintse meg [Microsoft Azure Powershell dokumentációs](../powershell-azure-resource-manager.md) hogyan felügyelhető az Azure-előfizetés és a Windows Powershell-parancsfájlokkal erőforrások olvashat.

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. A tárolóhely egy megosztott fájlrendszerrel kiterjesztése
Adatszakértőkön megoszthatja a nagy adatkészleteket, kódot, valamint más hello munkacsoporton belül. hello maga DSVM készül 70GB szabad lemezterület van. tooextend tárhelyét, használhat hello Azure szolgáltatás, és csatlakoztassa azt hello DSVM vagy-e férni a REST API-n keresztül.   

> [!NOTE]
> hello maximális lemezterület hello Azure Fájlszolgáltatás megosztás 5TB, és egyes fájl maximális mérete 1TB.   
> 
> 

Használhatja az Azure Powershell toocreate egy Azure Fájlszolgáltatás megosztás. Ez az Azure PowerShell toocreate egy Azure szolgáltatás fájlmegosztás hello parancsfájl toorun.

    # Authenticate tooAzure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under hello FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List hello share tooconfirm that everything worked
    Get-AzureStorageFile -Share $s


Most, hogy létrehozta az Azure fájlmegosztások, a virtuális gép az Azure-ban csatlakoztathatja azt. Lehetőleg ugyanazon Azure adatközpontba, ahol hello tárolási fiók tooavoid késést és az adatok adatátviteli díjakkal adott hello virtuális gép. Az Azure Powershell futtatható DSVM hello az alábbiakban hello parancsok toomount hello meghajtó.

    # Get storage key of hello storage account that has hello Azure file share from Azure portal. Store it securely on hello VM tooavoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount hello Azure file share as Z: drive on hello VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Most már hozzáférhet a meghajtó mint bármely virtuális gép hello normál meghajtóján.

## <a name="6-share-code-with-your-team-using-github"></a>6. Kód megosztása a csapat GitHub használatával
GitHub tárháza kód megkeresésének mintakód és a források számos különböző eszközök különböző technológiákkal hello fejlesztői Közösség által megosztott használatával. Hello technológia tootrack és a tároló verziója hello kódfájlok Git azt használja. GitHub egyben a platform ahol létrehozhat saját tárház toostore a csoport megosztott kód és dokumentáció, verziókezelést és megvalósításához is vezérlő akik hozzáférés tooview és kód közre. Látogasson el a hello [GitHub súgóoldalak](https://help.github.com/) további információt a Git használatával. Használhatja a GitHub hello módon toocollaborate rendelkezésre álló munkatársaival, hello közösségi által fejlesztett kód használata és kód hátsó toohello közösségi közreműködés.

hello DSVM már előre betöltött az ügyféleszközök elől mindkét parancssori és grafikus felhasználói Felülettel tooaccess GitHub-tárházban. a Git és GitHub hello parancssori eszköz toowork nevezik Git bash eszközt. Visual Studio hello DSVM telepítve van a hello Git extensions. Ezek az eszközök hello start menüben és hello asztali kezdeti ikonok találhat meg.

toodownload kódját a GitHub-tárházban hello használata ```git clone``` parancsot. Például toodownload tudományos adattárház hello aktuális könyvtárba Microsoft által kiadott futtathatja a következő parancsot, Miután belépett hello ```git-bash```.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

A Visual Studio, megteheti ugyanazon Klónozás hello. a következő képernyőfelvétel hello bemutatja, hogyan tooaccess a Git szoftver, a GitHub a Visual Studio eszközök.

![A Visual Studio Git](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

További tájékoztatást Git toowork a github.com több erőforrást a GitHub-tárházban található. Hello [adatlap](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) hasznos hivatkozás.

## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Hozzáférjen a különböző Azure adatok és analitikák szolgáltatásokhoz
### <a name="azure-blob"></a>Azure-blob
Az Azure blob a kis és nagy adatok megbízható, gazdaságos felhőtárhelyére. Ossza meg velünk tekintse meg hogyan adatok tooAzure Blob és a hozzáférési adatok tárolódnak az Azure Blob helyezheti.

**Előfeltétel**

* **Az az Azure Blob storage-fiók létrehozása [Azure-portálon](https://portal.azure.com).**

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Győződjön meg arról, hogy hello előre telepített parancssori AzCopy eszköz a következő címen található ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. Hello tartalmazó hello azcopy.exe tooyour elérési út környezeti változó tooavoid gépelési hello teljes parancs könyvtárútvonal adhat hozzá, az eszköz futtatásakor. AzCopy eszközről további információért tekintse meg túl[AzCopy dokumentációját](../storage/common/storage-use-azcopy.md)
* Indítsa el a hello Azure Tártallózó eszközt. Letölthető a [Microsoft Azure Tártallózó](http://storageexplorer.com/). 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

**Adatok áthelyezése VM tooAzure Blob: AzCopy**

toomove adatok között a helyi fájlokat és a blob storage segítségével is AzCopy parancssori vagy a PowerShell használatával:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Cserélje le **C:\myfolder** toohello elérési útra, ahol a fájl található, **mystorageaccount** tooyour blob storage-fiók neve, **mycontainer** toohello Tárolónév esetén **tárfiók kulcsa** tooyour blob tárelérési kulcs. A tárfiók hitelesítő adatainak a található [Azure-portálon](https://portal.azure.com).

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

AzCopy parancs futtatása, a PowerShell vagy a parancssorból. Íme néhány példa használati AzCopy parancs:

    # Copy *.sql from local machine tooa Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container tooLocal machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



Az AzCopy parancs toocopy tooan látja, hogy a fájl megjelennek Azure Tártallózó hamarosan Azure blob futtatásakor.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

**Adatok áthelyezése VM tooAzure Blob: Azure Tártallózó**

A virtuális gép Azure Tártallózó is feltöltheti hello helyi fájl adatait:

* tooupload adatok tooa tároló, jelölje be hello cél konténert, és kattintson hello **feltöltése** gomb.![ A Tártallózó feltöltése](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)
* Kattintson a hello **...**  sarkában hello toohello **fájlok** mezőben, jelöljön ki egy vagy több fájlok tooupload hello fájlrendszer majd **feltöltése** hello fájlok feltöltése toobegin.![ Fájlok tooblob feltöltése](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)

**Adatokat olvasni az Azure Blob: gépi tanulás olvasó modul**

Az Azure Machine Learning Studióban használhatja egy **és adatokat importálhat modul** tooread adatokat a blobból.

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

**Adatokat olvasni az Azure Blob: Python ODBC**

Használhat **BlobService** könyvtár tooread adatok közvetlenül a Jupyter Notebook vagy Python programban blob.

Először importálja a szükséges csomagokat:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Majd a beépülő modul az Azure Blob-fiók hitelesítő adatait, és adatokat olvasni Blob:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds toodownload "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'hello size of hello data is: %d rows and  %d columns' % df1.shape

hello adatolvasás az adatok keretként:

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Az Azure Data Lake Storage egy kapacitású adattár a big data elemzés számítási feladatokat és a Hadoop elosztott fájlrendszerrel (HDFS) kompatibilis. Hello Hadoop ökoszisztémájának és hello Azure Data Lake Analytics is működik. Megmutatjuk, hogyan adatok áthelyezése az Azure Data Lake Store hello rendszerbe, és futtassa a analytics az Azure Data Lake Analytics használatának.

**Előfeltétel**

* Hozzon létre az Azure Data Lake Analytics a [Azure-portálon](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* Hello **Azure Data Lake Tools** a **Visual Studio** ezzel található [hivatkozás](https://www.microsoft.com/download/details.aspx?id=49504) hello amelyek hello virtuális gép a Visual Studio Community Edition már telepítve van. Visual Studio kezdő, és az Azure-előfizetése van bejelentkezve, megtekintheti az Azure Data Analytics-fiók és a tárolási hello bal oldali panel a Visual Studio.

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

**Adatok áthelyezése VM tooData Lake: Azure Data Lake-kezelővel**

Használhat **Azure Data Lake Explorer** tooupload adatokat a virtuális gép tooData Lake tárolóban hello helyi fájlokból.

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

Is létrehozható egy adatok adatcsatorna tooproductionize az adatok mozgása tooor az Azure Data Lake hello segítségével [Azure adatok Factory(ADF)](https://azure.microsoft.com/services/data-factory/). Azt tekintse meg, hogy toothis [cikk](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide hello lépéseket toobuild hello adatokat nyújt a folyamatok.

**Adatokat olvasni az Azure Blob tooData Lake: U-SQL**

Ha az adatok Azure Blob Storage tárolóban található, közvetlenül olvashatja adatokat az Azure storage-blobból az U-SQL-lekérdezésben. A U-SQL-lekérdezés létrehozása, előtt győződjön meg arról a blob storage-fiók csatolt tooyour Azure Data Lake. Nyissa meg túl**Azure-portálon**, az Azure Data Lake Analytics irányítópulton található, majd **adatforrás hozzáadása**, tárolási típusa túl**Azure Storage** , és csatlakoztassa az Azure Storage Fiók neve és kulcsa. Esetén a rendszer képes tooreference hello adatok hello tárfiók tárolja.

![Adja meg a tárfiók és a kulcs](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

A Visual Studio-adatok olvasása a blob-tároló, hajtson végre néhány adatkezelési, a szolgáltatás mérnöki csapathoz, és kimeneti hello eredményül kapott adatok tooeither Azure Data Lake vagy Azure Blob Storage. Ha hello adatokat a blob Storage tárolóban hivatkozik, **wasb: / /**; Ha hello adatok Azure Data Lake, használjon hivatkozik **swbhdfs: / /**

![Adatok keret](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

A következő U-SQL-lekérdezések Visual Studio hello használhatja:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    too"swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    too"wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



Miután a lekérdezés elküldött toohello kiszolgáló, ábra: a feladat állapotának hello jelenik meg.

![Feladat állapota diagramja](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

**A Data Lake adatait: U-SQL**

Miután az Azure Data Lake hello dataset van okozhatnak, [U-SQL nyelv](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery és hello adatokba. U-SQL nyelv hasonló eszköz SQL, de egyesíti az egyes szolgáltatások, a C#, hogy a felhasználók írhat, egyéni modulokat, felhasználó által definiált függvények és stb. Az előző lépésben hello hello parancsfájlokat használhat.

Miután hello lekérdezés elküldött tooserver, tripdata_summary. Fürt megosztott kötetei szolgáltatás található hamarosan **Azure Data Lake Explorer**, előfordulhat, hogy előzetes megtekintéséhez kattintson a jobb gombbal hello fájl hello adatait.

![Az Azure Data Lake Explorer fájl](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

toosee hello fájlinformációt:

![Fájl összegzése](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop-fürtök
Az Azure HDInsight egy olyan felügyelt Apache Hadoop, a Spark, a HBase és a Storm szolgáltatás hello felhő. Azure HDInsight-fürtök hello adatok tudományos virtuális gépről könnyen dolgozhat.

**Előfeltétel**

* Az az Azure Blob storage-fiók létrehozása [Azure-portálon](https://portal.azure.com). Ez a tárfiók nem HDInsight-fürtök használt toostore adatok.

![Azure Blob storage-fiók létrehozása](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Testre szabhatja az Azure HDInsight Hadoop-fürtök a [Azure-portálon](machine-learning-data-science-customize-hadoop-cluster.md)
  
  * Hello storage-fiók létrehozásakor a HDInsight-fürthöz létrehozott kell kapcsolni. Ez a tárfiók hello fürtön belül feldolgozható adatainak eléréséhez használatos.

![HDInsight-fürthöz létrehozott hivatkozás toostorage fiók](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* Engedélyeznie kell az **távelérési** toohello átjárócsomópont hello fürt létrehozása után. Az itt megadott (eltérnek a saját létrehozásakor hello fürt számára megadott) hello távoli hozzáférési hitelesítő adatok megjegyzése: hello későbbi eljárásban kell.

![Távelérés engedélyezése](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Hozzon létre egy Azure Machine Learning munkaterülettel. A gépi tanulási kísérletekhez a Machine Learning-munkaterület vannak tárolva. A kijelölt hello-beállítások kiválasztása a portálon, ahogy az alábbi képernyőfelvétel a hello:

![Azure Machine Learning-munkaterület létrehozása](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* Ezután írja be a hello paraméterek a munkaterület

![Adja meg a Machine Learning-munkaterület paraméterek](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* Töltse fel az adatok IPython Notebook használatával. Először importálja a szükséges csomagokat, a beépülő modul hitelesítő adatokat, egy adatbázis létrehozása a tárfiókban lévő, majd adatok tooHDI fürtök betölteni.

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create hello connection tooHive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage tooHDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* Ehelyett kövesse a [forgatókönyv](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC Taxi adatok tooHDI fürt. Fő lépések az alábbiak:
  
  * AzCopy: Töltse le a tömörített CSV nyilvános blob tooyour helyi mappából
  * AzCopy: töltse fel a tömörítetlen fürt megosztott kötetei szolgáltatás a helyi mappa tooHDI fürtből
  * Hadoop-fürt átjárócsomópontjához hello bejelentkezni, és készítse elő a felderítő adatelemzés

Miután hello adatok betöltött tooHDI fürthöz, ellenőrizheti a Azure Tártallózó adatai. És egy adatbázis nyctaxidb HDI-fürtöt létrehozni.

**Az adatok feltárása: a Python Hive-lekérdezések**

Mivel hello adatokat a Hadoop-fürttel, használhatja a hello pyodbc csomag tooconnect tooHadoop fürtök és az adatbázis lekérdezése Hive toodo feltárására és a szolgáltatás műszaki osztály használatával. Hello hello előfeltétel lépésben létrehozott meglévő táblák tekintheti meg.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Meglévő táblák megtekintése](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Nézzük hello bejegyzések száma a minden hónap és hello gyakoriságát Formabontó, vagy nem található a hello út táblában:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Bejegyzések száma havonta a ábrázolása](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![Tipp gyakoriságot ábrázolása](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Azt is számítási hello felvételi helye és dropoff helye közötti távolság szerint, és hasonlítsa össze toohello út távolságot.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![A felvételi és dropoff tábla](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![A felvételi/dropoff távolság tootrip távolság ábrázolása](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Most tegyük készítse el lefelé-mintát (1 %) adatokat a modellezési. A Machine Learning-olvasó modul használhatjuk ezeket az adatokat.

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of hello join into hello preceding internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Egy kis idő után látható hello adatok betöltése a Hadoop-fürtök:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Az adatok tábla](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

**A Machine Learning segítségével HDI-adatok olvasása: olvasó modul**

Is használhatja hello **olvasó** modul a Machine Learning Studio tooaccess hello adatbázisban az Hadoop-fürt. Csatlakoztassa a HDI-fürtökhöz hello hitelesítő adatait, és Azure Storage-fiók tooenable ing gépi tanulási modellek, adatbázist használ a HDI-fürtök létrehozása.

![Olvasó modul tulajdonságai](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

hello pontozott adatkészletet is megtekinthetők:

![Pontozott dataset megtekintése](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a>Az Azure SQL Data Warehouse & adatbázisok
Az SQL Data Warehouse az egy rugalmas data warehouse az SQL Server révén vállalati szintű szolgáltatásként.

Jelen hello utasításokat követve építhető ki az Azure SQL Data Warehouse [cikk](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md). Miután az Azure SQL Data Warehouse kiépítése ezzel [forgatókönyv](machine-learning-data-science-process-sqldw-walkthrough.md) toodo adatok feltöltése, áttekintését és modellezési hello SQL Data Warehouse adatainak használatával.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos-adatbázis egy NoSQL-adatbázis hello felhőben. Lehetővé teszi a dokumentumok például JSON toowork, és lehetővé teszi a toostore és lekérdezés hello dokumentumokat.

Egy szükséges lépéseket tooaccess Azure Cosmos DB követően – hello DSVM toodo hello van szüksége.

1. A DocumentDB Python SDK telepítése (Futtatás ```pip install pydocumentdb``` parancssorból)
2. Hozzon létre egy Azure Cosmos DB fiókot és az adatbázis [Azure-portálon](https://portal.azure.com)
3. "Azure Cosmos DB áttelepítési eszköz" letölthető [Itt](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) és bontsa ki az Ön által választott tooa könyvtár
4. Adatimportálás JSON (mexikói adatok) tárolja egy [nyilvános blob](https://cahandson.blob.core.windows.net/samples/volcano.json) be a következő parancs paraméterei toohello áttelepítési eszköz (dtui.exe hello Cosmos DB áttelepítési eszköz telepítési hello könyvtárból) Cosmos DB. Adja meg a hello forrása és célja helyet ezekkel a paraméterekkel:
   
    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[kulcs]; adatbázis mexikói /t.Collection:volcano1 =

Miután hello adatokat importál, elvégezheti a tooJupyter és a nyitott hello notebook című *DocumentDBSample* python, amely tartalmazza a DocumentDB tooaccess code, és tegye a néhány alapvető lekérdezése. További kapcsolatos Cosmos DB hello szolgáltatás felkeresésével [dokumentációs oldal](https://docs.microsoft.com/azure/cosmos-db/).

## <a name="8-build-reports-and-dashboard-using-hello-power-bi-desktop"></a>8. Jelentések és hello Power BI Desktop használatával hozhat létre.
Ossza meg velünk megjelenítheti a Power BI toogain visual insightsban hello adatokká Cosmos DB példa megelőző hello bekerül hello mexikói JSON-fájl. Részletes utasítások találhatók hello [Power BI-cikk](../cosmos-db/powerbi-visualize.md). Az alábbiakban hello magas szintű lépéseket:

1. Nyissa meg a Power BI Desktopban, és hajtsa végre az "Adatok beolvasása". Adja meg a hello az URL-címet: https://cahandson.blob.core.windows.net/samples/volcano.json
2. Megtekintheti az importált listaként hello JSON bejegyzések
3. Táblázatból hello lista tooa, a Power BI együttműködhet hello azonos
4. Bontsa ki a hello oszlopok hello kattintva bontsa ki a ikon (hello egy hello "balra nyíl billentyűt és a jobbra mutató nyílra" ikon sarkában hello oszlop hello)
5. Figyelje meg, hogy a helye "Rekord" mező. Bontsa ki a hello rekord, és csak hello koordináták válassza. Koordináta egy listaoszlop
6. Vegyen fel egy új oszlop tooconvert hello lista koordináta oszlopot vesszővel külön LatLong oszlopba hozzáfűzésével hello két elemeinek hello koordináta listamező hello képlet szerint ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Végül átalakítása hello ```Elevation``` oszlop tooDecimal és select hello **Bezárás** és **alkalmaz**.

Helyett az előző lépésekben, illessze be a következő kódot, amely a kimenő használt hello lépéseket hello hello speciális szerkesztő a Power bi-ban, amely lehetővé teszi egy lekérdezési nyelv a toowrite hello adatátalakítást.

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted tooTable" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted tooTable", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



Most már rendelkezik hello adatokat a Power BI adatmodell. A Power BI desktop megjelenjen-e az alábbiak szerint:

![Power BI Desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

Megkezdheti a jelentések és a képi megjelenítések hello adatok modell használatával. A lépésekkel hello ezen [Power BI-cikk](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild jelentést. hello end eredménye egy jelentést, amely a következőhöz hasonló hello.

![A Power BI Desktop jelentés nézet – Power BI connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-toomeet-your-project-needs"></a>9. A DSVM toomeet a projektet dinamikusan méretezése
A projekt kell hello DSVM toomeet fel és le méretezheti. Ha nem kívánja toouse hello VM hello este vagy a hétvégeken, csak leállíthat hello a virtuális gép hello [Azure-portálon](https://portal.azure.com).

> [!NOTE]
> Ön tudomásával költségeket, ha csupán hello operációs rendszer Leállítás gombját hello virtuális gép használ.  
> 
> 

Ha meg kell toohandle néhány nagy méretű elemzési és több Processzor és/vagy a memóriát és/vagy a lemez kapacitását CPU mag, memóriakapacitása és lemeztípusok (beleértve az SSD-meghajtókon) a számítási és a költségvetési igényeinek megfelelő Virtuálisgép-méretek széles választéka találja. hello az óránkénti számítási árképzési együtt a virtuális gépek teljes listája megtalálható hello [Azure Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/) lap.

Hasonlóképpen ha csökkenti a virtuális gép feldolgozási kapacitás szükség (például: egy nagyobb munkaterhelés tooa Hadoop vagy egy Spark-fürt helyezte), lefelé hello hello fürt méretezheti [Azure-portálon](https://portal.azure.com) toohello beállításokat a virtuális gép, és a példány. Íme egy képernyőkép.

![Virtuálisgép-példány beállítások](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. A virtuális gép további eszközök telepítése
A Microsoft csomagolását biztosak vagyunk abban, hogy a rendszer képes tooaddress hello közös adatok analytics igényeinek, és számos takaríthat meg kerülése tooinstall alkalommal és állítsa be a környezetek egyenként és takaríthat meg, hogy pénzt fizető csak olyan erőforrásokat, amikor több eszközt használjon.

Kihasználhatja a más az Azure data, és ez a cikk tooenhance analytics környezetét analytics szolgáltatások csatolást. Tudjuk, hogy bizonyos esetekben az igényeinek szükség lehet további eszközeit, beleértve az egyes védett külső eszközök. Teljes rendszergazdai hozzáféréssel rendelkezzen hello virtuális gép tooinstall új eszközök van szüksége. A Python és előre nem telepített R további csomagokat is telepíthet. Python-hez is használhatja ```conda``` vagy ```pip```. Az R hello használható ```install.packages()``` hello R-konzol vagy hello IDE használja, és válassza a "**csomagok** -> **csomagok telepítése...** ".

## <a name="summary"></a>Összefoglalás
Ezek a néhány hello műveleteket végezhet el hello Microsoft adatok tudományos virtuális gép. Számos dolgot további toomake teheti azt egy hatékony analytics környezetben.

