---
title: "a művelet fejlesztése a HDInsight - Azure aaaScript |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize Hadoop-fürtök parancsfájl művelettel. Parancsfájl művelet lehet használt tooinstall Hadoop fürthöz vagy toochange hello konfigurálása a fürt a telepített alkalmazások futó további szoftvereket."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 4fc3a389df8a003f7129ab00b4cd9bc7ad81a419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>A HDInsight-Windows-alapú fürtök parancsfájlművelet-parancsfájlok fejlesztése
Ismerje meg, hogyan toowrite parancsfájlművelet parancsfájlok, a HDInsight. Parancsfájlművelet-parancsfájlok használatával kapcsolatos információkért lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md). Linux-alapú HDInsight-fürtök esetén írt ugyanabból a cikkből lásd: hello [parancsfájlművelet fejlesztése parancsfájlok a HDInsight](hdinsight-hadoop-script-actions-linux.md).



> [!IMPORTANT]
> Ez a dokumentum a Windows-alapú HDInsight-fürtök csak munkahelyi hello szükséges lépések. HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). A Parancsfájlműveletek használata a Linux-alapú fürtökön információkért lásd: [parancsfájl-művelet fejlesztése a HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).
>
>



Parancsfájl művelet lehet használt tooinstall Hadoop fürthöz vagy toochange hello konfigurálása a fürt a telepített alkalmazások futó további szoftvereket. A Parancsfájlműveletek olyan parancsfájlok, futtathatók a hello fürtcsomópontokon, ha a HDInsight-fürtök vannak telepítve, és végrehajtás hello fürt csomópontja HDInsight konfigurálásának befejezése után. A parancsfájlművelet hajtja végre a fiók rendszergazdai jogosultságokkal, és teljes körű hozzáférési jogosultságokat biztosít toohello fürtcsomópontok. Az egyes fürtökön biztosítható, hogy az abban megadott vannak hello sorrendben hajtja végre a parancsfájl műveletek toobe listáját.

> [!NOTE]
> Ha hibaüzenet jelenik meg a következő hello:
>
> System.Management.Automation.CommandNotFoundException; ExceptionMessage: hello kifejezés "Save-HDIFile" nem ismerhető fel egy parancsmag, a függvény, a parancsfájl vagy a futtatható program hello nevét. Hello helyesírás hello neve, vagy ha egy elérési utat megtalálható, győződjön meg arról, hogy hello elérési útja helyes-e, és próbálkozzon újra.
> Ennek az oka hello segédmódszereket nem tartozik.  Lásd: [segédmódszereket egyéni parancsfájlok](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).
>
>

## <a name="sample-scripts"></a>Mintaszkriptek
A HDInsight-fürtök létrehozása a Windows operációs rendszeren, hello parancsfájlművelet esetén Azure PowerShell-parancsfájlt. hello alábbi parancsfájl a következő egy minta hello hely konfigurációs fájljainak konfigurálásához végrehajtott:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable tooconfigure $ConfigFileName because it is not part of hello HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

hello parancsfájl fogadja el a négy paraméterek, hello konfigurációs fájl nevét, hello tulajdonság kívánt toomodify, hello érték tooset, és egy leírást. Példa:

    hive-site.xml hive.metastore.client.socket.timeout 90

Ezek a paraméterek beállítása hello hive.metastore.client.socket.timeout érték too90 hello hive-site.xml fájlban.  hello alapértelmezett értéke 60 másodperc.

A parancsfájlpéldát is találhatók [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight tooinstall további összetevők biztosít több parancsfájlok a HDInsight-fürtökön:

| Név | Szkript |
| --- | --- |
| **Spark telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Lásd: [telepítése és használata a HDInsight Spark-fürtök][hdinsight-install-spark]. |
| **R telepítéséhez** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Lásd: [telepítése és használata R HDInsight-fürtök][hdinsight-r-scripts]. |
| **Solr telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Lásd: [telepítése és használata Solr a HDInsight-fürtök](hdinsight-hadoop-solr-install.md). |
| - **Giraph telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Lásd: [telepítése és használata Giraph a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md). |

Parancsfájlművelet is telepíthető a hello Azure-portálon az Azure PowerShell vagy a HDInsight .NET SDK hello.  További információkért lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet][hdinsight-cluster-customize].

> [!NOTE]
> hello mintaparancsfájlok működik, csak a HDInsight-fürt verziószáma 3.1-es vagy újabb. A HDInsight-fürt verziókról további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).
>
>

