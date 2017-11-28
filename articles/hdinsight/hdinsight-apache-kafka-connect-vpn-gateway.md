---
title: "Virtuális hálózatok - Azure HDInsight használatával Kafka csatlakozni |} Microsoft Docs"
description: "Útmutató a HDInsight az Azure virtuális hálózaton keresztül Kafka közvetlenül kapcsolódni. Megtudhatja, hogyan Kafka fejlesztési VPN-átjáró használatával, vagy a helyi hálózatban lévő ügyfelek használatával kapcsolódnak a VPN-átjáróeszközt."
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
ms.openlocfilehash: 245bee7c1dbb0236afdc2506e7ab84b5573cbc85
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-kafka-on-hdinsight-preview-through-an-azure-virtual-network"></a><span data-ttu-id="8e0a4-104">A HDInsight (előzetes verzió) egy Azure virtuális hálózaton keresztül Kafka kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="8e0a4-104">Connect to Kafka on HDInsight (preview) through an Azure Virtual Network</span></span>

<span data-ttu-id="8e0a4-105">Útmutató a HDInsight az Azure virtuális hálózatok használatával Kafka közvetlenül kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-105">Learn how to directly connect to Kafka on HDInsight using Azure Virtual Networks.</span></span> <span data-ttu-id="8e0a4-106">Ez a dokumentum információkat biztosít Kafka csatlakozni a következő konfigurációk használatával:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-106">This document provides information on connecting to Kafka using the following configurations:</span></span>

* <span data-ttu-id="8e0a4-107">Az egy helyi hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-107">From resources in an on-premises network.</span></span> <span data-ttu-id="8e0a4-108">Ezt a kapcsolatot a helyi hálózaton a VPN-eszköz (szoftveres vagy hardveres) használatával.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-108">This connection is established by using a VPN device (software or hardware) on your local network.</span></span>
* <span data-ttu-id="8e0a4-109">A környezet a VPN szoftver ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-109">From a development environment using a VPN software client.</span></span>

## <a name="architecture-and-planning"></a><span data-ttu-id="8e0a4-110">Architektúra és tervezése</span><span class="sxs-lookup"><span data-stu-id="8e0a4-110">Architecture and planning</span></span>

<span data-ttu-id="8e0a4-111">A HDInsight nem engedélyezi a Kafka közvetlen kapcsolatra a nyilvános interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-111">HDInsight does not allow direct connection to Kafka over the public internet.</span></span> <span data-ttu-id="8e0a4-112">Ehelyett azt Kafka ügyfelek (létrehozói és felhasználói) kell használnia, a következő kapcsolat módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-112">Instead, Kafka clients (producers and consumers) must use one of the following connection methods:</span></span>

