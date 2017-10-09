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
# <a name="connect-tookafka-on-hdinsight-preview-through-an-azure-virtual-network"></a><span data-ttu-id="df941-104">A HDInsight (előzetes verzió) tooKafka csatlakoznak az Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="df941-104">Connect tooKafka on HDInsight (preview) through an Azure Virtual Network</span></span>

<span data-ttu-id="df941-105">Ismerje meg, hogyan kapcsolódnak az toodirectly a tooKafka a HDInsight az Azure virtuális hálózatok használatával.</span><span class="sxs-lookup"><span data-stu-id="df941-105">Learn how toodirectly connect tooKafka on HDInsight using Azure Virtual Networks.</span></span> <span data-ttu-id="df941-106">Ez a dokumentum információkat biztosít a használatával a következő konfigurációk hello tooKafka csatlakozó:</span><span class="sxs-lookup"><span data-stu-id="df941-106">This document provides information on connecting tooKafka using hello following configurations:</span></span>

* <span data-ttu-id="df941-107">Az egy helyi hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="df941-107">From resources in an on-premises network.</span></span> <span data-ttu-id="df941-108">Ezt a kapcsolatot a helyi hálózaton a VPN-eszköz (szoftveres vagy hardveres) használatával.</span><span class="sxs-lookup"><span data-stu-id="df941-108">This connection is established by using a VPN device (software or hardware) on your local network.</span></span>
* <span data-ttu-id="df941-109">A környezet a VPN szoftver ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="df941-109">From a development environment using a VPN software client.</span></span>

## <a name="architecture-and-planning"></a><span data-ttu-id="df941-110">Architektúra és tervezése</span><span class="sxs-lookup"><span data-stu-id="df941-110">Architecture and planning</span></span>

<span data-ttu-id="df941-111">A HDInsight nem teszi lehetővé a közvetlen kapcsolat tooKafka keresztül hello nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="df941-111">HDInsight does not allow direct connection tooKafka over hello public internet.</span></span> <span data-ttu-id="df941-112">Ehelyett Kafka ügyfelek (létrehozói és felhasználói) egyikét kell használnia a következő kapcsolat módszerek hello:</span><span class="sxs-lookup"><span data-stu-id="df941-112">Instead, Kafka clients (producers and consumers) must use one of hello following connection methods:</span></span>

