---
title: "A virtuális hálózattal - Azure HDInsight kiterjesztése |} Microsoft Docs"
description: "Azure virtuális hálózat használata a HDInsight csatlakozni más felhőalapú erőforrásokat, vagy az adatközpontban lévő erőforrások"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: 380423ec42ad4905c73fcd57501102e9f7062e81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a><span data-ttu-id="689da-103">Azure virtuális hálózat használatával Azure HDInsight kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="689da-103">Extend Azure HDInsight using an Azure Virtual Network</span></span>

<span data-ttu-id="689da-104">A HDInsight használata egy [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="689da-104">Learn how to use HDInsight with an [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="689da-105">Azure virtuális hálózat használatával lehetővé teszi, hogy a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="689da-105">Using an Azure Virtual Network enables the following scenarios:</span></span>

* <span data-ttu-id="689da-106">HDInsight csatlakozik egy helyszíni hálózatról.</span><span class="sxs-lookup"><span data-stu-id="689da-106">Connecting to HDInsight directly from an on-premises network.</span></span>

* <span data-ttu-id="689da-107">A HDInsight-adatokhoz való kapcsolódásról tárolja egy Azure virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="689da-107">Connecting HDInsight to data stores in an Azure Virtual network.</span></span>

* <span data-ttu-id="689da-108">Közvetlen hozzáférés a Hadoop-szolgáltatás által nem biztosított nyilvánosan az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="689da-108">Directly accessing Hadoop services that are not available publicly over the internet.</span></span> <span data-ttu-id="689da-109">Például Kafka API-k vagy a HBase Java API-t.</span><span class="sxs-lookup"><span data-stu-id="689da-109">For example, Kafka APIs or the HBase Java API.</span></span>

> [!WARNING]
> <span data-ttu-id="689da-110">A jelen dokumentumban szereplő információk megértéséhez a TCP/IP-hálózat igényel.</span><span class="sxs-lookup"><span data-stu-id="689da-110">The information in this document requires an understanding of TCP/IP networking.</span></span> <span data-ttu-id="689da-111">Ha nem ismeri a TCP/IP-hálózat, meg kell felkereshetők a rendszer éles hálózati környezetben módosítások végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="689da-111">If you are not familiar with TCP/IP networking, you should partner with someone who is before making modifications to production networks.</span></span>

## <a name="planning"></a><span data-ttu-id="689da-112">Tervezés</span><span class="sxs-lookup"><span data-stu-id="689da-112">Planning</span></span>

<span data-ttu-id="689da-113">A kérdésekre kell válaszolnia a HDInsight telepítése egy virtuális hálózat tervezése során a következők:</span><span class="sxs-lookup"><span data-stu-id="689da-113">The following are the questions that you must answer when planning to install HDInsight in a virtual network:</span></span>

* <span data-ttu-id="689da-114">HDInsight telepíthető át egy meglévő virtuális hálózathoz kell?</span><span class="sxs-lookup"><span data-stu-id="689da-114">Do you need to install HDInsight into an existing virtual network?</span></span> <span data-ttu-id="689da-115">Új hálózat létrehozása vagy?</span><span class="sxs-lookup"><span data-stu-id="689da-115">Or are you creating a new network?</span></span>

    <span data-ttu-id="689da-116">Ha egy meglévő virtuális hálózatot használ, szükség lehet a hálózati konfiguráció módosításához a HDInsight telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="689da-116">If you are using an existing virtual network, you may need to modify the network configuration before you can install HDInsight.</span></span> <span data-ttu-id="689da-117">További információkért lásd: a [HDInsight hozzáadása egy meglévő virtuális hálózatot](#existingvnet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-117">For more information, see the [add HDInsight to an existing virtual network](#existingvnet) section.</span></span>

* <span data-ttu-id="689da-118">Végrehajtja a virtuális hálózat egy másik virtuális hálózati vagy a helyszíni hálózat HDInsight tartalmazó?</span><span class="sxs-lookup"><span data-stu-id="689da-118">Do you want to connect the virtual network containing HDInsight to another virtual network or your on-premises network?</span></span>

    <span data-ttu-id="689da-119">Segítségével egyszerűen dolgozhat erőforrások hálózatok között, szükség lehet hozzon létre egy egyéni DNS- és DNS-továbbító konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="689da-119">To easily work with resources across networks, you may need to create a custom DNS and configure DNS forwarding.</span></span> <span data-ttu-id="689da-120">További információkért lásd: a [kapcsolódó több hálózatok](#multinet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-120">For more information, see the [connecting multiple networks](#multinet) section.</span></span>

* <span data-ttu-id="689da-121">Szeretné korlátozni/átirányítás HDInsight bejövő vagy kimenő forgalom?</span><span class="sxs-lookup"><span data-stu-id="689da-121">Do you want to restrict/redirect inbound or outbound traffic to HDInsight?</span></span>

    <span data-ttu-id="689da-122">A HDInsight az IP-címek az Azure-adatközpontban kommunikálni kell üzemmódban.</span><span class="sxs-lookup"><span data-stu-id="689da-122">HDInsight must have unrestricted communication with specific IP addresses in the Azure data center.</span></span> <span data-ttu-id="689da-123">Van még számos portot, az ügyfél-kommunikációhoz tűzfalon keresztül kell engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="689da-123">There are also several ports that must be allowed through firewalls for client communication.</span></span> <span data-ttu-id="689da-124">További információkért lásd: a [hálózati forgalom vezérlése](#networktraffic) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-124">For more information, see the [controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="689da-125"><a id="existingvnet"></a>HDInsight hozzáadása egy meglévő virtuális hálózathoz</span><span class="sxs-lookup"><span data-stu-id="689da-125"><a id="existingvnet"></a>Add HDInsight to an existing virtual network</span></span>

<span data-ttu-id="689da-126">Ebben a szakaszban a lépések segítségével egy új HDInsight hozzáadása egy meglévő Azure virtuális hálózat felderítése.</span><span class="sxs-lookup"><span data-stu-id="689da-126">Use the steps in this section to discover how to add a new HDInsight to an existing Azure Virtual Network.</span></span>

> [!NOTE]
> <span data-ttu-id="689da-127">Nem adhat hozzá egy meglévő HDInsight-fürt virtuális hálózatba.</span><span class="sxs-lookup"><span data-stu-id="689da-127">You cannot add an existing HDInsight cluster into a virtual network.</span></span>

1. <span data-ttu-id="689da-128">Használja a klasszikus és Resource Manager üzembe helyezési modellben a virtuális hálózat?</span><span class="sxs-lookup"><span data-stu-id="689da-128">Are you using a classic or Resource Manager deployment model for the virtual network?</span></span>

    <span data-ttu-id="689da-129">HDInsight 3.4 és nagyobb erőforrás-kezelő virtuális hálózat szükséges.</span><span class="sxs-lookup"><span data-stu-id="689da-129">HDInsight 3.4 and greater requires a Resource Manager virtual network.</span></span> <span data-ttu-id="689da-130">A HDInsight korábbi verzióiban a klasszikus virtuális hálózatot szükséges.</span><span class="sxs-lookup"><span data-stu-id="689da-130">Earlier versions of HDInsight required a classic virtual network.</span></span>

    <span data-ttu-id="689da-131">Ha a meglévő hálózat a klasszikus virtuális hálózatot, majd kell erőforrás-kezelő virtuális hálózat létrehozása és a két csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="689da-131">If your existing network is a classic virtual network, then you must create a Resource Manager virtual network and then connect the two.</span></span> <span data-ttu-id="689da-132">[A hagyományos Vneteket kapcsolódás új Vnetekhez](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span><span class="sxs-lookup"><span data-stu-id="689da-132">[Connecting classic VNets to new VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span></span>

    <span data-ttu-id="689da-133">Miután csatlakozott, erőforrás-kezelő hálózatán telepített HDInsight kezelheti a hagyományos hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="689da-133">Once joined, HDInsight installed in the Resource Manager network can interact with resources in the classic network.</span></span>

2. <span data-ttu-id="689da-134">Használja a kényszerített bújtatás?</span><span class="sxs-lookup"><span data-stu-id="689da-134">Do you use forced tunneling?</span></span> <span data-ttu-id="689da-135">A kényszerített bújtatás az alhálózat-beállítással, amely arra kényszeríti a kimenő internetforgalom a vizsgálathoz eszközökre és a naplózás.</span><span class="sxs-lookup"><span data-stu-id="689da-135">Forced tunneling is a subnet setting that forces outbound Internet traffic to a device for inspection and logging.</span></span> <span data-ttu-id="689da-136">A HDInsight nem támogatja a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="689da-136">HDInsight does not support forced tunneling.</span></span> <span data-ttu-id="689da-137">Távolítsa el a kényszerített bújtatás HDInsight egy alhálózatba telepítése előtt, vagy hozzon létre egy új alhálózatot a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="689da-137">Either remove forced tunneling before installing HDInsight into a subnet, or create a new subnet for HDInsight.</span></span>

3. <span data-ttu-id="689da-138">Használ hálózati biztonsági csoportok, a felhasználó által definiált útvonalak és a virtuális hálózati berendezések korlátozzák a forgalmat a virtuális gépbe vagy onnan a virtuális hálózat számára?</span><span class="sxs-lookup"><span data-stu-id="689da-138">Do you use network security groups, user-defined routes, or Virtual Network Appliances to restrict traffic into or out of the virtual network?</span></span>

    <span data-ttu-id="689da-139">Felügyelt szolgáltatásként HDInsight az Azure-adatközpont több IP-címeivel korlátlan hozzáférést igényel.</span><span class="sxs-lookup"><span data-stu-id="689da-139">As a managed service, HDInsight requires unrestricted access to several IP addresses in the Azure data center.</span></span> <span data-ttu-id="689da-140">Engedélyezi a kommunikációt a IP-címekkel rendelkező, a meglévő hálózati biztonsági csoport vagy felhasználó által definiált útvonalak frissítése.</span><span class="sxs-lookup"><span data-stu-id="689da-140">To allow communication with these IP addresses, update any existing network security groups or user-defined routes.</span></span>

    <span data-ttu-id="689da-141">HDInsight tároló több szolgáltatásokat, amelyek különféle portokat.</span><span class="sxs-lookup"><span data-stu-id="689da-141">HDInsight  hosts multiple services, which use a variety of ports.</span></span> <span data-ttu-id="689da-142">Nem blokkolására ezeket a portokat.</span><span class="sxs-lookup"><span data-stu-id="689da-142">Do not block traffic to these ports.</span></span> <span data-ttu-id="689da-143">Engedélyezi a virtuális készülék tűzfalon keresztüli portok listája, tekintse meg a [biztonsági](#security) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-143">For a list of ports to allow through virtual appliance firewalls, see the [Security](#security) section.</span></span>

    <span data-ttu-id="689da-144">A meglévő biztonsági beállítások megkereséséhez használja a következő Azure PowerShell vagy Azure CLI parancsokat:</span><span class="sxs-lookup"><span data-stu-id="689da-144">To find your existing security configuration, use the following Azure PowerShell or Azure CLI commands:</span></span>

    * <span data-ttu-id="689da-145">Network security groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="689da-145">Network security groups</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="689da-146">További információkért lásd: a [hibaelhárítása a hálózati biztonsági csoportok](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-146">For more information, see the [Troubleshoot network security groups](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) document.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="689da-147">Hálózati biztonsági csoportszabályok szabály prioritási sorrendben alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="689da-147">Network security group rules are applied in order based on rule priority.</span></span> <span data-ttu-id="689da-148">Az első szabály, amely a forgalom bizonyos mintázatnak megfelelő vonatkozik, és nincs más erre a forgalomra alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="689da-148">The first rule that matches the traffic pattern is applied, and no others are applied for that traffic.</span></span> <span data-ttu-id="689da-149">Szabályok a leghatékonyabb a legkevésbé megengedő.</span><span class="sxs-lookup"><span data-stu-id="689da-149">Order rules from most permissive to least permissive.</span></span> <span data-ttu-id="689da-150">További információkért lásd: a [hálózati forgalmat hálózati biztonsági csoportokkal](../virtual-network/virtual-networks-nsg.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-150">For more information, see the [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    * <span data-ttu-id="689da-151">Felhasználó által megadott útvonalak</span><span class="sxs-lookup"><span data-stu-id="689da-151">User-defined routes</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="689da-152">További információkért lásd: a [útvonalak hibaelhárítása](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-152">For more information, see the [Troubleshoot routes](../virtual-network/virtual-network-routes-troubleshoot-portal.md) document.</span></span>

4. <span data-ttu-id="689da-153">HDInsight-fürtök létrehozása és konfigurálása során az Azure virtuális hálózatot választ.</span><span class="sxs-lookup"><span data-stu-id="689da-153">Create an HDInsight cluster and select the Azure Virtual Network during configuration.</span></span> <span data-ttu-id="689da-154">A következő dokumentumok a lépésekkel Fürtlétrehozási folyamatának ismertetése:</span><span class="sxs-lookup"><span data-stu-id="689da-154">Use the steps in the following documents to understand the cluster creation process:</span></span>

    * [<span data-ttu-id="689da-155">HDInsight létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="689da-155">Create HDInsight using the Azure portal</span></span>](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [<span data-ttu-id="689da-156">HDInsight létrehozása az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="689da-156">Create HDInsight using Azure PowerShell</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [<span data-ttu-id="689da-157">A HDInsight használata az Azure CLI 1.0 létrehozása</span><span class="sxs-lookup"><span data-stu-id="689da-157">Create HDInsight using Azure CLI 1.0</span></span>](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [<span data-ttu-id="689da-158">HDInsight használata az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="689da-158">Create HDInsight using an Azure Resource Manager template</span></span>](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > <span data-ttu-id="689da-159">HDInsight hozzáadása egy virtuális hálózathoz egy opcionális konfigurációs lépésre.</span><span class="sxs-lookup"><span data-stu-id="689da-159">Adding HDInsight to a virtual network is an optional configuration step.</span></span> <span data-ttu-id="689da-160">Győződjön meg arról, megadhatja a virtuális hálózaton, amikor a fürt konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="689da-160">Be sure to select the virtual network when configuring the cluster.</span></span>

## <span data-ttu-id="689da-161"><a id="multinet"></a>Több hálózatok csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="689da-161"><a id="multinet"></a>Connecting multiple networks</span></span>

<span data-ttu-id="689da-162">A legnagyobb kihívás a több hálózati konfiguráció a hálózatok közötti névfeloldás.</span><span class="sxs-lookup"><span data-stu-id="689da-162">The biggest challenge with a multi-network configuration is name resolution between the networks.</span></span>

<span data-ttu-id="689da-163">Azure névfeloldást biztosít az Azure virtuális hálózatban telepített szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="689da-163">Azure provides name resolution for Azure services that are installed in a virtual network.</span></span> <span data-ttu-id="689da-164">A beépített névfeloldás lehetővé teszi, hogy a HDInsight egy teljesen minősített tartománynevét (FQDN) használatával a következő erőforrások eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="689da-164">This built-in name resolution allows HDInsight to connect to the following resources by using a fully qualified domain name (FQDN):</span></span>

* <span data-ttu-id="689da-165">Bármely erőforrása, amely az interneten érhető el.</span><span class="sxs-lookup"><span data-stu-id="689da-165">Any resource that is available on the internet.</span></span> <span data-ttu-id="689da-166">Ha például a Microsoft.com webhelyre mutat, google.com.</span><span class="sxs-lookup"><span data-stu-id="689da-166">For example, microsoft.com, google.com.</span></span>

* <span data-ttu-id="689da-167">Az azonos Azure virtuális hálózatban lévő összes erőforrást a __belső DNS-név__ az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="689da-167">Any resource that is in the same Azure Virtual Network, by using the __internal DNS name__ of the resource.</span></span> <span data-ttu-id="689da-168">Például ha az alapértelmezett név feloldása, a következők példa HDInsight munkavégző csomópontokhoz rendelt belső DNS-nevek:</span><span class="sxs-lookup"><span data-stu-id="689da-168">For example, when using the default name resolution, the following are example internal DNS names assigned to HDInsight worker nodes:</span></span>

    * <span data-ttu-id="689da-169">wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="689da-169">wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>
    * <span data-ttu-id="689da-170">wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="689da-170">wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>

    <span data-ttu-id="689da-171">Mindkét ezeket a csomópontokat közvetlenül kommunikálhatnak egymással, és a Hdinsightban, más csomópontok belső DNS-nevek használatával.</span><span class="sxs-lookup"><span data-stu-id="689da-171">Both these nodes can communicate directly with each other, and other nodes in HDInsight, by using internal DNS names.</span></span>

<span data-ttu-id="689da-172">Az alapértelmezett név feloldása does __nem__ engedélyezése a HDInsight a feloldani az erőforrások hálózatokban, amelyek a virtuális hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="689da-172">The default name resolution does __not__ allow HDInsight to resolve the names of resources in networks that are joined to the virtual network.</span></span> <span data-ttu-id="689da-173">Például esetében gyakori, a helyszíni hálózat a virtuális hálózathoz csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="689da-173">For example, it is common to join your on-premises network to the virtual network.</span></span> <span data-ttu-id="689da-174">Csak az alapértelmezett a névfeloldás HDInsight neve nem tud hozzáférni a helyszíni hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="689da-174">With only the default name resolution, HDInsight cannot access resources in the on-premises network by name.</span></span> <span data-ttu-id="689da-175">A fordítottja is igaz, a helyszíni hálózati erőforrások neve nem tud hozzáférni a virtuális hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="689da-175">The opposite is also true, resources in your on-premises network cannot access resources in the virtual network by name.</span></span>

> [!WARNING]
> <span data-ttu-id="689da-176">Az egyéni DNS-kiszolgáló létrehozása és a virtuális hálózat a használatára a HDInsight-fürt létrehozása előtt konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="689da-176">You must create the custom DNS server and configure the virtual network to use it before creating the HDInsight cluster.</span></span>

<span data-ttu-id="689da-177">Ahhoz, hogy a névfeloldás a virtuális hálózat és a csatlakoztatott hálózatokon lévő erőforrások között, el kell végeznie a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="689da-177">To enable name resolution between the virtual network and resources in joined networks, you must perform the following actions:</span></span>

1. <span data-ttu-id="689da-178">Hozzon létre egy egyéni DNS-kiszolgáló az Azure Virtual Network, ahol HDInsight telepítését tervezi.</span><span class="sxs-lookup"><span data-stu-id="689da-178">Create a custom DNS server in the Azure Virtual Network where you plan to install HDInsight.</span></span>

2. <span data-ttu-id="689da-179">A virtuális hálózatot az egyéni DNS-kiszolgáló használatára konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="689da-179">Configure the virtual network to use the custom DNS server.</span></span>

3. <span data-ttu-id="689da-180">Megállapítja, hogy az Azure hozzárendelése a virtuális hálózat DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="689da-180">Find the Azure assigned DNS suffix for your virtual network.</span></span> <span data-ttu-id="689da-181">Ez az érték hasonlít `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="689da-181">This value is similar to `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span></span> <span data-ttu-id="689da-182">Az a DNS-utótag megkereséséről további információkért lásd: a [példa: egyéni DNS](#example-dns) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-182">For information on finding the DNS suffix, see the [Example: Custom DNS](#example-dns) section.</span></span>

4. <span data-ttu-id="689da-183">Konfigurálja a DNS-kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="689da-183">Configure forwarding between the DNS servers.</span></span> <span data-ttu-id="689da-184">A konfiguráció a távoli hálózati típusú függ.</span><span class="sxs-lookup"><span data-stu-id="689da-184">The configuration depends on the type of remote network.</span></span>

    * <span data-ttu-id="689da-185">Ha a távoli hálózati egy a helyszíni hálózat, konfigurálja a DNS az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="689da-185">If the remote network is an on-premises network, configure DNS as follows:</span></span>
        
        * <span data-ttu-id="689da-186">__Egyéni DNS__ (a virtuális hálózaton):</span><span class="sxs-lookup"><span data-stu-id="689da-186">__Custom DNS__ (in the virtual network):</span></span>

            * <span data-ttu-id="689da-187">A DNS-utótag, hogy az Azure rekurzív feloldó (168.63.129.16) a virtuális hálózat továbbítási kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="689da-187">Forward requests for the DNS suffix of the virtual network to the Azure recursive resolver (168.63.129.16).</span></span> <span data-ttu-id="689da-188">Azure virtuális hálózatban erőforrásokra vonatkozó kéréseket kezeli</span><span class="sxs-lookup"><span data-stu-id="689da-188">Azure handles requests for resources in the virtual network</span></span>

            * <span data-ttu-id="689da-189">A helyi DNS-kiszolgáló minden más kérelemhez továbbítja.</span><span class="sxs-lookup"><span data-stu-id="689da-189">Forward all other requests to the on-premises DNS server.</span></span> <span data-ttu-id="689da-190">A helyszíni DNS kezeli az összes többi feloldási kérések, még akkor is, kérelmek az internetes erőforrásokhoz, például a Microsoft.com webhelyre mutat.</span><span class="sxs-lookup"><span data-stu-id="689da-190">The on-premises DNS handles all other name resolution requests, even requests for internet resources such as Microsoft.com.</span></span>

        * <span data-ttu-id="689da-191">__A helyi DNS__: kérelmeket a virtuális hálózat DNS-utótag az egyéni DNS-kiszolgálóra továbbítja.</span><span class="sxs-lookup"><span data-stu-id="689da-191">__On-premises DNS__: Forward requests for the virtual network DNS suffix to the custom DNS server.</span></span> <span data-ttu-id="689da-192">Az egyéni DNS-kiszolgáló majd továbbítja az Azure rekurzív feloldó.</span><span class="sxs-lookup"><span data-stu-id="689da-192">The custom DNS server then forwards to the Azure recursive resolver.</span></span>

        <span data-ttu-id="689da-193">A konfigurációs útvonalak kérelmek teljes tartománynevek, amelyek tartalmazzák az egyéni DNS-kiszolgáló a virtuális hálózat DNS-utótagját.</span><span class="sxs-lookup"><span data-stu-id="689da-193">This configuration routes requests for fully qualified domain names that contain the DNS suffix of the virtual network to the custom DNS server.</span></span> <span data-ttu-id="689da-194">Minden más kérelemhez (akár még a nyilvános internet címek) a helyi DNS-kiszolgáló kezeli.</span><span class="sxs-lookup"><span data-stu-id="689da-194">All other requests (even for public internet addresses) are handled by the on-premises DNS server.</span></span>

    * <span data-ttu-id="689da-195">Ha a távoli hálózati egy másik Azure Virtual Network, konfigurálja a DNS az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="689da-195">If the remote network is another Azure Virtual Network, configure DNS as follows:</span></span>

        * <span data-ttu-id="689da-196">__Egyéni DNS__ (az egyes virtuális hálózati):</span><span class="sxs-lookup"><span data-stu-id="689da-196">__Custom DNS__ (in each virtual network):</span></span>

            * <span data-ttu-id="689da-197">A virtuális hálózatok a DNS-utótag kérelmeket a rendszer az egyéni DNS-kiszolgálókra továbbítja.</span><span class="sxs-lookup"><span data-stu-id="689da-197">Requests for the DNS suffix of the virtual networks are forwarded to the custom DNS servers.</span></span> <span data-ttu-id="689da-198">Az egyes virtuális hálózati DNS felelős megoldása érdekében a hálózati erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="689da-198">The DNS in each virtual network is responsible for resolving resources within its network.</span></span>

            * <span data-ttu-id="689da-199">Minden más kérelemhez továbbítja az Azure rekurzív feloldó.</span><span class="sxs-lookup"><span data-stu-id="689da-199">Forward all other requests to the Azure recursive resolver.</span></span> <span data-ttu-id="689da-200">A rekurzív feloldó felelős helyi feloldására és az internetes erőforrásokban.</span><span class="sxs-lookup"><span data-stu-id="689da-200">The recursive resolver is responsible for resolving local and internet resources.</span></span>

        <span data-ttu-id="689da-201">A DNS-kiszolgáló az egyes hálózati továbbítja a másikra, kérelmek alapján DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="689da-201">The DNS server for each network forwards requests to the other, based on DNS suffix.</span></span> <span data-ttu-id="689da-202">Más kérések használata az Azure rekurzív feloldó segítségével.</span><span class="sxs-lookup"><span data-stu-id="689da-202">Other requests are resolved using the Azure recursive resolver.</span></span>

    <span data-ttu-id="689da-203">Minden egyes konfigurációs példáért lásd: a [példa: egyéni DNS](#example-dns) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-203">For an example of each configuration, see the [Example: Custom DNS](#example-dns) section.</span></span>

<span data-ttu-id="689da-204">További információkért lásd: a [névfeloldás virtuális gépek és a Szerepkörpéldányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-204">For more information, see the [Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) document.</span></span>

## <a name="directly-connect-to-hadoop-services"></a><span data-ttu-id="689da-205">Közvetlenül csatlakozik a Hadoop-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="689da-205">Directly connect to Hadoop services</span></span>

<span data-ttu-id="689da-206">A legtöbb dokumentáció a HDInsight feltételezi, hogy rendelkezik-e a fürt eléréséhez az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="689da-206">Most documentation on HDInsight assumes that you have access to the cluster over the internet.</span></span> <span data-ttu-id="689da-207">Például, hogy a fürthöz https://CLUSTERNAME.azurehdinsight.net kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="689da-207">For example, that you can connect to the cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="689da-208">A cím használja a nyilvános átjáró, amely nem érhető el, ha már használta az NSG-k vagy udr-EK hozzáférés korlátozása az internetről.</span><span class="sxs-lookup"><span data-stu-id="689da-208">This address uses the public gateway, which is not available if you have used NSGs or UDRs to restrict access from the internet.</span></span>

<span data-ttu-id="689da-209">Ambari és más weblapok a virtuális hálózaton keresztül csatlakozhat, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="689da-209">To connect to Ambari and other web pages through the virtual network, use the following steps:</span></span>

1. <span data-ttu-id="689da-210">Annak megállapításához, a belső teljes tartománynevek (FQDN), a HDInsight-fürtcsomóponton, használja a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="689da-210">To discover the internal fully qualified domain names (FQDN) of the HDInsight cluster nodes, use one of the following methods:</span></span>

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

    <span data-ttu-id="689da-211">A visszaadott csomópontok közül a teljes Tartománynevet az átjárócsomópontokkal található, és a teljes tartománynevek használatával Ambari és egyéb webes szolgáltatásokhoz való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="689da-211">In the list of nodes returned, find the FQDN for the head nodes and use the FQDNs to connect to Ambari and other web services.</span></span> <span data-ttu-id="689da-212">Tegyük fel például, `http://<headnode-fqdn>:8080` Ambari eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="689da-212">For example, use `http://<headnode-fqdn>:8080` to access Ambari.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="689da-213">Az átjárócsomópontokkal tárolt egyes szolgáltatások aktívak csak az egyik csomópont egyszerre.</span><span class="sxs-lookup"><span data-stu-id="689da-213">Some services hosted on the head nodes are only active on one node at a time.</span></span> <span data-ttu-id="689da-214">Próbálja meg egy központi csomóponton szolgáltatások elérésére és a 404-es hibaüzenetet ad vissza, ha a többi átjárócsomópont váltani.</span><span class="sxs-lookup"><span data-stu-id="689da-214">If you try accessing a service on one head node and it returns a 404 error, switch to the other head node.</span></span>

2. <span data-ttu-id="689da-215">A csomópont és a portot, amelyet a szolgáltatás elérhető megállapításához lásd: a [a HDInsight Hadoop-szolgáltatás által használt portok](./hdinsight-hadoop-port-settings-for-services.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-215">To determine the node and port that a service is available on, see the [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

## <span data-ttu-id="689da-216"><a id="networktraffic"></a>Hálózati forgalom vezérlése</span><span class="sxs-lookup"><span data-stu-id="689da-216"><a id="networktraffic"></a> Controlling network traffic</span></span>

<span data-ttu-id="689da-217">Egy Azure virtuális hálózatot a hálózati forgalmat a következő módszerekkel is vezérelhető:</span><span class="sxs-lookup"><span data-stu-id="689da-217">Network traffic in an Azure Virtual Networks can be controlled using the following methods:</span></span>

* <span data-ttu-id="689da-218">**Hálózati biztonsági csoportok** (NSG) lehetővé teszi a bejövő és kimenő forgalmat a hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="689da-218">**Network security groups** (NSG) allow you to filter inbound and outbound traffic to the network.</span></span> <span data-ttu-id="689da-219">További információkért lásd: a [hálózati forgalmat hálózati biztonsági csoportokkal](../virtual-network/virtual-networks-nsg.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-219">For more information, see the [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    > [!WARNING]
    > <span data-ttu-id="689da-220">A HDInsight nem támogatja a kimenő forgalom korlátozása.</span><span class="sxs-lookup"><span data-stu-id="689da-220">HDInsight does not support restricting outbound traffic.</span></span>

* <span data-ttu-id="689da-221">**Felhasználó által definiált útvonalak** (UDR) határozza meg, hogyan a forgalom között a hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="689da-221">**User-defined routes** (UDR) define how traffic flows between resources in the network.</span></span> <span data-ttu-id="689da-222">További információkért lásd: a [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-222">For more information, see the [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md) document.</span></span>

* <span data-ttu-id="689da-223">**Virtuális készülékekre** például tűzfalak és az útválasztókat eszközök működésével replikálni.</span><span class="sxs-lookup"><span data-stu-id="689da-223">**Network virtual appliances** replicate the functionality of devices such as firewalls and routers.</span></span> <span data-ttu-id="689da-224">További információkért lásd: a [hálózati berendezések](https://azure.microsoft.com/solutions/network-appliances) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-224">For more information, see the [Network Appliances](https://azure.microsoft.com/solutions/network-appliances) document.</span></span>

<span data-ttu-id="689da-225">Felügyelt szolgáltatásként HDInsight Azure felhőben Azure állapotát és a felügyeleti szolgáltatások nem korlátozott hozzáférésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="689da-225">As a managed service, HDInsight requires unrestricted access to Azure health and management services in the Azure cloud.</span></span> <span data-ttu-id="689da-226">Az NSG-k és udr-EK használata esetén győződjön meg róla, hogy a HDInsight ezen szolgáltatások továbbra is kommunikál a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="689da-226">When using NSGs and UDRs, you must ensure that HDInsight these services can still communicate with HDInsight.</span></span>

<span data-ttu-id="689da-227">HDInsight szolgáltatások számos portot teszi elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="689da-227">HDInsight exposes services on several ports.</span></span> <span data-ttu-id="689da-228">Ha egy virtuális készülékre tűzfalat használ, engedélyeznie kell a forgalom a portokon folyik a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="689da-228">When using a virtual appliance firewall, you must allow traffic on the ports used for these services.</span></span> <span data-ttu-id="689da-229">További információkért lásd: a [szükséges portok] szakaszban.</span><span class="sxs-lookup"><span data-stu-id="689da-229">For more information, see the [Required ports] section.</span></span>

### <span data-ttu-id="689da-230"><a id="hdinsight-ip"></a>A hálózati biztonsági csoportok és a felhasználó által definiált útvonalak HDInsight</span><span class="sxs-lookup"><span data-stu-id="689da-230"><a id="hdinsight-ip"></a> HDInsight with network security groups and user-defined routes</span></span>

<span data-ttu-id="689da-231">Ha a kíván használni **hálózati biztonsági csoportok** vagy **felhasználó által definiált útvonalak** szabályozza a hálózati forgalmat, HDInsight telepítése előtt a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="689da-231">If you plan on using **network security groups** or **user-defined routes** to control network traffic, perform the following actions before installing HDInsight:</span></span>

1. <span data-ttu-id="689da-232">Azonosítsa a HDInsight használni kívánt Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="689da-232">Identify the Azure region that you plan to use for HDInsight.</span></span>

2. <span data-ttu-id="689da-233">A HDInsight által megkövetelt IP-címek azonosításához.</span><span class="sxs-lookup"><span data-stu-id="689da-233">Identify the IP addresses required by HDInsight.</span></span> <span data-ttu-id="689da-234">További információkért lásd: a [HDInsight által megkövetelt IP-címek](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-234">For more information, see the [IP Addresses required by HDInsight](#hdinsight-ip) section.</span></span>

3. <span data-ttu-id="689da-235">Hozzon létre, vagy módosítsa a hálózati biztonsági csoportok vagy a felhasználó által definiált útvonalak az alhálózat, amely a HDInsight telepítését tervezi.</span><span class="sxs-lookup"><span data-stu-id="689da-235">Create or modify the network security groups or user-defined routes for the subnet that you plan to install HDInsight into.</span></span>

    * <span data-ttu-id="689da-236">__Hálózati biztonsági csoportok__: engedélyezése __bejövő__ porton forgalom __443-as__ IP-címek.</span><span class="sxs-lookup"><span data-stu-id="689da-236">__Network security groups__: allow __inbound__ traffic on port __443__ from the IP addresses.</span></span>
    * <span data-ttu-id="689da-237">__Felhasználó által definiált útvonalak__: hozzon létre egy olyan útvonalat, minden IP-címre, és állítsa be a __a következő ugrás típusa__ való __Internet__.</span><span class="sxs-lookup"><span data-stu-id="689da-237">__User-defined routes__: create a route to each IP address and set the __Next hop type__ to __Internet__.</span></span>

<span data-ttu-id="689da-238">Hálózati biztonsági csoport vagy felhasználó által definiált útvonalak további információkért lásd az alábbi dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="689da-238">For more information on network security groups or user-defined routes, see the following documentation:</span></span>

* [<span data-ttu-id="689da-239">Hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="689da-239">Network security group</span></span>](../virtual-network/virtual-networks-nsg.md)

* [<span data-ttu-id="689da-240">Felhasználó által definiált útvonalak</span><span class="sxs-lookup"><span data-stu-id="689da-240">User-defined routes</span></span>](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a><span data-ttu-id="689da-241">Alagúthasználat kényszerítése</span><span class="sxs-lookup"><span data-stu-id="689da-241">Forced tunneling</span></span>

<span data-ttu-id="689da-242">A kényszerített bújtatás a beállítás a felhasználói útválasztási ahol alhálózatból származó összes forgalmat egy adott hálózaton vagy a helyre, például a helyszíni hálózat kényszeríti.</span><span class="sxs-lookup"><span data-stu-id="689da-242">Forced tunneling is a user-defined routing configuration where all traffic from a subnet is forced to a specific network or location, such as your on-premises network.</span></span> <span data-ttu-id="689da-243">HDInsight does __nem__ támogatási kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="689da-243">HDInsight does __not__ support forced tunneling.</span></span>

## <span data-ttu-id="689da-244"><a id="hdinsight-ip"></a>Szükséges IP-címek</span><span class="sxs-lookup"><span data-stu-id="689da-244"><a id="hdinsight-ip"></a> Required IP addresses</span></span>

> [!IMPORTANT]
> <span data-ttu-id="689da-245">Azure-állapot és a felügyeleti szolgáltatás képes kommunikálni a HDInsight kell lennie.</span><span class="sxs-lookup"><span data-stu-id="689da-245">The Azure health and management services must be able to communicate with HDInsight.</span></span> <span data-ttu-id="689da-246">A hálózati biztonsági csoport vagy felhasználó által definiált útvonalakat, ha ezen szolgáltatások HDInsight eléréséhez az IP-címekről érkező forgalom engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="689da-246">If you use network security groups or user-defined routes, allow traffic from the IP addresses for these services to reach HDInsight.</span></span>
>
> <span data-ttu-id="689da-247">Ha a forgalmat hálózati biztonsági csoport vagy felhasználó által definiált útvonalak nem használja, figyelmen kívül hagyhatja ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="689da-247">If you do not use network security groups or user-defined routes to control traffic, you can ignore this section.</span></span>

<span data-ttu-id="689da-248">Ha hálózati biztonsági csoport vagy felhasználó által definiált útvonalak, engedélyeznie kell a forgalmat az Azure állapot- és felügyeleti szolgáltatással HDInsight eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="689da-248">If you use network security groups or user-defined routes, you must allow traffic from the Azure health and management services to reach HDInsight.</span></span> <span data-ttu-id="689da-249">Az alábbi lépések segítségével engedélyezni kell az IP-címek keresése:</span><span class="sxs-lookup"><span data-stu-id="689da-249">Use the following steps to find the IP addresses that must be allowed:</span></span>

1. <span data-ttu-id="689da-250">Mindig engedélyeznie kell a következő IP-címekről érkező adatforgalmat:</span><span class="sxs-lookup"><span data-stu-id="689da-250">You must always allow traffic from the following IP addresses:</span></span>

    | <span data-ttu-id="689da-251">IP-cím</span><span class="sxs-lookup"><span data-stu-id="689da-251">IP address</span></span> | <span data-ttu-id="689da-252">Engedélyezett port</span><span class="sxs-lookup"><span data-stu-id="689da-252">Allowed port</span></span> | <span data-ttu-id="689da-253">Irány</span><span class="sxs-lookup"><span data-stu-id="689da-253">Direction</span></span> |
    | ---- | ----- | ----- |
    | <span data-ttu-id="689da-254">168.61.49.99</span><span class="sxs-lookup"><span data-stu-id="689da-254">168.61.49.99</span></span> | <span data-ttu-id="689da-255">443</span><span class="sxs-lookup"><span data-stu-id="689da-255">443</span></span> | <span data-ttu-id="689da-256">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-256">Inbound</span></span> |
    | <span data-ttu-id="689da-257">23.99.5.239</span><span class="sxs-lookup"><span data-stu-id="689da-257">23.99.5.239</span></span> | <span data-ttu-id="689da-258">443</span><span class="sxs-lookup"><span data-stu-id="689da-258">443</span></span> | <span data-ttu-id="689da-259">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-259">Inbound</span></span> |
    | <span data-ttu-id="689da-260">168.61.48.131</span><span class="sxs-lookup"><span data-stu-id="689da-260">168.61.48.131</span></span> | <span data-ttu-id="689da-261">443</span><span class="sxs-lookup"><span data-stu-id="689da-261">443</span></span> | <span data-ttu-id="689da-262">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-262">Inbound</span></span> |
    | <span data-ttu-id="689da-263">138.91.141.162</span><span class="sxs-lookup"><span data-stu-id="689da-263">138.91.141.162</span></span> | <span data-ttu-id="689da-264">443</span><span class="sxs-lookup"><span data-stu-id="689da-264">443</span></span> | <span data-ttu-id="689da-265">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-265">Inbound</span></span> |

2. <span data-ttu-id="689da-266">Ha a HDInsight-fürthöz a következő területek közül, majd engedélyeznie kell a forgalmat a régió felsorolt IP-címekről:</span><span class="sxs-lookup"><span data-stu-id="689da-266">If your HDInsight cluster is in one of the following regions, then you must allow traffic from the IP addresses listed for the region:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="689da-267">Ha nem szerepel az Azure-régió használ, csak használja az 1. lépés négy IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="689da-267">If the Azure region you are using is not listed, then only use the four IP addresses from step 1.</span></span>

    | <span data-ttu-id="689da-268">Ország</span><span class="sxs-lookup"><span data-stu-id="689da-268">Country</span></span> | <span data-ttu-id="689da-269">Régió</span><span class="sxs-lookup"><span data-stu-id="689da-269">Region</span></span> | <span data-ttu-id="689da-270">Engedélyezett IP-címek</span><span class="sxs-lookup"><span data-stu-id="689da-270">Allowed IP addresses</span></span> | <span data-ttu-id="689da-271">Engedélyezett port</span><span class="sxs-lookup"><span data-stu-id="689da-271">Allowed port</span></span> | <span data-ttu-id="689da-272">Irány</span><span class="sxs-lookup"><span data-stu-id="689da-272">Direction</span></span> |
    | ---- | ---- | ---- | ---- | ----- |
    | <span data-ttu-id="689da-273">Ázsia</span><span class="sxs-lookup"><span data-stu-id="689da-273">Asia</span></span> | <span data-ttu-id="689da-274">Kelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="689da-274">East Asia</span></span> | <span data-ttu-id="689da-275">23.102.235.122</span><span class="sxs-lookup"><span data-stu-id="689da-275">23.102.235.122</span></span></br><span data-ttu-id="689da-276">52.175.38.134</span><span class="sxs-lookup"><span data-stu-id="689da-276">52.175.38.134</span></span> | <span data-ttu-id="689da-277">443</span><span class="sxs-lookup"><span data-stu-id="689da-277">443</span></span> | <span data-ttu-id="689da-278">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-278">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-279">Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="689da-279">Southeast Asia</span></span> | <span data-ttu-id="689da-280">13.76.245.160</span><span class="sxs-lookup"><span data-stu-id="689da-280">13.76.245.160</span></span></br><span data-ttu-id="689da-281">13.76.136.249</span><span class="sxs-lookup"><span data-stu-id="689da-281">13.76.136.249</span></span> | <span data-ttu-id="689da-282">443</span><span class="sxs-lookup"><span data-stu-id="689da-282">443</span></span> | <span data-ttu-id="689da-283">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-283">Inbound</span></span> |
    | <span data-ttu-id="689da-284">Ausztrália</span><span class="sxs-lookup"><span data-stu-id="689da-284">Australia</span></span> | <span data-ttu-id="689da-285">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="689da-285">Australia East</span></span> | <span data-ttu-id="689da-286">104.210.84.115</span><span class="sxs-lookup"><span data-stu-id="689da-286">104.210.84.115</span></span></br><span data-ttu-id="689da-287">13.75.152.195</span><span class="sxs-lookup"><span data-stu-id="689da-287">13.75.152.195</span></span> | <span data-ttu-id="689da-288">443</span><span class="sxs-lookup"><span data-stu-id="689da-288">443</span></span> | <span data-ttu-id="689da-289">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-289">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-290">Délkelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="689da-290">Australia Southeast</span></span> | <span data-ttu-id="689da-291">13.77.2.56</span><span class="sxs-lookup"><span data-stu-id="689da-291">13.77.2.56</span></span></br><span data-ttu-id="689da-292">13.77.2.94</span><span class="sxs-lookup"><span data-stu-id="689da-292">13.77.2.94</span></span> | <span data-ttu-id="689da-293">443</span><span class="sxs-lookup"><span data-stu-id="689da-293">443</span></span> | <span data-ttu-id="689da-294">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-294">Inbound</span></span> |
    | <span data-ttu-id="689da-295">Brazília</span><span class="sxs-lookup"><span data-stu-id="689da-295">Brazil</span></span> | <span data-ttu-id="689da-296">Dél-Brazília</span><span class="sxs-lookup"><span data-stu-id="689da-296">Brazil South</span></span> | <span data-ttu-id="689da-297">191.235.84.104</span><span class="sxs-lookup"><span data-stu-id="689da-297">191.235.84.104</span></span></br><span data-ttu-id="689da-298">191.235.87.113</span><span class="sxs-lookup"><span data-stu-id="689da-298">191.235.87.113</span></span> | <span data-ttu-id="689da-299">443</span><span class="sxs-lookup"><span data-stu-id="689da-299">443</span></span> | <span data-ttu-id="689da-300">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-300">Inbound</span></span> |
    | <span data-ttu-id="689da-301">Kanada</span><span class="sxs-lookup"><span data-stu-id="689da-301">Canada</span></span> | <span data-ttu-id="689da-302">Kelet-Kanada</span><span class="sxs-lookup"><span data-stu-id="689da-302">Canada East</span></span> | <span data-ttu-id="689da-303">52.229.127.96</span><span class="sxs-lookup"><span data-stu-id="689da-303">52.229.127.96</span></span></br><span data-ttu-id="689da-304">52.229.123.172</span><span class="sxs-lookup"><span data-stu-id="689da-304">52.229.123.172</span></span> | <span data-ttu-id="689da-305">443</span><span class="sxs-lookup"><span data-stu-id="689da-305">443</span></span> | <span data-ttu-id="689da-306">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-306">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-307">Közép-Kanada</span><span class="sxs-lookup"><span data-stu-id="689da-307">Canada Central</span></span> | <span data-ttu-id="689da-308">52.228.37.66</span><span class="sxs-lookup"><span data-stu-id="689da-308">52.228.37.66</span></span></br><span data-ttu-id="689da-309">52.228.45.222</span><span class="sxs-lookup"><span data-stu-id="689da-309">52.228.45.222</span></span> | <span data-ttu-id="689da-310">443</span><span class="sxs-lookup"><span data-stu-id="689da-310">443</span></span> | <span data-ttu-id="689da-311">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-311">Inbound</span></span> |
    | <span data-ttu-id="689da-312">Kína</span><span class="sxs-lookup"><span data-stu-id="689da-312">China</span></span> | <span data-ttu-id="689da-313">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="689da-313">China North</span></span> | <span data-ttu-id="689da-314">42.159.96.170</span><span class="sxs-lookup"><span data-stu-id="689da-314">42.159.96.170</span></span></br><span data-ttu-id="689da-315">139.217.2.219</span><span class="sxs-lookup"><span data-stu-id="689da-315">139.217.2.219</span></span> | <span data-ttu-id="689da-316">443</span><span class="sxs-lookup"><span data-stu-id="689da-316">443</span></span> | <span data-ttu-id="689da-317">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-317">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-318">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="689da-318">China East</span></span> | <span data-ttu-id="689da-319">42.159.198.178</span><span class="sxs-lookup"><span data-stu-id="689da-319">42.159.198.178</span></span></br><span data-ttu-id="689da-320">42.159.234.157</span><span class="sxs-lookup"><span data-stu-id="689da-320">42.159.234.157</span></span> | <span data-ttu-id="689da-321">443</span><span class="sxs-lookup"><span data-stu-id="689da-321">443</span></span> | <span data-ttu-id="689da-322">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-322">Inbound</span></span> |
    | <span data-ttu-id="689da-323">Európa</span><span class="sxs-lookup"><span data-stu-id="689da-323">Europe</span></span> | <span data-ttu-id="689da-324">Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="689da-324">North Europe</span></span> | <span data-ttu-id="689da-325">52.164.210.96</span><span class="sxs-lookup"><span data-stu-id="689da-325">52.164.210.96</span></span></br><span data-ttu-id="689da-326">13.74.153.132</span><span class="sxs-lookup"><span data-stu-id="689da-326">13.74.153.132</span></span> | <span data-ttu-id="689da-327">443</span><span class="sxs-lookup"><span data-stu-id="689da-327">443</span></span> | <span data-ttu-id="689da-328">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-328">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-329">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="689da-329">West Europe</span></span>| <span data-ttu-id="689da-330">52.166.243.90</span><span class="sxs-lookup"><span data-stu-id="689da-330">52.166.243.90</span></span></br><span data-ttu-id="689da-331">52.174.36.244</span><span class="sxs-lookup"><span data-stu-id="689da-331">52.174.36.244</span></span> | <span data-ttu-id="689da-332">443</span><span class="sxs-lookup"><span data-stu-id="689da-332">443</span></span> | <span data-ttu-id="689da-333">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-333">Inbound</span></span> |
    | <span data-ttu-id="689da-334">Németország</span><span class="sxs-lookup"><span data-stu-id="689da-334">Germany</span></span> | <span data-ttu-id="689da-335">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="689da-335">Germany Central</span></span> | <span data-ttu-id="689da-336">51.4.146.68</span><span class="sxs-lookup"><span data-stu-id="689da-336">51.4.146.68</span></span></br><span data-ttu-id="689da-337">51.4.146.80</span><span class="sxs-lookup"><span data-stu-id="689da-337">51.4.146.80</span></span> | <span data-ttu-id="689da-338">443</span><span class="sxs-lookup"><span data-stu-id="689da-338">443</span></span> | <span data-ttu-id="689da-339">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-339">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-340">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="689da-340">Germany Northeast</span></span> | <span data-ttu-id="689da-341">51.5.150.132</span><span class="sxs-lookup"><span data-stu-id="689da-341">51.5.150.132</span></span></br><span data-ttu-id="689da-342">51.5.144.101</span><span class="sxs-lookup"><span data-stu-id="689da-342">51.5.144.101</span></span> | <span data-ttu-id="689da-343">443</span><span class="sxs-lookup"><span data-stu-id="689da-343">443</span></span> | <span data-ttu-id="689da-344">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-344">Inbound</span></span> |
    | <span data-ttu-id="689da-345">India</span><span class="sxs-lookup"><span data-stu-id="689da-345">India</span></span> | <span data-ttu-id="689da-346">Közép-India</span><span class="sxs-lookup"><span data-stu-id="689da-346">Central India</span></span> | <span data-ttu-id="689da-347">52.172.153.209</span><span class="sxs-lookup"><span data-stu-id="689da-347">52.172.153.209</span></span></br><span data-ttu-id="689da-348">52.172.152.49</span><span class="sxs-lookup"><span data-stu-id="689da-348">52.172.152.49</span></span> | <span data-ttu-id="689da-349">443</span><span class="sxs-lookup"><span data-stu-id="689da-349">443</span></span> | <span data-ttu-id="689da-350">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-350">Inbound</span></span> |
    | <span data-ttu-id="689da-351">Japán</span><span class="sxs-lookup"><span data-stu-id="689da-351">Japan</span></span> | <span data-ttu-id="689da-352">Kelet-Japán</span><span class="sxs-lookup"><span data-stu-id="689da-352">Japan East</span></span> | <span data-ttu-id="689da-353">13.78.125.90</span><span class="sxs-lookup"><span data-stu-id="689da-353">13.78.125.90</span></span></br><span data-ttu-id="689da-354">13.78.89.60</span><span class="sxs-lookup"><span data-stu-id="689da-354">13.78.89.60</span></span> | <span data-ttu-id="689da-355">443</span><span class="sxs-lookup"><span data-stu-id="689da-355">443</span></span> | <span data-ttu-id="689da-356">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-356">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-357">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="689da-357">Japan West</span></span> | <span data-ttu-id="689da-358">40.74.125.69</span><span class="sxs-lookup"><span data-stu-id="689da-358">40.74.125.69</span></span></br><span data-ttu-id="689da-359">138.91.29.150</span><span class="sxs-lookup"><span data-stu-id="689da-359">138.91.29.150</span></span> | <span data-ttu-id="689da-360">443</span><span class="sxs-lookup"><span data-stu-id="689da-360">443</span></span> | <span data-ttu-id="689da-361">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-361">Inbound</span></span> |
    | <span data-ttu-id="689da-362">Korea</span><span class="sxs-lookup"><span data-stu-id="689da-362">Korea</span></span> | <span data-ttu-id="689da-363">Korea középső régiója</span><span class="sxs-lookup"><span data-stu-id="689da-363">Korea Central</span></span> | <span data-ttu-id="689da-364">52.231.39.142</span><span class="sxs-lookup"><span data-stu-id="689da-364">52.231.39.142</span></span></br><span data-ttu-id="689da-365">52.231.36.209</span><span class="sxs-lookup"><span data-stu-id="689da-365">52.231.36.209</span></span> | <span data-ttu-id="689da-366">433</span><span class="sxs-lookup"><span data-stu-id="689da-366">433</span></span> | <span data-ttu-id="689da-367">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-367">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-368">Korea déli régiója</span><span class="sxs-lookup"><span data-stu-id="689da-368">Korea South</span></span> | <span data-ttu-id="689da-369">52.231.203.16</span><span class="sxs-lookup"><span data-stu-id="689da-369">52.231.203.16</span></span></br><span data-ttu-id="689da-370">52.231.205.214</span><span class="sxs-lookup"><span data-stu-id="689da-370">52.231.205.214</span></span> | <span data-ttu-id="689da-371">443</span><span class="sxs-lookup"><span data-stu-id="689da-371">443</span></span> | <span data-ttu-id="689da-372">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-372">Inbound</span></span>
    | <span data-ttu-id="689da-373">Egyesült Királyság</span><span class="sxs-lookup"><span data-stu-id="689da-373">United Kingdom</span></span> | <span data-ttu-id="689da-374">Az Egyesült Királyság nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="689da-374">UK West</span></span> | <span data-ttu-id="689da-375">51.141.13.110</span><span class="sxs-lookup"><span data-stu-id="689da-375">51.141.13.110</span></span></br><span data-ttu-id="689da-376">51.141.7.20</span><span class="sxs-lookup"><span data-stu-id="689da-376">51.141.7.20</span></span> | <span data-ttu-id="689da-377">443</span><span class="sxs-lookup"><span data-stu-id="689da-377">443</span></span> | <span data-ttu-id="689da-378">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-378">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-379">Az Egyesült Királyság déli régiója</span><span class="sxs-lookup"><span data-stu-id="689da-379">UK South</span></span> | <span data-ttu-id="689da-380">51.140.47.39</span><span class="sxs-lookup"><span data-stu-id="689da-380">51.140.47.39</span></span></br><span data-ttu-id="689da-381">51.140.52.16</span><span class="sxs-lookup"><span data-stu-id="689da-381">51.140.52.16</span></span> | <span data-ttu-id="689da-382">443</span><span class="sxs-lookup"><span data-stu-id="689da-382">443</span></span> | <span data-ttu-id="689da-383">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-383">Inbound</span></span> |
    | <span data-ttu-id="689da-384">Egyesült Államok</span><span class="sxs-lookup"><span data-stu-id="689da-384">United States</span></span> | <span data-ttu-id="689da-385">USA középső régiója</span><span class="sxs-lookup"><span data-stu-id="689da-385">Central US</span></span> | <span data-ttu-id="689da-386">13.67.223.215</span><span class="sxs-lookup"><span data-stu-id="689da-386">13.67.223.215</span></span></br><span data-ttu-id="689da-387">40.86.83.253</span><span class="sxs-lookup"><span data-stu-id="689da-387">40.86.83.253</span></span> | <span data-ttu-id="689da-388">443</span><span class="sxs-lookup"><span data-stu-id="689da-388">443</span></span> | <span data-ttu-id="689da-389">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-389">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-390">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="689da-390">North Central US</span></span> | <span data-ttu-id="689da-391">157.56.8.38</span><span class="sxs-lookup"><span data-stu-id="689da-391">157.56.8.38</span></span></br><span data-ttu-id="689da-392">157.55.213.99</span><span class="sxs-lookup"><span data-stu-id="689da-392">157.55.213.99</span></span> | <span data-ttu-id="689da-393">443</span><span class="sxs-lookup"><span data-stu-id="689da-393">443</span></span> | <span data-ttu-id="689da-394">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-394">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-395">USA nyugati középső régiója</span><span class="sxs-lookup"><span data-stu-id="689da-395">West Central US</span></span> | <span data-ttu-id="689da-396">52.161.23.15</span><span class="sxs-lookup"><span data-stu-id="689da-396">52.161.23.15</span></span></br><span data-ttu-id="689da-397">52.161.10.167</span><span class="sxs-lookup"><span data-stu-id="689da-397">52.161.10.167</span></span> | <span data-ttu-id="689da-398">443</span><span class="sxs-lookup"><span data-stu-id="689da-398">443</span></span> | <span data-ttu-id="689da-399">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-399">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="689da-400">USA nyugati régiója, 2.</span><span class="sxs-lookup"><span data-stu-id="689da-400">West US 2</span></span> | <span data-ttu-id="689da-401">52.175.211.210</span><span class="sxs-lookup"><span data-stu-id="689da-401">52.175.211.210</span></span></br><span data-ttu-id="689da-402">52.175.222.222</span><span class="sxs-lookup"><span data-stu-id="689da-402">52.175.222.222</span></span> | <span data-ttu-id="689da-403">443</span><span class="sxs-lookup"><span data-stu-id="689da-403">443</span></span> | <span data-ttu-id="689da-404">Bejövő</span><span class="sxs-lookup"><span data-stu-id="689da-404">Inbound</span></span> |

    <span data-ttu-id="689da-405">Az IP-címek az Azure Government használandó információkért lásd: a [Azure Government Eszközintelligencia + analitika](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-405">For information on the IP addresses to use for Azure Government, see the [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) document.</span></span>

3. <span data-ttu-id="689da-406">Ha egy egyéni DNS-kiszolgáló használ a virtuális hálózat, is engedélyeznie kell a hozzáférést a __168.63.129.16__.</span><span class="sxs-lookup"><span data-stu-id="689da-406">If you use a custom DNS server with your virtual network, you must also allow access from __168.63.129.16__.</span></span> <span data-ttu-id="689da-407">Ez a cím az Azure rekurzív feloldó.</span><span class="sxs-lookup"><span data-stu-id="689da-407">This address is Azure's recursive resolver.</span></span> <span data-ttu-id="689da-408">További információkért lásd: a [virtuális gépek és a szerepkör névfeloldását példányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-408">For more information, see the [Name resolution for VMs and Role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.</span></span>

<span data-ttu-id="689da-409">További információkért lásd: a [hálózati forgalom vezérlése](#networktraffic) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-409">For more information, see the [Controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="689da-410"><a id="hdinsight-ports"></a>Szükséges portok</span><span class="sxs-lookup"><span data-stu-id="689da-410"><a id="hdinsight-ports"></a> Required ports</span></span>

<span data-ttu-id="689da-411">Ha a hálózat használatával kíván **virtuális készülék tűzfal** megvédi a virtuális hálózatot, engedélyeznie kell a kimenő adatforgalmat a következő portokat:</span><span class="sxs-lookup"><span data-stu-id="689da-411">If you plan on using a network **virtual appliance firewall** to secure the virtual network, you must allow outbound traffic on the following ports:</span></span>

* <span data-ttu-id="689da-412">53</span><span class="sxs-lookup"><span data-stu-id="689da-412">53</span></span>
* <span data-ttu-id="689da-413">443</span><span class="sxs-lookup"><span data-stu-id="689da-413">443</span></span>
* <span data-ttu-id="689da-414">1433</span><span class="sxs-lookup"><span data-stu-id="689da-414">1433</span></span>
* <span data-ttu-id="689da-415">11000-11999</span><span class="sxs-lookup"><span data-stu-id="689da-415">11000-11999</span></span>
* <span data-ttu-id="689da-416">14000-14999</span><span class="sxs-lookup"><span data-stu-id="689da-416">14000-14999</span></span>

<span data-ttu-id="689da-417">A portok adott szolgáltatások listájáért lásd: a [a HDInsight Hadoop-szolgáltatás által használt portok](hdinsight-hadoop-port-settings-for-services.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-417">For a list of ports for specific services, see the [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

<span data-ttu-id="689da-418">A virtuális készülékek vonatkozó tűzfalszabályok további információkért lásd: a [virtuális berendezésre telepítik](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="689da-418">For more information on firewall rules for virtual appliances, see the [virtual appliance scenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) document.</span></span>

## <span data-ttu-id="689da-419"><a id="hdinsight-nsg"></a>Példa: hálózati biztonsági csoportok a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="689da-419"><a id="hdinsight-nsg"></a>Example: network security groups with HDInsight</span></span>

<span data-ttu-id="689da-420">Ebben a szakaszban szereplő példák bemutatják, hogyan lehet létrehozni a hálózati biztonsági csoportszabályok, amelyek lehetővé teszik a HDInsight az Azure felügyeleti szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="689da-420">The examples in this section demonstrate how to create network security group rules that allow HDInsight to communicate with the Azure management services.</span></span> <span data-ttu-id="689da-421">A példák használatához állítsa be úgy az IP-címek az Azure-régiót használ megfelelően.</span><span class="sxs-lookup"><span data-stu-id="689da-421">Before using the examples, adjust the IP addresses to match the ones for the Azure region you are using.</span></span> <span data-ttu-id="689da-422">Ezt az információt találja a [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-422">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-resource-management-template"></a><span data-ttu-id="689da-423">Az Azure erőforrás-kezelés sablon</span><span class="sxs-lookup"><span data-stu-id="689da-423">Azure Resource Management template</span></span>

<span data-ttu-id="689da-424">A következő erőforrás-kezelés sablon egy bejövő forgalmát, de lehetővé teszi, hogy a HDInsight által megkövetelt IP-címekről érkező forgalom virtuális hálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="689da-424">The following Resource Management template creates a virtual network that restricts inbound traffic, but allows traffic from the IP addresses required by HDInsight.</span></span> <span data-ttu-id="689da-425">Ez a sablon is létrehoz egy HDInsight-fürt a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="689da-425">This template also creates an HDInsight cluster in the virtual network.</span></span>

* [<span data-ttu-id="689da-426">A biztonságos Azure virtuális hálózat és egy HDInsight Hadoop-fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="689da-426">Deploy a secured Azure Virtual Network and an HDInsight Hadoop cluster</span></span>](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> <span data-ttu-id="689da-427">Módosítsa a megfelelő Azure-régiót használ ebben a példában használt IP-címek.</span><span class="sxs-lookup"><span data-stu-id="689da-427">Change the IP addresses used in this example to match the Azure region you are using.</span></span> <span data-ttu-id="689da-428">Ezt az információt találja a [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-428">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="689da-429">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="689da-429">Azure PowerShell</span></span>

<span data-ttu-id="689da-430">A következő PowerShell-parancsfájl segítségével hozzon létre egy virtuális hálózatot, amely korlátozza a bejövő forgalmat, és lehetővé teszi, hogy az IP-címet az Észak-Európa régió forgalmát.</span><span class="sxs-lookup"><span data-stu-id="689da-430">Use the following PowerShell script to create a virtual network that restricts inbound traffic and allows traffic from the IP addresses for the North Europe region.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="689da-431">Módosítsa a megfelelő Azure-régiót használ ebben a példában használt IP-címek.</span><span class="sxs-lookup"><span data-stu-id="689da-431">Change the IP addresses used in this example to match the Azure region you are using.</span></span> <span data-ttu-id="689da-432">Ezt az információt találja a [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-432">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "blockeverything" `
        -Description "Block everything else" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "*" `
        -SourceAddressPrefix "Internet" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Deny `
        -Priority 500 `
        -Direction Inbound
# Set the changes to the security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> <span data-ttu-id="689da-433">Ez a példa bemutatja, hogyan engedélyezze a bejövő forgalmat a szükséges IP-címek a szabályok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="689da-433">This example demonstrates how to add rules to allow inbound traffic on the required IP addresses.</span></span> <span data-ttu-id="689da-434">Egy szabályt, amely a más forrásokból bejövő hozzáférés korlátozása nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="689da-434">It does not contain a rule to restrict inbound access from other sources.</span></span>
>
> <span data-ttu-id="689da-435">A következő példa bemutatja az internetről SSH-hozzáférés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="689da-435">The following example demonstrates how to enable SSH access from the Internet:</span></span>
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a><span data-ttu-id="689da-436">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="689da-436">Azure CLI</span></span>

<span data-ttu-id="689da-437">Az alábbi lépések segítségével hozzon létre egy virtuális hálózatot, amely korlátozza a bejövő forgalmat, de lehetővé teszi, hogy a HDInsight által megkövetelt IP-címekről érkező forgalmat.</span><span class="sxs-lookup"><span data-stu-id="689da-437">Use the following steps to create a virtual network that restricts inbound traffic, but allows traffic from the IP addresses required by HDInsight.</span></span>

1. <span data-ttu-id="689da-438">Az alábbi parancs segítségével hozzon létre egy új hálózati biztonsági csoport nevű `hdisecure`.</span><span class="sxs-lookup"><span data-stu-id="689da-438">Use the following command to create a new network security group named `hdisecure`.</span></span> <span data-ttu-id="689da-439">Cserélje le **RESOURCEGROUPNAME** a erőforráscsoporttal, amely tartalmazza az Azure virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="689da-439">Replace **RESOURCEGROUPNAME** with the resource group that contains the Azure Virtual Network.</span></span> <span data-ttu-id="689da-440">Cserélje le **hely** , amely a csoport létrehozásának a helyét (régió).</span><span class="sxs-lookup"><span data-stu-id="689da-440">Replace **LOCATION** with the location (region) that the group was created in.</span></span>

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    <span data-ttu-id="689da-441">A csoport létrehozása után az új csoport tájékoztatást kapni.</span><span class="sxs-lookup"><span data-stu-id="689da-441">Once the group has been created, you receive information on the new group.</span></span>

2. <span data-ttu-id="689da-442">Használja a következő szabályok hozzáadása az új hálózati biztonsági csoportot, amely az Azure HDInsight állapotát és a felügyeleti szolgáltatás a 443-as portot a bejövő kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="689da-442">Use the following to add rules to the new network security group that allow inbound communication on port 443 from the Azure HDInsight health and management service.</span></span> <span data-ttu-id="689da-443">Cserélje le **RESOURCEGROUPNAME** az erőforráscsoport, amely tartalmazza az Azure virtuális hálózat nevével.</span><span class="sxs-lookup"><span data-stu-id="689da-443">Replace **RESOURCEGROUPNAME** with the name of the resource group that contains the Azure Virtual Network.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="689da-444">Módosítsa a megfelelő Azure-régiót használ ebben a példában használt IP-címek.</span><span class="sxs-lookup"><span data-stu-id="689da-444">Change the IP addresses used in this example to match the Azure region you are using.</span></span> <span data-ttu-id="689da-445">Ezt az információt találja a [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-445">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. <span data-ttu-id="689da-446">A hálózati biztonsági csoport egyedi azonosítója lekéréséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="689da-446">To retrieve the unique identifier for this network security group, use the following command:</span></span>

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    <span data-ttu-id="689da-447">Ez a parancs értéket ad vissza az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="689da-447">This command returns a value similar to the following text:</span></span>

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    <span data-ttu-id="689da-448">Ha nem a kívánt eredmény elérése érdekében használja a dupla-azonosító idézőjelbe elemet a parancsban.</span><span class="sxs-lookup"><span data-stu-id="689da-448">Use double-quotes around id in the command if you don't get the expected results.</span></span>

4. <span data-ttu-id="689da-449">A következő paranccsal egy alhálózatot a hálózati biztonsági csoport vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="689da-449">Use the following command to apply the network security group to a subnet.</span></span> <span data-ttu-id="689da-450">Cserélje le a __GUID__ és __RESOURCEGROUPNAME__ az értékeket az előző lépésben adja vissza.</span><span class="sxs-lookup"><span data-stu-id="689da-450">Replace the __GUID__ and __RESOURCEGROUPNAME__ values with the ones returned from the previous step.</span></span> <span data-ttu-id="689da-451">Cserélje le __VNETNAME__ és __SUBNETNAME__ a virtuálishálózat-névnek és a létrehozni kívánt alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="689da-451">Replace __VNETNAME__ and __SUBNETNAME__ with the virtual network name and subnet name that you want to create.</span></span>

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    <span data-ttu-id="689da-452">Ez a parancs után telepítheti a HDInsight a virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="689da-452">Once this command completes, you can install HDInsight into the Virtual Network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="689da-453">Ezeket a lépéseket csak nyissa meg az Azure felhőalapú HDInsight állapotát és a felügyeleti szolgáltatás elérését.</span><span class="sxs-lookup"><span data-stu-id="689da-453">These steps only open access to the HDInsight health and management service on the Azure cloud.</span></span> <span data-ttu-id="689da-454">A virtuális hálózaton kívül a HDInsight-fürt bármely más hozzáférését blokkolja.</span><span class="sxs-lookup"><span data-stu-id="689da-454">Any other access to the HDInsight cluster from outside the Virtual Network is blocked.</span></span> <span data-ttu-id="689da-455">Ahhoz, hogy a virtuális hálózaton kívülről való eléréshez, azonban további hálózati biztonsági csoport szabályokat kell hozzáadnia.</span><span class="sxs-lookup"><span data-stu-id="689da-455">To enable access from outside the virtual network, you must add additional Network Security Group rules.</span></span>
>
> <span data-ttu-id="689da-456">A következő példa bemutatja az internetről SSH-hozzáférés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="689da-456">The following example demonstrates how to enable SSH access from the Internet:</span></span>
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <span data-ttu-id="689da-457"><a id="example-dns"></a>Példa: DNS-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="689da-457"><a id="example-dns"></a> Example: DNS configuration</span></span>

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a><span data-ttu-id="689da-458">Névfeloldás egy virtuális és a csatlakoztatott helyszíni hálózat között</span><span class="sxs-lookup"><span data-stu-id="689da-458">Name resolution between a virtual network and a connected on-premises network</span></span>

<span data-ttu-id="689da-459">Ebben a példában a következő feltételek teszi:</span><span class="sxs-lookup"><span data-stu-id="689da-459">This example makes the following assumptions:</span></span>

* <span data-ttu-id="689da-460">Rendelkezik egy Azure virtuális hálózatot, amely egy VPN-átjáró használatával a helyszíni hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="689da-460">You have an Azure Virtual Network that is connected to an on-premises network using a VPN gateway.</span></span>

* <span data-ttu-id="689da-461">Az egyéni DNS-kiszolgáló a virtuális hálózat fut. Linux vagy Unix operációs rendszert.</span><span class="sxs-lookup"><span data-stu-id="689da-461">The custom DNS server in the virtual network is running Linux or Unix as the operating system.</span></span>

* <span data-ttu-id="689da-462">[Kötési](https://www.isc.org/downloads/bind/) az egyéni DNS-kiszolgálóra van telepítve.</span><span class="sxs-lookup"><span data-stu-id="689da-462">[Bind](https://www.isc.org/downloads/bind/) is installed on the custom DNS server.</span></span>

<span data-ttu-id="689da-463">Az egyéni DNS-kiszolgálón a virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="689da-463">On the custom DNS server in the virtual network:</span></span>

1. <span data-ttu-id="689da-464">Azure PowerShell vagy az Azure CLI segítségével a virtuális hálózat DNS-utótagját található:</span><span class="sxs-lookup"><span data-stu-id="689da-464">Use either Azure PowerShell or Azure CLI to find the DNS suffix of the virtual network:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="689da-465">Az egyéni DNS-kiszolgálón a virtuális hálózat, használja a következő szöveget a tartalmát a `/etc/bind/named.conf.local` fájlt:</span><span class="sxs-lookup"><span data-stu-id="689da-465">On the custom DNS server for the virtual network, use the following text as the contents of the `/etc/bind/named.conf.local` file:</span></span>

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    <span data-ttu-id="689da-466">Cserélje le a `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` értéket a virtuális hálózat DNS-utótagját.</span><span class="sxs-lookup"><span data-stu-id="689da-466">Replace the `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with the DNS suffix of your virtual network.</span></span>

    <span data-ttu-id="689da-467">Ez a konfiguráció összes DNS-kéréseket a virtuális hálózat DNS-utótag az Azure rekurzív feloldó irányítja.</span><span class="sxs-lookup"><span data-stu-id="689da-467">This configuration routes all DNS requests for the DNS suffix of the virtual network to the Azure recursive resolver.</span></span>

2. <span data-ttu-id="689da-468">Az egyéni DNS-kiszolgálón a virtuális hálózat, használja a következő szöveget a tartalmát a `/etc/bind/named.conf.options` fájlt:</span><span class="sxs-lookup"><span data-stu-id="689da-468">On the custom DNS server for the virtual network, use the following text as the contents of the `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="689da-469">Cserélje le a `10.0.0.0/16` érték és az IP-címtartomány a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="689da-469">Replace the `10.0.0.0/16` value with the IP address range of your virtual network.</span></span> <span data-ttu-id="689da-470">Ez a bejegyzés lehetővé teszi, hogy a név feloldása kérelmek címek ebbe a tartományba.</span><span class="sxs-lookup"><span data-stu-id="689da-470">This entry allows name resolution requests addresses within this range.</span></span>

    * <span data-ttu-id="689da-471">A helyszíni hálózat az IP-címtartomány hozzáadása a `acl goodclients { ... }` szakasz.</span><span class="sxs-lookup"><span data-stu-id="689da-471">Add the IP address range of the on-premises network to the `acl goodclients { ... }` section.</span></span>  <span data-ttu-id="689da-472">bejegyzés lehetővé teszi, hogy a források névfeloldási a helyszíni hálózat.</span><span class="sxs-lookup"><span data-stu-id="689da-472">entry allows name resolution requests from resources in the on-premises network.</span></span>
    
    * <span data-ttu-id="689da-473">Cserélje le a értékét `192.168.0.1` a helyi DNS-kiszolgáló IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="689da-473">Replace the value `192.168.0.1` with the IP address of your on-premises DNS server.</span></span> <span data-ttu-id="689da-474">Ez a bejegyzés a helyi DNS-kiszolgáló más DNS-kérelmek irányítja.</span><span class="sxs-lookup"><span data-stu-id="689da-474">This entry routes all other DNS requests to the on-premises DNS server.</span></span>

3. <span data-ttu-id="689da-475">A konfigurációt használja, indítsa újra a kötés.</span><span class="sxs-lookup"><span data-stu-id="689da-475">To use the configuration, restart Bind.</span></span> <span data-ttu-id="689da-476">Például: `sudo service bind9 restart`.</span><span class="sxs-lookup"><span data-stu-id="689da-476">For example, `sudo service bind9 restart`.</span></span>

4. <span data-ttu-id="689da-477">A feltételes továbbítók hozzáadása a helyi DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="689da-477">Add a conditional forwarder to the on-premises DNS server.</span></span> <span data-ttu-id="689da-478">A feltételes továbbító számára, hogy az 1. lépésben a DNS-utótag kérelmeket küldeni az egyéni DNS-kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="689da-478">Configure the conditional forwarder to send requests for the DNS suffix from step 1 to the custom DNS server.</span></span>

    > [!NOTE]
    > <span data-ttu-id="689da-479">A dokumentációban a DNS-szoftver arról, hogyan adhat a feltételes továbbítók.</span><span class="sxs-lookup"><span data-stu-id="689da-479">Consult the documentation for your DNS software for specifics on how to add a conditional forwarder.</span></span>

<span data-ttu-id="689da-480">A lépések elvégzése után csatlakozhat vagy teljes tartománynevét (FQDN) használatával hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="689da-480">After completing these steps, you can connect to resources in either network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="689da-481">Most már telepítheti HDInsight létrehozni a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="689da-481">You can now install HDInsight into the virtual network.</span></span>

### <a name="name-resolution-between-two-connected-virtual-networks"></a><span data-ttu-id="689da-482">Két csatlakoztatott virtuális hálózatok közötti névfeloldás</span><span class="sxs-lookup"><span data-stu-id="689da-482">Name resolution between two connected virtual networks</span></span>

<span data-ttu-id="689da-483">Ebben a példában a következő feltételek teszi:</span><span class="sxs-lookup"><span data-stu-id="689da-483">This example makes the following assumptions:</span></span>

* <span data-ttu-id="689da-484">VPN-átjáró használatával, vagy a társviszony-létesítés csatlakoztatott két Azure virtuális hálózat rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="689da-484">You have two Azure Virtual Networks that are connected using either a VPN gateway or peering.</span></span>

* <span data-ttu-id="689da-485">Az egyéni DNS-kiszolgáló mindkét hálózatokban fut. Linux vagy Unix operációs rendszert.</span><span class="sxs-lookup"><span data-stu-id="689da-485">The custom DNS server in both networks is running Linux or Unix as the operating system.</span></span>

* <span data-ttu-id="689da-486">[Kötési](https://www.isc.org/downloads/bind/) az egyéni DNS-kiszolgálókon telepítve van.</span><span class="sxs-lookup"><span data-stu-id="689da-486">[Bind](https://www.isc.org/downloads/bind/) is installed on the custom DNS servers.</span></span>

1. <span data-ttu-id="689da-487">Azure PowerShell vagy az Azure CLI segítségével mindkét virtuális hálózat DNS-utótagját található:</span><span class="sxs-lookup"><span data-stu-id="689da-487">Use either Azure PowerShell or Azure CLI to find the DNS suffix of both virtual networks:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="689da-488">Használja a következő szöveget a tartalmát a `/etc/bind/named.config.local` fájlt az egyéni DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="689da-488">Use the following text as the contents of the `/etc/bind/named.config.local` file on the custom DNS server.</span></span> <span data-ttu-id="689da-489">Ennek a módosításnak mindkét virtuális hálózat egyéni DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="689da-489">Make this change on the custom DNS server in both virtual networks.</span></span>

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    <span data-ttu-id="689da-490">Cserélje le a `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` értéket a DNS-utótagja a __más__ virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="689da-490">Replace the `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with the DNS suffix of the __other__ virtual network.</span></span> <span data-ttu-id="689da-491">Ez a bejegyzés az egyéni DNS-sel kapcsolatban, hogy a hálózati kérések a távoli hálózati DNS-utótag irányítja.</span><span class="sxs-lookup"><span data-stu-id="689da-491">This entry routes requests for the DNS suffix of the remote network to the custom DNS in that network.</span></span>

3. <span data-ttu-id="689da-492">Az egyéni DNS-kiszolgálókon mindkét virtuális hálózatban, használja a következő szöveget a tartalmát, a `/etc/bind/named.conf.options` fájlt:</span><span class="sxs-lookup"><span data-stu-id="689da-492">On the custom DNS servers in both virtual networks, use the following text as the contents of the `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="689da-493">Cserélje le a `10.0.0.0/16` és `10.1.0.0/16` értékeket az IP-címtartományok a virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="689da-493">Replace the `10.0.0.0/16` and `10.1.0.0/16` values with the IP address ranges of your virtual networks.</span></span> <span data-ttu-id="689da-494">Ez a bejegyzés lehetővé teszi a DNS-kiszolgálók kérelem létrehozására minden egyes hálózati erőforrások.</span><span class="sxs-lookup"><span data-stu-id="689da-494">This entry allows resources in each network to make requests of the DNS servers.</span></span>

    <span data-ttu-id="689da-495">Minden kérést, nem a virtuális hálózatok (Fontos) DNS-utótagokat az Azure rekurzív feloldó kezeli.</span><span class="sxs-lookup"><span data-stu-id="689da-495">Any requests that are not for the DNS suffixes of the virtual networks (for example, microsoft.com) is handled by the Azure recursive resolver.</span></span>

4. <span data-ttu-id="689da-496">A konfigurációt használja, indítsa újra a kötés.</span><span class="sxs-lookup"><span data-stu-id="689da-496">To use the configuration, restart Bind.</span></span> <span data-ttu-id="689da-497">Például `sudo service bind9 restart` mindkét DNS-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="689da-497">For example, `sudo service bind9 restart` on both DNS servers.</span></span>

<span data-ttu-id="689da-498">Ezek a lépések végrehajtását követően csatlakozhat a teljes tartománynevek (FQDN) használatával virtuális hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="689da-498">After completing these steps, you can connect to resources in the virtual network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="689da-499">Most már telepítheti HDInsight létrehozni a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="689da-499">You can now install HDInsight into the virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="689da-500">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="689da-500">Next steps</span></span>

* <span data-ttu-id="689da-501">Például egy végpontok közötti egy a helyszíni hálózathoz való kapcsolódáshoz a HDInsight konfigurálása, lásd: [egy a helyszíni hálózathoz való csatlakozás HDInsight](./connect-on-premises-network.md).</span><span class="sxs-lookup"><span data-stu-id="689da-501">For an end-to-end example of configuring HDInsight to connect to an on-premises network, see [Connect HDInsight to an on-premises network](./connect-on-premises-network.md).</span></span>

* <span data-ttu-id="689da-502">Azure virtuális hálózataihoz további információkért tekintse meg a [Azure Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="689da-502">For more information on Azure virtual networks, see the [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="689da-503">Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="689da-503">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="689da-504">A felhasználó által definiált útvonalak további információkért lásd: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="689da-504">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>