## <a name="helper-methods-for-custom-scripts"></a>Egyéni parancsfájlok segítő módszerei
Parancsfájl művelet segítő módszereket segédprogramok egyéni parancsfájlok írása közben használható. Ezek a módszerek definiált [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), és a parancsfájlok használata a következő minta hello tartalmazhat:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module toomake writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed tooload HDInsightUtilities module, exiting ...";
        exit;
    }

Az alábbiakban a parancsfájl által biztosított segédmódszereket hello:

| Segédmetódus | Leírás |
| --- | --- |
| **Mentés-HDIFile** |Hello fájl letöltése megadott egységes erőforrás-azonosító (URI) tooa helyét a helyi lemezen hello hello Azure virtuális Géphez hozzárendelt toohello gazdagépfürt társított. |
| **Bontsa ki a HDIZippedFile** |Bontsa ki a tömörített fájlt. |
| **Invoke-HDICmdScript** |Futtassa a parancsfájlt a cmd.exe. |
| **Írási-HDILog** |Hello parancsfájl művelethez használt egyéni parancsfájl kimeneti írása |
| **Get-szolgáltatások** |Ha végrehajtja a hello parancsfájl hello gépen futó szolgáltatásokat listájának lekérése. |
| **Get-szolgáltatás** |Hello adott szolgáltatás nevű bemeneti adatként, részletes információ egy adott szolgáltatáshoz (a szolgáltatás neve, folyamatazonosító, állapot, stb.) Ha végrehajtja a hello parancsfájl hello gépen. |
| **Get-HDIServices** |HDInsight szolgáltatások hello számítógépen fut, ahol hello parancsfájl végrehajtása listájának lekérése. |
| **Get-HDIService** |Hello adott HDInsight szolgáltatásnévvel bemeneti adatként, részletes információ egy adott szolgáltatáshoz (a szolgáltatás neve, folyamatazonosító, állapot, stb.) Ha végrehajtja a hello parancsfájl hello gépen. |
| **Get-ServicesRunning** |Futó szolgáltatások hello számítógépen ahol hello parancsfájl végrehajtása listájának lekérése. |
| **Get-ServiceRunning** |Ellenőrizze, hogy egy adott szolgáltatáshoz (a neve szerint) hello számítógépen ahol hello parancsfájl végrehajtása. |
| **Get-HDIServicesRunning** |HDInsight szolgáltatások hello számítógépen fut, ahol hello parancsfájl végrehajtása listájának lekérése. |
| **Get-HDIServiceRunning** |Ellenőrizze, hogy egy adott HDInsight-szolgáltatás (név) hello számítógépen ahol hello parancsfájl végrehajtása. |
| **Get-HDIHadoopVersion** |Hadoop, ahol hello parancsfájl végrehajtása hello számítógépre telepített verzióját hello beolvasása. |
| **Teszt-IsHDIHeadNode** |Ellenőrizze, hogy hol hello parancsfájl végrehajtása hello számítógép egy átjárócsomóponttal. |
| **Teszt-IsActiveHDIHeadNode** |Ellenőrizze, hogy ahol hello parancsfájl végrehajtása hello számítógép központi aktív csomópontra. |
| **Teszt-IsHDIDataNode** |Ellenőrizze, hogy hol hello parancsfájl végrehajtása hello számítógép adatok csomópont. |
| **Szerkesztés-HDIConfigFile** |Hive-site.xml hello konfigurációs fájlok, a core-site.xml, a hdfs-site.xml, a mapred-site.xml vagy a yarn-site.xml szerkesztéséhez. |

## <a name="best-practices-for-script-development"></a>Parancsfájl fejlesztési ajánlott eljárásai
A HDInsight-fürtök egyéni parancsfájl fejlesztésekor van néhány ajánlott eljárások tookeep figyelembe vételével:

* Hello Hadoop verziójának ellenőrzése

    Csak a HDInsight (Hadoop 2.4) 3.1-es verzióját vagy újabb parancsfájlművelet tooinstall egyéni összetevők használatával olyan fürtön támogatása. Az egyéni parancsfájlt kell használnia hello **Get-HDIHadoopVersion** segítő metódus toocheck hello Hadoop verziója más feladatok végrehajtása a hello parancsfájl folytatása előtt.
