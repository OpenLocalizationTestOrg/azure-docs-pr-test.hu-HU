---
title: "virtuális hálózatok – Azure belüli aaaConfigure HBase-fürt replikáció |} Microsoft Docs"
description: "Megtudhatja, hogyan a(z) terheléselosztást, a magas rendelkezésre állás, a nulla állásidő áttelepítés vagy frissítés egy HDInsight-verzió tooanother és vész-helyreállítási tooconfigure HBase-replikálás."
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
ms.openlocfilehash: ba1c44f26b7cbf4a7a88159b12b3e064ea9f9a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a>Virtuális hálózatok belül a HBase-fürt replikálás konfigurálása

Megtudhatja, hogyan tooconfigure HBase-replikálás belül egy virtuális hálózathoz (VNet) vagy virtuális hálózatok között.

Fürt replikáció forrás-ügyfélleküldéses módszer használ. A cél- és a HBase-fürtöt lehet, vagy teljesíteni tudja mindkét szerepkörök egyszerre. Replikációs aszinkron, és hello replikációs célja a végleges konzisztencia. Hello forrás egy Szerkesztés tooa oszlop termékcsalád megkapja a replikáció engedélyezett, a Szerkesztés esetén propagált tooall cél fürtök. Adatokat a rendszer replikálja a egy fürt tooanother, hello forrásfürt és minden olyan fürt, amely már rendelkezik felhasznált hello adatok nyomon követett tooprevent replikációs hurkok.

