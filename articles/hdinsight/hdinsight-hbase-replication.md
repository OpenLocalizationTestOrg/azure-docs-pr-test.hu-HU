---
title: "Virtuális hálózatok - Azure belül a HBase-fürt replikálás konfigurálása |} Microsoft Docs"
description: "Megtudhatja, hogyan kell a terheléselosztást, magas rendelkezésre állású, nulla állásidő áttelepítési/frissítési egy HDInsight-verzióról a másikra, és vész-helyreállítási HBase-replikálás konfigurálása."
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 895709391486acb4a9d7a54ef046956539913f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a>Virtuális hálózatok belül a HBase-fürt replikálás konfigurálása

Megtudhatja, hogyan konfigurálja a HBase-replikálás belül egy virtuális hálózathoz (VNet) vagy virtuális hálózatok között.

Fürt replikáció forrás-ügyfélleküldéses módszer használ. A cél- és a HBase-fürtöt lehet, vagy teljesíteni tudja mindkét szerepkörök egyszerre. Replikációs aszinkron, és a cél replikációs végleges konzisztencia. Amikor a forrás egy oszlop családhoz szerkesztési replikáció engedélyezett kap, hogy a szerkesztési összes cél fürt propagálja. Ha adatok az egyik fürtről a másikra replikálódik, a forrás fürt és az összes fürt, amely már rendelkezik felhasznált adatok replikációs hurkok megelőzése érdekében nyomon követi.