* Adja meg a stabil tooscript erőforrások hivatkozásokat tartalmaz.

    Felhasználók győződjön meg arról, hogy az összes hello parancsfájlok és egyéb összetevők hello testreszabási a fürt szerepel hello fürt hello élettartama során elérhetők maradnak, és, hogy a fájlok verzióinak hello ne változtassa meg hello idejére. Ezeket az erőforrásokat szükség, ha hello különösen hello fürtben található csomópontok szükség. hello célszerű toodownload, és minden eleme egy tárfiókot, amely a felhasználói vezérlők hello archiválja. Hello alapértelmezett tárfiókot, illetve hello további tárfiókok hello testreszabott fürt központi telepítés során megadott is lehet.
    Hello a Spark- és R testre szabott fürt minták megadott hello dokumentáció, például hajtottunk hello erőforrások helyi másolatát a tárfiók: https://hdiconfigactions.blob.core.windows.net/.
* Győződjön meg arról, hogy hello fürt testreszabási parancsfájl idempotent

    Várt kell, hogy egy HDInsight-fürt csomópontja hello rendszerképének hello fürt élettartama során. hello fürt testreszabási parancsfájl futtatása minden fürt rendszerképének. Ez a parancsfájl tervezett toobe idempotent hello értelemben, hogy, különösen akkor hello parancsfájl biztosítaniuk kell hello fürt ugyanazt a testreszabott állapotát a után hello parancsfájl lefutott hello az első időpontja hello fürt kezdetben toohello adja vissza kell lennie. létre. Például ha egyéni parancsfájl telepítették az alkalmazást D:\AppLocation első futtassa, és minden későbbi futtatáskor, különösen akkor hello parancsfájl ellenőrizze, hogy hello alkalmazás létezik hello D:\AppLocation hely más a folytatás előtt hello parancsfájlban szereplő lépéseket.
* Egyéni összetevők telepítéséhez hello optimális helyen

    Ha a fürtcsomópontok vannak lemezképet, hello C:\ erőforrás meghajtót és D:\ rendszermeghajtón is újraformázza, adatokat és alkalmazásokat, amelyek adott meghajtókon telepítette hello elvesztését. Ez is fordulhat, ha egy Azure virtuális gép (VM) csomópont hello fürt részét képező leáll, és olyan új csomópont cseréli le. Összetevők telepítheti a D:\ meghajtóra hello vagy hello C:\apps helyen hello fürtön. A C:\ meghajtó hello más helyeken vannak fenntartva. Adja meg a hol találhatók az alkalmazások és a szalagtárak hello fürt testreszabási parancsfájl telepített toobe hello helyét.
* Magas rendelkezésre állásának hello fürt architektúrája

    HDInsight van egy aktív-passzív architektúra a magas rendelkezésre állású, mely egy átjárócsomópont van aktív módot (ahol hello HDInsight szolgáltatások futnak) és más hello a átjárócsomópont (mely a hdinsight szolgáltatások nem futnak) készenléti üzemmódban. hello csomópontok aktív és passzív módban vált, ha a HDInsight szolgáltatások megszakadnak. Ha egy parancsfájlművelet használt tooinstall szolgáltatások mindkét központi csomópont a magas rendelkezésre állású, vegye figyelembe, hogy hello HDInsight feladatátvételi mechanizmus nem tud tooautomatically sikertelen keresztül a felhasználók által telepített szolgáltatások. Ezért felhasználók által telepített szolgáltatásokat, amelyek magas rendelkezésre állású várt toobe átjárócsomópontokkal HDInsight a kell vagy saját feladatátvételi mechanizmus, ha az aktív-passzív módban van, vagy aktív-aktív módban kell.

    Egy HDInsight-parancsfájlművelet parancs mindkét központi csomópontján fut, amikor hello átjárócsomópont szerepkör van megadva értékként hello *ClusterRoleCollection* paraméter. Egyéni parancsfájl tervezésekor ügyeljen, hogy, hogy a telepítő tisztában-e a parancsfájlt. A problémák, ahol hello azonos szolgáltatásokra van telepítve, mindkét hello központi csomópont elindult és azok egymással versengő végül nem kell futtatnia. Is vegye figyelembe, hogy adatok nem vesztek el során, különösen, így parancsfájlművelet keresztül telepített szoftverek toobe rugalmas toosuch események. Alkalmazások sok csomópontok között van elosztva, magas rendelkezésre állású adatokkal tervezett toowork kell lennie. Vegye figyelembe, hogy akár 1/5 hello fürtben található csomópontok a is lemezképet: hello ugyanannyi időt vesz igénybe.
