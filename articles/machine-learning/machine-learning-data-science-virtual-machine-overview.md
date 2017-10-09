---
title: "aaaWhat az adatok tudományos virtuális gép? | Microsoft Docs"
description: "Hogyan tooget el ez a kulcs analytics forgatókönyvek az adatok tudományos virtuális gépek."
keywords: "adatok tudományos eszközök, adatok tudományos virtuális gép, adattudomány, linux adattudomány eszközei"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 18c7a75208671c663f3b6be6ee8d0bf666772e01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Bevezetés toohello felhőalapú adatok tudományos virtuális gép Linux és Windows rendszerekhez
hello adatok tudományos virtuális gép (DSVM) a Microsoft Azure felhőbe kifejezetten a adattudomány beépített testreszabott Virtuálisgép-lemezképet. Számos népszerű adattudomány rendelkezik, és egyéb eszközök előre telepítve, és előre konfigurált toojump indítási speciális elemzésekre intelligens alkalmazások létrehozásához. Elérhető Windows Server és Linux rendszeren. A DSVM Windows-kiadását Server 2016 és Server 2012 rendszeren tesszük elérhetővé. Linux kiadása hello DSVM Ubuntu 16.04 LTS és OpenLogic 7.2 CentOS alapú Linux terjesztésekről fel. 

Ez a témakör ismerteti, mire képes a hello adatok tudományos VM, néhány főbb forgatókönyvek hello hello VM használatához, hello Windows és Linux rendszereken érhető el a legfontosabb jellemzők hello részletezi és indításának a használatuk tooget kapcsolatos utasításokat tartalmazza.

## <a name="what-can-i-do-with-hello-data-science-virtual-machine"></a>Mit tehetek hello adatok tudományos virtuális gépet?
hello hello adatok tudományos virtuális gép célja tooprovide adatok szakemberei összes szakértelem szintek és szerepkört és egy súrlódás szabad adatok tudományos környezetben. Ez a virtuális gép, ha Ön volt összehasonlítható környezet saját töltött idő jelentős menti. Ehelyett a tudományos adatprojekthez azonnal elindítani az újonnan létrehozott Virtuálisgép-példány. 

hello adatok tudományos virtuális gép tervezett és a széles körű használati forgatókönyvek való munkához konfigurálva van. Méretezheti a környezet felfelé vagy lefelé, a projekt igényeinek változását. Akkor tudja toouse a kívánt nyelvet tooprogram tudományos feladatok vannak. Más eszközök telepítése, és testre szabhatja a hello rendszer pontos igényeinek.

## <a name="key-scenarios"></a>Főbb forgatókönyvek
Ez a szakasz javasol néhány főbb forgatókönyvek, mely hello az adatok tudományos virtuális gép is telepíthető.

### <a name="preconfigured-analytics-desktop-in-hello-cloud"></a>Előre konfigurált analytics asztalához hello felhő
hello adatok tudományos VM adatok tudományos csoportjai tooreplace keresése a helyi asztali környezetek felügyelt felhőalapú asztali alapkonfiguráció biztosít. Ennél az alaptervnél biztosítja, hogy a csoport összes hello adatszakértőkön egy mely tooverify kísérletek az egységes telepítő-e, és együttműködés elősegítése. Költségeket csökkenti hello sysadmin terheket is csökkenti, és mentik hello idő a szükséges tooevaluate, telepítése és karbantartása hello különböző szoftvercsomagok advanced analytics toodo szükséges.  

### <a name="data-science-training-and-education"></a>Adatelemzési képzés és oktatás
Vállalati oktatók és nevelők, amely általában mutatja meg a tudományos adatosztályokat adja meg a virtuális gép lemezképének tooensure, a diákok egységes telepítő rendelkezik-e, és hello minták kiszámítható működik. hello adatok tudományos VM egy igény szerinti környezetet egy egységes telepítő, amely megkönnyíti a hello támogatás és kompatibilitási kihívást hoz létre. Olyan esetben, ha ezekben a környezetekben kell toobe beépített gyakran, különösen a rövidebb képzési osztályok, jelentősen előnyeit.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Igény szerinti rugalmas kapacitás nagyméretű projektek
Adatok tudományos hackathons/versenyek vagy nagy méretű adatok modellezési és kutatási funkciójával szükséges horizontálisan felskálázott hardverének kapacitása, általában rövid időre. Adatok tudományos VM hello segítségével replikálni hello adatok tudományos környezet gyorsabban igény szerint, a méretezett, kiszolgálók, amelyek lehetővé teszik a nagy teljesítményű számítástechnikai erőforrások toobe igénylő kísérletek futtassa.

