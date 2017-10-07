---
title: "az R Server on HDInsight - Azure használatába aaaGet |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Apache Spark on HDInsight-fürt, amely tartalmazza az R Server, és küldje el az R-parancsfájl hello fürtön."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: f7e418bbac48eee080a4b4cfbb33e246324ea5c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a>R Server a HDInsightban – első lépések

HDInsight egy R Server beállítás toobe integrálva a HDInsight-fürtöt tartalmaz. Ez a beállítás lehetővé teszi a R parancsfájlok toouse Spark és MapReduce toorun elosztott számítások. Ebből a dokumentumból megismerheti, hogyan toocreate egyik R Server, a HDInsight-fürtöt, majd azt mutatja be, a Spark használata parancsfájl futtatása egy R elosztott R-számítások.


## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**: Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie. Nyissa meg toohello cikk [beszerzése a Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) további információt.
* **A Secure Shell (SSH) ügyfél**: egy SSH-ügyfél használata tooremotely toohello HDInsight-fürthöz csatlakozzon, és futtassa a parancsokat közvetlenül hello fürtön. További információ: [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
* **SSH-kulcsok (nem kötelező)**: hello SSH használt fiók tooconnect toohello fürt jelszó vagy nyilvános kulcs használatával biztonságossá teheti. Jelszó használatával könnyebben, és lehetővé teszi, hogy Ön tooget anélkül, hogy toocreate egy nyilvános/titkos kulcspár elindult. A kulcs használata azonban biztonságosabb.

> [!NOTE]
> Ez a dokumentum hello lépések azt feltételezik, hogy a jelszó használatát.


## <a name="automated-cluster-creation"></a>Fürt automatikus létrehozása

Hello létrehozása HDInsight R kiszolgálók Azure Resource Manager sablonok hello SDK és is PowerShell használatával automatizálhatja azokat.

* toocreate egyik R Server, az Azure Resource Manager sablonnal, lásd: [R server a HDInsight-fürt üzembe helyezése](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
* az R Server hello .NET SDK használatával toocreate lásd [Linux-alapú fürtök létrehozása a Hdinsightban hello .NET SDK használatával.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* R Server toodeploy hello cikk powershellel, tekintse meg a [az R-kiszolgáló létrehozása a HDInsight a PowerShell-lel](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-hello-cluster-using-hello-azure-portal"></a>Hello Azure-portál használatával hello fürt létrehozása

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Válassza az **ÚJ** -> **Intelligencia és elemzés**, -> **HDInsight** lehetőséget.

    ![Új fürt létrehozásának képe](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. A hello **Gyorslétrehozás** tapasztalhat, adjon meg nevet hello fürtnek hello **fürtnév** mező. Ha több Azure-előfizetéssel rendelkezik, használja a hello **előfizetés** bejegyzés tooselect hello egy toouse szeretné.

    ![Fürt neve és az előfizetés kiválasztása](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. Válassza ki **típusú fürt** tooopen hello **fürtkonfiguráció** panelen. A hello **fürtkonfiguráció** panelen, jelölje be a következő beállítások hello:

    * **Fürt típusa**: R Server
    * **Verzió**: R Server tooinstall hello fürtön válassza hello verzióját. jelenleg elérhető hello verzió ***R Server 9.1 (HDI 3.6)***. Kibocsátási megjegyzések a hello elérhető R Server verziói érhetők el [Itt](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **Az R Serverhez R Studio community edition**: a böngésző alapú IDE hello élcsomópont alapértelmezés szerint telepítve van. Ha inkább toonot telepítve rendelkezik, majd törölje a jelet hello jelölőnégyzetet. Ha úgy dönt, hogy toohave még telepítve, hello URL-Címének elérése hello Rstudióból Server bejelentkezés megtalálható a portál alkalmazás panel a fürt létrehozása után van.
    * Hello alapértelmezett értékek hello egyéb beállításokat hagyja, és használja a hello **válasszon** gomb toosave hello fürt típusa.

        ![A Fürt típusa panel képernyőképe](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. Írja be a **Fürt bejelentkezési felhasználónevét** és a **Fürt bejelentkezési jelszavát**.

    Adjon meg egy **SSH-felhasználónevet**. SSH van használt tooremotely toohello fürt használatával csatlakozzon egy **Secure Shell (SSH)** ügyfél. Ezen a párbeszédpanelen, vagy (a hello konfiguráció lapon hello fürt) hello fürt létrehozása után hello SSH-felhasználó vagy adhat meg. R Server konfigurált tooexpect egy **SSH-felhasználónév** a "remoteuser".  **Ha más felhasználónevet használ, hello fürt létrehozása után egy további lépést kell végrehajtania.**

    Hagyja hello mezőben be van jelölve, a **ugyanazt a jelszót használják a fürt bejelentkezési** toouse **jelszó** típusúként hello hitelesítési kivéve, ha inkább a nyilvános kulcs használatához.  Szüksége van egy nyilvános/titkos kulcspár tooaccess R Server hello fürtön keresztül egy távoli ügyfélhez, mint például RTVS, Rstudióból vagy egy másik asztal IDE. Ha hello Rstudióból Server community edition telepíti, meg kell toochoose egy SSH-jelszó.     

    toocreate és használata a nyilvános és titkos kulcsból álló kulcspárt, törölje a jelet **ugyanazt a jelszót használják a fürt bejelentkezési** majd **nyilvános kulcs** , és folytassa az alábbiak szerint. Ezek az utasítások feltételezik, hogy a Cygwin with ssh-keygen vagy ennek megfelelő van telepítve.

    * Hozzon létre egy nyilvános/titkos kulcspár hello parancssort azon a hordozható számítógép:

        ssh-keygen -t rsa -b 2048

    * Hajtsa végre a hello Rákérdezés tooname kulcsfájlt, és írja be a nagyobb biztonság egy hozzáférési kódot. A képernyő hasonlóan kell kinéznie hello kép a következő:

        ![SSH parancssor Windows rendszerben](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * Ezzel a paranccsal létrejön egy titkos kulcs fájlja és a hello név < magánfelhő-key-fájlnév > .pub, például furiosa és furiosa.pub nyilvános kulcsfájlt.

        ![SSH dir](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * Majd adja meg a hello nyilvános kulcsot tartalmazó fájlt (&#42;. pub) hitelesítő adatok a fürt HDI hozzárendelése, és végül erősítse meg az erőforráscsoport és régiót, és válassza ki **következő**.

        ![Hitelesítő adatok panel](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * A hordozható számítógép hello a titkos kulcsfájl engedélyeinek módosítása:

        chmod 600 <titkos-kulcs-fájlnév>

   * A távoli bejelentkezéshez SSH használja hello titkos kulcsot tartalmazó fájlt:

        ssh –i <titkos-kulcs-fájlnév> remoteuser@<hostname public ip>

      Másik lehetőségként rész hello definíciófrissítések a Hadoop Spark a számítási környezet az R Serverhez hello ügyfélen. Lásd: hello **Microsoft R Server használata és a Hadoop ügyfélként** az alszakasz [számítási környezet létrehozása a Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).

6. hello Gyorslétrehozás közeledik, toohello **tárolási** panel tooselect hello tárfiók elsődleges helye hello hello használt beállítások toobe HDFS fájlrendszer hello fürt által használt. Válasszon új vagy meglévő Azure Storage-fiókot vagy meglévő Data Lake tárfiókot.

    - Ha egy Azure Storage-fiók lehetőséget választja, meglévő tárfiókot kiválasztásával van-e jelölve **válasszon egy tárfiókot** és jelölje ki a megfelelő fiók hello. Hozzon létre egy új fiókot hello segítségével **hozzon létre új** hello hivatkozásra **válasszon egy tárfiókot** szakasz.

      > [!NOTE]
      > Ha **új** hello új tárfiók nevet kell megadnia. Egy zöld pipa jelenik meg, ha hello neve el van fogadva.

      Hello **alapértelmezett tároló** alapértelmezett toohello hello fürt nevét. Ez az alapértelmezett hagyja hello értékként.

      Ha egy új tárolási fiók lehetőséget választotta egy kérdés tooselect **hely** van a megadott tooselect melyik régióban toocreate hello storage-fiók.  

         ![Adatforrás panel](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > Hello alapértelmezett adatforrás hello helyét is állítja be hello HDInsight-fürt hello helyét. hello fürt és az alapértelmezett adatforrás kell hello ugyanabban a régióban.

    - Ha azt szeretné, hogy egy meglévő Data Lake Store toouse, majd válassza ki hello ADLS tárolási fiók toouse, és adja hozzá hello fürtöt *Hozzáadás* identitás tooyour fürt tooallow hozzáférés toohello tárolására. Ezen folyamatról további információért tekintse át a [HDInsight-fürt létrehozása a Data Lake Store-ral az Azure Portalon](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) szakaszt.

    Használjon hello **válasszon** toosave hello gombra adatforrás konfigurálása.


7. Hello **összegzés** panel majd megjeleníti toovalidate a beállításokat. Itt módosíthatja a **a fürt méretét** toomodify hello száma a fürtben lévő kiszolgálókat, és adja meg a **parancsfájl-műveletek** toorun szeretné. Hacsak nem tudja, hogy kell-e automatikusan nagyobb fürt, hagyja feldolgozó csomópontok száma hello hello alapértelmezett `4`. hello becsült költség hello fürt belül hello panelen látható.

    ![fürt összegzése](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > Ha szükséges, a fürt később hello portálon keresztül átméretezheti (**fürt** -> **beállítások** -> **méretezési fürt**) tooincrease vagy Csökkentse a feldolgozó csomópontok száma hello.  Az átméretezés akkor lehet hasznos, üresjárati le hello fürt, ha nincsenek használatban, vagy a toomeet hello kapacitásigények a nagyobb feladatok hozzáadásához.
   >
   >

   Bizonyos tényezőket tookeep szem előtt, a fürt hello adatcsomópontokat és hello élcsomópont osztályozás a következők:  

   * a Spark elosztott R Server elemzések hello teljesítményének feldolgozó csomópontok száma arányos toohello esetén hello adatok mérete nagy.  

   * R Server elemzések hello teljesítményének lineáris hello méretű adatok elemzése folyamatban. Példa:  

     * Kis toomodest adatok teljesítmény esetén ajánlott, ha egy helyi számítási környezetben hello peremhálózati csomóponton elemzése.  Hello forgatókönyvek, amely alatt a helyi hello és Spark számítási környezetek további információk a legmegfelelőbb, tekintse meg a számítási környezet beállítások R Server a HDInsight.<br>
     * Ha toohello élcsomópont-e jelentkezni, és az R-parancsfájl futtatása, majd az összes hello ScaleR a rx-funkciók végrehajtása <strong>helyileg</strong> hello peremhálózati csomóponton. Így a memória hello és hello peremhálózati csomópont magok száma ennek megfelelően kell méretezni. Ugyanez vonatkozik, ha R Server HDI a távoli számítási környezetet a laptopján hello.

     ![Csomópontok árképzési szintjei panel](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     Használjon hello **válasszon** gomb toosave hello csomópont árképzési konfigurációs.

   Található egy **Sablon és paraméterek letöltése** hivatkozás is. Kattintson a hivatkozás toodisplay parancsfájlokat, amelyek idővel használt tooautomate hello létrehozása a fürt hello kijelölt konfigurációjával. A parancsfájlokat is elérhetőek hello Azure portál bejegyzés a fürt létrehozása után.

   > [!NOTE]
   > Némi időbe, hello fürt toobe létrehozása általában körülbelül 20 percet vesz igénybe. Hello Kezdőpulton hello csempe használja, vagy hello **értesítések** hello bevitelének maradt a hello lap toocheck hello létrehozási folyamata.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-toorstudio-server"></a>Csatlakozás tooRStudio kiszolgáló

Ha a kiválasztott tooinclude Rstudióból Server community edition telepítésében, majd hello Rstudióból bejelentkezési két különböző módszereket keresztül érheti el.

1. Nyissa meg a következő URL-cím toohello (ahol **CLUSTERNAME** létrehozott hello fürt hello neve):

    https://**FÜRTNÉV**.azurehdinsight.net/rstudio/

2. Nyissa meg a fürt hello bejegyzés hello Azure-portálon válassza hello **R Server irányítópultok** Gyorshivatkozás, és jelölje be hello **R Studio irányítópult**:

     ![Hello R studio irányítópulton](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Hello R studio irányítópulton](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Hello módszert használja, attól függetlenül hello első alkalommal jelentkezik be kell tooauthenticate kétszer.  Hello első hitelesítési, adja meg a hello *rendszergazda felhasználói azonosítóját a fürt* és *jelszó*. A parancssorból hello második adja meg a hello *SSH userid* és *jelszó*. Későbbi napló modulok csak hello *SSH-jelszónak* és *userid*.

<a name="connect-to-edge-node"></a>
## <a name="connect-toohello-r-server-edge-node"></a>Csatlakozás toohello R Server élcsomópont

Csatlakozás bemutató Server élcsomópont hello HDInsight-fürt SSH hello parancs használatával:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Hello található `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` hello Azure portálon, majd a fürt kiválasztásával címet **összes beállítás** -> **alkalmazások** -> **RServer**. Hello hello élcsomópont SSH végpont adatait jeleníti meg.
>
> ![Hello SSH végpont hello peremhálózati csomópont képe](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

Ha a jelszó toosecure az SSH-felhasználói fiókhoz,-e rákérdezéses tooenter azt. Ha egy nyilvános kulcshoz, előfordulhat, hogy toouse hello `-i` paraméter toospecify hello kapcsolódó titkos kulcs. Példa:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

További információkért lásd: [csatlakozzon az SSH használatával tooHDInsight (Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md).

A csatlakozás után egy kérdés hasonló toohello következő elvégzésével:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Több párhuzamos felhasználó engedélyezése

További felhasználók melyik hello Rstudióból közösségi verziója fut. hello élcsomópont való hozzáadásával több egyidejű felhasználók engedélyezheti.

HDInsight-fürt létrehozásakor két felhasználót kell megadnia, egy HTTP-felhasználót és egy SSH-felhasználót:

![1. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- **A fürt bejelentkezési felhasználónevének**: egy HTTP-felhasználó a hitelesítéshez, amely használt tooprotect hello HDInsight hello HDInsight átjárón keresztül fürtök létrehozott. A HTTP-felhasználóhoz használt tooaccess hello Ambari felhasználói felület, a YARN felhasználói felületen, valamint egyéb felhasználói felületi összetevőket.
- **Secure Shell (SSH) felhasználónév**: az SSH felhasználói tooaccess hello fürt biztonságos rendszerhéj keresztül. Ez a felhasználó a felhasználó az összes hello átjárócsomópontokkal, a feldolgozó csomópontok és a peremhálózati csomópontok Linux rendszer hello. Így biztonságos rendszerhéj tooaccess hello csomópontok bármelyikét távoli fürtben.

hello Microsoft R Server on HDInsight-fürt típusa használt hello R Studio Server közösségi verzió bejelentkezési mechanizmusaként csak Linux felhasználónevet és jelszót fogad el. Nem támogatja a jogkivonatok átadását. Ha létrehozott egy új fürtre, és toouse R Studio tooaccess Igen, van szüksége a toolog kétszer.

- Először jelentkezzen be hello HDInsight átjárón keresztül hello HTTP felhasználói hitelesítő adatok használatával: 

    ![2a párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- Majd használjon hello SSH felhasználói hitelesítő adatok toolog tooRStudio:
  
    ![2b párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

Jelenleg HDInsight-fürt üzembe helyezésekor csak egy SSH felhasználói fiókot lehet létrehozni. Tooenable több felhasználók tooaccess Microsoft az R Server on HDInsight clusters, így további felhasználóknak toocreate hello Linux rendszer szükséges.

Rstudióból Server közösségi hello fürt élcsomóponthoz fut, mert több lépésből áll itt:

1. Hello létre SSH felhasználói toolog toohello élcsomópont használata
2. További Linux-felhasználók hozzáadása az élcsomópontban
3. Rstudióból közösségi verzió használata hello felhasználó létrehozása

### <a name="step-1-use-hello-created-ssh-user-toolog-in-toohello-edge-node"></a>1. lépés: Hello létre SSH felhasználói toolog használata toohello élcsomópont

Töltse le a bármely SSH eszközt (például a Putty), és hello meglévő SSH felhasználói toolog a használja. Kövesse az utasításokat hello [csatlakozzon az SSH használatával tooHDInsight (Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello élcsomópont. hello peremhálózati csomópont cím R Server on HDInsight-fürt: *fürtnév-kell adnia végrehajtási adatokat-ssh.azurehdinsight.net*


### <a name="step-2-add-more-linux-users-in-edge-node"></a>2. lépés: További Linux-felhasználók hozzáadása az élcsomópontban

egy felhasználó toohello peremhálózati csomópont tooadd hello parancsokat hajthat végre:

    sudo useradd yournewusername -m
    sudo passwd yourusername

A következő visszaküldött elemek hello kell megjelennie: 

![3. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

Ha a rendszer kéri "aktuális Kerberos jelszó:", csak Entert kell **Enter** tooignore azt. Hello `-m` beállítást `useradd` parancs azt jelzi, hogy hello rendszer létrehoz egy mappát hello felhasználó esetében Rstudióból közösségi verzió szükséges.


### <a name="step-3-use-rstudio-community-version-with-hello-user-created"></a>3. lépés: Használja Rstudióból közösségi verzió hello felhasználó létrehozása

Használjon hello létrehozott felhasználói toolog tooRStudio:

![4. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

Figyelje meg, hogy Rstudióból azt jelzi, hogy hello új felhasználó használ (például itt *sshuser6*) toolog hello fürt: 

![5. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

Hello eredeti hitelesítő adataival is bejelentkezhet (alapértelmezés szerint van *sshuser*) egyszerre egy másik böngészőben ablakból.

Feladat küldéséhez használhatja a scaleR-függvényeket. Itt látható egy példa hello használt parancsok toorun egy feladatot:

    # Set hello HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data toohello tmp folder.
    remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set directory in bigDataDirRoot tooload hello data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create hello directory.
    rxHadoopMakeDir(inputDir)

    # Copy hello data from source tooinput.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define hello HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for hello airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all hello column names.
    varNames <- names(airlineColInfo)

    # Define hello text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define hello text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify hello formula toouse.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define hello Spark compute context.
    mySparkCluster <- RxSpark()

    # Set hello compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


Figyelje meg, hogy vannak-e hello feladat a különböző felhasználói neve a YARN felhasználói felületen:

![6. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

Megjegyzés is, hogy hello újonnan hozzáadott felhasználók nem rendelkeznek gyökérszintű jogosultsággal Linux rendszeren, de azok rendelkezik hello azonos tooall hello fájljainak hello HDFS- és a WASB távtároló eléréséhez.


<a name="use-r-console"></a>
## <a name="use-hello-r-console"></a>Hello R-konzollal

1. Hello SSH-munkamenetet használja a következő parancs toostart hello R konzol hello:  

        R

2. Kimeneti hasonló toohello következő kell megjelennie:
    
    R verzió 3.2.2 (2015-08-14)--"Tűz biztonsági" Copyright (C) 2015 R Foundation hello statisztikai számítások Platform: x86_64-pc-linux-gnu (64 bites)

    Az R egy ingyenes szoftver, és EGYÁLTALÁN SEMMILYEN JÓTÁLLÁS SEM vonatkozik rá.
    Üdvözli a tooredistribute áll ez bizonyos körülmények között.
    A terjesztésre vonatkozó részletekért írja be a 'license()' vagy a 'licence()' parancsot.

    Támogatja a természetes nyelveket, de angol területi beállítással fut

    Az R egy együttműködésen alapuló projekt, sok hozzájárulóval.
    Írja be a "contributors()" További információk és "citation()" hogyan toocite R vagy R csomagok kiadványokban a.

    Írja be a "demo()" néhány bemutatók, az online súgó "help()" vagy "help.start()" egy HTML-böngésző felület toohelp.
    Írja be a "q()" tooquit R.

    Microsoft R Server 8.0-s verzió: az R Microsoft-csomagok továbbfejlesztett terjesztési verziója Copyright (C) 2016 Microsoft Corporation

    A kiadási megjegyzések megtekintéséhez írja be a 'readme()' parancsot.
    >

3. A hello `>` parancssorba R kódot is megadhat. R server tartalmaz, amelyek lehetővé teszik tooeasily csomagok Hadoop kommunikál, és futtassa az elosztott számítások. Például használja a következő parancs tooview hello legfelső szintű hello alapértelmezett fájlrendszer hello HDInsight-fürthöz hello:

    rxHadoopListFiles("/")

4. Hello WASB stílus címzési is használható.

    rxHadoopListFiles("wasb:///")


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Az R Server használata HDI-n a Microsoft R Server vagy a Microsoft R ügyfél egy távoli példányáról

Már lehetséges tooset hozzáférés toohello HDI Hadoop Spark számítási környezet Microsoft R Server vagy a Microsoft R ügyfél egy asztali vagy hordozható futó távoli példányának létrehozása. Lásd: **Microsoft R Server használata és a Hadoop ügyfélként** alszakasz a hello [a számítási környezet létrehozásakor a Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md). toodo toospecify hello hello RxSpark számítási környezet meghatározása a hordozható számítógép esetén a következő beállításokat kell tehát: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches, és sshProfileScript. Példa:


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a>Számítási környezet használata

A számítási környezet lehetővé teszi a toocontrol hogy számítási helyben végzett hello élcsomópont vagy le van elosztva hello hello HDInsight-fürt csomópontjai között.

1. Rstudióból kiszolgálóról vagy hello R konzol (az SSH-munkamenetet) használja a következő kód tooload példaadatokat a hello alapértelmezett tároló hdinsight hello:

        # Set hello HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data toohello tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set directory in bigDataDirRoot tooload hello data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make hello directory
        rxHadoopMakeDir(inputDir)

        # Copy hello data from source tooinput
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. A következő hozzuk létre bizonyos adatok információ, és határozza meg két adatforrások, így hello adatokkal dolgozunk.

        # Define hello HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for hello airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all hello column names
        varNames <- names(airlineColInfo)

        # Define hello text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define hello text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula toouse
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Most futtassa logisztikai regresszió hello adatok hello helyi számítási környezetet használ.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    A következő sorokat hasonló toohello végződő kimenetnek kell megjelennie:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. A következő Futtatás most hello azonos logisztikai regresszió hello Spark-környezetet használ. hello Spark környezetben keresztül hello feldolgozó csomópontjaihoz hello HDInsight fürt feldolgozása hello terjeszti.

        # Define hello Spark compute context
        mySparkCluster <- RxSpark()

        # Set hello compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > Használhatja a MapReduce toodistribute számítási fürt csomópontjai között. A számítási környezetről további információért lásd: [Számítási környezeti beállítások a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-compute-contexts.md).


## <a name="distribute-r-code-toomultiple-nodes"></a>R-kód toomultiple csomópontok terjesztése

R Server, egyszerűen meglévő R-kód igénybe vehet, és futtassa hello fürt több csomópontja között használatával `rxExec`. Ez a függvény akkor hasznos, amikor paraméteres frissítést vagy szimulációkat végez. hello következő kódja: példa bemutatja, hogyan toouse `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Ha hello Spark vagy MapReduce környezet továbbra is használ, ez a parancs értékét adja vissza hello csomópontnév hello munkavégző csomópontokhoz hello kód `(Sys.info()["nodename"])` a futtatható. Például egy négy csomópontot tartalmazó fürtben, a várt tooreceive kimeneti hasonló toohello következő:

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="accessing-data-in-hive-and-parquet"></a>Adatok elérése a Hive és a Parquet eszközökben

R Server 9.1 egy szolgáltatása lehetővé teszi a közvetlen hozzáférést toodata a Hive és Parquet használatra ScaleR funkciók hello Spark számítási környezetben. Ezek a képességek új ScaleR adatok forrás funkciók RxHiveData és RxParquetData használata Spark SQL tooload adatok közvetlenül a egy Spark DataFrame ScaleR analízis működő keresztül érhetők el.  

a következő kód hello hello új funkciók használatát a néhány példakódot tartalmaz:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close hello Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


További információ ezen új funkciók használatát a hello online súgójában R Server hello használata `?RxHivedata` és `?RxParquetData` parancsok.  


## <a name="install-additional-r-packages-on-hello-edge-node"></a>Hello élcsomópont további R csomagok telepítése

Ha szeretne további R csomagokat tooinstall hello peremhálózati csomóponton, `install.packages()` közvetlenül belül hello R konzol amikor csatlakoztatott toohello él csomópont SSH-n keresztül. Ha tooinstall R csomagokat a hello munkavégző csomópontokhoz hello fürt, a parancsfájl műveletet kell használnia.

A Parancsfájlműveletek olyan Bash parancsfájlok, amelyek használt toomake konfigurációs módosítások toohello HDInsight fürt vagy tooinstall további szoftverek, például további R csomagokat. tooinstall egy parancsfájlművelet használatával további csomagokat a lépéseket követve hello használata:

> [!IMPORTANT]
> A Parancsfájlműveletek tooinstall további R csomagok használata csak akkor használható hello fürt létrehozása után. Ne használja ezt az eljárást fürt létrehozása során, hello parancsfájl támaszkodik R Server alatt teljesen telepítve és konfigurálva.
>
>

1. A hello [Azure-portálon](https://portal.azure.com), válassza ki az R Server on HDInsight-fürt.

2. A hello **beállítások** panelen válassza **Parancsfájlműveletek** , majd **nyújt új** toosubmit egy új parancsfájlművelet.

   ![A Szkriptműveletek panel képe](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. A hello **parancsfájlművelet** panelen adja meg a következő információ hello:

   * **Név**: egy rövid nevet tooidentify ezt a parancsfájlt

   * **Bash-szkript URI azonosítója**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Fej**: Ez az elem **ne legyen bejelölve**

   * **Feldolgozó**: Ez az elem **legyen bejelölve**

   * **Szegélycsomópontok**: Ez az elem **ne legyen bejelölve**.

   * **Zookeeper**: Ez az elem **ne legyen bejelölve**

   * **Paraméterek**: hello R csomagok toobe telepítve. Például: `bitops stringr arules`

   * **Ha megőrzi a...**: Ez az elem **legyen bejelölve**  

   > [!NOTE]
   > 1. Alapértelmezés szerint az összes R csomagot hello Microsoft MRAN tárház hello R Server telepített verziója megfelel egy pillanatképből telepítve. Ha azt szeretné, hogy a csomagok tooinstall újabb verzióját, majd nincs inkompatibilitás néhány kockázatát. Azonban ez a telepítési típus lehetséges áll megadásával `useCRAN` , hello hello csomag első elemének listájában, például `useCRAN bitops, stringr, arules`.  
   > 2. Néhány R csomag további Linux rendszerű könyvtárakat igényel. Kényelmi célokat szolgál azt előre telepített hello függőségek hello top 100 legnépszerűbb R csomag szükséges. Azonban ha hello R csomag(ok) telepítése van szükség a szalagtárak túl ezek majd kell itt használt hello alap parancsfájl letöltése és könyvtár lépéseit tooinstall hello hozzáadása. Meg kell majd feltöltés hello módosító parancsfájlt tooa nyilvános blob-tároló az Azure storage és módosított hello parancsfájl tooinstall hello csomagok használata.
   >    A szkriptműveletek fejlesztésével kapcsolatos további információért lásd: [Szkriptművelet fejlesztése](hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Szkriptműveletek hozzáadása](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. Válassza ki **létrehozása** toorun hello parancsfájl. Hello parancsfájl befejeztét követően az összes munkavégző csomóponton hello R csomagok érhetők el.


## <a name="using-microsoft-r-server-operationalization"></a>A Microsoft R Server operacionalizálás használata

Amikor befejeződött az adatok modellezési, üzembe hello modell toomake előrejelzéseket. a Microsoft R Server operationalization tooconfigure hello a következő lépéseket hajtsa végre:

Első, ssh hello peremhálózati csomópontjába. Például: 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Használata után ssh, módosítsa a könyvtárat hello megfelelő verzióját és a sudo hello dotnet dll: 

- Microsoft R Server 9.1 esetén:

    cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

- Microsoft R Server 9.0 esetén:

    cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

egy egy-mezőben konfigurációjával tooconfigure Microsoft R Server operationalization hello a következő:

1. Válassza a „Configure R Server for Operationalization” (Az R Server konfigurálása a Működőképessé tétel művelethez) lehetőséget
2. Válassza az „A. One-box (web + compute nodes)” (Beépített (web + számítási csomópontok)) elemet
3. Írja be a jelszót hello **admin** felhasználó

![one box op](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Választható lépésként diagnosztikai ellenőrzéseket végezhet az alább látható diagnosztikai tesztelés futtatásával:

1. Válassza a „6. Run diagnostic tests” (Diagnosztikai tesztek futtatása) elemet
2. Válassza az „A. Test configuration” (Tesztkonfiguráció) elemet
3. Írja be a Username = „admin” parancsot, valamint a jelszót az előző konfigurációs lépésből
4. Erősítse meg az Overall Health = pass parancsot
5. Kilépés hello felügyeleti segédprogram
6. Lépjen ki az SSH-ból

![Diagnostic for op](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>**Hosszú késések a webszolgáltatások Sparkban történő felhasználásakor**
>
>Ha nagy késleltetéseket mrsdeploy funkciók Spark számítási környezetben létre tooconsume egy webszolgáltatás-bővítmény közben, esetleg tooadd egyes hiányzó mappák. Spark alkalmazás hello tartozik tooa felhasználói, úgynevezett "*rserve2*" mrsdeploy függvények használatával webszolgáltatásból meghívni, amikor. a probléma megoldásához toowork:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


Ebben a szakaszban Operationalization hello konfigurációja befejeződött. Mostantól a hello "mrsdeploy" csomagot a RClient tooconnect toohello Operationalization peremhálózati csomóponton máris használhatja a szolgáltatásokat, például [távoli végrehajtás](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) és [-webszolgáltatások](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). Attól függően, hogy a fürt beállítása a virtuális hálózaton vagy nem szükség lehet a tooset mentése port előre tunneling keresztül SSH-bejelentkezéskor. hello alábbi szakaszok azt ismertetik, hogyan tooset fel ezt az alagutat.

### <a name="rserver-cluster-on-virtual-network"></a>Az RServer fürt virtuális hálózaton van

Ellenőrizze, hogy forgalmat 12800 toohello élcsomópont porton keresztül. Ily módon használhatja hello peremhálózati csomópont tooconnect toohello Operationalization szolgáltatást.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Ha hello `remoteLogin()` nem toohello élcsomópont, csatlakozni, de SSH toohello élcsomópontot is, majd tooverify kell, hogy port 12800 hello szabály tooallow forgalmát megfelelő vagy nem lett beállítva. Tooface hello a probléma továbbra is, ha megkerüléséhez azt port előre tunneling SSH-n keresztül beállításával. Útmutatásért lásd a következő szakasz hello.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>Az RServer fürt nem virtuális hálózaton van beállítva

Ha a fürt nem a virtuális hálózaton van beállítva vagy problémás a kapcsolódás a virtuális hálózaton keresztül, akkor használhatja az SSH porttovábbító alagutat:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

A Putty szoftveren is beállítható.

![putty ssh kapcsolat](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Ha az SSH-munkamenetet aktív, a gép portról 12800 hello forgalmat toohello peremhálózati csomópont port 12800 SSH-kapcsolaton keresztül továbbítja. A `127.0.0.1:12800` címet használja a `remoteLogin()` metódusban. A rendszer a toohello peremhálózati csomópont operationalization port továbbítási keresztül.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-tooscale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Hogyan tooscale Microsoft R Server Operationalization számítási csomópontok HDInsight munkavégző csomópontokhoz

### <a name="decommission-hello-worker-nodes"></a>Hello munkavégző csomópont leszerelése

A Microsoft R Servert jelenleg nem a Yarnon keresztül kezeli a rendszer. Ha hello munkavégző csomópontokhoz nincs leszerelve, hello Yarn erőforrás-kezelő nem működik megfelelően, mivel nem lesz kompatibilis foglalja el hello server hello erőforrások. A sorrend tooavoid ebben az esetben ajánlott leszerelése munkavégző csomópontokhoz hello hello számítási csomópontok a horizontális előtt.

Toodecommissioning munkavégző csomópontokhoz lépéseket:

* Jelentkezzen be tooHDI fürt Ambari konzol és a "gazdagép" lapon kattintson a
* Válassza ki a feldolgozó csomópontok (toobe szerelni), kattintson a "Műveletek" > "Kiválasztott gazdagép" > "Gazdagép" > kattintson a "Kapcsolja be a karbantartási mód". Például a következő kép hello jelenleg kijelölt wn3 és wn4 toodecommission.  

   ![feldolgozó csomópont(ok) leszerelése](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **DataNodes** (AdatCsomópontok) elemet > kattintson a **Decommission** (Leszerelés) gombra
* Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **NodeManagers** (CsomópontKezelők) elemet > kattintson a **Decommission** (Leszerelés) gombra
* Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **DataNodes** (AdatCsomópontok) elemet > kattintson a **Stop** gombra
* Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **NodeManagers** (CsomópontKezelők) elemet > kattintson a **Stop** gombra
* Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott gazdagépek) > **Hosts** (Gazdagépek) elemet > kattintson a **Stop All Components** (Összes összetevő leállítása) gombra
* Törölje a hello munkavégző csomópontokhoz, és válassza ki a hello átjárócsomópontokkal
* Válassza az **Actions**(Műveletek) > **Selected Hosts** (Kiválasztott gazdagépek) > "**Hosts**(Gazdagépek) > **Restart All Components**(Összes gazdagép újraindítása) elemet

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Számítási csomópontok konfigurálása az összes leszerelt feldolgozó csomóponton

1. Jelentkezzen be SSH-n keresztül minden egyes leszerelt feldolgozó csomópontba.
2. Futtassa az admin segédprogramot a következő paranccsal: `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
3. Adjon meg "1" tooselect beállítást "Konfigurálása R Server a Operationalization".
4. Adja meg a "c"c"tooselect beállítás Compute node” (Számítási csomópont) lehetőség kijelöléséhez. Ez konfigurálja az hello számítási csomópont hello munkavégző csomóponton.
5. Kilépés hello felügyeleti eszközt.

### <a name="add-compute-nodes-details-on-web-node"></a>Számítási csomópontok részleteinek megadása a Web csomóponton

Leszerelt feldolgozó csomópontjaihoz konfigurálása után toorun számítási csomópont, térjen vissza hello peremhálózati csomóponton, és adja hozzá a leszerelt munkavégző csomópontokhoz IP-címek hello Microsoft R Server webes csomópont-konfiguráció:

* SSH-ból hello élcsomópont.
* Futtassa az `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` parancsot.
* Hello "URI" szakaszban keresse meg, és adja hozzá a munkavégző csomópont IP-cím és port részleteit.

    ![feldolgozó csomópont(ok) leszerelési parancssora](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).


## <a name="next-steps"></a>Következő lépések

Most meg kell ismernie, hogyan toocreate egy új HDInsight-fürtöt, amely tartalmazza az hello R Server és hello használatának alapjaival hello R konzol az SSH-munkamenetet. hello következő témaköreiben egyéb módjai kezelése és az R Server on HDInsight használata:

* [Adja hozzá a Rstudióból Server tooHDInsight (Ha nincs telepítve a fürt létrehozása során)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Számítási környezeti beállítások a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage lehetőségek a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-storage.md)