* Hello egyéni összetevők toouse Azure Blob-tároló konfigurálása

    Előfordulhat, hogy az egyéni összetevők hello hello fürtcsomópontokon telepített egy alapértelmezett konfigurációs toouse Hadoop elosztott fájlrendszerrel (HDFS) tároló. Ehelyett meg kell változtatni hello konfigurációs toouse Azure Blob Storage tárolóban. A fürt lemezkép alaphelyzetbe hello HDFS fájlrendszer formázott lekérdezi és elveszítik az ott tárolt adatokat. Azure Blob storage használatával helyette biztosítja, hogy az adatok őrződnek meg.

## <a name="common-usage-patterns"></a>Gyakori használati minták
Ez a szakasz néhány hello gyakori használati szokásokról, amelyek a saját egyéni parancsfájl írásakor mutatjuk be végrehajtási nyújt útmutatást.

### <a name="configure-environment-variables"></a>Környezeti változók konfigurálása
Gyakran a parancsfájl művelet fejlesztési, úgy érzi, hogy hello kell tooset környezeti változókat. Például egy legvalószínűbb forgatókönyv esetén bináris egy külső webhelyről töltheti le, telepítse azt hello fürt és hello helye telepített tooyour "PATH" környezeti változó hozzáadása. a következő kódrészletet hello bemutatja, hogyan tooset környezeti változók hello egyéni parancsfájl.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

A jelen nyilatkozat hello környezeti változó beállítása **MDS_RUNNER_CUSTOM_CLUSTER** toohello értéke "true", és beállítja a változó toobe gépre kiterjedő hatókörének hello. Esetenként fontos, hogy a környezeti változók hello megfelelő hatókörben – számítógép vagy felhasználó van beállítva. Tekintse meg a [Itt] [ 1] környezeti változók beállításával kapcsolatos további információt.

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a>Hozzáférés toolocations hello egyéni parancsfájlok tároló
Használt toocustomize egy fürt igények tooeither hello alapértelmezett tárfiók hello fürt vagy egy nyilvános csak olvasható tároló bármely más tárfiók lennie. Ha a parancsfájl hozzáfér máshol található erőforrások ezeknek kell-e a nyilvánosan elérhető toobe (legalább nyilvános csak olvasható). Például előfordulhat, hogy szeretné, hogy tooaccess egy fájlt, és mentse hello SaveFile-HDI-paranccsal.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

Ebben a példában meg kell győződnie arról hello tároló "somecontainer" tárfiókban "somestorageaccount" nyilvánosan elérhető. Ellenkező esetben hello parancsfájl "Nem található" kivételt okoz, és sikertelen lesz.

### <a name="pass-parameters-toohello-add-azurermhdinsightscriptaction-cmdlet"></a>Paraméterek hozzáadása-AzureRmHDInsightScriptAction toohello parancsmag fázis
toopass több paraméterek toohello Add-AzureRmHDInsightScriptAction parancsmag kell tooformat hello karakterlánc érték toocontain minden paraméter hello parancsfájlt. Példa:

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

vagy

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Sikertelen fürttelepítés kivétel throw
Ha azt szeretné, hogy a tooget pontosan értesítés hello tényt, hogy a fürt testreszabása sikertelen volt a várt módon, fontos toothrow kivétel és hello fürt létrehozása sikertelen. Például előfordulhat, hogy szeretné tooprocess egy fájlt, ha létezik, és kezelni hello hiba esetet, ahol hello fájl nem létezik. Ez lenne győződjön meg arról, hogy hello parancsfájl kilép szabályosan hello fürt hello állapotának megfelelően ismert. hello alábbi részlet egy példán hogyan tooachieve ezt:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

Ezt a kódrészletet a hello fájl nem létezett, ha vezet, ahol hello parancsfájl ténylegesen kilép szabályosan hello hibaüzenet nyomtatás után, és hello fürt eléri futó állapotban, feltéve, hogy a "sikeres" befejeződött a fürt szabásának tooa állapota. Ha azt szeretné, hogy a toobe pontosan értesítés hello tényt, hogy a fürt testreszabási alapvetően nem sikerült egy hiányzó fájlok miatt várt módon, jobban megfelelő toothrow kivétel és hello fürt testreszabási lépés sikertelen lesz. tooachieve ez inkább a következő kódrészlet példa hello kell használnia.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Ellenőrzőlista a üzembe helyezéséhez egy parancsfájlművelet
Az alábbiakban azt tartott, ezek a parancsfájlok toodeploy előkészítésekor hello lépéseket:

