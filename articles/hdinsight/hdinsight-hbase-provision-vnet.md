---
title: "a virtuális hálózat - Azure fürtök aaaCreate HBase |} Microsoft Docs"
description: "Ismerkedés az Azure HDInsight HBase használatával. Ismerje meg, hogyan toocreate HDInsight HBase clusters Azure virtuális hálózaton."
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 097338a5a650bb607a9f6f9ddb59bb88d098b56f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Hozzon létre HBase-fürtökkel a HDInsight az Azure-beli virtuális hálózathoz
Ismerje meg, hogyan toocreate Azure HDInsight HBase clusters a egy [Azure Virtual Network][1].

A virtuális hálózati integráció, HBase-fürtökkel telepített toohello azonos virtuális hálózat az alkalmazások, ezért lehet, hogy alkalmazásokat közvetlenül kommunikálhatnak a HBase. hello előnyöket nyújtja:

* Közvetlen kapcsolatot hello webes alkalmazás toohello csomópontokból hello HBase-fürtöt, amely lehetővé teszi a kommunikációt keresztül HBase Java távoli eljáráshívásnak (RPC) API-k hívása.
* Javítja a teljesítményt a forgalom nem rendelkezik több átjárók és terheléselosztók lépjen.
* hello képességét tooprocess bizalmas adatokat anélkül, hogy egy nyilvános végpontot több biztonságos módon.

### <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Munkaállomás Azure PowerShell-lel**. Lásd: [telepítése és használata az Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).

