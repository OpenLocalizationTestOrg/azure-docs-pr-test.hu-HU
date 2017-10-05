---
title: "A Microsoft Data tudományos virtuális gép kiépítéséhez |} Microsoft Docs"
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
ms.openlocfilehash: 76cd54cd234dfe43e8f0d61f0b66f0ed0c09e8b7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="provision-the-microsoft-data-science-virtual-machine"></a>A Microsoft Data Science virtuális gép üzembe helyezése
A Microsoft adatokat tudományos virtuális gép, a Windows Azure virtuális gép (VM) előtelepített és konfigurált számos népszerű eszköz adatelemzés és a gépi tanulás általánosan használt lemezkép. A rendszer részét képező eszközök:

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
  * [Rattle](http://rattle.togaware.com/) (az R analitikai eszköz a további könnyen): olyan eszköz, amely lehetővé teszi az első lépések adatelemzés és a gép R GUI-alapú adatok feltárása, ezzel megkönnyítik a tanulási, és az R-kód automatikus generálása modellezési.
  * [mxnet](https://github.com/dmlc/mxnet): a teljesítmény és a rugalmasságot biztosít a részletes tanulási-keretrendszert
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : A látványelemek adatainak adatbányászati és gépi tanulási szoftver Java nyelven.
  * [Apache részletezési](https://drill.apache.org/): sémamentes SQL lekérdezési motorja a Hadoop, a nosql-alapú és a felhőalapú tárolást.  ODBC és JDBC felületek engedélyezése lekérdező nosql-alapú és a szabványos Üzletiintelligencia-eszközök, például a Power BI, az Excel, a Tableau fájlokat támogatja.
* Az R és Python a szalagtárak használja az Azure Machine Learning és más Azure-szolgáltatásokkal
* Többek között beleértve a github webhelyen, a Visual Studio Team Services forráskódú adattárakban dolgozni a Git Bash Git
* Számos népszerű Linux parancssori segédprogram (beleértve a awk, csökkentésének, perl, grep, keresés, wget, curl stb) Windows-port parancssor keresztül érhető el. 

Adattudomány Ez magában foglalja a feladatok sorozata léptetés:

1. Keresése, betöltése, és előzetesen feldolgozni az adatokat
2. Összeállításakor és tesztelésekor modellek
3. A modelleket a fogyasztás intelligens alkalmazások telepítése

Adatszakértőkön számos különféle eszközre segítségével ezeket a feladatokat. Túl sok időt vesz igénybe találja a szoftver, a megfelelő verzióját, majd töltse le és telepítse őket a lehet. A Microsoft Data tudományos virtuális gép a terheket megkönnyítheti azzal, hogy biztosít egy használatra kész lemezképnek, amely telepíthető az Azure összes számos népszerű eszköz előtelepített és konfigurált. 

A Microsoft Data tudományos virtuális gép jump-starts analytics projektjéhez. Ez lehetővé teszi, hogy működik a feladatok R, Python, SQL és C# például különböző nyelveken. A Visual Studio biztosít egy IDE fejlesztéséhez és teszteléséhez a kódot, amely könnyen használható. Az Azure SDK tartalmazza a virtuális Gépet a különböző szolgáltatások segítségével a Microsoft felhőalapú platformja alkalmazások létrehozását teszi lehetővé. 

Nincsenek az adatok tudományos VM-lemezkép szoftver költségek. Csak kell fizetnie Azure használati díjak mely a virtuális gép kiépítése méretétől függ. További részleteket a számítási díjakat található az árazás részletes adatait tartalmazó részben található meg a [adatok tudományos virtuális gép](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) lap. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Az adatok tudományos virtuális gép egyéb verziói
A [CentOS](machine-learning-data-science-linux-dsvm-intro.md) lemezkép érhető el, legtöbb ugyanazokat az eszközöket, mint a Windows-lemezkép. Egy [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md) kép is, amelyek számos hasonló eszközök és keretrendszerek tanulási mély érhető el.

## <a name="prerequisites"></a>Előfeltételek
A Microsoft Data tudományos virtuális gép létrehozásához, az alábbiakkal kell rendelkeznie:

* **Azure-előfizetés**: egy beszerzéséről [beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure-tárfiók**: szeretne létrehozni egyet, lásd: [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account). Alternatív megoldásként a tárfiók a virtuális gép létrehozása, ha nem szeretné, hogy a meglévő fiók részeként is létrehozható.

## <a name="create-your-microsoft-data-science-virtual-machine"></a>A Microsoft Data tudományos virtuális gép létrehozása
Az alábbiakban egy példányát, a Microsoft Data tudományos virtuális gép létrehozásához szükséges lépéseket:

1. Keresse meg a virtuális gépet, a listaelem [Azure-portálon](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2. Válassza ki a **létrehozása** panel alján, a varázsló veendő.![ Konfigurálja-adatok-tudományos-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3. A varázsló a Microsoft Data tudományos virtuális gép létrehozásához használt szükséges **bemenetek** az egyes a **öt lépése** a jobb oldali ábra számba. Az alábbiakban az egyes lépéseket konfigurálásához szükséges adatokat:
   
   1. **Alapvető beállítások**
      
      1. **Név**: az adatok tudományos kiszolgáló létrehozásakor nevét.
      2. **Felhasználónév**: rendszergazdai fiók bejelentkezési azonosító.
      3. **Jelszó**: rendszergazdai fiók jelszavát.
      4. **Előfizetés**: Ha több előfizetéssel rendelkezik, válassza ki a amelyiken a gép létrehozását és számlázva van.
      5. **Erőforráscsoport**: hozhat létre egy új vagy meglévő csoport használata.
      6. **Hely**: válassza ki a legjobban megfelelő adatközpont. Általában az adatközpont, amely az adatok, vagy a helynév leggyorsabb hálózati hozzáférési legközelebb.
   2. **Méret**: válassza ki a kiszolgáló típusát, amely megfelel a funkcionális és költség megkötések. További lehetőségek a VM-méretek kaphat kiválasztása a "Nézet All".
   3. **Beállítások**:
      
      1. **Lemez típusa**: válassza a prémium szintű Ha inkább egy SSD-meghajtót (SSD), ellenkező esetben válassza a "Standard".
      2. **A Tárfiók**: hozzon létre egy új Azure-tárfiók az előfizetésben, vagy használjon egy meglévőt ugyanazon *hely* , hogy a választott a **alapjai** a varázsló.
      3. **Más paraméterek**: általában csak használja az alapértelmezett értékeket. Az egyes mezőkkel tájékoztató hivatkozásra rámutat is, ha meg kívánja használni a nem az alapértelmezett értékeket.
   4. **Összefoglalás**: Győződjön meg arról, hogy az összes megadott adatok helyesek.
   5. **Vásároljon**: kattintson a **megvásárlása** kiépítési elindításához. Hivatkozás a tranzakció feltételeit valósul meg. A virtuális gép nem rendelkezik a kiválasztott kiszolgáló méretéhez számítási túl további díjakat a **mérete** lépés. 

> [!NOTE]
> A kiépítése körülbelül 10-20 percet kell végrehajtani. A kiépítési állapotát az Azure portálon jelenik meg.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Hogyan érhetők el a Microsoft Data tudományos virtuális gép
A virtuális gép létrehozása után azokat a rendszergazdai fiók hitelesítő adatait az előző konfigurált használatával is a távoli asztal **alapjai** szakasz. 

Miután a virtuális gép létrehozása és üzembe helyezve, készen áll indíthatja azon eszközöket, amelyek telepítése és konfigurálása történik meg. Start menü csempék és számos eszközt az asztali ikonok vannak. 

## <a name="how-to-create-a-strong-password-for-jupyter-and-start-the-notebook-server"></a>Hozzon létre egy erős jelszót a Jupyter, és indítsa el a notebook kiszolgáló
Alapértelmezés szerint a Jupyter notebook kiszolgáló előre konfigurálva van, de a virtuális Gépen le van tiltva, amíg a Jupyter jelszót állíthat be. Hozzon létre egy erős jelszót a Jupyter notebook kiszolgáló telepítve a számítógépen, hogy futtassa a következő parancsot a adatok tudományos virtuális gépen vagy a parancssorból, kattintson duplán az asztali parancsikonra adtunk nevű **Jupyter beállítása Jelszó & Start** a virtuális gép helyi rendszergazdai fiók.

    C:\dsvm\tools\setup\JupyterSetPasswordAndStart.cmd

Kövesse a megjelenő utasításokat, és válasszon egy erős jelszót, amikor a rendszer kéri.

Az előző parancsfájlja létrehozza a Jelszókivonat és azt a Jupyter konfigurációs fájlban található tároló: **C:\ProgramData\jupyter\jupyter_notebook_config.py** paraméter neve alatt ***c.NotebookApp.password***.

A parancsfájl is lehetővé teszi, és a Jupyter kiszolgálón fusson a háttérben. Jupyter kiszolgáló akkor jön létre, mivel a WIndows Feladatütemező windows feladat neve **Start_IPython_Notebook**.  Előfordulhat, hogy a böngésző a notebook megnyitása előtt a jelszó beállítása után néhány másodpercet vár. Kifejezéseit leíró szakasza az alábbi **Jupyter Notebook** a Jupyter notebook kiszolgáló elérésével. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Eszközök vannak telepítve a Microsoft Data tudományos virtuális gép

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Ha szeretné használni az R az elemzésekhez, a virtuális Gépnek legyen telepítve Microsoft R Server Developer kiadásában. Microsoft R Server a széles körű telepíthető vállalati szintű analytics platform támogatott R alapján, méretezhető és biztonságos. Big Data típusú adatok statisztikák, a prediktív modellezési és a gépi tanulási képességek számos támogató, R Server támogatja analytics – feltárása, elemzés, adatmegjelenítési és modellezési teljes skáláját. Használatával, és nyílt forráskódú R kiterjesztése, Microsoft R-kiszolgáló nem teljes mértékben kompatibilis R parancsfájlokat, funkciók és CRAN csomagok vállalati léptékű adatok elemzésére. Adja hozzá az adatok párhuzamos és darabolt feldolgozása nyitott forrás R memórián belüli vonatkozó korlátozások is javítja. Ez lehetővé teszi, hogy futhat az analytics adatok nagyobb, mint mi elfér a fizikai memóriát.  Visual Studio Community Edition szerepel-e a virtuális gép tartalmazza, amely egy teljes IDE R. használata biztosítja a Visual Studio bővítmény az R eszközöket Is töltse le és használja is, mint más IDEs [Rstudióból](http://www.rstudio.com). 

### <a name="python"></a>Python
A fejlesztési pythonos környezetekben Anaconda Python elosztási 2.7 és 3.5-ös telepítve van. Ehhez a terjesztéshez együtt a legnépszerűbb matematikai, tervezés és adatok analytics csomagok körülbelül 300 alap Python tartalmazza. Python Tools használhatja a Visual Studio (PTVS) belül a Visual Studio 2015 Community edition vagy kötegelt IDEs Anaconda ÜRESJÁRATBAN vagy Spyder például az egyik telepített. Indítja el az egyik meg a keresési sávon rákeresve (**Win** + **S** kulcs).

> [!NOTE]
> A Python Tools for Visual Studio Anaconda Python 2.7-es és 3.5-ös mutasson létrehozásához szükséges egyéni környezetek egyes verzióihoz. A Visual Studio 2015 Community Edition környezet elérési utak beállításához navigáljon **eszközök** -> **Python Tools** -> **Python-környezetek**majd **+ egyéni**. 
> 
> 

Anaconda Python 2.7 telepítőmappájában található C:\Anaconda és Anaconda Python 3.5 c:\Anaconda\envs\py35 telepítőmappájában található. Lásd: [PVTS dokumentációban](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) a részletes lépéseket. 

### <a name="jupyter-notebook"></a>Jupyter notebook
Jupyter notebook, egy környezet kóddal és elemzési anaconda terjesztési is tartalmaz. A Jupyter notebook kiszolgáló előre beállított Python 2.7-es, a Python 3.4-es, a Python 3.5-ös és a R kernelek. Egy asztali látható ikon nevű "Jupyter notebook használatához indítsa el a böngészőt a Notebook kiszolgálóhoz való hozzáféréshez. Ha a távoli asztalon keresztül a virtuális gép, meglátogathatja [https://localhost:9999 /](https://localhost:9999/) Ha jelentkezett be a virtuális gép a Jupyter notebook-kiszolgálóhoz való hozzáféréshez.

> [!NOTE]
> Folytassa, ha kapott tanúsítványt figyelmeztetéseket. 
> 
> 

A Microsoft több minta jegyzetfüzetet r és Python csomagolását A Jupyter notebookok megjelenítése után jelentkezik be a Jupyter Microsoft R Server, SQL Server 2016 R Services (adatbázis-analytics), Python, Microsoft kognitív ToolKit (CNTK) részletes biztonságával és más Azure technológiák használata. A notebook kezdőlapján a hivatkozásra kattintva a minták után a hitelesítést a Jupyter notebook a korábbi lépésben létrehozott jelszó használatával tekintheti meg. 

### <a name="visual-studio-2015-community-edition"></a>A Visual Studio 2015 Community edition
A Visual Studio Community edition telepítve a virtuális Gépen. A népszerű IDE kis csoportjai és tesztelési célokra használható Microsoft ingyenes verzióját is. A licencfeltételek megtekintheti [Itt](https://www.visualstudio.com/support/legal/mt171547).  Az asztali ikonra duplán kattintva nyissa meg a Visual Studio vagy a **Start** menü. Programokat is kereshet **Win** + **S** , majd gépelje be a "Visual Studio". Ha van például a C#, Python, R, node.js nyelvű projektek is létrehozhat. Beépülő modulok települnek, amely együttműködik az Azure-szolgáltatásokat, mint az Azure Data Catalog, az Azure HDInsight (Hadoop, Spark) és az Azure Data Lake kényelmes. 

> [!NOTE]
> Üzenet jelenik meg, hogy a próbaidőszak lejárt kaphat. Microsoft-fiók hitelesítő adatait, vagy hozzon létre egy új ingyenes fiókot, a Visual Studio Community Edition elérésének. 
> 
> 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
A fejlesztői verzióját az SQL Server 2016 R Services adatbázis-analytics futtatásához a virtuális Gépre valósul meg. R biztosítanak a platform fejlesztéséhez és intelligens alkalmazások telepítéséhez. A hatékony és erőteljes R nyelv és a Közösség számos csomag segítségével modellek létrehozása és előrejelzéseket létrehozni az SQL Server-adatok. Beállíthatja, hogy az adatok közel analytics mert R szolgáltatások (az adatbázis-) az R nyelv integrálható az SQL Server. Ezzel a megoldással a költségek és az adatátvitelt jelölik a kapcsolódó biztonsági kockázatokról.

> [!NOTE]
> Az SQL Server 2016 developer kiadásában csak fejlesztési és tesztelési célokra használható. Licenccel kell rendelkeznie a termelési futtatásához. 
> 
> 

Az SQL server férhetnek megnyitása **SQL Server Management Studio**. A virtuális gép neve van feltöltve, a kiszolgáló neve. Ha a Windows rendszergazdaként jelentkezett be Windows-hitelesítés használatára. Miután az SQL Server Management Studio más felhasználók létrehozása, adatbázisok létrehozására, importálhat adatokat, és az SQL-lekérdezések futtatása. 

Adatbázis-analytics Microsoft R használatának engedélyezéséhez futtassa a következő parancsot ütemezésként idő a kiszolgáló-rendszergazdai bejelentkezés után az SQL Server management studio művelet. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Több Azure-eszközök telepítve vannak a virtuális Gépet:

* Nincs az Azure SDK-dokumentáció eléréséhez asztali parancsikonjára. 
* **AzCopy**: áthelyezni az adatokat mindkét a Microsoft Azure Storage-fiókot használni. Használat megtekintéséhez írja be **Azcopy** parancsot a parancssorba a használat megtekintéséhez. 
* **A Microsoft Azure Tártallózó**: az Azure Storage-fiókot és átviteli adatokat az Azure storage érkező vagy oda irányuló tárolt objektumok között használt. Beírhatja **Tártallózó** a Keresés vagy nem található, a Windows Start menüben, az eszköz eléréséhez. 
* **Adlcopy**: adatok áthelyezése az Azure Data Lake használt. Használat megtekintéséhez írja be **adlcopy** parancssorban. 
* **dtui**: áthelyezni az adatokat, és az Azure Cosmos DB, egy NoSQL-adatbázis, a felhő használt. Típus **dtui** a parancssorból. 
* **A Microsoft adatkezelési átjáró**: lehetővé teszi, hogy a helyszíni adatforrások és a felhő közötti adatátvitelt jelölik. Például az Azure Data Factory belül használják. 
* **A Microsoft Azure Powershell**: a Powershell programozási nyelv is telepítve van a virtuális Gépet az Azure-erőforrások felügyeletéhez használt eszköz. 

### <a name="power-bi"></a>Power BI
Irányítópultok és a kiváló megjelenítések létrehozásához a **Power BI Desktop** telepítve van. Az eszköz segítségével szerez adatokat különböző forrásokból származó az irányítópultokat és jelentéseket készít, és közzéteheti a felhőben. További információ: a [Power BI](http://powerbi.microsoft.com) hely. A Start menüben található Power BI desktopban. 

> [!NOTE]
> Power BI eléréséhez az Office 365-fiók szükséges. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>További Microsoft fejlesztőeszközök
A [ **Microsoft Webplatform-telepítő** ](https://www.microsoft.com/web/downloads/platform.aspx) felderítése és más Microsoft fejlesztői eszközök is használható. Az eszköz a Microsoft Data tudományos virtuális gép asztalán mutató hivatkozás is van.  

## <a name="important-directories-on-the-vm"></a>A virtuális gép fontos könyvtárak létrehozása sikerült
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
> Példányok, a Microsoft Data tudományos létrehozott virtuális gépek előtt 1.5.0 (előtt 2016 szeptemberétől 3.) egy némileg eltérő könyvtárstruktúrát használja, mint az előző táblázatban megadott. 
> 
> 

## <a name="next-steps"></a>Következő lépések
Az alábbiakban a lépéseket, ahol folytathatja a tanulási és kutatási funkciójával. 

* Megismerkedhet a különböző adatok tudományos eszközök az adatok tudományos virtuális gép által a start menü és az a menü felsorolt eszközök.
* Navigáljon a **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** mintákat az R, amely támogatja a vállalati léptékű adatelemzés RevoScaleR szalagtár használatával.  
* A következő cikkben: [10 lehetősége van az adatok tudományos virtuális gép](http://aka.ms/dsvmtenthings)
* Ismerje meg, hogyan hozhat létre a teljes körű elemzési megoldásokat rendszeresen használatával a [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Látogasson el a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com) machine learning és az adatok analytics mintákat a Cortana Intelligence Suite használó. Egy ikont is adtunk meg a **Start** menü és a virtuális gépet a tár az asztalon.