Az oktatóanyag a forrás-cél replikációs állít. Más fürt topológiája esetén lásd: [Apache HBase referencia-útmutató](http://hbase.apache.org/book.html#_cluster_replication).

A HBase replikációs használati esetekben egyetlen virtuális hálózat:

* Terheléselosztás – például vizsgálatok vagy MapReduce-feladatok hello cél fürtön fut, és hello kiindulási fürt adatok bevitele
* Magas rendelkezésre állás
* A HBase-fürtöt egy tooanother adatok áttelepítése
* Egy verzió tooanother Azure HDInsight-fürtök frissítése

A HBase replikációs használati esetek két virtuális hálózat:

* Vészhelyreállítás
* Terheléselosztás és a particionálás hello alkalmazás
* Magas rendelkezésre állás

Fürtök használatával replikálja [parancsfájl-művelet](hdinsight-hadoop-customize-cluster-linux.md) található parancsfájlok [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="configure-hello-environments"></a>Hello környezetek konfigurálása

Három lehetséges konfiguráció van:

- Egy Azure virtuális hálózatban két HBase fürtök
- A két különböző virtuális hálózatokon, a HBase fürtök hello ugyanabban a régióban
- Két HBase-fürtökkel a két különböző régiókban (georeplikáció) két különböző virtuális hálózatokon

toomake azt tooconfigure hello környezetek egyszerűbb, a rendszer készített néhány [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-overview.md). Ha jobban szeret tooconfigure hello környezetek egyéb módszerek, lásd:

- [Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban](hdinsight-hadoop-provision-linux-clusters.md)
- [Hozzon létre HBase-fürtökkel Azure virtuális hálózat](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a>Egy virtuális hálózat konfigurálása

Kattintson a következő kép toocreate két HBase-fürtökkel a hello hello ugyanazt a virtuális hálózatot. hello sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

### <a name="configure-two-virtual-networks-in-hello-same-region"></a>Két virtuális hálózatok konfigurálása a hello ugyanabban a régióban

Kattintson a következő kép toocreate két virtuális hálózatokat és a Vnetben társviszony-létesítés és két HBase-fürtökkel a hello hello ugyanabban a régióban. hello sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>



Ebben az esetben [Vnetben társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md). hello sablon lehetővé teszi, hogy a Vnetben társviszony-létesítés.   

HBase-replikálás hello ZooKeeper virtuális gépek IP-címét használja. Hello cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia.

**tooconfigure statikus IP-címek**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldali menüben kattintson **erőforráscsoportok**.
3. Kattintson a hello cél HBase-fürtöt tartalmazó erőforráscsoportot. Ez a hello Resource Manager sablon toocreate hello környezet használatakor a megadott erőforráscsoport hello. Hello szűrő toonarrow hello listából is használhatja. Hello két virtuális hálózatot tartalmaz erőforrás listáját láthatja.
4. Kattintson a virtuális hálózati hello hello cél HBase-fürtöt tartalmazó. Kattintson például **xxxx-vnet2**. Megtekintheti a kezdetű névvel rendelkező három eszközök **nic-zookeepermode -**. Az eszközök három hello ZooKeeper virtuális gépek.
5. Kattintson az egyik hello ZooKeeper virtuális gépeket.
6. Kattintson a **IP-konfigurációk**.
7. Kattintson a **ipConfig1** hello listából.
8. Kattintson a **statikus**, és jegyezze fel a hello tényleges IP-címet. Hello IP-címet kell hello parancsfájl művelet tooenable replikációs futtatásakor.

  ![HDInsight HBase replikációs ZooKeeper statikus IP-cím](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Ismételje meg a lépés 6 tooset hello statikus IP-cím hello más két ZooKeeper csomópontok.

A hello kereszt-VNet-forgatókönyv, kell használnia hello **- ip** Váltás a következő meghívásakor: hello **hdi_enable_replication.sh** parancsfájl-művelet.

### <a name="configure-two-virtual-networks-in-two-different-regions"></a>Két különböző régiókban két virtuális hálózat konfigurálása

Kattintson a következő kép toocreate két virtuális hálózatok két különböző régiókban hello. hello sablon egy nyilvános Azure Blob-tároló tárolja.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

Hozzon létre egy VPN-átjáró hello virtuális hálózatok között. Útmutatásért lásd: [hozzon létre egy Vnetet a pont-pont kapcsolat](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

HBase-replikálás hello ZooKeeper virtuális gépek IP-címét használja. Hello cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia. tooconfigure statikus IP-címet, lásd: hello "lévő virtuális hálózatok konfigurálása a két hello ugyanabban a régióban" című részben.

A hello kereszt-VNet-forgatókönyv, kell használnia hello **- ip** Váltás a következő meghívásakor: hello **hdi_enable_replication.sh** parancsfájl-művelet.

## <a name="load-test-data"></a>Vizsgálati adatok betöltése

A replikált egy fürtöt, meg kell adnia hello táblák tooreplicate. Ez a szakasz néhány adat betölti hello forrás fürtbe. A következő szakaszban hello engedélyezi hello fürtök közötti replikáció.

Hello utasításait követve [HBase oktatóanyag: az Apache HBase Linux-alapú hadooppal a Hdinsightban első lépéseiben](hdinsight-hbase-tutorial-get-started-linux.md) toocreate egy **névjegyek** tábla és adatok beszúrása hello táblába.

## <a name="enable-replication"></a>A replikáció engedélyezése

hello lépések bemutatják, hogyan toocall hello parancsfájl parancsfájlművelet hello Azure-portálon a. Parancsfájl műveletek futtatása az Azure PowerShell és hello Azure parancssori felület (CLI) használatával, lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

**tooenable HBase replikáció hello Azure-portálon**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a hello forrás HBase-fürtöt.
3. Hello fürt menüben kattintson **Parancsfájlműveletek**.
4. Kattintson a **nyújt új** a hello felső hello panelről.
5. Válassza ki vagy adja meg a következő információ hello:

  - **Név**: Adjon meg **engedélyezze a replikálást**.
  - **Parancsprogram URL-Címének bash**: Adjon meg **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  - **HEAD**: kiválasztott. Törölje a jelet hello más csomóponttípusok.
  - **Paraméterek**: hello alábbi paraméterek a replikáció engedélyezése az összes hello meglévő táblázatban példa, és az összes hello adatának átmásolásához hello forrás fürt toohello célfürtöt:

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. Kattintson a **Create** (Létrehozás) gombra. hello parancsfájl eltarthat egy ideig, különösen akkor hello - copydata argumentum használatakor.

Szükséges argumentumokkal:

|Név|Leírás|
|----|-----------|
|-s,--src-fürt | Adja meg a DNS-neve hello hello forrás HBase-fürtöt. Például: -s hbsrccluster, a fürt--src = hbsrccluster |
|-d, nyári időszámítás – a fürt | Adja meg a DNS-neve hello hello cél (replika) HBase-fürtöt. Például: -s dsthbcluster, a fürt--src = dsthbcluster |
|--src-ambari-jelszó - sp, | Ambari hello kiindulási HBase fürt hello rendszergazdai jelszó megadása. |
|-dp, nyári időszámítás –--jelszavaknak | Ambari hello cél HBase fürtön hello rendszergazdai jelszó megadása.|

Választható argumentumok:

|Név|Leírás|
|----|-----------|
|--src-ambari-user - su, | Hello kiindulási HBase fürt Ambari hello rendszergazda felhasználónevét adja meg. hello alapértelmezett értéke **admin**. |
|-du, nyári időszámítás –-ambari-felhasználó | Hello cél HBase fürtön Ambari hello rendszergazda felhasználónevét adja meg. hello alapértelmezett értéke **admin**. |
|-t,--tábla-lista | Adja meg a replikált hello táblák toobe. Például:--tábla-lista = "table1; table2; Tábl3". Ha nem adja meg a táblák, az összes meglévő HBase táblák replikálódnak.|
|-m,--gép | Adja meg a hello átjárócsomópont ahol hello parancsfájlművelet futni fog. hn1 vagy hn0 hello értéke. Mivel a hn0 általában busier, hn1 használatát javasoljuk. Akkor használja ezt a beállítást, ha a számítógépén hello $0 parancsfájl parancsfájl műveletként hello HDInsight portálon vagy az Azure PowerShell.|
|-ip | Az argumentum megadása akkor kötelező, ha két virtuális hálózatok közötti replikáció folyamatban engedélyezi. Ez az argumentum egy kapcsoló toouse hello statikus IP-címek a ZooKeeper csomópontok replika fürtök FQDN-nevek helyett funkcionál. hello statikus IP-címet kell toobe előre konfigurált a replikáció engedélyezése előtt. |
|-cp, - copydata | Hello áttelepítése a meglévő adatok hello táblákon, amelyben engedélyezve van a replikáció engedélyezése. |
|-fordulat/perces, - replikálás-phoenix-meta | Engedélyezheti a replikálást Phoenix rendszertáblákra. <br><br>*Használja ezt a beállítást a kellő körültekintéssel járjon el.* Azt javasoljuk, hogy Ön hozza létre újból Phoenix táblák replika fürtökön használja ezt a parancsfájlt. |
|-h, – Súgó | Használati információk jelennek meg. |

hello print_usage() szakasza hello [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) paraméterek részletesen ismerteti.

Hello parancsfájl művelet végeztével sikeresen telepített, akkor is SSH tooconnect toohello cél HBase-fürtöt használ, és ellenőrizze hello adatokat a rendszer replikálta.

### <a name="replication-scenarios"></a>A replikáció eseteire

hello alábbi lista mutatja azokat, néhány általános használati esetek és a paraméter beállítások:

- **Minden tábla hello két fürtök közötti replikáció engedélyezése**. Ez a forgatókönyv nem igényel hello másolási/áttelepítése a meglévő adatok hello táblákon, és nem használ Phoenix táblákat. A következő paraméterek hello használata:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Engedélyezze a replikálást a megadott táblák**. Használja a következő paraméterek tooenable replikációs table1, table2 és Tábl3 hello:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **A megadott táblák replikálás engedélyezése és hello meglévő adatok másolása**. Használja a következő paraméterek tooenable replikációs table1, table2 és Tábl3 hello:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **A phoenix metaadatok replikálásához forrás toodestination összes táblán a replikáció engedélyezése**. Phoenix metaadatok replikációs nincs tökéletes megoldás, és engedélyezve legyenek-e figyelmeztetés.

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Másolja, majd az adatok áttelepítése

Két külön parancsfájl művelet parancsprogramok másolása/történő áttelepítésére vonatkozó adatok a replikáció engedélyezése után:

- [Kis táblák parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (néhány gigabájttól méret és a teljes másolás várt toofinish kisebb, mint egy óra)

- [Nagy méretű táblákra vonatkozó parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (várt hosszabb, mint egy órával toocopy tootake)

Kövesse ugyanezt az eljárást a hello [Replikálásengedélyezési](#enable-replication) toocall hello parancsfájl a művelet a következő paraméterek hello:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

hello print_usage() szakasza hello [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) paraméterek részletes leírását.

### <a name="scenarios"></a>Forgatókönyvek

- **Adott táblákra (Teszt1 Teszt2 és Teszt3) másolja az összes sorát, vége: jelenleg szerkesztett (aktuális időbélyeg)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  vagy

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Másolja át a megadott időtartomány adott táblák**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Tiltsa le a replikációt

toodisable replikációs parancsfájllal egy másik parancsfájl művelet található [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Kövesse ugyanezt az eljárást a hello [Replikálásengedélyezési](#enable-replication) toocall hello parancsfájl a művelet a következő paraméterek hello:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

hello print_usage() szakasza hello [parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) paraméterek részletesen ismerteti.

### <a name="scenarios"></a>Forgatókönyvek

- **Tiltsa le a replikációt minden tábla**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  vagy

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- **Tiltsa le a replikációt a megadott táblák (table1 table2 és Tábl3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban, megtudta, hogyan tooconfigure HBase-replikálás két adatközpont között. További információk a HDInsight és HBase, toolearn lásd:

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