Az oktatóanyag a forrás-cél replikációs állít. Más fürt topológiája esetén lásd: [Apache HBase referencia-útmutató](http://hbase.apache.org/book.html#_cluster_replication).

A HBase replikációs használati esetekben egyetlen virtuális hálózat:

* Terheléselosztás – például a cél fürtön futó vizsgálatokat vagy MapReduce-feladatok és a kiindulási fürt adatok bevitele
* Magas rendelkezésre állás
* Adatok áttelepítése a HBase egyik fürtről a másikra
* Azure HDInsight-fürtök egy-es verzióról a másikra

A HBase replikációs használati esetek két virtuális hálózat:

* Vészhelyreállítás
* Terheléselosztás és az alkalmazás particionálás
* Magas rendelkezésre állás

Fürtök használatával replikálja [parancsfájl-művelet](hdinsight-hadoop-customize-cluster-linux.md) található parancsfájlok [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="configure-the-environments"></a>A környezet konfigurálása

Három lehetséges konfiguráció van:

- Egy Azure virtuális hálózatban két HBase fürtök
- Két HBase-fürtökkel a ugyanabban a régióban két különböző virtuális hálózatokon
- Két HBase-fürtökkel a két különböző régiókban (georeplikáció) két különböző virtuális hálózatokon

Konfigurálja a környezetek megkönnyítése létrehoztunk egy [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-overview.md). Ha inkább a környezetek egyéb módszerek konfigurálása, lásd:

- [Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban](hdinsight-hadoop-provision-linux-clusters.md)
- [Hozzon létre HBase-fürtökkel Azure virtuális hálózat](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a>Egy virtuális hálózat konfigurálása

Kattintson az alábbi képre kattintva hozzon létre két HBase-fürtökkel ugyanabban a virtuális hálózatban. A sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

### <a name="configure-two-virtual-networks-in-the-same-region"></a>Két virtuális hálózat ugyanabban a régióban konfigurálása

Kattintson az alábbi képre kattintva hozzon létre két virtuális hálózatokat és a Vnetben társviszony-létesítés és két HBase-fürtökkel ugyanabban a régióban. A sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>



Ebben az esetben [Vnetben társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md). A sablon lehetővé teszi, hogy a Vnetben társviszony-létesítés.   

HBase-replikálás ZooKeeper virtuális gépek IP-címeket használ. A cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia.

**Statikus IP-címek konfigurálása**

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Kattintson a bal oldali menü **erőforráscsoportok**.
3. Kattintson az erőforráscsoport, amely a célként megadott HBase-fürtöt tartalmaz. Ez az az erőforráscsoport, amelyet a környezet létrehozása a Resource Manager-sablon használatakor. A szűrő segítségével listáját. Láthatja, hogy az erőforrások listáját, amelyek a két virtuális hálózatot tartalmaz.
4. Kattintson a virtuális hálózat, amely a célként megadott HBase-fürtöt tartalmaz. Kattintson például **xxxx-vnet2**. Megtekintheti a kezdetű névvel rendelkező három eszközök **nic-zookeepermode -**. Az eszközöket a három ZooKeeper virtuális gépek.
5. Kattintson a ZooKeeper virtuális gépeket.
6. Kattintson a **IP-konfigurációk**.
7. Kattintson a **ipConfig1** a listából.
8. Kattintson a **statikus**, és jegyezze fel az aktuális IP-címet. A replikáció engedélyezése parancsfájlművelet futtatásakor szüksége lesz az IP-címet.

  ![HDInsight HBase replikációs ZooKeeper statikus IP-cím](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Ismételje meg a 6. a két ZooKeeper csomópontok a statikus IP-címének beállítása.

A kereszt-VNet-forgatókönyv kell használnia a **- ip** kapcsoló meghívásakor a **hdi_enable_replication.sh** parancsfájl-művelet.

### <a name="configure-two-virtual-networks-in-two-different-regions"></a>Két különböző régiókban két virtuális hálózat konfigurálása

Kattintson az alábbi képen két különböző régiókban két virtuális hálózatok létrehozásához. A sablon egy nyilvános Azure Blob-tároló tárolja.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Hozzon létre két virtuális hálózatok közötti VPN-átjáró. Útmutatásért lásd: [hozzon létre egy Vnetet a pont-pont kapcsolat](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

HBase-replikálás ZooKeeper virtuális gépek IP-címeket használ. A cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia. Statikus IP-cím konfigurálása, a cikkben "Konfigurálása két virtuális hálózat ugyanabban a régióban" című szakaszában talál.

A kereszt-VNet-forgatókönyv kell használnia a **- ip** kapcsoló meghívásakor a **hdi_enable_replication.sh** parancsfájl-művelet.

## <a name="load-test-data"></a>Vizsgálati adatok betöltése

A replikált egy fürtöt, meg kell adnia replikálásához táblák. Ebben a szakaszban néhány adat betölti a forrás-fürthöz. A következő szakaszban akkor engedélyezi a fürtök közötti replikáció.

Kövesse az utasításokat a [HBase oktatóanyag: az Apache HBase Linux-alapú hadooppal a Hdinsightban első lépéseiben](hdinsight-hbase-tutorial-get-started-linux.md) létrehozásához egy **névjegyek** tábla és adatok beszúrása a táblába.

## <a name="enable-replication"></a>A replikáció engedélyezése

A következő lépések bemutatják, hogyan hívhatja meg a parancsfájl parancsfájlművelet Azure-portálról. Parancsfájl műveletek futtatása az Azure PowerShell és az Azure parancssori felület (CLI) használatával, lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

**Azure-portálról HBase-replikálás engedélyezése**

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Nyissa meg a forrás HBase-fürtöt.
3. Kattintson a fürt menü **Parancsfájlműveletek**.
4. Kattintson a **nyújt új** a panel tetején.
5. Válassza ki vagy adja meg a következő adatokat:

  - **Név**: Adjon meg **engedélyezze a replikálást**.
  - **Parancsprogram URL-Címének bash**: Adjon meg **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  - **HEAD**: kiválasztott. Törölje a csomóponttípusok.
  - **Paraméterek**: A következő minta paraméterek engedélyezze a replikációját a létező táblák, és másolja az adatokat a forráskiszolgálóról a fürt a fürt céltárhelyét, az összes:

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. Kattintson a **Create** (Létrehozás) gombra. A parancsfájl eltarthat egy ideig, különösen a - copydata argumentum használatakor.

Szükséges argumentumokkal:

|Név|Leírás|
|----|-----------|
|-s,--src-fürt | Adja meg a forrás HBase fürt DNS-nevét. Például: -s hbsrccluster, a fürt--src = hbsrccluster |
|-d, nyári időszámítás – a fürt | Adja meg a cél (replika) HBase-fürtöt DNS-nevét. Például: -s dsthbcluster, a fürt--src = dsthbcluster |
|--src-ambari-jelszó - sp, | Adja meg a rendszergazdai jelszó az Ambari a kiindulási HBase fürt. |
|-dp, nyári időszámítás –--jelszavaknak | Adja meg a rendszergazdai jelszó az Ambari a HBase célfürtön.|

Választható argumentumok:

|Név|Leírás|
|----|-----------|
|--src-ambari-user - su, | A kiindulási HBase fürt Ambari a rendszergazda felhasználónevét adja meg. Az alapértelmezett érték **admin**. |
|-du, nyári időszámítás –-ambari-felhasználó | A rendszergazda felhasználóneve az Ambari meg a cél HBase-fürtöt. Az alapértelmezett érték **admin**. |
|-t,--tábla-lista | Adja meg a táblák replikálni. Például:--tábla-lista = "table1; table2; Tábl3". Ha nem adja meg a táblák, az összes meglévő HBase táblák replikálódnak.|
|-m,--gép | Adja meg az átjárócsomóponthoz, ahol a parancsfájlművelet futni fog. Hn1 vagy hn0 értéke. Mivel a hn0 általában busier, hn1 használatát javasoljuk. Akkor használja ezt a beállítást, ha a számítógépén a $0 parancsfájl parancsfájl műveletként a HDInsight portálon vagy az Azure PowerShell.|
|-ip | Az argumentum megadása akkor kötelező, ha két virtuális hálózatok közötti replikáció folyamatban engedélyezi. Ezt az argumentumot úgy működik, mint a kapcsoló használatához a statikus IP-címek a ZooKeeper csomópontok replika fürtök FQDN-nevek helyett. A statikus IP-címet kell előre konfigurált replikáció engedélyezése előtt. |
|-cp, - copydata | Az áttelepítés a meglévő adatok a táblák, amelyben engedélyezve van a replikáció engedélyezése. |
|-fordulat/perces, - replikálás-phoenix-meta | Engedélyezheti a replikálást Phoenix rendszertáblákra. <br><br>*Használja ezt a beállítást a kellő körültekintéssel járjon el.* Azt javasoljuk, hogy Ön hozza létre újból Phoenix táblák replika fürtökön használja ezt a parancsfájlt. |
|-h, – Súgó | Használati információk jelennek meg. |

A print_usage() szakasza a [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) paraméterek részletesen ismerteti.

A parancsfájlművelet sikeres telepítése után SSH segítségével csatlakozzon a cél HBase-fürtöt, és ellenőrizze az adatokat a rendszer replikálta.

### <a name="replication-scenarios"></a>A replikáció eseteire

Az alábbi listában láthatók az egyes általános használati esetek és a paraméter beállítások:

- **Minden tábla a fürtök közötti replikáció engedélyezése**. Ez a forgatókönyv nem igényel a Másolás/áttelepítése a meglévő adatok a táblák, és nem használ Phoenix táblák. Használja a következő paramétereket:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Engedélyezze a replikálást a megadott táblák**. A table1, table2 és Tábl3 replikáció engedélyezéséhez használja a következő paramétereket:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **A megadott táblák replikálás engedélyezése és a meglévő adatok másolása**. A table1, table2 és Tábl3 replikáció engedélyezéséhez használja a következő paramétereket:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Engedélyezze a replikálást a forrás célhelyre phoenix metaadatok replikálásához minden tábla**. Phoenix metaadatok replikációs nincs tökéletes megoldás, és engedélyezve legyenek-e figyelmeztetés.

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Másolja, majd az adatok áttelepítése

Két külön parancsfájl művelet parancsprogramok másolása/történő áttelepítésére vonatkozó adatok a replikáció engedélyezése után:

- [Kis táblák parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (néhány gigabájttól méret és a teljes másolás várható befejezés kisebb, mint egy óra)

- [Nagy méretű táblákra vonatkozó parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (várható másolása egy óránál több időt vesz igénybe)

Hajtsa végre az ugyanezt az eljárást [engedélyezze a replikálást](#enable-replication) hívni a parancsfájl művelet a következő paraméterekkel:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

A print_usage() szakasza a [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) paraméterek részletes leírását.

### <a name="scenarios"></a>Forgatókönyvek

- **Adott táblákra (Teszt1 Teszt2 és Teszt3) másolja az összes sorát, vége: jelenleg szerkesztett (aktuális időbélyeg)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  vagy

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Másolja át a megadott időtartomány adott táblák**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Tiltsa le a replikációt

Tiltsa le a replikációt, helye: egy másik parancsfájl parancsfájlművelet használja [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Hajtsa végre az ugyanezt az eljárást [engedélyezze a replikálást](#enable-replication) hívni a parancsfájl művelet a következő paraméterekkel:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

A print_usage() szakasza a [parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) paraméterek részletesen ismerteti.

### <a name="scenarios"></a>Forgatókönyvek

- **Tiltsa le a replikációt minden tábla**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  vagy

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- **Tiltsa le a replikációt a megadott táblák (table1 table2 és Tábl3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban megismerte HBase-replikálás konfigurálása két adatközpont között. HDInsight és HBase kapcsolatos további információkért lásd:

* [Ismerkedés az Apache HBase a hdinsight eszközben][hdinsight-hbase-get-started]
* [HDInsight HBase áttekintése][hdinsight-hbase-overview]
* [Hozzon létre HBase-fürtökkel Azure virtuális hálózat][hdinsight-hbase-provision-vnet]
* [A hbase eszközzel valós idejű Twitter sentiment elemzése][hdinsight-hbase-twitter-sentiment]
* [Érzékelőadatok elemzése a Storm és HBase a HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