1. Hello egyéni parancsfájlok, amely elérhető a hello fürtcsomópontok üzembe helyezése során helyen tartalmazó hello fájlokat helyezze el. Ez lehet bármely hello alapértelmezett vagy a fürtöt tartalmazó környezetben, vagy bármely más nyilvánosan elérhető tároló hello időpontjában megadott további tárfiókok.
2. Adja hozzá a parancsfájlok toomake meg arról, hogy azok végrehajtási idempotently, ellenőrzi, így hello parancsfájl hajtható végre több alkalommal hello ugyanahhoz a csomóponthoz.
3. Használjon hello **Write-Output** Azure PowerShell parancsmag tooprint tooSTDOUT, valamint a stderr-en. Ne használjon **Write-Host**.
4. Ideiglenes mappát, például a $env: TEMP, tookeep hello hello parancsfájlok által használt letöltött fájlt, majd eltávolítással parancsfájlok rendelkezik végrehajtása után.
5. Csak a D:\ vagy C:\apps egyéni szoftver telepítése. Más helyein hello C: meghajtó nem használható, azok le foglalva. Vegye figyelembe, hogy hello C:\apps mappán kívüli hello C: meghajtó a fájlok telepítése azt eredményezheti, a telepítő hibákat reimages hello csomópont alatt.
6. Hello esemény, hogy az operációs rendszer szintű beállításokat vagy Hadoop szolgáltatás konfigurációs fájlok módosultak érdemes lehet toorestart HDInsight szolgáltatások így kiválaszthatja a bármely az operációs rendszer szintű beállításokat, például hello parancsfájlokban beállított hello környezeti változókat.

## <a name="debug-custom-scripts"></a>Egyéni parancsfájlok hibakeresése
hello parancsfájl hibanaplókat más kimeneti hello alapértelmezett tárfiók hello fürt, ahol létrehozták megadott együtt tárolják. hello naplófájljainak tárolása a hello nevű tábla *u < \cluster-name-fragment >< \time-stamp > setuplog*. Ezek a összesített naplófájlokat, amelyek alapján az összes hello csomópontok (átjárócsomópont és munkavégző csomópontokhoz), mely hello a parancsfájl hello fürtben fut a rekordok.
Egyszerűen toocheck hello naplók toouse HDInsight Visual Studio eszközök. Hello eszközök telepítése, lásd: [első lépések a Visual Studio Hadoop tools for HDInsight használatával](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)

**Visual Studio használatával toocheck hello napló**

1. Nyissa meg a Visual Studiót.
2. Kattintson a **nézet**, és kattintson a **Server Explorer**.
3. Kattintson a jobb gombbal az "Azure", kattintson a kapcsolódás túl**Microsoft Azure-előfizetések**, és írja be a hitelesítő adatait.
4. Bontsa ki a **tárolási**, bontsa ki a használt hello alapértelmezett fájlrendszer hello Azure storage-fiók, **táblák**, majd kattintson duplán a hello tábla neve.

Is távoli is hello fürt csomópontjai toosee az STDOUT és az STDERR egyéni parancsfájlok. hello naplók minden egyes csomóponton adott csak toothat csomópont és be vannak jelentkezve **C:\HDInsightLogs\DeploymentAgent.log**. Ezekben a naplófájlokban hello egyéni parancsfájl az összes kimenetének rögzíti. A külső parancsfájl művelet egy példa napló részlet így néz ki:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


Ez a napló az egyszerű, hogy hello Spark parancsfájl művelet lett végrehajtva a hello HEADNODE0 nevű virtuális gép és, hogy a kivételek fordultak elő hello végrehajtása során is.

Hello esemény, amely egy végrehajtási hiba történik ez a naplófájl hello kimeneti leíró azt is található. Ezek a naplók található hello információk is segíthetnek megoldani az esetleg felmerülő problémákat parancsfájl.

## <a name="see-also"></a>Lásd még:
* [Parancsfájlművelet HDInsight-fürtök testreszabása][hdinsight-cluster-customize]
* [Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]
* [Telepítheti és használhatja a HDInsight-fürtök R][hdinsight-r-scripts]
* [Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install.md).
* [Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
