---
title: "virtuális hálózatok - Azure HDInsight használatával aaaConnect tooKafka |} Microsoft Docs"
description: "Ismerje meg, hogyan toodirectly tooKafka a HDInsight-on keresztül csatlakozni az Azure virtuális hálózat. Ismerje meg, hogyan tooconnect tooKafka fejlesztési VPN-átjáró használatával, vagy az ügyfelek a helyszíni hálózati, a VPN-átjáró eszköz segítségével."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.custom: hdinsightactive
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/01/2017
ms.author: larryfr
ms.openlocfilehash: 03542fe14b9a1d010dffa22a8f8d96b098a1576e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tookafka-on-hdinsight-preview-through-an-azure-virtual-network"></a>A HDInsight (előzetes verzió) tooKafka csatlakoznak az Azure virtuális hálózat

Ismerje meg, hogyan kapcsolódnak az toodirectly a tooKafka a HDInsight az Azure virtuális hálózatok használatával. Ez a dokumentum információkat biztosít a használatával a következő konfigurációk hello tooKafka csatlakozó:

* Az egy helyi hálózatán lévő erőforrásokat. Ezt a kapcsolatot a helyi hálózaton a VPN-eszköz (szoftveres vagy hardveres) használatával.
* A környezet a VPN szoftver ügyfél használatával.

## <a name="architecture-and-planning"></a>Architektúra és tervezése

A HDInsight nem teszi lehetővé a közvetlen kapcsolat tooKafka keresztül hello nyilvános internethez. Ehelyett Kafka ügyfelek (létrehozói és felhasználói) egyikét kell használnia a következő kapcsolat módszerek hello:

* Hello hello ügyféllel a HDInsight Kafka megegyező virtuális hálózatban. Ebben a konfigurációban használatos hello [Apache Kafka (előzetes verzió) a HDInsight kezdődnie](hdinsight-apache-kafka-get-started.md) dokumentum. hello közvetlenül az ügyfél futtatása hello HDInsight fürtcsomópontok vagy egy másik virtuális gépén hello azonos hálózati.