## <a name="create-hbase-cluster-into-virtual-network"></a>HBase-fürt a virtuális hálózat létrehozása
Ebben a szakaszban egy Linux-alapú HBase fürt létrehozása hello függő Azure Storage-fiók egy Azure-beli virtuális hálózat használatával egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md). Egyéb Fürtlétrehozási módszerekhez és hello beállításainak ismertetése, tanulmányozza a [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md). Egy sablon toocreate Hadoop használatáról további információt a HDInsight-fürtök, a következő témakörben: [létrehozása Hadoop-fürtök Azure Resource Manager-sablonok használata a hdinsightban](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [!NOTE]
> A rendszer néhány tulajdonságokat kódolt hello sablonba. Példa:
>
> * **Hely**: USA keleti régiója 2. régiója
> * **Fürt verziója**: 3.5
> * **A fürt feldolgozó csomópontok száma**: 2. régiója
> * **Alapértelmezett tárfiók**: egy egyedi karakterlánc
> * **Virtuális hálózati név**: &lt;fürt neve >-hálózatok
> * **Virtuális hálózat címtere**: 10.0.0.0/16
> * **Alhálózati név**: Alhalozat_1
> * **Alhálózati címtartományt**: 10.0.0.0/24
>
> &lt;A fürt neve > helyére hello fürtnév hello sablon használata esetén adja meg.
>
>

1. Kattintson a következő kép tooopen hello sablon hello Azure-portálon hello. hello sablon található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. A hello **egyéni telepítési** panelen adja meg a következő tulajdonságai hello:

   * **Előfizetés**: válassza ki a használt Azure-előfizetést toocreate hello HDInsight-fürtöt, a függő tárfiókot hello és hello Azure-beli virtuális hálózat.
   * **Erőforráscsoport**: válasszon **hozzon létre új**, és adjon meg egy új erőforráscsoport neve.
   * **Hely**: hello erőforráscsoport helyének kiválasztására.
   * **ClusterName**: Adjon meg egy nevet hello Hadoop-fürt toobe létrehozni.
   * **A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az **admin**.
   * **SSH-felhasználónév és jelszó**: hello alapértelmezett felhasználónév az **sshuser**.  Ezt át lehet nevezni.
   * **Elfogadom toohello feltételek mellett hello fenti**: (válassza ki)
3. Kattintson a **Purchase** (Vásárlás) gombra. Vesz igénybe körülbelül 20 percet toocreate egy fürt. Hello fürt létrehozása után kattintson a fürt paneljén hello hello portál tooopen azt.

Hello az oktatóanyag befejezése után érdemes toodelete hello fürt. A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban. Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban. Mivel hello díjak hello fürt sokszor több mint hello a tárolási, érdemes gazdasági toodelete fürtök amikor nincsenek használatban. A fürtök törlésével a hello utasításokért lásd: [kezelése Hadoop-fürtöket a HDInsight használatával hello Azure-portálon](hdinsight-administer-use-management-portal.md#delete-clusters).

az új HBase-fürtöt használata toobegin található hello eljárásokkal [HBase a Hadoop HDInsight használatának megkezdésében](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-toohello-hbase-cluster-using-hbase-java-rpc-apis"></a>Csatlakozás toohello HBase-fürtöt HBase Java RPC API-k használatával
1. Hozzon létre egy infrastruktúra (IaaS) szolgáltatás virtuális gépként történő hello azonos Azure virtuális hálózatban, és hello ugyanazon az alhálózaton. Egy új infrastruktúra-szolgáltatási virtuális gép létrehozása, lásd: [hozzon létre egy virtuális gép futó Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Hello lépések ebben a dokumentumban, a következő értékek hello hálózati konfiguráció hello kell használni:

   * **Virtuális hálózati**: &lt;fürt neve >-hálózatok
   * **Alhálózati**: Alhalozat_1

   > [!IMPORTANT]
   > Cserélje le &lt;fürtnév > hello nevű előző lépésekben hello HDInsight-fürt létrehozásakor használt.
   >
   >

   Használja ezeket az értékeket, hello virtuális gép el van helyezve a hello azonos virtuális hálózati és alhálózati hello HDInsight-fürttel. Ez a konfiguráció lehetővé teszi, hogy azok toodirectly kommunikálnak egymással. Nincs olyan módon toocreate egy üres élcsomópontot a HDInsight-fürtöt. hello élcsomópont használt toomanage hello tárolófürt is lehet.  További információkért lásd: [üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md).

2. Ha egy Java-alkalmazás tooconnect tooHBase távolról, hello teljesen minősített tartománynevét (FQDN) kell használnia. toodetermine, ha előbb telepítik azokra hello kapcsolatspecifikus DNS-utótagja hello HBase-fürtöt. toodo, használható hello a következő módszerek egyikét:

   * Egy webes böngésző toomake egy Ambari hívást használhatja:

     Keresse meg a toohttps: / /&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / üzemeltető? minimal_response = true. A JSON-fájl, a DNS-utótagok hello változik.
   * Hello Ambari webhelyet használja:

     1. Keresse meg a túl https://&lt;ClusterName >. azurehdinsight.net.
     2. Kattintson a **állomások** hello felső menüjében.
   * Használja a Curl toomake REST-hívást:

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     Hello JavaScript Object Notation (JSON) visszaadott adatok hello "gazdaszámítógép_neve" bejegyzés található. Hello FQDN hello fürt hello csomópontján tartalmaz. Példa:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     hello hello tartomány neve hello fürtnév kezdetű része hello DNS-utótagot. Például mycluster.b1.cloudapp.net.
   * Azure PowerShell használatával

     A következő Azure PowerShell parancsfájl tooregister hello használata hello **Get-ClusterDetail** függvény, amely lehet használt tooreturn hello DNS-utótag:

    ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information toofacilitate an HDInsight cluster-to-cluster scenario within hello same virtual network.
            .Description
            This command shows hello following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run hello HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows hello FQDN suffix of hosts in hello cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows hello value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run hello HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows hello FQDN suffix of hosts in hello cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
    ```

     Hello Azure PowerShell parancsfájl futtatását, miután használata hello következő tooreturn hello DNS-utótag parancs hello segítségével **Get-ClusterDetail** függvény. Ez a parancs használata esetén adja meg a HDInsight HBase fürt nevét, a felügyeleti neve és a rendszergazdai jelszó.

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     Ez a parancs visszaadja a hello DNS-utótagot. Például **yourclustername.b4.internal.cloudapp.net**.


<!--
3.    Change hello primary DNS suffix configuration of hello virtual machine. This enables hello virtual machine tooautomatically resolve hello host name of hello HBase cluster without explicit specification of hello suffix. For example, hello *workernode0* host name will be correctly resolved tooworkernode0 of hello HBase cluster.

    toomake hello configuration change:

    1. RDP into hello virtual machine.
    2. Open **Local Group Policy Editor**. hello executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** toohello value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot hello virtual machine.
-->

amely a virtuális gép hello tooverify is kommunikálni hello HBase-fürtöt, hello paranccsal `ping headnode0.<dns suffix>` hello virtuális gépről. Például a ping headnode0.mycluster.b1.cloudapp.net.

toouse ezt az információt Java-alkalmazások, a lépésekkel hello a [Maven használata toobuild Java-alkalmazások a HBase a HDInsight (Hadoop) használó](hdinsight-hbase-build-java-maven.md) toocreate kérelmet. Csatlakozzon a távoli kiszolgáló tooa a HBase, előbb módosítsa a hello toohave hello alkalmazás **hbase-site.xml** Zookeeper a példa toouse hello FQDN fájlt. Példa:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> További információ a névfeloldás az Azure virtuális hálózataihoz egyebek között toouse a saját DNS-kiszolgáló lásd: [Name (DNS) névfeloldási](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
>
>

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban, megtudta, hogyan toocreate HBase-fürtöt. toolearn több, lásd:

* [Első lépései a hdinsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md)
* [HBase-replikálás konfigurálása a HDInsightban](hdinsight-hbase-replication.md)
* [Hdinsight Hadoop-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md)
* [A HBase a Hadoop HDInsight használatának megkezdése](hdinsight-hbase-tutorial-get-started.md)
* [A HBase a hdinsight eszközben Twitter sentiment elemzése](hdinsight-hbase-analyze-twitter-sentiment.md)
* [Virtual Network áttekintése][vnet-overview]

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: /powershell/azureps-cmdlets-docs


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Hello új HBase fürtöt rendelkezés részletei"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Parancsfájlművelet toocustomize HBase-fürtöt használja"

[azure-preview-portal]: https://portal.azure.com
