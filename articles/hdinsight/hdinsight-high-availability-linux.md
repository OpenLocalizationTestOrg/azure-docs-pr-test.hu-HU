---
title: "Hadoop - Azure HDInsight aaaHigh rendelkezésre állásának |} Microsoft Docs"
description: "Ismerje meg, hogyan HDInsight-fürtök javítása a megbízhatóság és rendelkezésre állás további központi csomópontra. Ismerje meg, milyen hatással van ez az Ambari és a Hive, például a Hadoop-szolgáltatás is, hogyan tooindividually csatlakozás tooeach átjárócsomópont SSH használatával."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
keywords: "hadoop magas rendelkezésre állás"
ms.assetid: 99c9f59c-cf6b-4529-99d1-bf060435e8d4
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/28/2017
ms.author: larryfr
ms.openlocfilehash: 9ff62afe6b63b241cb984225233157219f8d7411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a>A HDInsight-beli Hadoop-fürtök rendelkezésre állása és megbízhatósága

A HDInsight-fürtök adja meg a két átjárócsomópontokkal tooincrease hello rendelkezésre állásának és megbízhatóságának Hadoop-szolgáltatás és a futó feladatok.

Hadoop éri el a magas rendelkezésre állást és megbízhatóságot által egy fürt több csomópontja replikálása szolgáltatásaikat és adataikat. Standard disztribúciók Hadoop azonban általában csak egy átjárócsomóponttal rendelkeznek. Bármely kimaradás hello egyetlen központi csomópont hello fürt toostop működő okozhat. A HDInsight két headnodes tooimprove Hadoop rendelkezésre állást és megbízhatóságot biztosít.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="availability-and-reliability-of-nodes"></a>Rendelkezésre állásának és megbízhatóságának csomópontok

HDInsight-fürtök csomópontjai Azure virtuális gépek segítségével lehet létrehozni. hello következő szakaszok tárgyalják hello egyes csomóponttípusok HDInsight együtt. 