* A magánhálózaton, mint például a helyszíni hálózat toohello virtuális hálózati kapcsolatot. Ez a konfiguráció lehetővé teszi az ügyfelek a helyi hálózati toodirectly használata Kafka. tooenable ezt a konfigurációt, hajtsa végre a következő feladatok hello:

    1. Hozzon létre egy virtuális hálózatot.
    2. Hozzon létre egy VPN-átjáró, amely webhelyek konfigurációt használ. a dokumentumban használt hello konfigurációs tooa VPN-átjáróeszközt a helyszíni hálózat csatlakozik.
    3. Hozzon létre egy DNS-kiszolgáló hello virtuális hálózat.
    4. Konfigurálja mindegyik hálózati hello DNS-kiszolgáló között.
    5. Telepítse a HDInsight Kafka hello virtuális hálózat.

    További információkért lásd: hello [tooKafka csatlakozás egy helyszíni hálózatból](#on-premises) szakasz. 

* Csatlakoztassa az egyes gépek toohello virtuális hálózat VPN-átjáró és a VPN-ügyfél segítségével. tooenable ezt a konfigurációt, hajtsa végre a következő feladatok hello:

    1. Hozzon létre egy virtuális hálózatot.
    2. Hozzon létre egy pont – hely konfigurációt használó VPN-átjáró. Ez a konfiguráció biztosítja a VPN-ügyfél, amely a Windows-ügyfelek is telepíthető.
    3. Telepítse a HDInsight Kafka hello virtuális hálózat.
    4. IP-hirdetési Kafka konfigurálása. Ez a konfiguráció lehetővé teszi, hogy a hello ügyfél tooconnect IP-címek helyett tartománynevek használatával.
    5. Töltse le, és használja a VPN-ügyfél hello hello fejlesztői rendszeren.

    További információkért lásd: hello [tooKafka csatlakozzon a VPN-ügyfél](#vpnclient) szakasz.

    > [!WARNING]
    > Ez a konfiguráció csak a következő korlátozások hello miatt ajánlott fejlesztési célokra szolgál:
    >
    > * Minden ügyfél szoftver VPN-ügyfél használatával kell csatlakoztatni. Azure csak egy Windows-alapú ügyfelek tartalmazza.
    > * hello ügyfél nem felel meg neve feloldási kérések toohello virtuális hálózat, így IP-címzési toocommunicate Kafka együtt kell használni. Integrációs csomaggal folytatott kommunikációhoz hello Kafka fürt további konfigurálása szükséges.

A HDInsight eszközzel virtuális hálózatban további információkért lásd: [kiterjesztése HDInsight Azure virtuális hálózatok használatával](./hdinsight-extend-hadoop-virtual-network.md).

## <a id="on-premises"></a>TooKafka csatlakozás egy helyszíni hálózatból

a helyszíni hálózat kommunikáló Kafka fürt toocreate kövesse hello hello [csatlakozás HDInsight tooyour a helyszíni hálózat](./connect-on-premises-network.md) dokumentum.

> [!IMPORTANT]
> Hello HDInsight-fürt létrehozásakor válassza ki a hello __Kafka__ fürt típusa.

Ezeket a lépéseket hozza létre a következő konfigurációs hello:

* Azure Virtual Network
* Webhelyek közötti VPN átjáró
* Az Azure Storage-fiók (a HDInsight által használt)
* A HDInsight Kafka

hogy Kafka ügyfél toohello fürt kapcsolódhatnak a helyszíni hello használata hello lépéseit tooverify [példa: Python ügyfél](#python-client) szakasz.

## <a id="vpnclient"></a>Csatlakozás tooKafka egy VPN-ügyfél

Kövesse a következő konfigurációs szakasz toocreate hello hello lépéseket:

* Azure Virtual Network
* Pont-pont VPN-átjáró
* Az Azure Storage-fiók (a HDInsight által használt)
* A HDInsight Kafka

1. Hello kövesse hello [pont – hely kapcsolatok önaláírt tanúsítványok használata](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentum. Ez a dokumentum hello átjáró számára szükséges hello tanúsítványokat hoz létre.

2. Nyisson meg egy PowerShell-parancssorba, és használja a következő kód toolog az Azure-előfizetés tooyour hello:

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment tooset hello subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. A következő konfigurációs adatokat tartalmazó kód toocreate változók hello használata:

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is hello resource group name?"
    $baseName = Read-Host "What is hello base name? It is used toocreate names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want toocreate hello resources in?"
    $rootCert = Read-Host "What is hello file path toohello root certificate? It is used toosecure hello VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter hello HTTPS user name and password for hello HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter hello SSH user name and password for hello HDInsight cluster" -UserName "sshuser"

    # Names for Azure resources
    $networkName = "net-$baseName"
    $clusterName = "kafka-$baseName"
    $storageName = "store$baseName" # Can't use dashes in storage names
    $defaultContainerName = $clusterName
    $defaultSubnetName = "default"
    $gatewaySubnetName = "GatewaySubnet"
    $gatewayPublicIpName = "GatewayIp"
    $gatewayIpConfigName = "GatewayConfig"
    $vpnRootCertName = "rootcert"
    $vpnName = "VPNGateway"

    # Network settings
    $networkAddressPrefix = "10.0.0.0/16"
    $defaultSubnetPrefix = "10.0.0.0/24"
    $gatewaySubnetPrefix = "10.0.1.0/24"
    $vpnClientAddressPool = "172.16.201.0/24"

    # HDInsight settings
    $HdiWorkerNodes = 4
    $hdiVersion = "3.5"
    $hdiType = "Kafka"
    ```

4. Használjon hello következő kódot a toocreate hello Azure-erőforráscsoportot és a virtuális hálózat:

    ```powershell
    # Create hello resource group that contains everything
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create hello subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get hello network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for hello gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get hello certificate info
    # Get hello full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create hello VPN gateway
    New-AzureRmVirtualNetworkGateway -Name $vpnName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -IpConfigurations $gatewayIpConfig `
        -GatewayType Vpn `
        -VpnType RouteBased `
        -EnableBgp $false `
        -GatewaySku Standard `
        -VpnClientAddressPool $vpnClientAddressPool `
        -VpnClientRootCertificates $p2sRootCert
    ```

    > [!WARNING]
    > A folyamat toocomplete több percet is igénybe vehet.

5. A következő kód toocreate hello Azure-Tárfiókot és blob tároló hello használata:

    ```powershell
    # Create hello storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get hello storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create hello default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. A következő kód toocreate hello HDInsight-fürt hello használata:

    ```powershell
    # Create hello HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $hdiWorkerNodes `
        -ClusterType $hdiType `
        -OSType Linux `
        -Version $hdiVersion `
        -HttpCredential $adminCreds `
        -SshCredential $sshCreds `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageKey `
        -DefaultStorageContainer $defaultContainerName `
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

  > [!WARNING]
  > Ez a folyamat toocomplete körülbelül 20 percet vesz igénybe.

8. A következő parancsmag tooretrieve hello URL-címe hello Windows VPN-ügyfél hello virtuális hálózat hello használata:

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    toodownload hello Windows VPN-ügyfél használata hello URI vissza a webböngészőben.

### <a name="configure-kafka-for-ip-advertising"></a>IP-hirdetési Kafka konfigurálása

Alapértelmezés szerint a Zookeeper hello Kafka brókerek tooclients hello tartomány nevét adja vissza. Ez a konfiguráció nem működik hello VPN szoftver ügyfél, mivel névfeloldás hello virtuális hálózaton található entitások nem használható. Ehhez a konfigurációhoz, használja a hello következő lépések tooconfigure Kafka tooadvertise IP-címek tartománynevek helyett:

1. Webböngésző segítségével, nyissa meg toohttps://CLUSTERNAME.azurehdinsight.net. Cserélje le __CLUSTERNAME__ hello hello Kafka HDInsight-fürt nevére.

    Amikor a rendszer kéri, hello fürt használni hello HTTPS felhasználónevet és jelszót. Ambari webes felhasználói felületén hello hello fürt jelenik meg.

2. tooview információk Kafka, válassza ki __Kafka__ hello bal hello listáról.

    ![A kijelölt Kafka szolgáltatás lista](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. tooview Kafka konfigurációs kiválasztása __Configs__ hello felső középről.

    ![Kafka Configs hivatkozások](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. toofind hello __kafka-env__ konfigurációs, adja meg `kafka-env` a hello __szűrő__ hello jobb felső részén található.

    ![Kafka konfigurációját, kafka-env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. tooconfigure Kafka tooadvertise IP-címek, adja hozzá a következő szöveg toohello alsó részén hello hello __kafka-env-sablon__ mező:

    ```
    # Configure Kafka tooadvertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. tooconfigure hello felület, amely figyeli Kafka, adja meg `listeners` a hello __szűrő__ hello jobb felső részén található.

7. a hálózati adaptereken, hello értékének módosítása a hello Kafka toolisten tooconfigure __figyelői__ túl mezőben`PLAINTEXT://0.0.0.0:9092`.

8. toosave hello konfigurációs módosításokat, használja a hello __mentése__ gombra. Adjon meg egy szöveges üzenetet, hello változásokat ismertető. Válassza ki __OK__ hello módosítások mentése után.

    ![Mentse a konfigurációs gomb](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. tooprevent hibák Kafka, újraindításához használja hello __szolgáltatás műveletek__ gombra, majd az __kapcsolja be a karbantartási mód__. Válassza ki a OK toocomplete ezt a műveletet.

    ![Szolgáltatási művelet, amely a kijelölt karbantartási bekapcsolása](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. toorestart Kafka, használja a hello __indítsa újra a__ gombra, majd az __indítsa újra az összes érintett__. Erősítse meg a hello újraindítása, és kövesse a hello __OK__ hello művelet befejeződése után gombra.

    ![Indítsa újra a gomb, újraindítással minden érintett](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. toodisable a karbantartási mód használata hello __szolgáltatás műveletek__ gombra, majd az __kapcsolja ki a karbantartási mód__. Válassza ki **OK** toocomplete ezt a műveletet.

### <a name="connect-toohello-vpn-gateway"></a>Csatlakozás toohello VPN-átjáró

tooconnect toohello VPN-átjárót egy __Windows ügyfél__, hello használata __tooAzure csatlakozás__ hello szakasza [egy pont – hely kapcsolat beállítása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentum.

## <a id="python-client"></a>Példa: Python ügyfél

toovalidate kapcsolat tooKafka, használja a következő lépéseket toocreate hello, és futtassa a Python-készítő és a fogyasztói:

1. Használja a következő módszerek tooretrieve hello teljesen hello a minősített tartománynevét (FQDN) és a hello csomópontok hello Kafka fürt IP-címek:

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Ez a parancsfájl feltételezi, hogy `$resourceGroupName` hello neve, amely tartalmazza a virtuális hálózati hello hello Azure erőforráscsoport.

    Visszaadott adatokat hello következő lépésekben hello mentése.

2. A következő tooinstall hello használata hello [kafka-python](http://kafka-python.readthedocs.io/) ügyfél:

        pip install kafka-python

3. toosend adatok tooKafka, használjon hello Python kódját a következő:

  ```python
  from kafka import KafkaProducer
  # Replace hello `ip_address` entries with hello IP address of your worker nodes
  # NOTE: you don't need hello full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    Cserélje le a hello `'kafka_broker'` hello címekkel bejegyzések kapott ebben a szakaszban 1. lépés:

    * Használatakor a __szoftver VPN-ügyfél__, cserélje le a hello `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok hello IP-címét.

    * Ha rendelkezik __engedélyezve van egy egyéni DNS-kiszolgálón keresztül névfeloldás__, cserélje le a hello `kafka_broker` hello hello munkavégző csomópont FQDN-bejegyzést.

    > [!NOTE]
    > Ez a kód küldi hello karakterlánc `test message` toohello témakör `testtopic`. hello alapértelmezett konfigurálása a HDInsight Kafka toocreate hello témakör számára, ha nem létezik.

4. tooretrieve köszönőüzenetei Kafka, a Python kódját a következő hello használata:

   ```python
   from kafka import KafkaConsumer
   # Replace hello `ip_address` entries with hello IP address of your worker nodes
   # Again, you only need one or two, not hello full list.
   # Note: auto_offset_reset='earliest' resets hello starting offset toohello beginning
   #       of hello topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    Cserélje le a hello `'kafka_broker'` hello címekkel bejegyzések kapott ebben a szakaszban 1. lépés:

    * Használatakor a __szoftver VPN-ügyfél__, cserélje le a hello `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok hello IP-címét.

    * Ha rendelkezik __engedélyezve van egy egyéni DNS-kiszolgálón keresztül névfeloldás__, cserélje le a hello `kafka_broker` hello hello munkavégző csomópont FQDN-bejegyzést.

## <a name="next-steps"></a>Következő lépések

További információ a HDInsight használata a virtuális hálózaton: hello [kiterjesztése Azure HDInsight Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md) dokumentum.

Az Azure virtuális hálózat létrehozása a pont – hely típusú VPN-átjáróval további információkért lásd: a következő dokumentumok hello:

* [Egy pont – hely kapcsolat hello Azure-portál konfigurálása](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Az Azure PowerShell pont – hely kapcsolat konfigurálása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

További információk a hdinsight Kafka tekintse meg a következő dokumentumok hello:

* [A HDInsighton futó Kafka használatának első lépései](hdinsight-apache-kafka-get-started.md)
* [A HDInsight Kafka a tükrözés használata](hdinsight-apache-kafka-mirroring.md)