* <span data-ttu-id="df941-113">Hello hello ügyféllel a HDInsight Kafka megegyező virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="df941-113">Run hello client in hello same virtual network as Kafka on HDInsight.</span></span> <span data-ttu-id="df941-114">Ebben a konfigurációban használatos hello [Apache Kafka (előzetes verzió) a HDInsight kezdődnie](hdinsight-apache-kafka-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="df941-114">This configuration is used in hello [Start with Apache Kafka (preview) on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span> <span data-ttu-id="df941-115">hello közvetlenül az ügyfél futtatása hello HDInsight fürtcsomópontok vagy egy másik virtuális gépén hello azonos hálózati.</span><span class="sxs-lookup"><span data-stu-id="df941-115">hello client runs directly on hello HDInsight cluster nodes or on another virtual machine in hello same network.</span></span>

* <span data-ttu-id="df941-116">A magánhálózaton, mint például a helyszíni hálózat toohello virtuális hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="df941-116">Connect a private network, such as your on-premises network, toohello virtual network.</span></span> <span data-ttu-id="df941-117">Ez a konfiguráció lehetővé teszi az ügyfelek a helyi hálózati toodirectly használata Kafka.</span><span class="sxs-lookup"><span data-stu-id="df941-117">This configuration allows clients in your on-premises network toodirectly work with Kafka.</span></span> <span data-ttu-id="df941-118">tooenable ezt a konfigurációt, hajtsa végre a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="df941-118">tooenable this configuration, perform hello following tasks:</span></span>

    1. <span data-ttu-id="df941-119">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="df941-119">Create a virtual network.</span></span>
    2. <span data-ttu-id="df941-120">Hozzon létre egy VPN-átjáró, amely webhelyek konfigurációt használ.</span><span class="sxs-lookup"><span data-stu-id="df941-120">Create a VPN gateway that uses a site-to-site configuration.</span></span> <span data-ttu-id="df941-121">a dokumentumban használt hello konfigurációs tooa VPN-átjáróeszközt a helyszíni hálózat csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="df941-121">hello configuration used in this document connects tooa VPN gateway device in your on-premises network.</span></span>
    3. <span data-ttu-id="df941-122">Hozzon létre egy DNS-kiszolgáló hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="df941-122">Create a DNS server in hello virtual network.</span></span>
    4. <span data-ttu-id="df941-123">Konfigurálja mindegyik hálózati hello DNS-kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="df941-123">Configure forwarding between hello DNS server in each network.</span></span>
    5. <span data-ttu-id="df941-124">Telepítse a HDInsight Kafka hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="df941-124">Install Kafka on HDInsight into hello virtual network.</span></span>

    <span data-ttu-id="df941-125">További információkért lásd: hello [tooKafka csatlakozás egy helyszíni hálózatból](#on-premises) szakasz.</span><span class="sxs-lookup"><span data-stu-id="df941-125">For more information, see hello [Connect tooKafka from an on-premises network](#on-premises) section.</span></span> 

* <span data-ttu-id="df941-126">Csatlakoztassa az egyes gépek toohello virtuális hálózat VPN-átjáró és a VPN-ügyfél segítségével.</span><span class="sxs-lookup"><span data-stu-id="df941-126">Connect individual machines toohello virtual network using a VPN gateway and VPN client.</span></span> <span data-ttu-id="df941-127">tooenable ezt a konfigurációt, hajtsa végre a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="df941-127">tooenable this configuration, perform hello following tasks:</span></span>

    1. <span data-ttu-id="df941-128">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="df941-128">Create a virtual network.</span></span>
    2. <span data-ttu-id="df941-129">Hozzon létre egy pont – hely konfigurációt használó VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="df941-129">Create a VPN gateway that uses a point-to-site configuration.</span></span> <span data-ttu-id="df941-130">Ez a konfiguráció biztosítja a VPN-ügyfél, amely a Windows-ügyfelek is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="df941-130">This configuration provides a VPN client that can be installed on Windows clients.</span></span>
    3. <span data-ttu-id="df941-131">Telepítse a HDInsight Kafka hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="df941-131">Install Kafka on HDInsight into hello virtual network.</span></span>
    4. <span data-ttu-id="df941-132">IP-hirdetési Kafka konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="df941-132">Configure Kafka for IP advertising.</span></span> <span data-ttu-id="df941-133">Ez a konfiguráció lehetővé teszi, hogy a hello ügyfél tooconnect IP-címek helyett tartománynevek használatával.</span><span class="sxs-lookup"><span data-stu-id="df941-133">This configuration allows hello client tooconnect using IP addressing instead of domain names.</span></span>
    5. <span data-ttu-id="df941-134">Töltse le, és használja a VPN-ügyfél hello hello fejlesztői rendszeren.</span><span class="sxs-lookup"><span data-stu-id="df941-134">Download and use hello VPN client on hello development system.</span></span>

    <span data-ttu-id="df941-135">További információkért lásd: hello [tooKafka csatlakozzon a VPN-ügyfél](#vpnclient) szakasz.</span><span class="sxs-lookup"><span data-stu-id="df941-135">For more information, see hello [Connect tooKafka with a VPN client](#vpnclient) section.</span></span>

    > [!WARNING]
    > <span data-ttu-id="df941-136">Ez a konfiguráció csak a következő korlátozások hello miatt ajánlott fejlesztési célokra szolgál:</span><span class="sxs-lookup"><span data-stu-id="df941-136">This configuration is only recommended for development purposes because of hello following limitations:</span></span>
    >
    > * <span data-ttu-id="df941-137">Minden ügyfél szoftver VPN-ügyfél használatával kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="df941-137">Each client must connect using a VPN software client.</span></span> <span data-ttu-id="df941-138">Azure csak egy Windows-alapú ügyfelek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="df941-138">Azure only provides a Windows-based client.</span></span>
    > * <span data-ttu-id="df941-139">hello ügyfél nem felel meg neve feloldási kérések toohello virtuális hálózat, így IP-címzési toocommunicate Kafka együtt kell használni.</span><span class="sxs-lookup"><span data-stu-id="df941-139">hello client does not pass name resolution requests toohello virtual network, so you must use IP addressing toocommunicate with Kafka.</span></span> <span data-ttu-id="df941-140">Integrációs csomaggal folytatott kommunikációhoz hello Kafka fürt további konfigurálása szükséges.</span><span class="sxs-lookup"><span data-stu-id="df941-140">IP communication requires additional configuration on hello Kafka cluster.</span></span>

<span data-ttu-id="df941-141">A HDInsight eszközzel virtuális hálózatban további információkért lásd: [kiterjesztése HDInsight Azure virtuális hálózatok használatával](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="df941-141">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

## <span data-ttu-id="df941-142"><a id="on-premises"></a>TooKafka csatlakozás egy helyszíni hálózatból</span><span class="sxs-lookup"><span data-stu-id="df941-142"><a id="on-premises"></a> Connect tooKafka from an on-premises network</span></span>

<span data-ttu-id="df941-143">a helyszíni hálózat kommunikáló Kafka fürt toocreate kövesse hello hello [csatlakozás HDInsight tooyour a helyszíni hálózat](./connect-on-premises-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="df941-143">toocreate a Kafka cluster that communicates with your on-premises network, follow hello steps in hello [Connect HDInsight tooyour on-premises network](./connect-on-premises-network.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df941-144">Hello HDInsight-fürt létrehozásakor válassza ki a hello __Kafka__ fürt típusa.</span><span class="sxs-lookup"><span data-stu-id="df941-144">When creating hello HDInsight cluster, select hello __Kafka__ cluster type.</span></span>

<span data-ttu-id="df941-145">Ezeket a lépéseket hozza létre a következő konfigurációs hello:</span><span class="sxs-lookup"><span data-stu-id="df941-145">These steps create hello following configuration:</span></span>

* <span data-ttu-id="df941-146">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="df941-146">Azure Virtual Network</span></span>
* <span data-ttu-id="df941-147">Webhelyek közötti VPN átjáró</span><span class="sxs-lookup"><span data-stu-id="df941-147">Site-to-site VPN gateway</span></span>
* <span data-ttu-id="df941-148">Az Azure Storage-fiók (a HDInsight által használt)</span><span class="sxs-lookup"><span data-stu-id="df941-148">Azure Storage account (used by HDInsight)</span></span>
* <span data-ttu-id="df941-149">A HDInsight Kafka</span><span class="sxs-lookup"><span data-stu-id="df941-149">Kafka on HDInsight</span></span>

<span data-ttu-id="df941-150">hogy Kafka ügyfél toohello fürt kapcsolódhatnak a helyszíni hello használata hello lépéseit tooverify [példa: Python ügyfél](#python-client) szakasz.</span><span class="sxs-lookup"><span data-stu-id="df941-150">tooverify that a Kafka client can connect toohello cluster from on-premises, use hello steps in hello [Example: Python client](#python-client) section.</span></span>

## <span data-ttu-id="df941-151"><a id="vpnclient"></a>Csatlakozás tooKafka egy VPN-ügyfél</span><span class="sxs-lookup"><span data-stu-id="df941-151"><a id="vpnclient"></a> Connect tooKafka with a VPN client</span></span>

<span data-ttu-id="df941-152">Kövesse a következő konfigurációs szakasz toocreate hello hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="df941-152">Use hello steps in this section toocreate hello following configuration:</span></span>

* <span data-ttu-id="df941-153">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="df941-153">Azure Virtual Network</span></span>
* <span data-ttu-id="df941-154">Pont-pont VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="df941-154">Point-to-site VPN gateway</span></span>
* <span data-ttu-id="df941-155">Az Azure Storage-fiók (a HDInsight által használt)</span><span class="sxs-lookup"><span data-stu-id="df941-155">Azure Storage Account (used by HDInsight)</span></span>
* <span data-ttu-id="df941-156">A HDInsight Kafka</span><span class="sxs-lookup"><span data-stu-id="df941-156">Kafka on HDInsight</span></span>

1. <span data-ttu-id="df941-157">Hello kövesse hello [pont – hely kapcsolatok önaláírt tanúsítványok használata](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="df941-157">Follow hello steps in hello [Working with self-signed certificates for Point-to-site connections](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) document.</span></span> <span data-ttu-id="df941-158">Ez a dokumentum hello átjáró számára szükséges hello tanúsítványokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="df941-158">This document creates hello certificates needed for hello gateway.</span></span>

2. <span data-ttu-id="df941-159">Nyisson meg egy PowerShell-parancssorba, és használja a következő kód toolog az Azure-előfizetés tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="df941-159">Open a PowerShell prompt and use hello following code toolog in tooyour Azure subscription:</span></span>

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment tooset hello subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. <span data-ttu-id="df941-160">A következő konfigurációs adatokat tartalmazó kód toocreate változók hello használata:</span><span class="sxs-lookup"><span data-stu-id="df941-160">Use hello following code toocreate variables that contain configuration information:</span></span>

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

4. <span data-ttu-id="df941-161">Használjon hello következő kódot a toocreate hello Azure-erőforráscsoportot és a virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="df941-161">Use hello following code toocreate hello Azure resource group and virtual network:</span></span>

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
    > <span data-ttu-id="df941-162">A folyamat toocomplete több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="df941-162">It can take several minutes for this process toocomplete.</span></span>

5. <span data-ttu-id="df941-163">A következő kód toocreate hello Azure-Tárfiókot és blob tároló hello használata:</span><span class="sxs-lookup"><span data-stu-id="df941-163">Use hello following code toocreate hello Azure Storage Account and blob container:</span></span>

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

6. <span data-ttu-id="df941-164">A következő kód toocreate hello HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="df941-164">Use hello following code toocreate hello HDInsight cluster:</span></span>

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
  > <span data-ttu-id="df941-165">Ez a folyamat toocomplete körülbelül 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="df941-165">This process takes around 20 minutes toocomplete.</span></span>

8. <span data-ttu-id="df941-166">A következő parancsmag tooretrieve hello URL-címe hello Windows VPN-ügyfél hello virtuális hálózat hello használata:</span><span class="sxs-lookup"><span data-stu-id="df941-166">Use hello following cmdlet tooretrieve hello URL for hello Windows VPN client for hello virtual network:</span></span>

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    <span data-ttu-id="df941-167">toodownload hello Windows VPN-ügyfél használata hello URI vissza a webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="df941-167">toodownload hello Windows VPN client, use hello returned URI in your web browser.</span></span>

### <a name="configure-kafka-for-ip-advertising"></a><span data-ttu-id="df941-168">IP-hirdetési Kafka konfigurálása</span><span class="sxs-lookup"><span data-stu-id="df941-168">Configure Kafka for IP advertising</span></span>

<span data-ttu-id="df941-169">Alapértelmezés szerint a Zookeeper hello Kafka brókerek tooclients hello tartomány nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="df941-169">By default, Zookeeper returns hello domain name of hello Kafka brokers tooclients.</span></span> <span data-ttu-id="df941-170">Ez a konfiguráció nem működik hello VPN szoftver ügyfél, mivel névfeloldás hello virtuális hálózaton található entitások nem használható.</span><span class="sxs-lookup"><span data-stu-id="df941-170">This configuration does not work with hello VPN software client, as it cannot use name resolution for entities in hello virtual network.</span></span> <span data-ttu-id="df941-171">Ehhez a konfigurációhoz, használja a hello következő lépések tooconfigure Kafka tooadvertise IP-címek tartománynevek helyett:</span><span class="sxs-lookup"><span data-stu-id="df941-171">For this configuration, use hello following steps tooconfigure Kafka tooadvertise IP addresses instead of domain names:</span></span>

1. <span data-ttu-id="df941-172">Webböngésző segítségével, nyissa meg toohttps://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="df941-172">Using a web browser, go toohttps://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="df941-173">Cserélje le __CLUSTERNAME__ hello hello Kafka HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="df941-173">Replace __CLUSTERNAME__ with hello name of hello Kafka on HDInsight cluster.</span></span>

    <span data-ttu-id="df941-174">Amikor a rendszer kéri, hello fürt használni hello HTTPS felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="df941-174">When prompted, use hello HTTPS user name and password for hello cluster.</span></span> <span data-ttu-id="df941-175">Ambari webes felhasználói felületén hello hello fürt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="df941-175">hello Ambari Web UI for hello cluster is displayed.</span></span>

2. <span data-ttu-id="df941-176">tooview információk Kafka, válassza ki __Kafka__ hello bal hello listáról.</span><span class="sxs-lookup"><span data-stu-id="df941-176">tooview information on Kafka, select __Kafka__ from hello list on hello left.</span></span>

    ![A kijelölt Kafka szolgáltatás lista](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. <span data-ttu-id="df941-178">tooview Kafka konfigurációs kiválasztása __Configs__ hello felső középről.</span><span class="sxs-lookup"><span data-stu-id="df941-178">tooview Kafka configuration, select __Configs__ from hello top middle.</span></span>

    ![Kafka Configs hivatkozások](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. <span data-ttu-id="df941-180">toofind hello __kafka-env__ konfigurációs, adja meg `kafka-env` a hello __szűrő__ hello jobb felső részén található.</span><span class="sxs-lookup"><span data-stu-id="df941-180">toofind hello __kafka-env__ configuration, enter `kafka-env` in hello __Filter__ field on hello upper right.</span></span>

    ![Kafka konfigurációját, kafka-env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. <span data-ttu-id="df941-182">tooconfigure Kafka tooadvertise IP-címek, adja hozzá a következő szöveg toohello alsó részén hello hello __kafka-env-sablon__ mező:</span><span class="sxs-lookup"><span data-stu-id="df941-182">tooconfigure Kafka tooadvertise IP addresses, add hello following text toohello bottom of hello __kafka-env-template__ field:</span></span>

    ```
    # Configure Kafka tooadvertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. <span data-ttu-id="df941-183">tooconfigure hello felület, amely figyeli Kafka, adja meg `listeners` a hello __szűrő__ hello jobb felső részén található.</span><span class="sxs-lookup"><span data-stu-id="df941-183">tooconfigure hello interface that Kafka listens on, enter `listeners` in hello __Filter__ field on hello upper right.</span></span>

7. <span data-ttu-id="df941-184">a hálózati adaptereken, hello értékének módosítása a hello Kafka toolisten tooconfigure __figyelői__ túl mezőben`PLAINTEXT://0.0.0.0:9092`.</span><span class="sxs-lookup"><span data-stu-id="df941-184">tooconfigure Kafka toolisten on all network interfaces, change hello value in hello __listeners__ field too`PLAINTEXT://0.0.0.0:9092`.</span></span>

8. <span data-ttu-id="df941-185">toosave hello konfigurációs módosításokat, használja a hello __mentése__ gombra.</span><span class="sxs-lookup"><span data-stu-id="df941-185">toosave hello configuration changes, use hello __Save__ button.</span></span> <span data-ttu-id="df941-186">Adjon meg egy szöveges üzenetet, hello változásokat ismertető.</span><span class="sxs-lookup"><span data-stu-id="df941-186">Enter a text message describing hello changes.</span></span> <span data-ttu-id="df941-187">Válassza ki __OK__ hello módosítások mentése után.</span><span class="sxs-lookup"><span data-stu-id="df941-187">Select __OK__ once hello changes have been saved.</span></span>

    ![Mentse a konfigurációs gomb](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. <span data-ttu-id="df941-189">tooprevent hibák Kafka, újraindításához használja hello __szolgáltatás műveletek__ gombra, majd az __kapcsolja be a karbantartási mód__.</span><span class="sxs-lookup"><span data-stu-id="df941-189">tooprevent errors when restarting Kafka, use hello __Service Actions__ button and select __Turn On Maintenance Mode__.</span></span> <span data-ttu-id="df941-190">Válassza ki a OK toocomplete ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="df941-190">Select OK toocomplete this operation.</span></span>

    ![Szolgáltatási művelet, amely a kijelölt karbantartási bekapcsolása](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. <span data-ttu-id="df941-192">toorestart Kafka, használja a hello __indítsa újra a__ gombra, majd az __indítsa újra az összes érintett__.</span><span class="sxs-lookup"><span data-stu-id="df941-192">toorestart Kafka, use hello __Restart__ button and select __Restart All Affected__.</span></span> <span data-ttu-id="df941-193">Erősítse meg a hello újraindítása, és kövesse a hello __OK__ hello művelet befejeződése után gombra.</span><span class="sxs-lookup"><span data-stu-id="df941-193">Confirm hello restart, and then use hello __OK__ button after hello operation has completed.</span></span>

    ![Indítsa újra a gomb, újraindítással minden érintett](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. <span data-ttu-id="df941-195">toodisable a karbantartási mód használata hello __szolgáltatás műveletek__ gombra, majd az __kapcsolja ki a karbantartási mód__.</span><span class="sxs-lookup"><span data-stu-id="df941-195">toodisable maintenance mode, use hello __Service Actions__ button and select __Turn Off Maintenance Mode__.</span></span> <span data-ttu-id="df941-196">Válassza ki **OK** toocomplete ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="df941-196">Select **OK** toocomplete this operation.</span></span>

### <a name="connect-toohello-vpn-gateway"></a><span data-ttu-id="df941-197">Csatlakozás toohello VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="df941-197">Connect toohello VPN gateway</span></span>

<span data-ttu-id="df941-198">tooconnect toohello VPN-átjárót egy __Windows ügyfél__, hello használata __tooAzure csatlakozás__ hello szakasza [egy pont – hely kapcsolat beállítása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="df941-198">tooconnect toohello VPN gateway from a __Windows client__, use hello __Connect tooAzure__ section of hello [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) document.</span></span>

## <span data-ttu-id="df941-199"><a id="python-client"></a>Példa: Python ügyfél</span><span class="sxs-lookup"><span data-stu-id="df941-199"><a id="python-client"></a> Example: Python client</span></span>

<span data-ttu-id="df941-200">toovalidate kapcsolat tooKafka, használja a következő lépéseket toocreate hello, és futtassa a Python-készítő és a fogyasztói:</span><span class="sxs-lookup"><span data-stu-id="df941-200">toovalidate connectivity tooKafka, use hello following steps toocreate and run a Python producer and consumer:</span></span>

1. <span data-ttu-id="df941-201">Használja a következő módszerek tooretrieve hello teljesen hello a minősített tartománynevét (FQDN) és a hello csomópontok hello Kafka fürt IP-címek:</span><span class="sxs-lookup"><span data-stu-id="df941-201">Use one of hello following methods tooretrieve hello fully qualified domain name (FQDN) and IP addresses of hello nodes in hello Kafka cluster:</span></span>

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

    <span data-ttu-id="df941-202">Ez a parancsfájl feltételezi, hogy `$resourceGroupName` hello neve, amely tartalmazza a virtuális hálózati hello hello Azure erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="df941-202">This script assumes that `$resourceGroupName` is hello name of hello Azure resource group that contains hello virtual network.</span></span>

    <span data-ttu-id="df941-203">Visszaadott adatokat hello következő lépésekben hello mentése.</span><span class="sxs-lookup"><span data-stu-id="df941-203">Save hello returned information for use in hello next steps.</span></span>

2. <span data-ttu-id="df941-204">A következő tooinstall hello használata hello [kafka-python](http://kafka-python.readthedocs.io/) ügyfél:</span><span class="sxs-lookup"><span data-stu-id="df941-204">Use hello following tooinstall hello [kafka-python](http://kafka-python.readthedocs.io/) client:</span></span>

        pip install kafka-python

3. <span data-ttu-id="df941-205">toosend adatok tooKafka, használjon hello Python kódját a következő:</span><span class="sxs-lookup"><span data-stu-id="df941-205">toosend data tooKafka, use hello following Python code:</span></span>

  ```python
  from kafka import KafkaProducer
  # Replace hello `ip_address` entries with hello IP address of your worker nodes
  # NOTE: you don't need hello full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    <span data-ttu-id="df941-206">Cserélje le a hello `'kafka_broker'` hello címekkel bejegyzések kapott ebben a szakaszban 1. lépés:</span><span class="sxs-lookup"><span data-stu-id="df941-206">Replace hello `'kafka_broker'` entries with hello addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="df941-207">Használatakor a __szoftver VPN-ügyfél__, cserélje le a hello `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="df941-207">If you are using a __Software VPN client__, replace hello `kafka_broker` entries with hello IP address of your worker nodes.</span></span>

    * <span data-ttu-id="df941-208">Ha rendelkezik __engedélyezve van egy egyéni DNS-kiszolgálón keresztül névfeloldás__, cserélje le a hello `kafka_broker` hello hello munkavégző csomópont FQDN-bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="df941-208">If you have __enabled name resolution through a custom DNS server__, replace hello `kafka_broker` entries with hello FQDN of hello worker nodes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df941-209">Ez a kód küldi hello karakterlánc `test message` toohello témakör `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="df941-209">This code sends hello string `test message` toohello topic `testtopic`.</span></span> <span data-ttu-id="df941-210">hello alapértelmezett konfigurálása a HDInsight Kafka toocreate hello témakör számára, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="df941-210">hello default configuration of Kafka on HDInsight is toocreate hello topic if it does not exist.</span></span>

4. <span data-ttu-id="df941-211">tooretrieve köszönőüzenetei Kafka, a Python kódját a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="df941-211">tooretrieve hello messages from Kafka, use hello following Python code:</span></span>

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

    <span data-ttu-id="df941-212">Cserélje le a hello `'kafka_broker'` hello címekkel bejegyzések kapott ebben a szakaszban 1. lépés:</span><span class="sxs-lookup"><span data-stu-id="df941-212">Replace hello `'kafka_broker'` entries with hello addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="df941-213">Használatakor a __szoftver VPN-ügyfél__, cserélje le a hello `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="df941-213">If you are using a __Software VPN client__, replace hello `kafka_broker` entries with hello IP address of your worker nodes.</span></span>

    * <span data-ttu-id="df941-214">Ha rendelkezik __engedélyezve van egy egyéni DNS-kiszolgálón keresztül névfeloldás__, cserélje le a hello `kafka_broker` hello hello munkavégző csomópont FQDN-bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="df941-214">If you have __enabled name resolution through a custom DNS server__, replace hello `kafka_broker` entries with hello FQDN of hello worker nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df941-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df941-215">Next steps</span></span>

<span data-ttu-id="df941-216">További információ a HDInsight használata a virtuális hálózaton: hello [kiterjesztése Azure HDInsight Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="df941-216">For more information on using HDInsight with a virtual network, see hello [Extend Azure HDInsight using an Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

<span data-ttu-id="df941-217">Az Azure virtuális hálózat létrehozása a pont – hely típusú VPN-átjáróval további információkért lásd: a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="df941-217">For more information on creating an Azure Virtual Network with Point-to-Site VPN gateway, see hello following documents:</span></span>

* [<span data-ttu-id="df941-218">Egy pont – hely kapcsolat hello Azure-portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="df941-218">Configure a Point-to-Site connection using hello Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [<span data-ttu-id="df941-219">Az Azure PowerShell pont – hely kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="df941-219">Configure a Point-to-Site connection using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

<span data-ttu-id="df941-220">További információk a hdinsight Kafka tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="df941-220">For more information on working with Kafka on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="df941-221">A HDInsighton futó Kafka használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="df941-221">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="df941-222">A HDInsight Kafka a tükrözés használata</span><span class="sxs-lookup"><span data-stu-id="df941-222">Use mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