* <span data-ttu-id="8e0a4-113">Futtassa az ügyfelet a HDInsight a Kafka azonos virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-113">Run the client in the same virtual network as Kafka on HDInsight.</span></span> <span data-ttu-id="8e0a4-114">Ez a konfiguráció szerepel a [Apache Kafka (előzetes verzió) a HDInsight kezdődnie](hdinsight-apache-kafka-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-114">This configuration is used in the [Start with Apache Kafka (preview) on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span> <span data-ttu-id="8e0a4-115">Az ügyfél futtatja a közvetlenül a HDInsight-fürtcsomóponton, vagy egy másik virtuális gép ugyanazon a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-115">The client runs directly on the HDInsight cluster nodes or on another virtual machine in the same network.</span></span>

* <span data-ttu-id="8e0a4-116">A magánhálózaton, például a helyszíni hálózathoz csatlakozni a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-116">Connect a private network, such as your on-premises network, to the virtual network.</span></span> <span data-ttu-id="8e0a4-117">Ez a konfiguráció lehetővé teszi az ügyfelek a helyszíni hálózat Kafka közvetlenül együttműködni.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-117">This configuration allows clients in your on-premises network to directly work with Kafka.</span></span> <span data-ttu-id="8e0a4-118">Ahhoz, hogy ez a konfiguráció, a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-118">To enable this configuration, perform the following tasks:</span></span>

    1. <span data-ttu-id="8e0a4-119">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-119">Create a virtual network.</span></span>
    2. <span data-ttu-id="8e0a4-120">Hozzon létre egy VPN-átjáró, amely webhelyek konfigurációt használ.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-120">Create a VPN gateway that uses a site-to-site configuration.</span></span> <span data-ttu-id="8e0a4-121">Az ebben a dokumentumban használt konfigurációs csatlakozik a helyszíni hálózaton a VPN-átjáróeszközt.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-121">The configuration used in this document connects to a VPN gateway device in your on-premises network.</span></span>
    3. <span data-ttu-id="8e0a4-122">Hozzon létre egy DNS-kiszolgáló a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-122">Create a DNS server in the virtual network.</span></span>
    4. <span data-ttu-id="8e0a4-123">Konfigurálja a DNS-kiszolgáló minden hálózat között.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-123">Configure forwarding between the DNS server in each network.</span></span>
    5. <span data-ttu-id="8e0a4-124">A HDInsight Kafka telepítse a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-124">Install Kafka on HDInsight into the virtual network.</span></span>

    <span data-ttu-id="8e0a4-125">További információkért lásd: a [Kafka csatlakozás egy helyszíni hálózatból](#on-premises) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-125">For more information, see the [Connect to Kafka from an on-premises network](#on-premises) section.</span></span> 

* <span data-ttu-id="8e0a4-126">Az egyes gépek csatlakozni a virtuális hálózat VPN-átjáró és a VPN-ügyfél segítségével.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-126">Connect individual machines to the virtual network using a VPN gateway and VPN client.</span></span> <span data-ttu-id="8e0a4-127">Ahhoz, hogy ez a konfiguráció, a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-127">To enable this configuration, perform the following tasks:</span></span>

    1. <span data-ttu-id="8e0a4-128">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-128">Create a virtual network.</span></span>
    2. <span data-ttu-id="8e0a4-129">Hozzon létre egy pont – hely konfigurációt használó VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-129">Create a VPN gateway that uses a point-to-site configuration.</span></span> <span data-ttu-id="8e0a4-130">Ez a konfiguráció biztosítja a VPN-ügyfél, amely a Windows-ügyfelek is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-130">This configuration provides a VPN client that can be installed on Windows clients.</span></span>
    3. <span data-ttu-id="8e0a4-131">A HDInsight Kafka telepítse a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-131">Install Kafka on HDInsight into the virtual network.</span></span>
    4. <span data-ttu-id="8e0a4-132">IP-hirdetési Kafka konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-132">Configure Kafka for IP advertising.</span></span> <span data-ttu-id="8e0a4-133">Ez a konfiguráció lehetővé teszi, hogy az ügyfél történő csatlakozáshoz az IP-címzés tartománynevek helyett.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-133">This configuration allows the client to connect using IP addressing instead of domain names.</span></span>
    5. <span data-ttu-id="8e0a4-134">Töltse le, és a fejlesztői rendszeren a VPN-ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-134">Download and use the VPN client on the development system.</span></span>

    <span data-ttu-id="8e0a4-135">További információkért lásd: a [Kafka egy VPN-ügyfél a kapcsolódás](#vpnclient) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-135">For more information, see the [Connect to Kafka with a VPN client](#vpnclient) section.</span></span>

    > [!WARNING]
    > <span data-ttu-id="8e0a4-136">Ez a konfiguráció csak ajánlott fejlesztési célra miatt a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-136">This configuration is only recommended for development purposes because of the following limitations:</span></span>
    >
    > * <span data-ttu-id="8e0a4-137">Minden ügyfél szoftver VPN-ügyfél használatával kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-137">Each client must connect using a VPN software client.</span></span> <span data-ttu-id="8e0a4-138">Azure csak egy Windows-alapú ügyfelek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-138">Azure only provides a Windows-based client.</span></span>
    > * <span data-ttu-id="8e0a4-139">Az ügyfél nem felel meg névfeloldási a virtuális hálózathoz, így IP-címzési Kafka kommunikálni kell használni.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-139">The client does not pass name resolution requests to the virtual network, so you must use IP addressing to communicate with Kafka.</span></span> <span data-ttu-id="8e0a4-140">Integrációs csomaggal folytatott kommunikációhoz az Kafka fürtön további konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-140">IP communication requires additional configuration on the Kafka cluster.</span></span>

<span data-ttu-id="8e0a4-141">A HDInsight eszközzel virtuális hálózatban további információkért lásd: [kiterjesztése HDInsight Azure virtuális hálózatok használatával](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="8e0a4-141">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

## <span data-ttu-id="8e0a4-142"><a id="on-premises"></a>Csatlakozás egy helyszíni hálózatból Kafka</span><span class="sxs-lookup"><span data-stu-id="8e0a4-142"><a id="on-premises"></a> Connect to Kafka from an on-premises network</span></span>

<span data-ttu-id="8e0a4-143">A helyszíni hálózattal, melyekkel Kafka fürt létrehozásához kövesse a [a helyszíni hálózathoz való csatlakozás HDInsight](./connect-on-premises-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-143">To create a Kafka cluster that communicates with your on-premises network, follow the steps in the [Connect HDInsight to your on-premises network](./connect-on-premises-network.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e0a4-144">A HDInsight-fürt létrehozása esetén a __Kafka__ fürt típusa.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-144">When creating the HDInsight cluster, select the __Kafka__ cluster type.</span></span>

<span data-ttu-id="8e0a4-145">Ezeket a lépéseket hozza létre az alábbi konfigurációt:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-145">These steps create the following configuration:</span></span>

* <span data-ttu-id="8e0a4-146">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="8e0a4-146">Azure Virtual Network</span></span>
* <span data-ttu-id="8e0a4-147">Webhelyek közötti VPN átjáró</span><span class="sxs-lookup"><span data-stu-id="8e0a4-147">Site-to-site VPN gateway</span></span>
* <span data-ttu-id="8e0a4-148">Az Azure Storage-fiók (a HDInsight által használt)</span><span class="sxs-lookup"><span data-stu-id="8e0a4-148">Azure Storage account (used by HDInsight)</span></span>
* <span data-ttu-id="8e0a4-149">A HDInsight Kafka</span><span class="sxs-lookup"><span data-stu-id="8e0a4-149">Kafka on HDInsight</span></span>

<span data-ttu-id="8e0a4-150">Győződjön meg arról, hogy egy Kafka kapcsolódni tud a fürt a helyszíni, kövesse a lépéseket a a [példa: ügyfél Python](#python-client) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-150">To verify that a Kafka client can connect to the cluster from on-premises, use the steps in the [Example: Python client](#python-client) section.</span></span>

## <span data-ttu-id="8e0a4-151"><a id="vpnclient"></a>A VPN-ügyfél Kafka csatlakozni</span><span class="sxs-lookup"><span data-stu-id="8e0a4-151"><a id="vpnclient"></a> Connect to Kafka with a VPN client</span></span>

<span data-ttu-id="8e0a4-152">Ebben a szakaszban a lépések segítségével hozza létre a következő konfigurációt:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-152">Use the steps in this section to create the following configuration:</span></span>

* <span data-ttu-id="8e0a4-153">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="8e0a4-153">Azure Virtual Network</span></span>
* <span data-ttu-id="8e0a4-154">Pont-pont VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="8e0a4-154">Point-to-site VPN gateway</span></span>
* <span data-ttu-id="8e0a4-155">Az Azure Storage-fiók (a HDInsight által használt)</span><span class="sxs-lookup"><span data-stu-id="8e0a4-155">Azure Storage Account (used by HDInsight)</span></span>
* <span data-ttu-id="8e0a4-156">A HDInsight Kafka</span><span class="sxs-lookup"><span data-stu-id="8e0a4-156">Kafka on HDInsight</span></span>

1. <span data-ttu-id="8e0a4-157">Kövesse a [pont – hely kapcsolatok önaláírt tanúsítványok használata](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-157">Follow the steps in the [Working with self-signed certificates for Point-to-site connections](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) document.</span></span> <span data-ttu-id="8e0a4-158">Ez a dokumentum az átjáró szükséges tanúsítványokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-158">This document creates the certificates needed for the gateway.</span></span>

2. <span data-ttu-id="8e0a4-159">Nyisson meg egy PowerShell-parancssorba, és jelentkezzen be az Azure-előfizetéshez az alábbi kód használatával:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-159">Open a PowerShell prompt and use the following code to log in to your Azure subscription:</span></span>

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. <span data-ttu-id="8e0a4-160">Konfigurációs adatokat tartalmazó változók létrehozásához használja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-160">Use the following code to create variables that contain configuration information:</span></span>

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is the resource group name?"
    $baseName = Read-Host "What is the base name? It is used to create names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want to create the resources in?"
    $rootCert = Read-Host "What is the file path to the root certificate? It is used to secure the VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter the HTTPS user name and password for the HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter the SSH user name and password for the HDInsight cluster" -UserName "sshuser"

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

4. <span data-ttu-id="8e0a4-161">Az Azure-erőforráscsoportot és a virtuális hálózat létrehozásához használja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-161">Use the following code to create the Azure resource group and virtual network:</span></span>

    ```powershell
    # Create the resource group that contains everything
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
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
    > <span data-ttu-id="8e0a4-162">A folyamat befejezése több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-162">It can take several minutes for this process to complete.</span></span>

5. <span data-ttu-id="8e0a4-163">Az Azure-Tárfiókot és blob tároló létrehozásához használja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-163">Use the following code to create the Azure Storage Account and blob container:</span></span>

    ```powershell
    # Create the storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. <span data-ttu-id="8e0a4-164">A HDInsight-fürt létrehozásához használja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-164">Use the following code to create the HDInsight cluster:</span></span>

    ```powershell
    # Create the HDInsight cluster
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
  > <span data-ttu-id="8e0a4-165">Ez a folyamat befejezéséhez körülbelül 20 percet vesz.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-165">This process takes around 20 minutes to complete.</span></span>

8. <span data-ttu-id="8e0a4-166">A Windows VPN-ügyfelek a virtuális hálózat az URL-cím lekéréséhez használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-166">Use the following cmdlet to retrieve the URL for the Windows VPN client for the virtual network:</span></span>

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    <span data-ttu-id="8e0a4-167">A Windows VPN-ügyfél letöltéséhez, a visszaadott URI használata a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-167">To download the Windows VPN client, use the returned URI in your web browser.</span></span>

### <a name="configure-kafka-for-ip-advertising"></a><span data-ttu-id="8e0a4-168">IP-hirdetési Kafka konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e0a4-168">Configure Kafka for IP advertising</span></span>

<span data-ttu-id="8e0a4-169">Alapértelmezés szerint Zookeeper az ügyfelek számára a Kafka brókerek tartomány nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-169">By default, Zookeeper returns the domain name of the Kafka brokers to clients.</span></span> <span data-ttu-id="8e0a4-170">Ez a konfiguráció nem működik a VPN-, szoftver-ügyfél, a virtuális hálózat entitások névfeloldás nem használható.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-170">This configuration does not work with the VPN software client, as it cannot use name resolution for entities in the virtual network.</span></span> <span data-ttu-id="8e0a4-171">Ehhez a konfigurációhoz az alábbi lépések segítségével Kafka hivatkozik tartománynevek helyett IP-címek konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-171">For this configuration, use the following steps to configure Kafka to advertise IP addresses instead of domain names:</span></span>

1. <span data-ttu-id="8e0a4-172">Webböngésző segítségével, folytassa a https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-172">Using a web browser, go to https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="8e0a4-173">Cserélje le __CLUSTERNAME__ HDInsight-fürt Kafka nevével.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-173">Replace __CLUSTERNAME__ with the name of the Kafka on HDInsight cluster.</span></span>

    <span data-ttu-id="8e0a4-174">Amikor a rendszer kéri, használja a HTTPS-felhasználónevet és jelszót a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-174">When prompted, use the HTTPS user name and password for the cluster.</span></span> <span data-ttu-id="8e0a4-175">Az Ambari webes felhasználói felületén, a fürt akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-175">The Ambari Web UI for the cluster is displayed.</span></span>

2. <span data-ttu-id="8e0a4-176">Kafka adatok megtekintéséhez válassza ki a __Kafka__ a bal oldali listában.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-176">To view information on Kafka, select __Kafka__ from the list on the left.</span></span>

    ![A kijelölt Kafka szolgáltatás lista](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. <span data-ttu-id="8e0a4-178">Válassza ki, ha Kafka konfigurációs __Configs__ felső közepén.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-178">To view Kafka configuration, select __Configs__ from the top middle.</span></span>

    ![Kafka Configs hivatkozások](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. <span data-ttu-id="8e0a4-180">Található a __kafka-env__ konfigurációs, adja meg `kafka-env` a a __szűrő__ jobb felső részén található.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-180">To find the __kafka-env__ configuration, enter `kafka-env` in the __Filter__ field on the upper right.</span></span>

    ![Kafka konfigurációját, kafka-env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. <span data-ttu-id="8e0a4-182">Konfigurálja az IP-címek hivatkozik Kafka, vegye fel a következő szöveg alsó részén található a __kafka-env-sablon__ mező:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-182">To configure Kafka to advertise IP addresses, add the following text to the bottom of the __kafka-env-template__ field:</span></span>

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. <span data-ttu-id="8e0a4-183">A felület, amely figyeli Kafka konfigurálásához írja be a következőt `listeners` a a __szűrő__ jobb felső részén található.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-183">To configure the interface that Kafka listens on, enter `listeners` in the __Filter__ field on the upper right.</span></span>

7. <span data-ttu-id="8e0a4-184">Minden hálózati interfészen figyelésére Kafka konfigurálásához módosítsa a __figyelői__ mezőről `PLAINTEXT://0.0.0.0:9092`.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-184">To configure Kafka to listen on all network interfaces, change the value in the __listeners__ field to `PLAINTEXT://0.0.0.0:9092`.</span></span>

8. <span data-ttu-id="8e0a4-185">A konfigurációs módosítások mentéséhez használja a __mentése__ gombra.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-185">To save the configuration changes, use the __Save__ button.</span></span> <span data-ttu-id="8e0a4-186">Adja meg a módosítások leíró üzenet.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-186">Enter a text message describing the changes.</span></span> <span data-ttu-id="8e0a4-187">Válassza ki __OK__ a módosítások mentése után.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-187">Select __OK__ once the changes have been saved.</span></span>

    ![Mentse a konfigurációs gomb](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. <span data-ttu-id="8e0a4-189">Megakadályozhatja, hogy hibák Kafka újraindításakor a __szolgáltatás műveletek__ gombra, majd az __kapcsolja be a karbantartási mód__.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-189">To prevent errors when restarting Kafka, use the __Service Actions__ button and select __Turn On Maintenance Mode__.</span></span> <span data-ttu-id="8e0a4-190">Kattintson az OK gombra a művelet végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-190">Select OK to complete this operation.</span></span>

    ![Szolgáltatási művelet, amely a kijelölt karbantartási bekapcsolása](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. <span data-ttu-id="8e0a4-192">Indítsa újra a Kafka, használja a __indítsa újra a__ gombra, majd az __indítsa újra az összes érintett__.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-192">To restart Kafka, use the __Restart__ button and select __Restart All Affected__.</span></span> <span data-ttu-id="8e0a4-193">Erősítse meg az újraindítás, és használja a __OK__ gombra kattint, a művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-193">Confirm the restart, and then use the __OK__ button after the operation has completed.</span></span>

    ![Indítsa újra a gomb, újraindítással minden érintett](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. <span data-ttu-id="8e0a4-195">A karbantartási mód letiltásához használja a __szolgáltatás műveletek__ gombra, majd az __kapcsolja ki a karbantartási mód__.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-195">To disable maintenance mode, use the __Service Actions__ button and select __Turn Off Maintenance Mode__.</span></span> <span data-ttu-id="8e0a4-196">Válassza ki **OK** a művelet végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-196">Select **OK** to complete this operation.</span></span>

### <a name="connect-to-the-vpn-gateway"></a><span data-ttu-id="8e0a4-197">A VPN-átjárón</span><span class="sxs-lookup"><span data-stu-id="8e0a4-197">Connect to the VPN gateway</span></span>

<span data-ttu-id="8e0a4-198">A VPN-átjárót csatlakozni egy __Windows ügyfél__, használja a __csatlakozás az Azure__ szakasza a [egy pont – hely kapcsolat beállítása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-198">To connect to the VPN gateway from a __Windows client__, use the __Connect to Azure__ section of the [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) document.</span></span>

## <span data-ttu-id="8e0a4-199"><a id="python-client"></a>Példa: Python ügyfél</span><span class="sxs-lookup"><span data-stu-id="8e0a4-199"><a id="python-client"></a> Example: Python client</span></span>

<span data-ttu-id="8e0a4-200">Kafka kapcsolat ellenőrzéséhez tegye a következőket hozhat létre és futtathat a Python-készítő és a fogyasztói:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-200">To validate connectivity to Kafka, use the following steps to create and run a Python producer and consumer:</span></span>

1. <span data-ttu-id="8e0a4-201">A következő módszerek egyikét használja a teljesen minősített tartománynevét (FQDN) és az IP-címek a fürtben található csomópontok a Kafka beolvasásához:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-201">Use one of the following methods to retrieve the fully qualified domain name (FQDN) and IP addresses of the nodes in the Kafka cluster:</span></span>

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

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

    <span data-ttu-id="8e0a4-202">Ez a parancsfájl feltételezi, hogy `$resourceGroupName` az Azure erőforráscsoport, amely tartalmazza a virtuális hálózat neve.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-202">This script assumes that `$resourceGroupName` is the name of the Azure resource group that contains the virtual network.</span></span>

    <span data-ttu-id="8e0a4-203">Mentse a visszakapott információk használatra a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-203">Save the returned information for use in the next steps.</span></span>

2. <span data-ttu-id="8e0a4-204">A következő segítségével telepítse a [kafka-python](http://kafka-python.readthedocs.io/) ügyfél:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-204">Use the following to install the [kafka-python](http://kafka-python.readthedocs.io/) client:</span></span>

        pip install kafka-python

3. <span data-ttu-id="8e0a4-205">Kafka adatokat küldeni, használja a következő Python kódot:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-205">To send data to Kafka, use the following Python code:</span></span>

  ```python
  from kafka import KafkaProducer
  # Replace the `ip_address` entries with the IP address of your worker nodes
  # NOTE: you don't need the full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    <span data-ttu-id="8e0a4-206">Cserélje le a `'kafka_broker'` bejegyzést, amelyben a címek által visszaadott ebben a szakaszban 1. lépés:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-206">Replace the `'kafka_broker'` entries with the addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="8e0a4-207">Ha használ egy __szoftver VPN-ügyfél__, cserélje le a `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok IP-címét.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-207">If you are using a __Software VPN client__, replace the `kafka_broker` entries with the IP address of your worker nodes.</span></span>

    * <span data-ttu-id="8e0a4-208">Ha rendelkezik __engedélyezve van egy egyéni DNS-kiszolgálón keresztül névfeloldás__, cserélje le a `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok teljes Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-208">If you have __enabled name resolution through a custom DNS server__, replace the `kafka_broker` entries with the FQDN of the worker nodes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8e0a4-209">Ez a kód elküldi a karakterlánc `test message` a témakör `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-209">This code sends the string `test message` to the topic `testtopic`.</span></span> <span data-ttu-id="8e0a4-210">A HDInsight Kafka alapértelmezett konfigurálása nem hozhat létre a témakört, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-210">The default configuration of Kafka on HDInsight is to create the topic if it does not exist.</span></span>

4. <span data-ttu-id="8e0a4-211">Az üzenetek beolvasásához Kafka, a következő Python kódot használja:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-211">To retrieve the messages from Kafka, use the following Python code:</span></span>

   ```python
   from kafka import KafkaConsumer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # Again, you only need one or two, not the full list.
   # Note: auto_offset_reset='earliest' resets the starting offset to the beginning
   #       of the topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    <span data-ttu-id="8e0a4-212">Cserélje le a `'kafka_broker'` bejegyzést, amelyben a címek által visszaadott ebben a szakaszban 1. lépés:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-212">Replace the `'kafka_broker'` entries with the addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="8e0a4-213">Ha használ egy __szoftver VPN-ügyfél__, cserélje le a `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok IP-címét.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-213">If you are using a __Software VPN client__, replace the `kafka_broker` entries with the IP address of your worker nodes.</span></span>

    * <span data-ttu-id="8e0a4-214">Ha rendelkezik __engedélyezve van egy egyéni DNS-kiszolgálón keresztül névfeloldás__, cserélje le a `kafka_broker` bejegyzést, amelyben a feldolgozó csomópontok teljes Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-214">If you have __enabled name resolution through a custom DNS server__, replace the `kafka_broker` entries with the FQDN of the worker nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e0a4-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8e0a4-215">Next steps</span></span>

<span data-ttu-id="8e0a4-216">A HDInsight használata a virtuális hálózaton további információkért lásd: a [kiterjesztése Azure HDInsight Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="8e0a4-216">For more information on using HDInsight with a virtual network, see the [Extend Azure HDInsight using an Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

<span data-ttu-id="8e0a4-217">Az Azure virtuális hálózat létrehozásával pont – hely típusú VPN-átjáróval további információkért lásd a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-217">For more information on creating an Azure Virtual Network with Point-to-Site VPN gateway, see the following documents:</span></span>

* [<span data-ttu-id="8e0a4-218">Az Azure portál használatával pont – hely kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e0a4-218">Configure a Point-to-Site connection using the Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [<span data-ttu-id="8e0a4-219">Az Azure PowerShell pont – hely kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e0a4-219">Configure a Point-to-Site connection using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

<span data-ttu-id="8e0a4-220">A HDInsight-beli Kafka használatával kapcsolatos további információk a következő dokumentumokban találhatók:</span><span class="sxs-lookup"><span data-stu-id="8e0a4-220">For more information on working with Kafka on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="8e0a4-221">A HDInsighton futó Kafka használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="8e0a4-221">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="8e0a4-222">A HDInsight Kafka a tükrözés használata</span><span class="sxs-lookup"><span data-stu-id="8e0a4-222">Use mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