### <a name="short-term-experimentation-and-evaluation"></a>Rövid távú kísérletezhet és kiértékelése
hello adatok tudományos VM használt tooevaluate és ismerje meg az eszközök például a Microsoft R Server, SQL Server, a Visual Studio eszközök Jupyter, mély tanulási / ML eszközök gazdag, és új eszközök a népszerű közösségi a telepítő minimális erőfeszítéssel hello. Hello adatok tudományos VM gyorsan állítható be, mert azt más rövid távú használati forgatókönyvek például a közzétett kísérleteket, bemutatók, online munkamenetet vagy konferencia oktatóanyagok a következő forgatókönyvek végrehajtása replikálása lehet alkalmazni.

### <a name="deep-learning"></a>A részletes tanulás
hello adattudomány VM mély tanulási algoritmusok használata GPU (grafikus feldolgozóegység) alapú hardveren tanítási modell használható. Virtuális gép az Azure-felhőbe képességei skálázás okhoz, DSVM segít GPU-alapú hardveres hello felhő szükség szerint. Megváltoztathatja az egyik tooa GPU-alapú virtuális gép nagy modell betanításakor vagy kell megőrzéséről során a nagy sebességű számítások hello azonos operációsrendszer-lemez.  Windows Server 2016 hello kiadása DSVM előre előre telepített GPU-illesztőprogrammal keretrendszerek és részletes tanulási algoritmus hello GPU verziója. A hello Linux, mély learning GPU csak engedélyezett hello [adatok tudományos virtuális gép Linux (Ubuntu) edition](http://aka.ms/dsvm/ubuntu). Hello Ubuntu Windows-2016. december edition adatok tudományos VM toonon GPU-alapú Azure virtuális gép ebben az esetben minden hello mély tanulási keretrendszerek fog tartalék toohello CPU módban telepítheti. Korábban, a Windows Server 2012 a Microsoft közzétett egy [mély eszközkészlet tanulási](http://aka.ms/dsvm/deeplearning) , de most azt javasoljuk, Windows Server 2016-os Windows-alapú mély tanulási munkaterhelések. hello DSVM hello CentOS alapú Linux kiadása csak hello CPU-buildekről néhány hello mély tanulási eszközök (CNTK, Tensorflow, MXNet), de nem előtelepített hello GPU illesztőprogramok és keretrendszerek tartalmazza. 

## <a name="whats-included-in-hello-data-science-vm"></a>Hello adatok tudományos VM tartalmát?
hello adatok tudományos virtuális gép számos népszerű adattudomány és mély tanulási eszközök már telepítve és konfigurálva van. Eszközök, melyek könnyen toowork Azure adatok és analitikák termékeivel is tartalmaz. A prediktív modellek létrehozása nagy méretű adatkészletek hello Microsoft R Server használatával vagy az SQL Server 2016-kiszolgálón, és fel. Az állomást, más eszközök hello nyílt forráskódú Közösségből származó és a Microsoft egyaránt tartalmazza, valamint minta kódot és notebookok. hello alábbi táblázat részletezi, és összehasonlítja hello fő összetevői a hello Windows és Linux hello adatok tudományos virtuális gép verziója.


| **Eszköz**                                                           | **Windows-kiadás** | **Linux Edition** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R nyitott](https://mran.microsoft.com/open/) előtelepített népszerű csomagokkal   |I                      | I             |
| [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/) Developer Edition tartalmazza, <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-getting-started) párhuzamos és elosztott nagy teljesítményű R keretrendszer<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml-introduction) -a Microsoft új-az-a-legkorszerűbb ML algoritmusok <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R Operationalization](https://msdn.microsoft.com/microsoft-r/operationalize/about)                                            |I                      | I <br/> (MicrosoftML még nem érhető el)|
| [A Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) mellett plusz a megosztott aktiválási - Excel, a Word és a PowerPoint   |I                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7, előre telepített népszerű csomagokkal 3.5    |I                      |I              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) előtelepített Ágnes nyelvhez népszerű csomagokkal                         |I                      |I              |
| Relációs adatbázisok                                                            | [SQL Server 2016 SP1](https://www.microsoft.com/sql-server/sql-server-2016) <br/> Fejlesztői Edition| [PostgreSQL](https://www.postgresql.org/) |
| Adatbázis-eszközök                                                       | * Az SQL Server Management Studio <br/>* Az SQL Server Integration Services<br/>* [BCP, Sqlcmd használatával](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * ODBC/JDBC illesztőprogramok| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (eszköz lekérdezése), <br /> * bcp, Sqlcmd használatával <br /> * ODBC/JDBC illesztőprogramok|
| Az SQL Server R services méretezhető adatbázis-elemzés | I     |N              |
| **[Jupyter Notebook Server](http://jupyter.org/) a következő mag,**                                  | I     | I |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | I | I |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 & 3.5 | I | I |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Ágnes | I | I |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | N | I |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | I (csak Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | I |
| JupyterHub (többfelhasználós notebookok kiszolgáló)| N | I |
| **Fejlesztői eszközök, IDEs és kód szerkesztők**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [A Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) > Git beépülő modul, az Azure HDInsight (Hadoop), a Data Lake, SQL Server Data tools [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs), és [R eszközök a Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | I | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [A Visual Studio Code](https://code.visualstudio.com/) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rstudióból asztali](https://www.rstudio.com/products/rstudio/#Desktop) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rstudióból kiszolgáló](https://www.rstudio.com/products/rstudio/#Server) | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Az Atom](https://atom.io/) | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Ágnes IDE)](http://junolab.org/)| I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim és Emacs | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* A Git és GitBash | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* A .net-keretrendszer | I | N |
| Power bi Desktop | I | N |
| SDK-k tooaccess Azure és a Cortana Intelligence Suite szolgáltatások | I | I |
| **Adatmozgatást és a felügyeleti eszközök** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Az azure Storage Explorer | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Az Azure parancssori felület](https://docs.microsoft.com/cli/azure/overview) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* Az azure Powershell | I | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | I | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (az Azure Data Lake-tároló)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | I | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [DocDB adatáttelepítési eszköz](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | I | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [A Microsoft adatkezelési átjáró](https://msdn.microsoft.com/library/dn879362.aspx) : adatok áthelyezése a helyi üzemeltetésű és a felhő között | I | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* A Unix/Linux rendszerű parancssori segédeszközök | I | I |
| [Apache részletezési](http://drill.apache.org) az adatok feltárása | I | I |
| **Machine Learning-eszközök** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integráció [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](http://rattle.togaware.com/) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | I (csak Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | I (csak Ubuntu) |
| **GPU-alapú mély tanulási eszközök** |Windows Server 2016 edition  |Ubuntu edition |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft kognitív Toolkit (CNTK)](http://cntk.ai) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Tensorflow](https://www.tensorflow.org/) | I | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | I | I|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia számjegy](https://github.com/NVIDIA/DIGITS) | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, CUDNN, Nvidia illesztőprogram](https://developer.nvidia.com/cuda-toolkit) | I | I |
| **A big Data Platform (csak Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Helyi [Spark](http://spark.apache.org/) önálló | N | I |
| &nbsp;&nbsp;&nbsp;&nbsp;* Helyi [Hadoop](http://hadoop.apache.org/) (HDFS, YARN) | N | I |



## <a name="how-tooget-started-with-hello-windows-data-science-vm"></a>Tooget indításának hello Windows adatok tudományos VM
* Navigáljon hello szükséges Windows DSVM edition példányának létrehozása
  * [Windows Server 2016-alapú DSVM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.windows-data-science-vm)
  
  vagy 
  * [Windows Server 2012-alapú DSVM](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) 
* Kattintson a hello **most az beszerzése informatikai** gombra.
* Jelentkezzen be a virtuális gép toohello hello virtuális gép létrehozásakor megadott hello hitelesítő adatokkal a távoli asztalról.
* toodiscover, és indítsa el hello eszközök érhető el, kattintson a hello **Start** menü.

## <a name="get-started-with-hello-linux-data-science-vm"></a>Ismerkedés a Linux adatok tudományos VM hello
* Szükségeskonfiguráció-hello példányának létrehozása Linux DSVM edition útvonalon túl
  * [Ubuntu alapú DSVM](http://aka.ms/dsvm/ubuntu)

  vagy

  * [OpenLogic CentOS alapú DSVM](http://aka.ms/dsvm/centos)

  
* Kattintson a hello **most töltse le innen** gombra.
* Jelentkezzen be a virtuális gép toohello egy SSH-ügyfél, például a Putty vagy SSH-parancs, hello virtuális gép létrehozásakor megadott hello hitelesítő adataival.
* Hello rendszerhéj kér adja meg a dsvm-további-adatokat.
* Grafikus asztali, töltse le a saját ügyfélplatformjára hello X2Go ügyfél [Itt](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) hello Linux adatok tudományos VM dokumentumban hello utasítások [rendelkezés hello Linux adatok tudományos virtuális gép](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Következő lépések
### <a name="for-hello-windows-data-science-vm"></a>A Windows Data tudományos VM hello
* Hogyan toorun adott eszközökhöz érhető el a Windows hello-verzió a további információkért lásd: [Microsoft adatok tudományos virtuális gép kiépítése hello](machine-learning-data-science-provision-vm.md) és
* További információ a hogyan tooperform különböző feladatokat, a tudományos adatok projekt hello windowsos virtuális gép számára: [tíz lehetősége van a virtuális gép adattudomány hello](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-hello-linux-data-science-vm"></a>A Linux adatok tudományos VM hello
* Hogyan toorun meghatározott eszközök hello Linux verzió érhető el a további információkért lásd: [Linux adatok tudományos virtuális gép kiépítése hello](machine-learning-data-science-linux-dsvm-intro.md).
* Ez a forgatókönyv bemutatja, hogyan tooperform több közös adattudomány feladatokat a hello Linux virtuális gép, lásd: [adattudomány hello Linux adatok tudományos virtuális gép a](machine-learning-data-science-linux-dsvm-walkthrough.md).

