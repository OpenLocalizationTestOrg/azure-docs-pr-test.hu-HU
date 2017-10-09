---
title: "a Linux-alapú HDInsight - Azure Hadoop használatának aaaTips |} Microsoft Docs"
description: "Megvalósítási tippek a Linux-alapú HDInsight (Hadoop) fürtök használata a hello Azure felhőben futó ismerős Linux-környezeten."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: a555622605079c9beae88ece872042e36d540c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="information-about-using-hdinsight-on-linux"></a>Információ a HDInsight használata Linux rendszeren

Az Azure HDInsight-fürtök ismerős Linux környezetben, hello Azure felhőben futó Hadoop szolgálnak. A legtöbb feladat akkor működnek, pontosan a másik Hadoop a Linux-telepítés. Ez a dokumentum meghívja a kimenő, meg kell ismernie a speciális eltéréseket.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Előfeltételek

A következő segédprogramot, amelyeket esetleg a rendszerre telepített toobe hello hello lépések ebben a dokumentumban használja.

* [cURL](https://curl.haxx.se/) -toocommunicate használt webes szolgáltatások
* [jq](https://stedolan.github.io/jq/) -használt tooparse JSON-dokumentumok
* [Az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (előzetes verzió) – használt tooremotely kezelése az Azure-szolgáltatások

## <a name="users"></a>Felhasználók

Ha [tartományhoz](hdinsight-domain-joined-introduction.md), érdemes figyelembe venni a HDInsight egy **egyfelhasználós** rendszer. Egy SSH-felhasználói fiók rendszergazdai szintű engedélyekkel rendelkező hello fürt hozza létre. További SSH-fiókok hozhatók létre, de a rendszergazda hozzáférést toohello fürt is rendelkeznek.

Tartományhoz csatlakozó HDInsight támogatja egyszerre több felhasználó és részletesebb engedéllyel és szerepkör-beállítások. További információkért lásd: [kezelése tartományhoz a HDInsight-fürtök](hdinsight-domain-joined-manage.md).

## <a name="domain-names"></a>Tartománynevek

hello teljesen minősített tartomány neve (FQDN) toouse, az Internet hello toohello fürt kapcsolódáskor  **&lt;clustername >. azurehdinsight.net** vagy (az SSH csak)  **&lt;fürtnév-ssh >. azurehdinsight.net**.

Belső hello fürt minden csomópontja rendelkezik a nevet, amely hozzá van rendelve, fürt konfigurálása során. toofind hello fürtnevek, lásd: hello **állomások** oldalon, hello Ambari webes felhasználói felületén. Hello tooreturn hello Ambari REST API-állomások listáját a következő is használhatja:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Cserélje le **jelszó** hello jelszóval hello rendszergazdai fiók, és **CLUSTERNAME** hello néven a fürt. Ez a parancs visszaadja a hello fürt állomásai között hello listáját tartalmazó JSON-dokumentumból. Jq használt tooextract hello `host_name` elemérték minden állomás számára.

Ha egy adott szolgáltatáshoz hello csomópont toofind hello neve, az adott összetevő lekérheti Ambari. Például toofind hello állomások hello HDFS neve csomópont, használja a következő parancs hello:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Ez a parancs visszaadja a hello szolgáltatás leíró JSON-dokumentum, és majd jq kérjen-e kimenő csak hello `host_name` hello gazdagépek érték.

## <a name="remote-access-tooservices"></a>Távelérés tooservices

* **Ambari (webalkalmazás)** -https://&lt;clustername >. azurehdinsight.net

    Hello fürt rendszergazda felhasználó és jelszó használatával hitelesíteni, és ezután a tooAmbari.

    Hitelesítés egyszerű szövegként – mindig használjon HTTPS toohelp győződjön meg arról, hogy hello kapcsolat biztonságos.

    > [!IMPORTANT]
    > Néhány hello web UI egy belső tartománynév használatával Ambari hozzáférés csomóponton keresztül érhető el. Belső tartományban nevek nincsenek keresztül nyilvánosan elérhető internetes hello. "A kiszolgáló nem található" hibák léphetnek, közben tooaccess néhány funkció hello interneten keresztül.
    >
    > toouse hello összes funkciójának hello Ambari webes felhasználói felület, egy SSH alagút tooproxy webes forgalom toohello átjárócsomóponthoz használja. Lásd: [használata SSH Tunneling tooaccess Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie és egyéb web UI](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;clustername >.azurehdinsight.net/ambari

    > [!NOTE]
    > Hello fürt rendszergazda felhasználó és jelszó használatával hitelesíteni.
    >
    > Hitelesítés egyszerű szövegként – mindig használjon HTTPS toohelp győződjön meg arról, hogy hello kapcsolat biztonságos.

* **WebHCat (Templeton)** -https://&lt;clustername >.azurehdinsight.net/templeton

    > [!NOTE]
    > Hello fürt rendszergazda felhasználó és jelszó használatával hitelesíteni.
    >
    > Hitelesítés egyszerű szövegként – mindig használjon HTTPS toohelp győződjön meg arról, hogy hello kapcsolat biztonságos.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net 22-es vagy 23-porton. 22-es port foglalt tooconnect toohello elsődleges headnode, addig, amíg 23 használt tooconnect toohello másodlagos. Hello átjárócsomópontokkal további információkért lásd: [rendelkezésre állásának és megbízhatóságának hadoop clusters in HDInsight](hdinsight-high-availability-linux.md).

    > [!NOTE]
    > Csak elérhető hello központi fürtcsomópontok SSH-n keresztül egy ügyfélszámítógépre. Miután csatlakozott, majd hozzáférhet hello munkavégző csomópontokhoz egy headnode SSH használatával.

## <a name="file-locations"></a>Fájlhelyek

Hadoop kapcsolatos fájlok találhatók fürtcsomópontokon hello, `/usr/hdp`. Ez a könyvtár a következő alkönyvtárak hello tartalmazza:

* **2.2.4.9-1**: hello könyvtárnév hello Hortonworks Data Platform HDInsight által használt hello verziója telepítve. hello szám a fürt egyik az itt felsorolt hello eltérő lehet.
* **aktuális**: Ez a könyvtár tartalmaz hivatkozásokat toosubdirectories hello alatt **2.2.4.9-1** könyvtár. Ez a könyvtár létezik, így nem kell tooremember hello verziószáma.

Példa adatok és a JAR-fájlok megtalálhatók a Hadoop elosztott fájlrendszer, `/example` és`/HdiSamples`

## <a name="hdfs-azure-storage-and-data-lake-store"></a>HDFS, az Azure Storage és a Data Lake Store

A legtöbb Hadoop azokat a terjesztéseket, a HDFS biztonsági helyi tároló hello fürt hello gépeken. A helyi tároló költséges lehet egy felhőalapú megoldás hol van szó, óránként vagy számítási erőforrások percenként.

A HDInsight az Azure Storage blobs, vagy az Azure Data Lake Store hello alapértelmezett tároló használja. Ezek a szolgáltatások hello a következő előnyöket biztosítják:

* Olcsó hosszú távú tárolás
* Kisegítő lehetőségek külső szolgáltatásokból, például a webhelyek, a fájl feltöltése/letöltés segédprogramok, a különböző nyelvi SDK-k és a böngészők

> [!WARNING]
> Csak a HDInsight támogatja __általános célú__ Azure Storage-fiókokat. Jelenleg nem támogatja hello __Blob-tároló__ fiók típusa.

Egy Azure Storage-fiók mentése too4.75 TB, tárolható, bár egyes blobok (vagy a HDInsight szempontból fájl) csak lépjen be too195 GB. Azure Data Lake Store milyen mértékben növelhető dinamikusan toohold trillions-fájlok nagyobb, mint egy petabájtnyi egyedi fájlokkal. További információkért lásd: [ismertetése blobok](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) és [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).

Azure Storage vagy a Data Lake Store használatakor toodo semmi sincs különleges HDInsight tooaccess hello adatokból. Például a következő parancs hello felsorolja hello fájlok `/example/data` függetlenül attól, hogy tárolása Azure Storage vagy a Data Lake Store mappába:

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a>URI és a rendszer

Néhány parancsok igényelheti toospecify hello séma hello URI részeként fájl elérésekor. Például hello Storm-HDFS összetevő az toospecify hello séma szükséges. Ha a nem alapértelmezett tároló ("tárhely" toohello fürtként hozzáadott tárhely), mindig kell használnia hello séma hello URI részeként.

Használata esetén __Azure Storage__, használja a következő URI-séma hello egyikét:

* `wasb:///`: A hozzáférés alapértelmezett tárolási titkosítatlan kommunikáció használata.

* `wasbs:///`: A hozzáférés alapértelmezett tárolási titkosított kommunikáció használata.  hello wasbs séma csak a HDInsight 3.6 verzió és újabb verziók esetében támogatott.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: A nem alapértelmezett tárfiók való kommunikáció során használt. Például, ha rendelkezik egy további storage-fiókot, vagy amikor tárolt adatok elérése a nyilvánosan elérhető tárfiók.

Használata esetén __Data Lake Store__, használja a következő URI-séma hello egyikét:

* `adl:///`: Hello alapértelmezett Data Lake Store hello fürt eléréséhez.

* `adl://<storage-name>.azuredatalakestore.net/`: Egy nem alapértelmezett Data Lake Store való kommunikáció során használt. A HDInsight-fürt hello gyökérkönyvtárában kívül tooaccess adatok is használhatók.

> [!IMPORTANT]
> Data Lake Store használata a HDInsight hello alapértelmezett tárolóként, meg kell adnia egy elérési utat hello tároló toouse belül hello legfelső szintű HDInsight tároló. hello alapértelmezett elérési út `/clusters/<cluster-name>/`.
>
> Használata esetén `/` vagy `adl:///` tooaccess adatokat, csak az adatok eléréséhez hello legfelső szintű tárolja (például `/clusters/<cluster-name>/`) hello fürt. bárhol hello tároló tooaccess adatok használják hello `adl://<storage-name>.azuredatalakestore.net/` formátumban.

### <a name="what-storage-is-hello-cluster-using"></a>Milyen tárolási hello fürt használ

Ambari tooretrieve hello alapértelmezett tárolási konfigurációt használható hello fürtökhöz. Használjon hello következő tooretrieve HDFS konfigurációs információit curl parancsot, és szűrés használatával [jq](https://stedolan.github.io/jq/):

```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> Ez visszaad hello első alkalmazott konfiguráció toohello kiszolgáló (`service_config_version=1`), amely tartalmazza ezt az információt. Szükség lehet toolist toofind hello legújabb összes konfigurációs verzió.

Ez a parancs visszaadja a hasonló toohello érték, a következő URI-azonosítók:

* `wasb://<container-name>@<account-name>.blob.core.windows.net`Ha egy Azure Storage-fiók használatával.

    hello fióknév hello hello Azure Storage-fiók nevét, a hello tároló neve pedig hello blob tároló, amely hello fürttároló hello gyökérmappájában.

* `adl://home`Ha az Azure Data Lake Store használatára. tooget hello Data Lake Store nevét, a REST-hívást a következő hello használata:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    Ez a parancs visszaadja a következő állomásnév hello: `<data-lake-store-account-name>.azuredatalakestore.net`.

    tooget hello directory hello tárolóban, amely a HDInsight, REST-hívást a következő használatát hello hello gyökér:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    Ez a parancs visszaadja egy elérési utat a következő elérési út hasonló toohello: `/clusters/<hdinsight-cluster-name>/`.

Hello tárolással kapcsolatos lépések hello segítségével hello Azure-portál használatával is találhat:

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki a HDInsight-fürthöz.

2. A hello **tulajdonságok** szakaszban jelölje be **Tárfiókok**. hello tárolással kapcsolatos hello fürt jelenik meg.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Hogyan érhetem el fájlokat a külső HDInsight-ból

A különböző módon is külső hello HDInsight-fürt tooaccess adatait. hello az alábbiakban néhány hivatkozások tooutilities és SDK-k, amelyek az adatok használt toowork lehetnek:

Ha használ __Azure Storage__, tekintse meg a következő módon, hogy az adatok elérhető hivatkozások hello:

* [Az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): használata az Azure parancssori felület parancsai. Miután telepítette, használja a hello `az storage` parancsot a Súgó a tárolót, vagy `az storage blob` a blob-specifikus parancsok.
* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): A python parancsfájl az Azure Storage blobs használata a.
* Különböző SDK-k:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Storage REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Ha használ __Azure Data Lake Store__, tekintse meg a következő módon, hogy az adatok elérhető hivatkozások hello:

* [Webböngésző](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI 2.0](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS REST API-n](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [A Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>A fürt méretezése

hello fürt skálázás funkció lehetővé teszi toodynamically módosítás hello fürt által használt adatok csomópontok száma. Méretezési műveleteket közben egyéb feladatokat hajthat végre, vagy egy fürtön folyamatok futnak.

hello különböző fürttípussal érhető érinti skálázás az alábbiak szerint:

* **Hadoop**: skálázás le hello fürtben található csomópontok számát, ha néhány hello fürt hello szolgáltatás újraindul. Műveletek skálázás feladatok eredményezheti fut, vagy függőben lévő művelet skálázás hello hello megvalósításának következő toofail. Hello feladatok is újraküldése, hello művelet végrehajtása után.
* **A HBase**: területi kiszolgálók automatikusan elosztott terhelésű hello skálázás művelet befejezése után néhány percen belül. toomanually egyenleg területi kiszolgálók, a lépéseket követve hello használata:

    1. Csatlakozás toohello HDInsight-fürthöz SSH használatával. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

    2. A következő toostart hello HBase rendszerhéj hello használata:

            hbase shell

    3. Amikor hello HBase rendszerhéj be van töltve, használja a következő toomanually egyenleg hello területi kiszolgálók hello:

            balancer

* **A Storm**: minden futó Storm-topológiák kell egyensúlyba, a méretezési művelet végrehajtását követően. Újraelosztás lehetővé teszi, hogy a hello topológia tooreadjust párhuzamossági beállítások hello új hello fürtben lévő csomópontok száma alapján. toorebalance futó topológiákat, használja az alábbi beállítások hello egyikét:

    * **SSH**: csatlakoztassa toohello a kiszolgálót, és használja hello következő parancsot a toorebalance a topológia:

            storm rebalance TOPOLOGYNAME

        Paraméterek toooverride hello párhuzamossági mutatók eredetileg hello topológia által biztosított is megadható. Például `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` Átkonfigurálás hello topológia too5 munkavégző folyamatok, hello kék-spout összetevő 3 végrehajtója és hello sárga-bolt összetevő 10 végrehajtója.

    * **Storm UI**: használata hello alábbi lépéseit toorebalance egy topológiáját hello Storm felhasználói felülete.

        1. Nyissa meg **https://CLUSTERNAME.azurehdinsight.net/stormui** a böngészőben, ahol CLUSTERNAME az hello a Storm-fürt nevére. Ha a rendszer kéri, adja meg a hello HDInsight fürt Fürtrendszergazda (rendszergazda) nevét és hello fürt létrehozásakor megadott jelszót.
        2. Válassza ki a hello topológia toorebalance kívánja, majd válassza ki a hello **egyensúlyba** gombra. Adja meg a hello késleltetés hello egyensúlyozza ki újra a művelet végrehajtása előtt.

A HDInsight-fürt méretezése a részletes információkat lásd:

* [Hdinsight Hadoop-fürtök kezelése hello Azure-portál használatával](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Hdinsight Hadoop-fürtök kezelése az Azure PowerShell használatával](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hogyan kell telepíteni a Hue (vagy más Hadoop-összetevők)?

HDInsight egy olyan felügyelt szolgáltatás. Azure hello fürttel problémát észlel, ha azt is sikertelenek lesznek csomópont hello törölje, majd hozzon létre egy csomópont tooreplace azt. Ha manuálisan telepíti dolgot hello fürt, akkor nem maradnak meg ez a művelet esetén. Ehelyett használjon [HDInsight Parancsfájlműveletek](hdinsight-hadoop-customize-cluster.md). A parancsfájl művelet lehet használt toomake hello a következő módosításokat:

* Telepítsen és konfiguráljon egy szolgáltatás vagy a webhely, például a Spark vagy Hue.
* Telepítsen és konfiguráljon olyan összetevő, amely szükséges konfigurációs módosítások hello fürt több csomópontja. Például egy kötelező környezeti változót egy naplózási könyvtárat, vagy létrehozott konfigurációs fájl létrehozása.

A Parancsfájlműveletek olyan Bash parancsfájlok. hello futtassa a fürt telepítése során, és a használt tooinstall és parancsfájlok adja meg a további összetevők hello fürtön. A következő összetevők hello telepítésének parancsfájlpéldákat biztosítja:

* [Hue](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Az egyéni parancsfájlművelet-fejlesztéssel kapcsolatos további információkért lásd: [Script Action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Parancsfájlműveletek fejlesztése a HDInsighttal).

### <a name="jar-files"></a>JAR-fájlok

Néhány Hadoop technológiák részeként MapReduce feladatot, vagy a használt funkciók önálló jar-fájlok szerepelnek a Pig- vagy Hive belül. Ezek is telepíthető, a Parancsfájlműveletek segítségével, amíg azok gyakran nem feltétlenül szükséges a telepítés és feltöltött toohello tárolófürt is lehet kiépítése után és használt közvetlenül. Ha azt szeretné, hogy toomake meg arról, hogy hello összetevő survives különösen hello fürt, a fürt (WASB vagy ADL) tárolhatja hello jar-fájlra hello alapértelmezett tároló.

Például, ha azt szeretné, hogy toouse hello legújabb verziójának [DataFu](http://datafu.incubator.apache.org/), töltse le a hello projektet tartalmazó jar, és töltse fel az toohello HDInsight-fürthöz. Kövesse a hello DataFu dokumentáció hogyan toouse Pig- vagy Hive le.

> [!IMPORTANT]
> Néhány összetevőt, amelyek az önálló jar-fájlok a HDInsight által biztosított, de nem hello elérési úton találhatók. Ha egy adott összetevőre keres, a fürt hozzá használhatja hello kövesse toosearch:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> A parancs hello bármely megfelelő jar-fájlok elérési útját adja vissza.

toouse feltöltés hello version kell, és használhatja a feladatok egy összetevő egy másik verziója.

> [!WARNING]
> Hello HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support tooisolate segítségével, és hárítsa el a problémákat kapcsolódó toothese összetevők.
>
> Egyéni összetevők kap minden üzleti szempontból ésszerű támogatási toohelp meg toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez. Hello probléma megoldását, vagy kérni a tooengage elérhető csatorna az hello megnyitja részletes segítséget, hogy a technológiát találhatók technológiák eredményezhet. Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Következő lépések

* [Telepítse át a tooLinux-alapú Windows-alapú HDInsight-ból](hdinsight-migrate-from-windows-to-linux.md)
* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [MapReduce-feladatok használata a HDInsightban](hdinsight-use-mapreduce.md)
