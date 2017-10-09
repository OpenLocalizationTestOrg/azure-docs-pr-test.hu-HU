---
title: "aaaProvision hello Linux adatok tudományos virtuális gép |} Microsoft Docs"
description: "Konfigurálja és Linux adatok tudományos virtuális gép létrehozása az Azure toodo elemzés és a gépi tanulás."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: 81dfa90f6cd4b4f33535a20fb97442bf9152d829
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-hello-linux-data-science-virtual-machine"></a>Hello Linux adatok tudományos virtuális gép kiépítése
hello Linux adatok tudományos virtuális gép CentOS-alapú Azure virtuális gép a előre telepített eszközök gyűjteménye. Ezek az eszközök a adatelemzés gyakran használják, és a gépi tanulás. hello kulcs szoftverösszetevőket tartalmazza a következők:

* Operációs rendszer: Linux CentOS terjesztési.
* Microsoft R Server Developer Edition
* Anaconda Python elosztási (2.7 és 3.5-ös verzióját. verzió), beleértve a népszerű adatok elemzőkönyvtárak
* JuliaPro - Ágnes nyelvi népszerű tudományos és az adatok analytics könyvtárakkal válogatott megoszlása
* Önálló Spark-példányban és egyetlen csomópont Hadoop (HDFS, Yarn)
* JupyterHub - R, Python, PySpark, Ágnes kernelek támogató többfelhasználós Jupyter notebook kiszolgáló
* Azure Storage Explorer
* Azure parancssori felület (CLI) Azure-erőforrások kezelése
* PostgresSQL adatbázis
* Machine learning-eszközök
  * [Számítási hálózati Toolkit (CNTK)](https://github.com/Microsoft/CNTK): A Microsoft Research software eszközkészletet tanulási mély.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): gyors machine learning-rendszer támogatása, például a online, a kivonatoló, allreduce, csökkentése, learning2search, aktív, és interaktív tanulási.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): gyors és pontos súlyozott fa megvalósítási biztosító eszközt.
  * [Rattle](http://rattle.togaware.com/) (R analitikai eszköz tooLearn hello könnyen): olyan eszköz, amely lehetővé teszi az első lépések adatelemzés és a gép R GUI-alapú adatok feltárása, ezzel megkönnyítik a tanulási, és az R-kód automatikus generálása modellezési.
* Az Azure SDK, Java, Python, node.js, a Ruby, a PHP
* Az R és Python a szalagtárak használja az Azure Machine Learning és más Azure-szolgáltatásokkal
* Fejlesztői eszközök és szerkesztők (Rstudióból, PyCharm, IntelliJ, Emacs, gedit, VI.)


Adattudomány Ez magában foglalja a feladatok sorozata léptetés:

1. Keresése, betöltése, és előzetesen feldolgozni az adatokat
2. Összeállításakor és tesztelésekor modellek
3. Az intelligens alkalmazásokban felhasználásra hello modellek telepítéséről

Adatszakértőkön használható különböző eszközök toocomplete ezeket a feladatokat. Ez lehet a túl sok időt vesz igénybe toofind hello megfelelő hello szoftver verzióját és toodownload, majd lefordítani, és ezen verziói telepítése.

hello Linux adatok tudományos virtuális gép jelentősen ezt a terhet megkönnyítése érdekében. Toojump indítási használják a analytics projekthez. Ez lehetővé teszi a különböző nyelveken, beleértve az R, Python, SQL, Java és C++ feladatok toowork. Eclipse egy IDE-toodevelop biztosít, és tesztelheti a kódját, amely könnyen toouse. hello szerepel a virtuális gép hello Azure SDK toobuild lehetővé teszi az alkalmazások különböző szolgáltatások használatával Linux hello Microsoft cloud platform. Ezenkívül hozzáférést tooother például Ruby, Perl, PHP vagy node.js is előre telepített nyelvek rendelkezik.

Nincsenek az adatok tudományos VM-lemezkép szoftver költségek. Csak hello Azure hardver használati díjak értékelni hello virtuális gép, Virtuálisgép-lemezkép hello megadó hello mérete alapján kell fizetnie. További részleteket a hello számítási díjakat hello található [VM listaelem oldalon, hello Azure piactér ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-hello-data-science-virtual-machine"></a>Hello adatok tudományos virtuális gép egyéb verziói
Egy [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md) kép is elérhető, legtöbb hello azonos eszközök, a CentOS kép és a részletes tanulási keretrendszerek hello. A [Windows](machine-learning-data-science-provision-vm.md) kép érhető el is.

## <a name="prerequisites"></a>Előfeltételek
Adatok tudományos Linux virtuális gépek létrehozása előtt hello következő kell rendelkeznie:

* **Azure-előfizetés**: tooobtain egy, lásd: [beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/free/).
* **Azure-tárfiók**: toocreate egy, lásd: [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account). Alternatív megoldásként hello tárfiók létrehozásának hello virtuális gép, ha nem szeretné, hogy egy meglévő fiókkal toouse hello folyamat részeként is létrehozható.

## <a name="create-your-linux-data-science-virtual-machine"></a>A Linux-adatok tudományos virtuális gép létrehozása
Az alábbiakban hello lépéseket toocreate hello Linux adatok tudományos virtuális gép egy példányát:

1. Keresse meg a hello listázása toohello virtuális gép [Azure-portálon](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2. Kattintson a **létrehozása** (hello alul) hello varázsló toobring.![ Konfigurálja-adatok-tudományos-vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3. a következő szakaszok hello segítségével a hello bemeneti adatokat az egyes hello lépéseket hello varázslóban (a hello sarkában. ábra fenti hello számba) használt toocreate hello Microsoft adatok tudományos virtuális gépet. Az alábbiakban hello szükséges bemeneti adatok tooconfigure minden az alábbi lépéseket:
   
   a. **Alapvető**:
   
   * **Név**: az adatok tudományos kiszolgáló létrehozásakor nevét.
   * **Felhasználónév**: első fiók bejelentkezhet azonosítóját.
   * **Jelszó**: első fiók jelszavát (használható nyilvános SSH-kulcs jelszó helyett).
   * **Előfizetés**: Ha több előfizetéssel rendelkezik, válasszon hello egyik mely hello a gép toobe létrehozott és számlázható. Ehhez az előfizetéshez erőforrás-létrehozási jogosultsággal kell rendelkeznie.
   * **Erőforráscsoport**: hozhat létre egy új vagy meglévő csoport használata.
   * **Hely**: Select hello adatközpont, amely a legjobban megfelelő. Általában akkor hello adatközpont, amely rendelkezik az adatok, vagy legközelebbi tooyour helynév leggyorsabb hálózati hozzáféréshez.
   
   b. **Méret**:
   
   * Válassza ki, amely megfelel a funkcionális és költség megkötések hello kiszolgáló típusát. Válassza ki **nézet összes** toosee a VM-méretek több lehetőséget.
   
   c. **Beállítások**:
   
   * **Lemez típusa**: válasszon **prémium** Ha inkább a tartós állapotú meghajtót (SSD). Ellenkező esetben válassza **szabványos**.
   * **A Tárfiók**: hozzon létre egy új Azure-tárfiók az előfizetésben, vagy használjon egy meglévőt a hello ugyanazon a helyen a hello a kiválasztott **alapjai** hello varázsló.
   * **Más paraméterek**: A legtöbb esetben csak hello alapértelmezett értéket használ. tooconsider nem az alapértelmezett értékeket, hello tájékoztató hivatkozásra az egérmutatót adott mezők hello súgót.
   
   d. **Összefoglalás**:
   
   * Győződjön meg arról, hogy az összes megadott adatok helyesek.
   
   e. **Vásároljon**:
   
   * toostart hello kiépítés, kattintson a **megvásárlása**. Találhatóak hello tranzakció toohello feltételeit. hello virtuális gép nem rendelkezik a további díjakat hello számítási hello kiválasztott hello server méret túl **mérete** lépés.

hello kiépítése körülbelül 10-20 percet kell végrehajtani. hello hello kiépítési állapotának a hello Azure-portálon jelenik meg.

## <a name="how-tooaccess-hello-linux-data-science-virtual-machine"></a>Hogyan tooaccess hello Linux adatok tudományos virtuális gép
Hello a virtuális gép kerül létrehozásra, után bejelentkezhet tooit SSH használatával. A hello hello fiók hitelesítő adatait használja **alapjai** szakasz 3. lépés hello szöveg rendszerhéj illesztő. Windows, egy SSH-ügyfél eszköz, például letöltheti [Putty](http://www.putty.org). Ha jobban szeret grafikus asztali (X Windows rendszerben), használja a Putty továbbítási X11, vagy hello X2Go ügyfél telepítése.

> [!NOTE]
> hello X2Go ügyfél végre jelentősen jobb, mint a tesztelése továbbítás X11. Egy asztali grafikus felület hello X2Go ügyfél használatát javasoljuk.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Telepítése és konfigurálása X2Go ügyfél
hello Linux virtuális gép már megtörtént a X2Go Servert és készen áll a tooaccept ügyfélkapcsolatokat. Linux virtuális gép grafikus asztali, tooconnect toohello hello az ügyfélen a következő:

1. Töltse le és telepítse a saját ügyfélplatformjára hello X2Go ügyfél [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Hello X2Go ügyfél futtassa, és válassza a **új munkamenet**. Több lap egy konfigurációs ablak nyílik meg. Adja meg a következő konfigurációs paraméterek hello:
   * **Munkamenet lapon**:
     * **Állomás**: hello állomásnév vagy IP-címét a Linux adatok tudományos virtuális Gépet.
     * **Bejelentkezési**: felhasználónév a hello Linux virtuális gép.
     * **SSH-Port**: 22, hello alapértelmezett értéken hagyhatja.
     * **Munkamenet típusa**: módosítás hello érték tooXFCE. Hello Linux virtuális gép jelenleg csak XFCE desktop támogatja.
   * **Az adathordozó lapon**: hang támogatást, és ha nincs szüksége toouse nyomtatás ügyfél kikapcsolhatja azokat.
   * **Megosztott mappák**: Ha az ügyfél gépek hello Linux virtuális gép csatlakoztatott könyvtárak, hello ügyfél gép címtárakat, amelyet a virtuális gép hello ezen a lapon tooshare fel.

Miután bejelentkezik toohello virtuális gép SSH-ügyfél hello vagy XFCE grafikus asztali hello X2Go ügyfélen keresztül, készen áll a toostart eszközökkel hello telepített és konfigurált hello VM áll. A XFCE látható alkalmazások parancsikonjai és asztali ikonok számos hello eszközök.

## <a name="tools-installed-on-hello-linux-data-science-virtual-machine"></a>Hello Linux adatok tudományos virtuális gép telepítve eszközök
### <a name="microsoft-r-server"></a>Microsoft R Server
R az egyik legnépszerűbb nyelvet hello adatelemzés és a gépi tanulás. Toouse R használni szeretne az elemzéseket, ha a virtuális gép hello Microsoft R Server (Asszony) a Microsoft R megnyitása (MRO) hello és matematikai Kernel könyvtár (MKL) rendelkezik. hello MKL matematikai műveletek közös az olyan analitikai algoritmusok optimalizálja. MRO 100 százalék CRAN-R kompatibilis, és bármely közzétett CRAN hello R szalagtárak hello MRO is telepíthető. Asszony lehetővé teszi az méretezés és a webszolgáltatások R modellek operationalization. Szerkesztheti a R programok hello alapértelmezett szerkesztőt, például a Rstudióból, vi, Emacs vagy gedit egyikében. Ha használ hello Emacs szerkesztő, vegye figyelembe, hogy hello Emacs csomag ELREJTÉSE (Emacs beszél statisztika), amely egyszerűbbé teszi belül hello Emacs szerkesztő, R-fájlok használata előre telepítve van.

toolaunch R konzol, csak gépelje **R** hello rendszerhéjban. Ezzel megnyitná tooan interaktív környezetben. toodevelop az R program egy szövegszerkesztőben, például a Emacs vagy vi vagy gedit általában használja, és futtassa az r parancsfájlok hello Rstudióból az informatikai részleg a teljes grafikus IDE környezetben toodevelop az R program.

Szerepel továbbá tooinstall hello az R-parancsfájl [első 20 R csomagok](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) Ha azt szeretné. Ezt a parancsfájlt után hello R interaktív felületén, amely is meg lehet adni (említett) írja be a **R** hello rendszerhéjban.  

### <a name="python"></a>Python
A fejlesztési pythonos környezetekben Anaconda Python elosztási 2.7 és 3.5-ös telepítve van. Ehhez a terjesztéshez tartalmaz hello kiinduló Python körülbelül 300 hello legnépszerűbb matematikai, tervezés és adatok analytics csomagok együtt. Hello alapértelmezett szöveg szerkesztők is használhatja. Ezenkívül Spyder, a Python IDE Anaconda Python terjesztéseket mellékelt is használhatja. Spyder kell egy grafikus asztali vagy X11 továbbítása. Egy helyi tooSpyder hello grafikus asztali valósul meg.

Python 2.7-es és 3.5-ös verzióját is van, mivel szüksége toospecifically hello szükséges Python verziót (conda környezet), amelybe a toowork hello a jelenlegi munkamenet aktiválása. hello aktiválási folyamat beállítja hello ELÉRÉSI változó toohello a Python kívánt verziójával.

tooactivate hello Python 2.7 conda környezetet, futtassa a hello következő hello rendszerhéjból:

    source /anaconda/bin/activate root

Python 2.7 telepített */anaconda/bin*.

tooactivate hello Python 3.5 conda környezetet, futtassa a hello következő hello rendszerhéjból:

    source /anaconda/bin/activate py35


Python 3.5 telepített */anaconda/envs/py35/bin*.

tooinvoke egy interaktív munkamenet Python, csak gépelje **python** hello rendszerhéjban. Ha Ön a grafikus felület vagy X11 továbbítási állítsa be, írja **pycharm** toolaunch hello PyCharm Python IDE.

tooinstall további Python könyvtárak kell toorun ```conda``` vagy ````pip```` a sudo parancsot, és adja meg a teljes elérési útja hello Python package Manager (conda vagy pip) tooinstall toohello megfelelő Python-környezetben. Példa:

    sudo /anaconda/bin/pip install <package> #for Python 2.7 environment
    sudo /anaconda/envs/py35/bin/pip install <package> # for Python 3.5 environment


### <a name="jupyter-notebook"></a>Jupyter notebook
hello Anaconda terjesztési is Jupyter notebook, egy környezeti tooshare kódot és az elemzések tartalmaz. hello Jupyter notebook JupyterHub keresztül érhető el. A helyi Linux-felhasználónév és jelszó használatával bejelentkezik.

hello Jupyter notebook server előre beállított Python 2, a Python 3 és az R kernelek. Nincs "Jupyter Notebook" toolaunch hello böngésző tooaccess hello notebook kiszolgáló nevű asztali ikon. Ha a virtuális gép hello SSH vagy X2Go ügyfél keresztül, meglátogathatja [https://localhost:8000 /](https://localhost:8000/) tooaccess hello Jupyter notebook kiszolgáló.

> [!NOTE]
> Folytassa, ha kapott tanúsítványt figyelmeztetéseket.
> 
> 

Hello Jupyter notebook kiszolgáló bármely állomásról érheti el. Csak gépelje *https://\<VM DNS-nevét vagy IP-cím\>: 8000 /*

> [!NOTE]
> Port 8000 meg van nyitva a hello tűzfal alapértelmezett amikor hello virtuális gép ki van építve.
> 
> 

A Microsoft csomagolását minta notebookok – egy a Python és egy-egy R. Hello notebook kezdőlapján hello hivatkozás toohello minták után toohello Jupyter notebook hitelesítik a helyi Linux-felhasználónév és jelszó használatával tekintheti meg. Létrehozhat egy új notebook kiválasztásával **új**, és ezután hello megfelelő nyelvi kernel. Ha nem lát hello **új** gombra, majd hello **Jupyter** hello bal felső toogo toohello kezdőlapján hello notebook kiszolgáló ikonra.

### <a name="apache-spark-standalone"></a>Apache Spark önálló 
Az Apache Spark egy önálló példányát OEM hello Linux DSVM toohelp most kialakított Spark-alkalmazások helyi először tesztelése és nagy fürtjein telepítése előtt. PySpark programok hello Jupyter kernel segítségével futtathatja. Nyissa meg a Jupyter és hello "Új" gombra kattintva megjelenik az elérhető kernelek listáját. hello "Spark – Python" hello PySpark kernel, amely lehetővé teszi a Spark olyan alkalmazásokat hozhat létre Python nyelven. Használhatja például PyCharm vagy Spyder toobuild Spark, a Python IDE program. Óta ez egy önálló példányát, hello Spark verem ügyfélprogram hívása hello belül futtatja. Ez lehetővé teszi gyorsabb és egyszerűbb tootroubleshoot problémák képest toodeveloping Spark-fürtön. 

Egy minta PySpark notebook valósul meg a Jupyter, amely a Jupyter ($ OTTHONI/notebookok/SparkML/pySpark) hello kezdőkönyvtárát hello "SparkML" könyvtárban található. 

Ha a Spark on az R programozási, használhatja a Microsoft R Server, SparkR vagy sparklyr. 

Futtatása Spark a környezetben, Microsoft R Server, előtt kell toodo egy egy idő beállítása lépés tooenable egy helyi csomóponthoz Hadoop HDFS és Yarn példányt. Alapértelmezés szerint Hadoop szolgáltatások vannak telepítve, de a hello DSVM letiltva. A sorrend tooenable, kell a következő parancsokat, legfelső szintű hello először toorun hello:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Le is hello Hadoop kapcsolódó szolgáltatások, ha már nincs szükség rájuk futtatásával ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` bemutatásához, hogyan toodevelop és tesztelési Asszony távoli Spark környezetben (amely hello önálló Spark-példányban lévő hello DSVM)-e a megadott és elérhető az hello minta`/dsvm/samples/MRS` könyvtár. 

### <a name="ides-and-editors"></a>IDEs és szerkesztők
Azt, hogy több kód szerkesztők. Ez magában foglalja a vi/VIM, Emacs, gEdit, PyCharm, Rstudióból, Eclipse és intellij-t. gEdit, eclipse-ben, az IntelliJ, Rstudióból és PyCharm grafikus szerkesztőt, és meg kell toobe bejelentkezve tooa grafikus asztali toouse őket. Ezek szerkesztők rendelkezik asztali és alkalmazás menü parancsikonok toolaunch őket.

**VIM** és **Emacs** szöveges szerkesztők vannak. Emacs, a jelenleg telepített egy Emacs beszél statisztika (ELREJTÉSE), amely megkönnyíti a r munkanapon belül hello Emacs szerkesztő nevű bővítmény csomagot. További információ található [ELREJTÉSE](http://ess.r-project.org/).

**Eclipse** a megnyitott forrás, amely támogatja több nyelv bővíthető IDE. hello Java fejlesztői edition hello példány hello virtuális gép telepítve. Nincsenek telepített tooextend hello környezetben használható számos népszerű nyelvhez elérhető beépülő modulok. Az eclipse-ben nevű telepített beépülő modul is tudunk **Eclipse Azure eszköztára**. Toocreate lehetővé teszi, fejleszthet, tesztelése és hello Eclipse fejlesztői környezetet, amely támogatja a például a Java nyelven használata Azure-alkalmazások telepítéséhez. Szerepel továbbá egy **Javához készült Azure SDK** , amely lehetővé teszi, hogy hozzáférési toodifferent Azure szolgáltatások egy Java-környezeten belül. További információk az Azure eszközkészlet az eclipse-ben található [Eclipse Azure eszköztára](../azure-toolkit-for-eclipse.md).

**LaTex** hello texlive csomaggal együtt egy Emacs bővítmény segítségével telepíthető [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) csomagot, amely egyszerűbbé teszi a szerzői LaTex dokumentumok Emacs belül.  

### <a name="databases"></a>Adatbázisok
#### <a name="postgres"></a>Postgres
hello nyílt forráskódú adatbázis **Postgres** érhető el a virtuális gép, hello hello futó szolgáltatások és initdb már befejeződött. Toocreate adatbázisok és a felhasználók továbbra is kell. További információkért lásd: hello [Postgres dokumentáció](https://www.postgresql.org/docs/).  

#### <a name="graphical-sql-client"></a>Grafikus SQL-ügyfél
**SQuirrel SQL**, tooconnect toodifferent adatbázisok (például a Microsoft SQL Server Postgres és MySQL) és az SQL-lekérdezések toorun megadva grafikus SQL-ügyfélen. Egy grafikus asztali munkamenetből (hello X2Go ügyfél, például használata) futtatható. tooinvoke SQuirrel SQL, indítsa el a hello asztal hello ikonjára, vagy futtassa a következő parancsot a hello rendszerhéj hello.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Hello először használhatja, az illesztőprogramokat és adatbázis-aliasok beállítása. hello JDBC illesztőprogramokat a helyen találhatók:

*/usr/share/Java/jdbcdrivers*

További információkért lásd: [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server eléréséhez használt parancssori eszközök
hello ODBC illesztőprogram-csomag az SQL Server is tartalmaz két parancssori eszközei:

**BCP**: hello bcp segédprogram tömeges között a Microsoft SQL Server példányának és adatfájlt adatainak másolása egy felhasználó által megadott formátumban. hello bcp segédprogram lehet használt tooimport nagy mennyiségű új sorok SQL Server-táblákra vagy adatfájlokba táblák tooexport adatokat. tooimport adatok táblába, állítsa táblához tartozó létrehozott formátumú fájl használata, vagy hello tábla és, amelyek érvényesek az oszlopok hello típusú hello struktúra ismertetése.

További információkért lásd: [összeköti a bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**Sqlcmd**: Adja meg a Transact-SQL-utasítások hello sqlcmd segédprogram, valamint a rendszer eljárásokat, és parancsfájlok hello parancssorban. Ez a segédprogram ODBC tooexecute Transact-SQL kötegek használja.

További információkért lásd: [csatlakozni az Sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Néhány ezt a segédprogramot a Linux és Windows platformok közötti különbségek vannak. Hello dokumentációjában talál.
> 
> 

#### <a name="database-access-libraries"></a>Adatbázis-hozzáférési függvénytárak
Nincsenek elérhető R és Python tooaccess adatbázisokban szalagtárak.

* Az R, hello **RODBC** csomag vagy **dplyr** csomag lehetővé teszi a tooquery, vagy hajtsa végre az SQL-utasítások hello adatbázis-kiszolgálón.
* A Python, hello **pyodbc** könyvtár ODBC adatbázis hozzáférést biztosít, mint az alapul szolgáló réteg hello.  

tooaccess **Postgres**:

* A R: Használata hello csomag **RPostgreSQL**.
* A Python: Használata hello **psycopg2** könyvtárban.

### <a name="azure-tools"></a>Azure-eszközök
Azure-eszközök a következő hello telepített virtuális gép hello:

* **Azure parancssori felület**: hello Azure parancssori felület lehetővé teszi a toocreate és rendszerhéj parancsok Azure-erőforrások kezeléséhez. tooinvoke hello Azure-eszközökkel, csak gépelje **azure súgó**. További információkért lásd: hello [Azure CLI-dokumentáció oldalán](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **A Microsoft Azure Tártallózó**: Microsoft Azure Tártallózó egy grafikus eszköz, amely a Azure storage-fiók, és az Azure-blobokat tooupload és a letöltési adatok tooand tárolt hello objektumokon keresztül használt toobrowse. A Tártallózó elérhető hello asztali parancsikonra. Hívhat meg azt a rendszerhéj parancssorából írja be a **StorageExplorer**. Egy X2Go ügyfélről bejelentkezve toobe kell, vagy X11 set továbbítás be van.
* **Az Azure szalagtárak**: hello az alábbiakban néhány hello előre telepített könyvtárak.
  
  * **Python**: hello Azure-hoz kapcsolódó függvénytárak telepített pythonban **azure**, **azureml**, **pydocumentdb**, és **pyodbc** . Az első három szalagtárak hello, érheti el az Azure storage szolgáltatások, Azure Machine Learning és Azure Cosmos DB (egy NoSQL-adatbázis az Azure-on). hello negyedik könyvtár, pyodbc (együtt hello Microsoft ODBC-illesztőprogram az SQL Server), lehetővé teszi, hogy hozzáférési tooSQL kiszolgáló, az Azure SQL Database és Azure SQL Data Warehouse a Python egy ODBC-illesztő segítségével. Adja meg **pip lista** összes hello toosee felsorolt szalagtárak. Lehet, hogy toorun parancsot a mindkét hello Python 2.7-es és 3.5-ös környezetekben.
  * **R**: hello Azure kapcsolatos-függvénytárak az R telepített **AzureML** és **RODBC**.
  * **Java**: hello Azure Java könyvtárak listája hello könyvtárban található **/dsvm/sdk/AzureSDKJava** a virtuális gép hello. hello kulcs szalagtárak az Azure tárolás és a felügyeleti API-k, Azure Cosmos DB és JDBC illesztőprogramok legyenek az SQL Server.  

Van-e hozzáférési hello [Azure-portálon](https://portal.azure.com) hello előre telepített Firefox böngészőben. Hello Azure-portálon, a létrehozása, kezelése és figyelése az Azure-erőforrások.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Az Azure Machine Learning egy teljes körűen felügyelt felhőszolgáltatás, amely lehetővé teszi a toobuild, telepítheti, és prediktív elemzési megoldások megosztása. A kísérletek és -modellek létrehozása az Azure Machine Learning Studio. Az elérhető egy webböngészőből hello adatok tudományos virtuális gépen látogasson el [Microsoft Azure Machine Learning](https://studio.azureml.net).

TooAzure Machine Learning Studio bejelentkezés után hozzáférés tooan kísérletezhet vászonra, ahol hozhat létre egy logikai folyamata hello gépi tanulási algoritmus. Is rendelkezik hozzáféréssel tooa Jupyter notebook Azure Machine Learning szolgáltatásban üzemeltetett, és zavartalanul tud működni hello kísérletek a Machine Learning Studióban. Azok a hello machine learning modellek, amelyek használatával őket egy webes szolgáltatás felületén létrehozása. Ez lehetővé teszi az ügyfelek bármely nyelvi tooinvoke előrejelzéseket hello gépi tanulási modellek az írt. További információkért lásd: hello [Machine Learning dokumentációs](https://azure.microsoft.com/documentation/services/machine-learning/).

Is létrehozhatja a virtuális gép hello a modellek R vagy Python nyelven, és majd telepítenie kell azt az Azure Machine Learning éles környezetben. R szalagtárak telepített azt (**AzureML**) és a Python (**azureml**) tooenable ezt a funkciót.

Hogyan toodeploy modellek R és Python az Azure Machine Learning információkért lásd: [tíz lehetősége van a virtuális gép adattudomány hello](machine-learning-data-science-vm-do-ten-things.md) (különösen hello szakasz "R vagy Python modellek létrehozása, és azok őket Azure Machine Learning "használata).

> [!NOTE]
> Ezek az utasítások verziójához hello Windows hello adatok tudományos VM készültek. Hello információ azonban nincs megadva üzembe helyezni a Machine Learning modellek tooAzure alkalmazható toohello Linux virtuális gép.
> 
> 

### <a name="machine-learning-tools"></a>Machine learning-eszközök
virtuális gép hello tartalmaz néhány gépi tanulási a eszközök és algoritmusok, előre összeállított és előre telepítve helyileg. Ezek a következők:

* **CNTK** (számítási hálózati eszközkészletet Microsoft Research): A tanulási eszközkészlet mély.
* **Vowpal Wabbit**: gyors online tanulási algoritmus.
* **xgboost**: olyan eszköz, amely biztosítja az optimalizált, súlyozott algoritmus.
* **Python**: Anaconda Python gépi tanulási algoritmusok könyvtárakkal Scikit – ismerje meg, például a csomagolt származik. Hello segítségével telepítheti a más szalagtárak `pip install` parancsot.
* **R**: a machine learning funkciók gazdag könyvtár érhető el a R. Hello szalagtárak előre telepített lm, glm, randomForest, rpart. Más szalagtárak futtatásával telepíthető:
  
        install.packages(<lib name>)

Ez hello kapcsolatos további információkat első három machine learning eszközök hello listában.

#### <a name="cntk"></a>CNTK
Ez az egy nyílt forráskódú, mély eszközkészlet tanulási. A parancssori eszköz (cntk), és már szerepel a hello elérési ÚTJA.

Alapszintű minta toorun hajtható végre a következő parancsok hello rendszerhéj hello:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

További információkért lásd: hello CNTK szakasza [GitHub](https://github.com/Microsoft/CNTK), és hello [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit a machine learning-rendszer által használt módszerek például online, a kivonatoló, allreduce, csökkentése, learning2search, aktív, és interaktív tanulási.

egy nagyon egyszerű példában toorun hello eszközt hello a következő:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Nincsenek más, nagyobb bemutatók ebben a könyvtárban. VW további információkért lásd: [ebben a szakaszban a Githubon](https://github.com/JohnLangford/vowpal_wabbit), és hello [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Ez az egy könyvtárban tervezett, súlyozott (fa) algoritmusok optimalizálva. hello szalagtár célja toopush hello számítási határértéke gépek toohello szélső érték között szükség tooprovide nagyméretű fa kiemelése méretezhető, hordozható és pontos.

Azt, a parancssorból, valamint az R tár biztosítja.

toouse R tárra elindíthatja egy interaktív R munkamenetet (beírásával **R** hello rendszerhéj), és hello függvénytár betöltése.

Íme egy egyszerű példa R kér futtathatja:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

toorun hello xgboost parancssor, az alábbiakban hello parancsok tooexecute hello rendszerhéj:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Egy .model fájl beíródik megadott toohello könyvtár. Ez a bemutató példa kapcsolatos információk is találhatók [a Githubon](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Xgboost kapcsolatos további információkért lásd: hello [xgboost dokumentációs oldal](https://xgboost.readthedocs.org/en/latest/), és a [GitHub-tárházban](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle (hello **R** **A**nalytical **T**öz **T**o **L**vett szolgáltatás érvényessége alatt **E** asily) GUI-alapú adatok feltárása és modellezési használja. Statisztikai mutatja be és adatok, könnyen modellezhető, átalakítások adatok vizuális összegzéseinek buildek felügyeletlen és a felügyelt modellek hello adatokból, megadja grafikusan hello modell teljesítményétől és új adatkészletek pontszámait. Azt is, ami R-kód replikálása hello műveletek a felhasználói felület hello kell közvetlenül R futtatását, illetve további elemzés céljából kiindulási pontként használható.

toorun Rattle, kell toobe grafikus asztali bejelentkezési munkamenetben. Hello terminált, írja be a ```R``` tooenter hello R környezetben. Hello R parancssorba írja be a következő parancsok hello:

    library(rattle)
    rattle()

Most már egy grafikus felület megnyílik lapok vannak beállítva. Az alábbiakban hello gyors üzembe helyezési szükséges Rattle toouse egy minta időjárási adathalmaz a lépések és a modell létrehozása. Egyes hello lépéseket tooautomatically felszólítás utáni telepítése, és néhány szükséges R csomagokat, amelyek még nem hello rendszeren történik.

> [!NOTE]
> Hello rendszerkönyvtár (hello alapértelmezett) nincs hozzáférési tooinstall hello csomagot, ha a kérdés jelenhet meg a R konzol ablakának tooinstall csomagok tooyour személyes könyvtár. Válasz *y* Ha ezek az üzenetek.
> 
> 

1. Kattintson az **Execute** (Végrehajtás) parancsra.
2. Egy párbeszédpanel jelenik meg, kéri fel, ha például a toouse hello példa időjárási adatkészlet. Kattintson a **Igen** tooload hello példa.
3. Kattintson a hello **modell** fülre.
4. Kattintson a **Execute** toobuild a döntési fában.
5. Kattintson a **megrajzolásához** toodisplay hello döntési fában.
6. Hello kattintson **erdő** választógomb, és kattintson a **Execute** toobuild véletlenszerű erdő.
7. Kattintson a hello **Evaluate** fülre.
8. Hello kattintson **kockázati** választógomb, és kattintson a **Execute** toodisplay két kockázat (eloszlásfv) teljesítmény előkészítésére.
9. Kattintson a hello **napló** lapon tooshow hello műveletek megelőző hello R-kód készítése.
   (Rattle jelenlegi kiadásában hello tooa hiba miatt tooinsert kell egy  *#*  karakter elé *... Ez a napló exportálása*  hello szöveges napló hello.)
10. Kattintson a hello **exportálása** gomb toosave hello R parancsfájlt nevű *weather_script. R* toohello kezdőmappa.

Kiléphet Rattle és R. Most létrehozott hello R-parancsfájl módosíthatja, vagy használja, mert az toorun azt bármikor toorepeat minden, ami hello Rattle UI belül végezhető el. Különösen az R kezdők, ez pedig tooquickly elemzést és a gépi tanulás egyszerű grafikus felületen, automatikusan az R toomodify a kód generálása és/vagy további egyszerűen.

## <a name="next-steps"></a>Következő lépések
Ez hogyan folytathatja a tanulási és feltárása:

* Hello [adattudomány hello Linux adatok tudományos virtuális gép a](machine-learning-data-science-linux-dsvm-walkthrough.md) a bemutató ismerteti, hogyan tooperform több közös adattudomány feladatokat a hello Linux adatok tudományos virtuális gép kiépítése itt. 
* Fedezze fel hello adatok tudományos különböző eszközök, a virtuális gép adattudomány hello próbálhatja ki ebben a cikkben leírt hello eszközök által. Is futtathatja *dsvm-további-információ* a hello rendszerhéj a bevezetés és mutatók toomore kapcsolatos alapvető tudnivalókat hello eszközök vannak telepítve, a virtuális gép hello hello virtuális gépekről.  
* Ismerje meg, hogyan toobuild végpont elemzési megoldásokat rendszeresen használatával hello [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* A Microsoft hello [Cortana Analytics Gallery](http://gallery.cortanaanalytics.com) a machine learning és az elemzés, hogy használjon hello Cortana Analytics Suite – minták.