> [!NOTE]
> A fürt típusa nem minden csomópont-típusok használhatók. Például egy Hadoop-fürt típusa nem rendelkezik egyetlen Nimbus csomópontot. További információ a HDInsight-fürttípusok által használt csomópontok című hello fürt típusok hello [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentum.

### <a name="head-nodes"></a>HEAD csomópontok

tooensure magas rendelkezésre állású Hadoop szolgáltatásokat, a HDInsight két átjárócsomópontokkal biztosít. Mindkét átjárócsomópontokkal érvényesek hello HDInsight-fürtön belül futó egyidejűleg. Egyes szolgáltatások, például a HDFS vagy YARN, egy adott időpontban csak "active", egy központi csomóponton. Más szolgáltatásokon, például a hiveserver2-n vagy a Hive Metaadattárhoz aktívak: hello mindkét központi csomópontján ugyanannyi időt vesz igénybe.

HEAD csomópontok (és más csomópontok a Hdinsightban) hello állomásnév hello csomópont részeként numerikus értéknek kell tartoznia. Például `hn0-CLUSTERNAME` vagy `hn4-CLUSTERNAME`.

> [!IMPORTANT]
> Ne társítson hello numerikus érték-e a csomópont a elsődleges vagy másodlagos. hello numerikus érték csak a jelenlegi tooprovide egy egyedi nevet az egyes csomópontok.

### <a name="nimbus-nodes"></a>Nimbus-csomópontok

Nimbus csomópont alatt futó Storm-fürtökkel érhetők el. hello Nimbus csomópontot hasonló funkciókat toohello Hadoop JobTracker terjesztése és a feldolgozó csomópontok átívelő figyelésére is alkalmas feldolgozási adja meg. A HDInsight két Nimbus csomóponttal rendelkezik a Storm-fürtök

### <a name="zookeeper-nodes"></a>Zookeeper csomópontok

[ZooKeeper](http://zookeeper.apache.org/) csomópontok használt fő szolgáltatást az átjárócsomópontokkal vezető választás. Azok is, hogy szolgáltatások, a adatcsomópontokat (munkavégző) és az átjárók tudja, melyik szolgáltatás főkulcsának aktív központi csomópont használt tooinsure. Alapértelmezés szerint a HDInsight nyújt három ZooKeeper csomópontok.

### <a name="worker-nodes"></a>Munkavégző csomópontokhoz

Munkavégző csomópontokhoz hello tényleges adatelemzés hajtható végre, ha egy feladat elküldött toohello fürt. Ha egy feldolgozó csomópont leáll, hello azt végző feladata elküldött tooanother munkavégző csomópont. Alapértelmezés szerint a HDInsight létrehoz négy munkavégző csomópontokhoz. Módosíthatja a szám toosuit igényeinek alatt és a fürt létrehozása után.

### <a name="edge-node"></a>Élcsomópont

Egy élcsomópontot nem aktívan részt adatelemzés hello fürtön belül. Amikor olyan Hadoop fejlesztők vagy adatszakértőkön szolgál. hello peremhálózati csomópont életét a hello azonos Azure virtuális hálózaton hello hello fürt többi csomópontjára, és közvetlenül hozzáférhet az összes többi csomópontok. hello élcsomópont kritikus Hadoop-szolgáltatás vagy a feladatok távol erőforrások anélkül is használható.

Az R Server on HDInsight jelenleg hello csak fürt típusa, amely alapértelmezés szerint egy élcsomópontot biztosít. Az R Server on HDInsight, hello élcsomópont használjuk teszt R kód helyileg hello csomóponton való elküldése toohello fürt elosztott feldolgozás előtt.

Egy élcsomópontot fürt típusa nem R Server használatáról információkért lásd: hello [peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md) dokumentum.

## <a name="accessing-hello-nodes"></a>Hello csomópontok elérése

Hozzáférés toohello fürt keresztül hello internet biztosított nyilvános átjárón keresztül. Hozzáférése korlátozott tooconnecting toohello átjárócsomópontokkal és (ha van ilyen) hello élcsomópont. Hozzáférés tooservices hello központi csomópontokon futó azzal, hogy több átjárócsomópontokkal nem történik. hello nyilvános átjáró útvonalak kérelmek toohello átjárócsomópont hello üzemeltető kért szolgáltatás. Például ha Ambari kiszolgálóvá hello másodlagos átjárócsomópont, hello átjáró irányítja a bejövő kérelmek Ambari toothat csomópont.

A rendszer korlátozott tooport 443-as (HTTPS), a 22-es és 23 elérést hello nyilvános átjárón keresztül.

* Port __443-as__ használt tooaccess az Ambari és egyéb webes felhasználói felületén vagy a REST API-k hello átjárócsomópontokkal üzemeltet.

* Port __22__ használt tooaccess hello elsődleges központi csomópont és él SSH-csomópont.

* Port __23__ használt tooaccess hello másodlagos átjárócsomópont az SSH van. Például `ssh username@mycluster-ssh.azurehdinsight.net` toohello elsődleges nevű hello fürt átjárócsomópontjához csatlakozik **sajátfürt**.

További információ az SSH használatával, lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Belső teljes tartománynevek (FQDN)

HDInsight-fürtök csomópontjai rendelkeznek, egy belső IP-cím és a teljes Tartománynevet, amellyel csak hello fürtből érhető el. Szolgáltatások hello belső FQDN vagy IP-cím használatával hello fürtön elérésekor használandó Ambari tooverify hello IP-cím vagy FQDN toouse hello szolgáltatás elérésével.

Például hello szolgáltatást csak futtathatja egy átjárócsomópont Oozie és hello használata `oozie` parancsot SSH-munkamenetet a hello URL-cím toohello szolgáltatás szükséges. Az URL-cím lehet beolvasni az Ambari hello a következő parancs használatával:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Ez a parancs visszaadja egy értéket a következő parancsot, amely tartalmaz hello belső URL-cím toouse hello a hasonló toohello `oozie` parancs:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Hello Ambari REST API használatával további információkért lásd: [figyelése és kezelése HDInsight hello Ambari REST API-t a](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Egyéb elérése

Toonodes, amelyek nem érhető el over internet hello hello a következő módszerek használatával közvetlenül kapcsolódhatnak:

* **SSH**: Miután csatlakozott az SSH, használatával tooa átjárócsomópont ezután használhatja az SSH a hello átjárócsomópont tooconnect tooother hello fürt csomópontja. További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.

* **SSH-alagút**: Ha egy webszolgáltatás-bővítmény üzemeltetett tooaccess van szüksége, amely nem hello csomópontok egyikét kitett toohello internet, használnia kell az SSH-alagút. További információkért lásd: hello [használhat SSH-alagutat a hdinsight eszközzel](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.

* **Azure-beli virtuális hálózat**: Ha a HDInsight fürt része egy Azure virtuális hálózatra, az erőforrásoknál hello ugyanaz a virtuális hálózati közvetlenül hozzáférhet hello fürt összes csomópontján. További információkért lásd: hello [kiterjesztése HDInsight az Azure Virtual Network a](hdinsight-extend-hadoop-virtual-network.md) dokumentum.

## <a name="how-toocheck-on-a-service-status"></a>Hogyan toocheck a szolgáltatás állapota

hello átjárócsomópontokról futó szolgáltatások állapotának toocheck hello hello Ambari webes felhasználói felület használata, vagy hello Ambari REST API-t.

### <a name="ambari-web-ui"></a>Ambari webes felhasználói felület

Ambari webes felhasználói felületén hello: https://CLUSTERNAME.azurehdinsight.net megtekinthető. Cserélje le **CLUSTERNAME** hello néven a fürt. Ha a rendszer kéri, adja meg a hello HTTP felhasználói hitelesítő adatok a fürt számára. hello alapértelmezett HTTP felhasználónév **admin** hello jelszó pedig hello fürt létrehozásakor megadott hello jelszó.

Hello Ambari oldalon érkezésekor hello balra hello lap hello telepített szolgáltatások listája látható.

![Telepített szolgáltatások](./media/hdinsight-high-availability-linux/services.png)

Nincsenek tovább tooa szolgáltatás tooindicate állapota látható ikonok sorozata. E riasztások kapcsolódó tooa szolgáltatás megtekinthetők a hello **riasztások** hivatkozás hello oldal hello tetején. Minden szolgáltatás tooview választhat további tájékoztatást.

Hello szolgáltatás lapján információt nyújt hello állapotát, és minden egyes szolgáltatás konfigurációját, amíg azt nem szolgál információval, amelyen átjárócsomópont hello szolgáltatás fut a. tooview ezen információk, használjon hello **állomások** hivatkozás hello oldal hello tetején. Ez a lap megjeleníti a hello fürtben, beleértve a hello átjárócsomópontokkal állomások.

![állomások listájához](./media/hdinsight-high-availability-linux/hosts.png)

Egy hello átjárócsomópontokkal kiválasztásával hello hivatkozás hello szolgáltatás és a leállt csomóponton összetevők megjelenítése.

![Összetevő-állapot](./media/hdinsight-high-availability-linux/nodeservices.png)

Ambari használatával kapcsolatos további információkért lásd: [figyelése és kezelése a HDInsight hello Ambari webes felhasználói felületén a](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambari REST API-n

hello Ambari REST API-n keresztül hello érhető internet. hello HDInsight nyilvános átjáró útválasztási kérelmek toohello átjárócsomópont jelenleg hello REST API-t futtató kezeli.

A következő parancs toocheck hello hello Ambari REST API-n keresztül szolgáltatás állapotának hello használhatja:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Cserélje le **jelszó** a hello HTTP (rendszergazda) felhasználói fiók jelszavát.
* Cserélje le **CLUSTERNAME** hello nevű hello fürt.
* Cserélje le **szolgáltatásnév** hello nevű hello szolgáltatást szeretné toocheck hello állapotát.

Például toocheck hello állapotának hello **HDFS** szolgáltatás nevű fürt **sajátfürt**, a jelszó **jelszó**, hello a következő parancsot használja:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

a rendszer a következő JSON hasonló toohello hello választ:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

hello URL-cím közli velünk, hogy hello szolgáltatás jelenleg fut egy átjárócsomópont nevű **hn0-CLUSTERNAME**.

hello állapot jelzi, hogy hello szolgáltatás jelenleg fut, vagy **elindítva**.

Ha nem tudja, milyen szolgáltatások telepítve vannak a hello fürtön, használhatja a következő parancs tooretrieve listáját hello:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Hello Ambari REST API használatával további információkért lásd: [figyelése és kezelése HDInsight hello Ambari REST API-t a](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Szolgáltatás-összetevők

Szolgáltatások tartalmazhatnak toocheck hello állapotának külön-külön kívánja összetevőket. Például a HDFS hello NameNode összetevőt tartalmaz. egy összetevő tooview információkért hello parancs a következő lesz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Ha nem tudja, milyen összetevők szolgáltatás által biztosított, a következő parancs tooretrieve listáját hello is használhatja:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-tooaccess-log-files-on-hello-head-nodes"></a>Hogyan tooaccess naplófájljai hello átjárócsomópontokkal

### <a name="ssh"></a>SSH

Csatlakoztatott tooa átjárócsomópont SSH-n keresztül, miközben naplófájlok alatt található **/var/log**. Például **/var/log/hadoop-yarn/yarn** YARN naplók tartalmazzák.

Minden egyes átjárócsomópont egyedi naplóbejegyzések, van így ellenőrizni kell a hello bejelentkezik is.

### <a name="sftp"></a>SFTP

Toohello átjárócsomópont hello SSH File Transfer Protocol vagy biztonságos fájl Transfer Protocol (SFTP) segítségével csatlakozzon, és közvetlenül letöltheti hello naplófájlokat is.

Egy SSH-ügyfél, meg kell adnia toohello fürt kapcsolódáskor hasonló toousing hello SSH felhasználói fiók nevét és hello SSH-cím hello fürt. Például: `sftp username@mycluster-ssh.azurehdinsight.net`. Hello jelszót adjon meg hello fiókot, amikor a rendszer kéri, vagy adjon meg egy nyilvános kulcsot használó hello `-i` paraméter.

Miután csatlakozott, lehetősége lesz a `sftp>` kérdés. A parancssorból módosíthatja, könyvtárak, fájlok feltöltését és letöltését. Például a következő parancsok hello módosítása könyvtárak toohello **/var/log/hadoop/hdfs** directory és a majd letöltési hello directory összes fájlját.

    cd /var/log/hadoop/hdfs
    get *

Elérhető parancsok listáját, írja be a `help` : hello `sftp>` kérdés.

> [!NOTE]
> Van grafikus felületek, amelyek lehetővé teszik toovisualize hello fájlrendszer SFTP használatával csatlakozáskor. Például [MobaXTerm](http://mobaxterm.mobatek.net/) lehetővé teszi egy felület hasonló tooWindows Explorer használt toobrowse hello fájlrendszer.

### <a name="ambari"></a>Ambari

> [!NOTE]
> tooaccess naplófájlok Ambari használatával, az SSH-alagút kell használnia. az egyes szolgáltatások hello hello webes felületek nem érhetők el nyilvánosan az interneten hello. Az SSH-alagúton keresztül információkért lásd: hello [használata SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.

Hello Ambari webes felhasználói felületén jelölje ki kívánja tooview naplók (például YARN) a hello szolgáltatást. Ezután **Gyorshivatkozások** tooselect mely átjárócsomópont tooview hello naplózza.

![Tooview naplók segítségével gyors hivatkozásokat tartalmaz.](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-tooconfigure-hello-node-size"></a>Hogyan tooconfigure hello csomópont mérete

egy csomópont hello mérete csak akkor jelölhető ki, fürt létrehozása során. Található hello listája elérhető különböző VM méretű hdinsight hello [árképzést ismertető oldalra HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

A fürt létrehozásakor megadhatja hello csomópontok hello méretét. hello alábbi információkat nyújt útmutatást hogyan toospecify hello méret használatával hello [Azure-portálon][preview-portal], [Azure PowerShell][azure-powershell], és hello [Azure CLI][azure-cli]:

* **Azure-portálon**: a fürt létrehozásakor beállíthatja hello fürt által használt hello csomópontok hello mérete:

    ![A méret kiválasztása a fürt létrehozása varázsló képe](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Az Azure CLI**: hello használatakor `azure hdinsight cluster create` parancs hello segítségével beállíthatja a hello mérete hello head, munkavégző és ZooKeeper csomópontok `--headNodeSize`, `--workerNodeSize`, és `--zookeeperNodeSize` paraméterek.

* **Az Azure PowerShell**: hello használatakor `New-AzureRmHDInsightCluster` parancsmaggal beállíthatja hello mérete hello head, munkavégző és ZooKeeper csomópontok hello segítségével `-HeadNodeVMSize`, `-WorkerNodeSize`, és `-ZookeeperNodeSize` paraméterek.

## <a name="next-steps"></a>Következő lépések

A következő hivatkozások toolearn további ebben a dokumentumban említett szempontot hello használata.

* [Ambari REST-referencia](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Telepítse és konfigurálja a hello Azure parancssori felület](../cli-install-nodejs.md)
* [Telepítse és konfigurálja az Azure PowerShellt](/powershell/azure/overview)
* [HDInsight a Ambari kezelése](hdinsight-hadoop-manage-ambari.md)
* [Linux-alapú HDInsight-fürtök kiépítése](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
