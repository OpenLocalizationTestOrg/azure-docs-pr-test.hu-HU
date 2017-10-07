---
title: "a Microsoft Data tudományos virtuális gép aaaProvision hello |} Microsoft Docs"
description: "Konfigurálja és tudományos adatok virtuális gép létrehozása az Azure elemzéséhez és a gépi tanulás."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: 907a3bdc7e480d05e8e245f5e50d632900fcf471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-hello-microsoft-data-science-virtual-machine"></a>Hello Microsoft adatok tudományos virtuális gép kiépítése
hello Microsoft adatok tudományos virtuális gép a Windows Azure virtuális gép (VM) előtelepített és konfigurált számos népszerű eszköz adatelemzés és a gépi tanulás általánosan használt lemezkép. hello részét képező eszközök a következők:

* Microsoft R Server Developer Edition
* Anaconda Python elosztási
* Jupyter notebook (az R, Python mag)
* A Visual Studio Community Edition
* Power BI Desktop
* SQL Server 2016 Developer Edition
* Gépi tanulás és adatelemzés eszközök
  * [Számítási hálózati Toolkit (CNTK)](https://github.com/Microsoft/CNTK): A Microsoft Research software eszközkészletet tanulási mély.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): gyors machine learning-rendszer támogatása, például a online, a kivonatoló, allreduce, csökkentése, learning2search, aktív, és interaktív tanulási.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): gyors és pontos súlyozott fa megvalósítási biztosító eszközt.
  * [Rattle](http://rattle.togaware.com/) (R analitikai eszköz tooLearn hello könnyen): olyan eszköz, amely lehetővé teszi az első lépések adatelemzés és a gép R GUI-alapú adatok feltárása, ezzel megkönnyítik a tanulási, és az R-kód automatikus generálása modellezési.
  * [mxnet](https://github.com/dmlc/mxnet): a teljesítmény és a rugalmasságot biztosít a részletes tanulási-keretrendszert
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : A látványelemek adatainak adatbányászati és gépi tanulási szoftver Java nyelven.
  * [Apache részletezési](https://drill.apache.org/): sémamentes SQL lekérdezési motorja a Hadoop, a nosql-alapú és a felhőalapú tárolást.  ODBC és JDBC felületek tooenable lekérdezése nosql-alapú és a szabványos Üzletiintelligencia-eszközök, például a Power BI, az Excel, a Tableau fájlokat támogatja.
* Az R és Python a szalagtárak használja az Azure Machine Learning és más Azure-szolgáltatásokkal
* Git bash eszközt toowork beleértve többek között a github webhelyen, a Visual Studio Team Services forráskódú adattárakban Git
* Számos népszerű Linux parancssori segédprogram (beleértve a awk, csökkentésének, perl, grep, keresés, wget, curl stb) Windows-port parancssor keresztül érhető el. 

Adattudomány Ez magában foglalja a feladatok sorozata léptetés:

1. Keresése, betöltése, és előzetesen feldolgozni az adatokat
2. Összeállításakor és tesztelésekor modellek
3. Az intelligens alkalmazásokban felhasználásra hello modellek telepítéséről

Adatszakértőkön használható különböző eszközök toocomplete ezeket a feladatokat. Azt kell túl sok időt vesz igénybe toofind hello hello szoftver, megfelelő verzióját, és majd letöltheti és telepítheti őket. hello Microsoft adatok tudományos virtuális gép a terheket megkönnyítheti azzal, hogy biztosít egy használatra kész lemezképnek, amely telepíthető az Azure összes számos népszerű eszköz előtelepített és konfigurált. 

a Microsoft Data tudományos virtuális gép hello jump-starts analytics projektjéhez. R, Python, SQL és C# például különböző nyelveken feladatok toowork lehetővé teszi. A Visual Studio egy IDE-toodevelop biztosít, és tesztelheti a kódját, amely könnyen toouse. hello szerepel a virtuális gép hello Azure SDK lehetővé teszi toobuild az alkalmazások különböző szolgáltatások segítségével a Microsoft felhőalapú platformja. 

Nincsenek az adatok tudományos VM-lemezkép szoftver költségek. Csak kell fizetnie hello Azure használati díjak mely hello virtuális gép kiépítése hello méretétől függ. További részleteket a hello számítási díjakat hello hello az árazás részletes adatait tartalmazó részben található [adatok tudományos virtuális gép](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) lap. 

## <a name="other-versions-of-hello-data-science-virtual-machine"></a>Hello adatok tudományos virtuális gép egyéb verziói
A [CentOS](machine-learning-data-science-linux-dsvm-intro.md) kép is elérhető, legtöbb hello azonos eszközei, Windows-lemezkép hello. Egy [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md) kép is, amelyek számos hasonló eszközök és keretrendszerek tanulási mély érhető el.

## <a name="prerequisites"></a>Előfeltételek
A Microsoft Data tudományos virtuális gép létrehozásához, hello következő kell rendelkeznie:

* **Azure-előfizetés**: tooobtain egy, lásd: [beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure-tárfiók**: toocreate egy, lásd: [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account). Másik lehetőségként hello tárfiók hello virtuális gép létrehozása, ha nem szeretné, hogy egy meglévő fiókkal toouse hello folyamat részeként is létrehozható.

## <a name="create-your-microsoft-data-science-virtual-machine"></a>A Microsoft Data tudományos virtuális gép létrehozása
Az alábbiakban hello lépéseket toocreate hello Microsoft adatok tudományos virtuális gép egy példányát:

1. Keresse meg a listázása toohello virtuális gép [Azure-portálon](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2. Jelölje be hello **létrehozása** gombra, és egy varázsló figyelembe hello alsó toobe.![ Konfigurálja-adatok-tudományos-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3. hello használt varázsló toocreate hello Microsoft adatok tudományos virtuális géphez szükséges **bemenetek** az egyes hello **öt lépése** hello sarkában található ezen az ábrán a számba. Az alábbiakban hello szükséges bemeneti adatok tooconfigure minden az alábbi lépéseket:
   
   1. **Alapvető beállítások**
      
      1. **Név**: az adatok tudományos kiszolgáló létrehozásakor nevét.
      2. **Felhasználónév**: rendszergazdai fiók bejelentkezési azonosító.
      3. **Jelszó**: rendszergazdai fiók jelszavát.
      4. **Előfizetés**: Ha több előfizetéssel rendelkezik, válasszon hello egyik mely hello a gép toobe létrehozott és számlázható.
      5. **Erőforráscsoport**: hozhat létre egy új vagy meglévő csoport használata.
      6. **Hely**: Select hello adatközpont, amely a legjobban megfelelő. Általában akkor hello adatközpont, amely az adatok, vagy legközelebbi tooyour helynév leggyorsabb hálózati hozzáféréshez.
   2. **Méret**: válassza ki, amely megfelel a funkcionális és költség megkötések hello kiszolgáló típusát. További lehetőségek a VM-méretek kaphat kiválasztása a "Nézet All".
   3. **Beállítások**:
      
      1. **Lemez típusa**: válassza a prémium szintű Ha inkább egy SSD-meghajtót (SSD), ellenkező esetben válassza a "Standard".
      2. **A Tárfiók**: hozzon létre egy új Azure-tárfiók az előfizetésben, vagy használjon egy meglévőt a hello azonos *hely* , amely a hello választott **alapjai** hello varázsló.
      3. **Más paraméterek**: általában csak hello alapértelmezett értékeket használunk. Abban az esetben, ha azt szeretné, hogy nem alapértelmezett értékek felhasználása tooconsider hello is rámutat hello egyes mezőkkel hello tájékoztató hivatkozásra.
   4. **Összefoglalás**: Győződjön meg arról, hogy az összes megadott adatok helyesek.
   5. **Vásároljon**: kattintson a **megvásárlása** toostart hello kiépítés. Találhatóak hello tranzakció toohello feltételeit. hello virtuális gép nem rendelkezik a további díjakat hello számítási hello kiválasztott hello server méret túl **mérete** lépés. 

> [!NOTE]
> hello kiépítése körülbelül 10-20 percet kell végrehajtani. hello hello kiépítési állapotának a hello Azure-portálon jelenik meg.
> 
> 

## <a name="how-tooaccess-hello-microsoft-data-science-virtual-machine"></a>Hogyan tooaccess hello Microsoft adatok tudományos virtuális gép
Egyszer hello a virtuális gép létrehozása, akkor a távoli asztal hello rendszergazdai fiók hitelesítő adatait az előző hello konfigurált használatával történő **alapjai** szakasz. 

Miután a virtuális gép létrehozása és üzembe helyezve, áll készen toostart hello eszközökkel, amelyek telepítése és konfigurálása történik meg. Start menü csempék és asztali ikonok számos hello eszközök vannak. 

## <a name="how-toocreate-a-strong-password-for-jupyter-and-start-hello-notebook-server"></a>Hogyan toocreate Jupyter, és indítson el egy erős jelszót hello notebook kiszolgáló
Alapértelmezés szerint hello Jupyter notebook server előre konfigurálva van, de a virtuális gép hello tiltott, amíg a Jupyter jelszót állíthat be. toocreate hello Jupyter notebook server hello gépen telepített egy erős jelszót, futtassa a következő parancsot a parancssorban az adatok tudományos virtuális gépet, vagy kattintson duplán a hello asztali parancsikonjára adtunk hello hello nevű  **Jupyter jelszavát & Start** a virtuális gép helyi rendszergazdai fiók.

    C:\dsvm\tools\setup\JupyterSetPasswordAndStart.cmd

Kövesse a köszönőüzenetei, és válasszon egy erős jelszót, amikor a rendszer kéri.

hello előző parancsfájlja létrehozza a Jelszókivonat és a tároló azt hello Jupyter konfigurációs fájlban található: **C:\ProgramData\jupyter\jupyter_notebook_config.py** hello paraméter neve alatt ***c. NotebookApp.password***.

hello parancsfájlt is lehetővé teszi, hogy és hello Jupyter server hello háttérben futnak. Jupyter kiszolgáló akkor jön létre, mivel a windows hello WIndows Feladatütemező feladat neve **Start_IPython_Notebook**.  Előfordulhat, hogy toowait hello jelszó beállítása előtt a böngésző hello notebook megnyitása után néhány másodpercig. Lásd: hello című az alábbi **Jupyter Notebook** a hogyan tooaccess hello Jupyter notebook kiszolgáló. 


## <a name="tools-installed-on-hello-microsoft-data-science-virtual-machine"></a>A Microsoft Data tudományos virtuális gép hello telepített eszközök

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Ha toouse R az elemzésekhez, hello virtuális gép van telepítve Microsoft R Server Developer kiadásában. Microsoft R Server a széles körű telepíthető vállalati szintű analytics platform támogatott R alapján, méretezhető és biztonságos. Big Data típusú adatok statisztikák, a prediktív modellezési és a gépi tanulási képességek számos támogató, R Server hello teljes körű elemzés – feltárása, elemzés, adatmegjelenítési és modellezési támogatja. Használatával, és nyílt forráskódú R kiterjesztése, Microsoft R-kiszolgáló nem teljes mértékben kompatibilis R parancsfájlokat, funkciók és CRAN csomagok, vállalati léptékű tooanalyze adatokat. Párhuzamos és darabolt adatok feldolgozása hozzáadásával nyitott forrás R hello memórián belüli korlátozásai is javítja. Ez lehetővé teszi toorun analytics nagyobb, mint mi elfér a memóriahasználatot adatokon.  A Visual Studio Community Edition szerepel-e virtuális gép tartalmazza, amely egy teljes IDE R. használata biztosítja a Visual Studio bővítmény hello R eszközöket hello Is töltse le és használja is, mint más IDEs [Rstudióból](http://www.rstudio.com). 

### <a name="python"></a>Python
A fejlesztési pythonos környezetekben Anaconda Python elosztási 2.7 és 3.5-ös telepítve van. Ehhez a terjesztéshez tartalmaz hello kiinduló Python körülbelül 300 hello legnépszerűbb matematikai, tervezés és adatok analytics csomagok együtt. Python Tools használhatja a Visual Studio (PTVS) belül hello Visual Studio 2015 Community edition vagy IDEs mellékelhető ÜRESJÁRATBAN vagy Spyder Anaconda hello egyik telepített. Indítja el az egyik meg hello keresősáv rákeresve (**Win** + **S** kulcs).

> [!NOTE]
> toopoint hello a Python Tools for Visual Studio Anaconda Python 2.7, és 3.5-ös verzióját, egyes verzióihoz kell toocreate egyéni környezetekben. tooset környezet elérési utak a Visual Studio 2015 Community Edition hello lépjen túl**eszközök** -> **Python Tools** -> **Python-környezetek** majd **+ egyéni**. 
> 
> 

Anaconda Python 2.7 telepítőmappájában található C:\Anaconda és Anaconda Python 3.5 c:\Anaconda\envs\py35 telepítőmappájában található. Lásd: [PVTS dokumentációban](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) a részletes lépéseket. 

### <a name="jupyter-notebook"></a>Jupyter notebook
Anaconda terjesztési is Jupyter notebook, egy környezeti tooshare kódot és az elemzések tartalmaz. A Jupyter notebook kiszolgáló előre beállított Python 2.7-es, a Python 3.4-es, a Python 3.5-ös és a R kernelek. Egy asztali látható ikon nevű, "Jupyter Notebook toolaunch hello böngésző tooaccess hello Notebook kiszolgáló. Ha a virtuális gép hello távoli asztalon keresztül, meglátogathatja [https://localhost:9999 /](https://localhost:9999/) tooaccess hello Jupyter notebook kiszolgáló toohello Virtuálisgép jelentkezni.

> [!NOTE]
> Folytassa, ha kapott tanúsítványt figyelmeztetéseket. 
> 
> 

Azt a Python több minta jegyzetfüzetet csomagolását és az r hello Jupyter notebookok megjelenítése hogyan toowork Microsoft R Server, SQL Server 2016 R Services (adatbázis-analytics), Python, Microsoft kognitív ToolKit (CNTK) részletes biztonságával és más Azure Miután jelentkezik be tooJupyter technológiák. Hello notebook kezdőlapján hello hivatkozás toohello minták után Önnek a hitelesítéshez toohello Jupyter notebook hello korábbi lépésben létrehozott jelszó használatával tekintheti meg. 

### <a name="visual-studio-2015-community-edition"></a>A Visual Studio 2015 Community edition
A Visual Studio Community edition hello VM telepítve. Hello szabad változatának kis csoportjai és tesztelési célokra használható Microsoft népszerű IDE. Licencelési időszakonként hello megtekintheti [Itt](https://www.visualstudio.com/support/legal/mt171547).  Nyissa meg a Visual Studio hello asztali ikonok vagy hello duplán kattintva **Start** menü. Programokat is kereshet **Win** + **S** , majd gépelje be a "Visual Studio". Ha van például a C#, Python, R, node.js nyelvű projektek is létrehozhat. Beépülő modulok települnek, melyek az Azure-szolgáltatásokat, mint az Azure Data Catalog, az Azure HDInsight (Hadoop, Spark) és az Azure Data Lake kényelmes toowork. 

> [!NOTE]
> Üzenet jelenik meg, hogy a próbaidőszak lejárt kaphat. Microsoft-fiók hitelesítő adatait, vagy hozzon létre egy új ingyenes fiók tooget hozzáférés toohello Visual Studio Community Edition. 
> 
> 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
A fejlesztői verzió, az SQL Server 2016 R szolgáltatások toorun adatbázis-elemzés a virtuális gép hello valósul meg. R biztosítanak a platform fejlesztéséhez és intelligens alkalmazások telepítéséhez. Hello hatékony és erőteljes R nyelv használatával és hello hello közösségi toocreate modelleket csomagok számát és előrejelzéseket létrehozni az SQL Server-adatok. Elemzés Bezárás toohello adatok biztosítható, mert R szolgáltatások (az adatbázis-) hello R nyelv integrálható az SQL Server. Ezzel a megoldással hello költségek és az adatátvitelt jelölik a kapcsolódó biztonsági kockázatokról.

> [!NOTE]
> SQL Server 2016 hello developer kiadásában csak akkor használható fejlesztéshez, és vizsgálati célra. A licenc toorun van szüksége az éles környezetben. 
> 
> 

Hello SQL server férhetnek megnyitása **SQL Server Management Studio**. A virtuális gép nevét, hello kiszolgálónév fel van töltve. Ha jelentkezett be, mint a rendszergazda a Windows hello Windows-hitelesítés használatára. Miután az SQL Server Management Studio más felhasználók létrehozása, adatbázisok létrehozására, importálhat adatokat, és az SQL-lekérdezések futtatása. 

adatbázis-tooenable analytics Microsoft R, futtassa a következő parancsot egy egyik hello használatának hello rendszergazdaként a bejelentkezés után az SQL Server management studio művelet idő. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace hello %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Virtuális gép hello több Azure eszközök telepítve vannak:

* Van egy asztali parancsikonjára tooaccess hello Azure SDK-dokumentáció. 
* **AzCopy**: toomove adatok átviteléhez a Microsoft Azure Storage-fiókot használni. toosee használati, típus **Azcopy** , a parancssor toosee hello használat. 
* **A Microsoft Azure Tártallózó**: használt toobrowse belül az Azure Storage-fiókot és átviteli adatok tooand az Azure storage tárolt hello objektumok között. Beírhatja **Tártallózó** keresési vagy keresés azt a hello Windows Start menü tooaccess ezt az eszközt. 
* **Adlcopy**: toomove adatok tooAzure Data Lake használt. toosee használati, típus **adlcopy** parancssorban. 
* **dtui**: toomove adatok tooand a Azure Cosmos-Adatbázisból, egy NoSQL-adatbázis hello felhő használt. Típus **dtui** a parancssorból. 
* **A Microsoft adatkezelési átjáró**: lehetővé teszi, hogy a helyszíni adatforrások és a felhő közötti adatátvitelt jelölik. Például az Azure Data Factory belül használják. 
* **A Microsoft Azure Powershell**: egy eszközt az Azure-erőforrások használt tooadminister hello Powershell programozási nyelv is telepítve van a virtuális Gépet. 

### <a name="power-bi"></a>Power BI
toohelp készít az irányítópultok és a kiváló képi megjelenítéseket, hello **Power BI Desktop** telepítve van. Az eszköz toopull adatokat különböző forrásokból, tooauthor használja az irányítópultok és jelentések és toopublish őket toohello felhő. További információ: hello [Power BI](http://powerbi.microsoft.com) hely. A Power BI desktop hello Start menüben található. 

> [!NOTE]
> Az Office 365-fiók tooaccess Power bi-ban van szüksége. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>További Microsoft fejlesztőeszközök
Hello [ **Microsoft Webplatform-telepítő** ](https://www.microsoft.com/web/downloads/platform.aspx) is használt toodiscover és más Microsoft fejlesztőeszközök letöltése. Hello Microsoft adatok tudományos virtuális gép asztalán helyi toohello eszközt is van.  

## <a name="important-directories-on-hello-vm"></a>Virtuális gép hello fontos könyvtárak létrehozása sikerült
| Elem | Címtár |
| --- | --- |
| Jupyter notebook kiszolgáló konfigurációk |C:\ProgramData\jupyter |
| Jupyter Notebook minták kezdőkönyvtár |c:\dsvm\notebooks |
| Más minták |c:\dsvm\samples |
| Anaconda (alapértelmezett: Python 2.7) |c:\Anaconda |
| Anaconda Python 3.5 környezet |c:\Anaconda\envs\py35 |
| R Server önálló példány könyvtár (az alapértelmezett R-példány R3.2.2 alapján) |C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R Server adatbázis-példány könyvtára (R3.2.2) |C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Microsoft R megnyitása (R3.3.1) példány könyvtár |C:\Program Files\Microsoft\MRO-3.3.1 |
| Egyéb eszközök |c:\dsvm\tools |

> [!NOTE]
> Microsoft adatok tudományos virtuális gép létrehozása előtt (előtt 3 2016 szeptemberétől kezdve) 1.5.0 hello példányai némileg eltérő könyvtárstruktúra használja, mint tábla megelőző hello megadott. 
> 
> 

## <a name="next-steps"></a>Következő lépések
Íme néhány következő lépések toocontinue a tanulási és kutatási funkciójával. 

* Megismerkedhet a különböző adatok tudományos eszközök hello adattudomány VM hello kattintson a start menü és az hello menü felsorolt hello eszközök hello.
* Keresse meg a túl**C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** minták hello RevoScaleR szalagtár R, amely támogatja a vállalati léptékű adatelemzés használatával.  
* A cikk elolvasása hello: [10 lehetősége van a virtuális gép adattudomány hello](http://aka.ms/dsvmtenthings)
* Ismerje meg, hogyan toobuild end tooend elemzési megoldásokat rendszeresen használatával hello [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* A Microsoft hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com) a machine learning és az elemzés, hogy használjon hello Cortana Intelligence Suite – minták. Egy ikont is adtunk meg hello **Start** menü és hello virtuális gép toothis tár hello asztalon.

