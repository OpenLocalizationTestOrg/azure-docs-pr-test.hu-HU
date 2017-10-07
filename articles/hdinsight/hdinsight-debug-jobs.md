---
title: "A HDInsight Hadoop Debug: naplók megtekintése és hibaüzenetek - Azure értelmezhetők |} Microsoft Docs"
description: "További információk a kaphat, ha a HDInsight a PowerShell és a lépéseket, amelyek toorecover felügyelete hello hibaüzenetek."
services: hdinsight
tags: azure-portal
editor: cgronlun
manager: jhubbard
author: mumian
documentationcenter: 
ms.assetid: 7e6ceb0e-8be8-4911-bc80-20714030a3ad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jgao
ms.openlocfilehash: 2f12a40e9dcac32ce2e11d66d60d8b3b212ecb53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-hdinsight-logs"></a>HDInsight-naplók elemzése
Minden Azure hdinsight Hadoop-fürt rendelkezik egy Azure storage-fiók hello alapértelmezett fájlrendszert használja. hello tárfiók hello alapértelmezett tárfiók kezeli. Fürt hello Azure Table storage és használja a Blob storage hello hello alapértelmezett tárolási fiók toostore naplók.  toofind hello alapértelmezett tárfiókot, a fürt számára, lásd: [Hadoop kezelése a HDInsight-fürtök](hdinsight-administer-use-management-portal.md#find-the-default-storage-account). hello naplók tartsa meg a tárfiók hello hello fürtök törlése után is.

## <a name="logs-written-tooazure-tables"></a>A naplók tooAzure táblák írása
hello tooAzure táblák írt naplók nyújtanak betekintést HDInsight-fürtöt, mi történik egy szintjének.

Amikor HDInsight fürtöt hoz létre, a 6 táblázatok automatikusan jönnek létre Linux-alapú fürtök hello alapértelmezett Table storage-ban:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

3 táblázatok jönnek létre a Windows-alapú fürtök:

* Setuplog: kiépítés/beállításához a HDInsight-fürtök észlelt események/kivételek napló.
* hadoopinstalllog: események/kivételek észlelt, amikor a Hadoop telepítését hello fürt naplóját. Ez a táblázat hasznosak lehetnek a hibakeresés kapcsolatos egyéni paraméterekkel létrehozott tooclusters.
* hadoopservicelog: összes Hadoop-szolgáltatás által rögzített események/kivételek naplóját. Ez a táblázat hasznosak lehetnek a problémák kapcsolódó toojob hibakeresésére a HDInsight-fürtökön.

hello tábla fájlnevek **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Ezek a táblázatok a következő mezők hello tartalmazza:

* ClusterDnsName
* Most letölthető a KomponensNév
* eventTimestamp
* Gazdagép
* MALoggingHash
* Üzenet
* N
* PreciseTimeStamp
* Szerepkör
* A RowIndex
* Bérlői
* IDŐBÉLYEG
* A TraceLevel

### <a name="tools-for-accessing-hello-logs"></a>Eszközök hello naplók elérése
Számos eszköz áll rendelkezésre egyes táblák adatait:

* Visual Studio
* Azure Storage Explorer
* A Power Query az Excel programhoz

#### <a name="use-power-query-for-excel"></a>Használja a Power Query az Excel programhoz
A Power Query telepíthető [www.microsoft.com/en-us/download/details.aspx?id=39379](http://www.microsoft.com/en-us/download/details.aspx?id=39379). Hello letöltési oldalát hello rendszerkövetelményeket lásd:

**toouse Power Query tooopen és hello szolgáltatás bejelentkezési elemzése**

1. Nyissa meg **Microsoft Excel**.
2. A hello **Power Query** menüben kattintson a **az Azure**, és kattintson a **a Microsoft Azure Table storage**.
   
    ![HDInsight Hadoop Excel PowerQuery nyissa meg az Azure Table storage](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Adja meg a hello tárfiók neve. Ez lehet hello rövid név vagy hello FQDN.
4. Adja meg a hello tárfiók kulcsa. A táblák listáját kell jelenik meg:
   
    ![Azure Table storage-ban tárolt HDInsight Hadoop-naplói](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Kattintson a jobb gombbal hello hadoopservicelog táblázat hello **Navigator** ablaktáblán, és válassza **szerkesztése**. 4 oszlop megjelenik. Szükség esetén törli az hello **partíciókulcs**, **Sorkulcsa**, és **időbélyeg** oszlopok kiválasztva, majd **oszlopok eltávolítása** hello kapcsolók hello szalagon.
6. Kattintson a hello bontsa ki a hello ikonra oszlop toochoose hello oszlopok tooimport hello Excel-számolótábla a kívánt tartalmat. Az ebben a bemutatóban elfogadása TraceLevel, és most letölthető a KomponensNév: azt biztosíthat me néhány alapvető információ, amelyre összetevők kellett problémákat.
   
    ![HDInsight Hadoop-naplói oszlopok kiválasztása](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Kattintson a **OK** tooimport hello adatokat.
8. Jelölje be hello **TraceLevel**, szerepkör, és **most letölthető a KomponensNév** oszlopok, és kattintson **Group By** vezérlő hello szalagon.
9. Kattintson a **OK** hello Group By párbeszédpanel
10. Kattintson ** alkalmazása & Bezárás **.

Most már használhatja Excel toofilter és rendezési szükség szerint. Természetesen előfordulhat, hogy a kívánt tooinclude rendelés toodrill le más oszlopok (pl. üzenet) kapcsolatos problémák jelentkeznek, de kiválasztásával, és a fent leírt hello oszlopok csoportosítási biztosít a Hadoop-szolgáltatás, mi történik egy decent képe. hello azonos ötlet lehet alkalmazott toohello setuplog és hadoopinstalllog tábla.

#### <a name="use-visual-studio"></a>A Visual Studio használata
**Visual Studio toouse**

1. Nyissa meg a Visual Studiót.
2. A hello **nézet** menüben kattintson a **Cloud Explorer**. Vagy egyszerűen kattintson **CTRL +\, CTRL + X**.
3. A **Cloud Explorer**, jelölje be **erőforrástípusok**.  hello más elérhető lehetőség az **erőforráscsoportok**.
4. Bontsa ki a **Tárfiókok**, hello alapértelmezett tárfiókot a fürt számára, majd **táblák**.
5. Kattintson duplán a **hadoopservicelog**.
6. Adjon hozzá egy szűrőt. Példa:
   
        TraceLevel eq 'ERROR'
   
    ![HDInsight Hadoop-naplói oszlopok kiválasztása](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Szűrők létrehozásával kapcsolatos további információkért lásd: [Szűrőkarakterláncokban összeállításához hello tábla Designer](../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-tooazure-blob-storage"></a>Naplók Written tooAzure Blob-tároló
[hello naplózza az írásbeli tooAzure táblák](#log-written-to-azure-tables) betekintést HDInsight-fürtöt, mi történik egy szintjének biztosítására. Azonban ezek a táblázatok nem biztosítanak tevékenység szintű naplófájlokban, lehet megjeleníthetők, tovább kapcsolatos hibák elhárításának előfordulásuk időpontjában naplózhatja őket. tooprovide a következő részletességi, HDInsight-fürtök konfigurált toowrite feladat naplók tooyour Blob Storage-fiók lépni a Templeton keresztül elküldött feladathoz szükséges. Gyakorlatilag ez azt jelenti, hogy az hello Microsoft Azure PowerShell-parancsmagjaival vagy hello .NET feladat elküldése API-k, nem az RDP/parancssor-line hozzáférés toohello fürt keresztül feladatok feladat. 

tooview hello naplókat, lásd: [Linux-alapú HDInsight bejelentkezik hozzáférést YARN alkalmazás](hdinsight-hadoop-access-yarn-app-logs-linux.md).

Az alkalmazás naplóiban kapcsolatos további információkért lásd: [egyszerűsítése a felhasználó-naplók kezelése és hozzáférés a YARN](http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).

## <a name="view-cluster-health-and-job-logs"></a>Fürt állapotának és a feladat naplók megtekintése
### <a name="access-hadoop-ui"></a>Hozzáférés Hadoop felhasználói felület
Hello Azure-portálon kattintson egy HDInsight fürt neve tooopen hello fürt paneljén. Hello-fürt panelén kattintson **irányítópult**.

![Indítsa el a fürt-irányítópult](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard.png)

Amikor a rendszer kéri, adja meg a hello fürt rendszergazdai hitelesítő adatokat. Hello megnyíló lekérdezés konzol, kattintson **Hadoop felhasználói felület**.

![Indítsa el a Hadoop felhasználói felület](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard-hadoop-ui.png)

### <a name="access-hello-yarn-ui"></a>Hozzáférés hello Yarn felhasználói felületen
Hello Azure-portálon kattintson egy HDInsight fürt neve tooopen hello fürt paneljén. Hello-fürt panelén kattintson **irányítópult**. Amikor a rendszer kéri, adja meg a hello fürt rendszergazdai hitelesítő adatokat. Hello megnyíló lekérdezés konzol, kattintson **YARN felhasználói felületen**.

Hello YARN felhasználói felületen toodo hello következő használható:

* **Fürt állapotának beolvasása**. Hello bal oldali ablaktáblán, bontsa ki a **fürt**, és kattintson a **kapcsolatos**. A jelen fürt állapot adatait, például teljes kiosztott memória, a használt, magok hello fürt erőforrás-kezelő állapotának, a fürt verzió stb.
  
    ![Indítsa el a fürt-irányítópult](./media/hdinsight-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Csomópont állapota**. Hello bal oldali ablaktáblán, bontsa ki a **fürt**, és kattintson a **csomópontok**. A parancs megjeleníti az összes hello csomópontján hello fürt, a HTTP-cím minden csomópont, erőforrások lefoglalt tooeach csomópont, stb.
* **Feladat állapotának figyelése**. Hello bal oldali ablaktáblán, bontsa ki a **fürt**, és kattintson a **alkalmazások** toolist összes hello hello fürt feladatok. Ha azt szeretné, hogy toolook, egy adott feladatok állapota (például az új, elküldött, futó, stb.), kattintson a hello megfelelő hivatkozásra a **alkalmazások**. További kattinthat hello feladat neve toofind további információk a hello feladat – ilyen például a hello kimeneti, naplói, stb.

### <a name="access-hello-hbase-ui"></a>Hozzáférés hello HBase felhasználói felület
Hello Azure-portálon kattintson egy HDInsight HBase fürt neve tooopen hello fürt paneljén. Hello-fürt panelén kattintson **irányítópult**. Amikor a rendszer kéri, adja meg a hello fürt rendszergazdai hitelesítő adatokat. Hello megnyíló lekérdezés konzol, kattintson **HBase felhasználói felület**.

## <a name="hdinsight-error-codes"></a>HDInsight hibakódok
hello ebben a szakaszban részletezett hibaüzenetek biztosított toohelp hello Azure hdinsight hadoop tisztában legyenek azzal, hogy is hello szolgáltatást Azure PowerShell és tooadvise felügyeletekor lehetséges hibaállapotok hello lépéseket őket amelyen átvihető toorecover hello hiba.

Bizonyos hibaüzenetek sikerült is látható hello Azure-portálon használt toomanage a HDInsight-fürtök esetén. Egyéb hibaüzeneteit, akkor léphetnek fel, de kevésbé részletes, ebben a környezetben lehetséges javító műveleteket hello toohello korlátai miatt van. Egyéb hibaüzeneteit, ahol hello megoldás az nyilvánvaló hello környezetek szerepelnek. 

### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
* **Leírás**: Adja meg Azure SQL adatbázis adatainak megadása az order toouse legalább egy összetevő struktúra és az Oozie metastores egyéni beállításait.
* **Megoldás**: hello felhasználónak toosupply SQL Azure metaadattárhoz, és próbálkozzon újra hello kérés van szüksége.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Leírás**: nem hozható létre fürt a régióban *nameOfYourRegion*. Egy érvényes HDInsight-régió, és ismételje meg a kérelmet.
* **Megoldás**: ügyfél által jelenleg támogatott hello fürt régióban kell létrehoznia: Délkelet-Ázsiában, Nyugat-Európában, Észak-Európa, USA keleti régiója vagy USA nyugati régiója.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Leírás**: hello kiszolgáló nem találta a hello kért fürt rekord.  
* **Megoldás**: újra hello műveletet.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Leírás**: fürt DNS-név *yourDnsName* érvénytelen. Győződjön meg arról, neve kezdődik alfanumerikus végződik, és csak "-" speciális karakter  
* **Megoldás**: Győződjön meg arról, hogy egy érvényes DNS-nevet használta, az a fürt és végződik alfanumerikus és tartalmaz semmilyen különleges karakterek hello kötőjel nem "-", és próbálkozzon újra a művelettel hello.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Leírás**: fürtnév *yourClusterName* nem érhető el. Válasszon másik nevet.  
* **Megoldás**: hello felhasználói adjon meg egy egyedi fürtnév, és nem létezik és próbálja meg újra. Hello felhasználó hello Portal használ, ha a felhasználói felület fog értesítést küldhet nekik, ha a fürt neve már használatban van során hello hello létrehozása lépéseket.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Leírás**: fürt jelszó érvénytelen. Jelszó legalább 10 karakter hosszúságúnak kell lennie, és tartalmaznia kell legalább egy számot, nagybetű, kisbetű és speciális karaktert, szóközök nélkül, és azt részeként hello felhasználónév nem tartalmazhat.  
* **Megoldás**: Adjon meg egy érvényes fürttel jelszót, majd próbálja megismételni a műveletet hello.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Leírás**: fürt felhasználónév érvénytelen. Győződjön meg arról, felhasználónév nem állhat, különleges karaktereket vagy a szóközt.  
* **Megoldás**: Adjon meg egy érvényes fürttel felhasználónévvel, és ismételje meg hello műveletet.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Leírás**: fürt DNS-név *yourDnsClusterName* érvénytelen. Győződjön meg arról, neve kezdődik alfanumerikus végződik, és csak "-" speciális karakter  
* **Megoldás**: Adjon meg egy érvényes DNS-fürt felhasználónevet, majd próbálja megismételni a műveletet hello.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Leírás**: az URI a tároló neve *yourcontainerURI* és a DNS-név *yourDnsName* kérelem törzse kell lennie hello azonos.  
* **Megoldás**: Ellenőrizze, hogy a tároló neve és a DNS-neve azonos hello, majd próbálja megismételni a műveletet hello.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Leírás**: Érvénytelen fürtkonfiguráció. Nem lehet toofind bármely adatmeghatározások csomópont csomópont-méretet.  
* **Megoldás**: újra hello műveletet.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Leírás**: hello fürt esetében nem sikerült törölni a központi telepítés  
* **Megoldás**: ismételje meg hello törlési műveletet.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Leírás**: Hiba történt a konfiguráció szolgáltatás. Szükséges DNS-hozzárendelése információ nem található.  
* **Megoldás**: törli a fürtöt, és hozzon létre egy új fürtöt.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Leírás**: duplikált fürt tároló létrehozása kísérlet. Létezik rekord *nameOfYourContainer* de ETag-EK nem egyeznek.
* **Megoldás**: Adjon meg egy egyedi nevet, a hello tároló és az újrapróbálkozási hello létrehozási műveletet.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Leírás**: üzemeltetett szolgáltatás *nameOfYourHostedService* már tartalmaz egy fürt. Üzemeltetett szolgáltatás nem tartalmazhat több fürt  
* **Megoldás**: hello gazdagépfürt egy másik üzemeltetett szolgáltatásban.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Leírás**: hello kiszolgáló nem tudta frissíteni a hello hello fürt központi telepítésének állapotát.  
* **Megoldás**: újra hello műveletet. Ilyen esetben többször forduljon az.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Leírás**: fürt *yourClusterName* karbantartás részeként törölve lett. Hozza létre hello fürt.
* **Megoldás**: hello fürt hozza létre újra.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Leírás**: Érvénytelen fürtkonfiguráció. Nem található a csomópont méretű központi csomópont-konfiguráció szükséges.
* **Megoldás**: újra hello műveletet.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Leírás**: nem toocreate üzemeltetett szolgáltatás *nameOfYourHostedService*. Próbálja meg újra a kérelmet.  
* **Megoldás**: hello ismételje meg a kérést.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Leírás**: üzemeltetett szolgáltatás *nameOfYourHostedService* már az éles környezet. Üzemeltetett szolgáltatás nem tartalmazhat több éles központi telepítését. Próbálkozzon újra egy másik fürtnevet hello kérelmet.
* **Megoldás**: egy másik fürtnevet, és ismételje meg a hello kérelem.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Leírás**: üzemeltetett szolgáltatás *nameOfYourHostedService* hello fürt nem található.  
* **Megoldás**: Ha hello fürt hiba állapotban, törölje azt, és próbálkozzon újra.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Leírás**: üzemeltetett szolgáltatás *nameOfYourHostedService* nincs társított központi telepítéshez van.  
* **Megoldás**: Ha hello fürt hiba állapotban, törölje azt, és próbálkozzon újra.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Leírás**: hello SubscriptionId *yourSubscriptionId* nincs magok bal oldali toocreate fürt *yourClusterName*. Kötelező: *resourcesRequired*, elérhető: *resourcesAvailable*.  
* **Megoldás**: Szabadítson fel erőforrást az előfizetésében vagy hello erőforrások elérhető toohello előfizetés növelése, majd próbálja meg újból toocreate hello fürt.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Leírás**: előfizetés-azonosító *yourSubscriptionId* nem rendelkezik egy új üzemeltetett toocreate fürt kvótája *yourClusterName*.  
* **Megoldás**: Szabadítson fel erőforrást az előfizetésében vagy hello erőforrások elérhető toohello előfizetés növelése, majd próbálja meg újból toocreate hello fürt.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Leírás**: hello kiszolgáló belső hibába ütközött. Próbálja meg újra a kérelmet.  
* **Megoldás**: hello ismételje meg a kérést.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Leírás**: Azure tárolási helye *dataRegionName* már nem egy érvényes helyet. Ellenőrizze, hogy megfelelő-e a hello régiót, majd próbálja megismételni a kérelmet.
* **Megoldás**: Válasszon tárolási helyet, amely támogatja a HDInsight, ellenőrizze, hogy a fürt ugyanott-e, majd próbálja megismételni hello műveletet.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Leírás**: adatcsomópontokat érvénytelen Virtuálisgép-méretet. Csak a "Nagy méretű" mérete csomópontjaihoz adatok esetén támogatott.  
* **Megoldás**: Adja meg a hello adatcsomóponton hello támogatott csomópont méretét, majd próbálja megismételni a műveletet hello.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Leírás**: átjárócsomópont érvénytelen Virtuálisgép-méretet. Átjárócsomópont használható csak a "ExtraLarge VM" méret.  
* **Megoldás**: Adja meg a támogatott hello csomópont méretét hello átjárócsomópont, majd próbálja megismételni a műveletet hello

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Leírás**: előfizetés-azonosító *yourSubscriptionId* használt nem rendelkezik elegendő engedélyekkel tooexecute törlési művelet fürt *yourClusterName*.  
* **Megoldás**: Ha hello fürt hibaállapotban van, dobja el, és próbálkozzon újra.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Leírás**: külső tárfiók a blob tároló neve *yourContainerName* érvénytelen. Győződjön meg arról, nevének betűvel kezdődik, és csak kisbetűket, számokat és kötőjelet tartalmazza.  
* **Megoldás**: Adjon meg egy érvényes tárfiók a blob tároló neve, majd próbálja megismételni a műveletet hello.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Leírás**: külső Tárkonfigurációt *yourStorageAccountName* van szükség toohave titkos kulcs adatai toobe beállítása.  
* **Megoldás**: Adjon meg egy érvényes titkos kulcs hello tárfiók, majd próbálja megismételni a műveletet hello.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Leírás**: Version fejlécnek *yourVersionHeader* nincs érvényes formátuma éééé-hh-nn.  
* **Megoldás**: Adjon meg egy érvényes formátumú hello-verziófejléc, majd próbálja megismételni a hello kérelem.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Leírás**: Érvénytelen fürtkonfiguráció. Egynél több központi csomópont-konfiguráció található.  
* **Megoldás**: Szerkesztés hello konfigurációs úgy, hogy csak egy központi csomópont van megadva.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Leírás**: hello művelet nem sikerült belül engedélyezett idő hello vagy hello újrapróbálkozási kísérletek lehetséges. Próbálja meg újra a kérelmet.  
* **Megoldás**: hello ismételje meg a kérést.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Leírás**: paraméter *yourParameterName* nem lehet null vagy üres.  
* **Megoldás**: hello paraméterhez érvényes értéket adjon meg.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Leírás**: egy vagy több hello fürt létrehozási kérelem bemenet érvénytelen. Ellenőrizze, hogy a bemeneti értékek hello helyes-e, majd próbálja megismételni a kérelmet.  
* **Megoldás**: Ellenőrizze, hogy a bemeneti értékek hello helyes-e, majd próbálja megismételni a kérelmet.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Leírás**: régió funkció nem érhető el a régióhoz *yourRegionName* és előfizetés-azonosító *yourSubscriptionId*.  
* **Megoldás**: Adja meg, amely támogatja a HDInsight-fürtök egy régiót. hello nyilvánosan támogatott régiók a következők: Délkelet-Ázsiában, Nyugat-Európában, Észak-Európa, USA keleti régiója vagy USA nyugati régiója.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Leírás**: tárfiók *yourStorageAccountName* régióban *currentRegionName*. Hello fürt régió megegyezőnek kell lennie *yourClusterRegionName*.  
* **Megoldás**: Adjon meg egy tárfiókot a hello ugyanabban a régióban, amely a fürt, vagy ha az adatok már hello tárfiókja, hozzon létre egy új fürt hello hello meglévő tárfiókot és ugyanabban a régióban. Hello portált használja, ha felhasználói felület hello fog értesítést küldhet nekik ennek a problémának előre.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Leírás**: megadott előfizetés-azonosító *yourSubscriptionId* addig nem aktív.  
* **Megoldás**: az előfizetés aktiválásához újra, vagy szerezzen be új érvényes előfizetést.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Leírás**: előfizetés-azonosító *yourSubscriptionId* nem található.  
* **Megoldás**: Ellenőrizze, hogy az előfizetés-azonosító érvényes, majd próbálja megismételni a műveletet hello.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Leírás**: nem tooresolve DNS *yourDnsUrl*. Ellenőrizze, hogy teljesen minősített hello URL-cím hello blob végpont nyújtanak az.  
* **Megoldás**: Adjon meg egy érvényes blob URL-címet. URL-címet kell hello kell teljesen érvényes, beleértve a kezdve *http://* , és oda *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Leírás**: erőforrás helye nem tooverify *yourDnsUrl*. Ellenőrizze, hogy teljesen minősített hello URL-cím hello blob végpont nyújtanak az.  
* **Megoldás**: Adjon meg egy érvényes blob URL-címet. URL-címet kell hello kell teljesen érvényes, beleértve a kezdve *http://* , és oda *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Leírás**: verzió funkció nem érhető el a verzió *specifiedVersion* és előfizetés-azonosító *yourSubscriptionId*.  
* **Megoldás**: válassza a rendelkezésre álló, majd próbálja megismételni a műveletet hello.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Leírás**: verzió *specifiedVersion* nem támogatott.
* **Megoldás**: Válasszon egy támogatott verzióra, majd próbálja megismételni hello műveletet.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Leírás**: verzió *specifiedVersion* nem áll rendelkezésre az Azure-régió *specifiedRegion*.  
* **Megoldás**: Válasszon egy támogatott verziót határoz meg hello régióban, majd próbálja megismételni a műveletet hello.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Leírás**: Érvénytelen fürtkonfiguráció. Szükséges WASB fiók konfigurációja nem található a külső fiókok.  
* **Megoldás**: Győződjön meg arról, hogy hello fiók létezik-e, és megfelelően van-e megadva hello műveletben konfigurációs, és próbálkozzon újra.

## <a name="next-steps"></a>Következő lépések
* [Az Ambari nézetek toodebug Tez feladatokhoz a HDInsight használata](hdinsight-debug-ambari-tez-view.md)
* [Halommemória memóriaképek a Linux-alapú HDInsight Hadoop-szolgáltatások engedélyezése](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [A HDInsight-fürtök kezelése hello Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md)

