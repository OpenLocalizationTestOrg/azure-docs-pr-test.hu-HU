---
title: "a virtuális hálózattal - Azure HDInsight aaaExtend |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Virtual Network tooconnect HDInsight tooother felhőalapú erőforrásokat, vagy az adatközpontban lévő erőforrások"
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
ms.openlocfilehash: ba80be4d9f280c6c62fa8acc996ef5f921acdbbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a><span data-ttu-id="b03ac-103">Azure virtuális hálózat használatával Azure HDInsight kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="b03ac-103">Extend Azure HDInsight using an Azure Virtual Network</span></span>

<span data-ttu-id="b03ac-104">Megtudhatja, hogyan toouse HDInsight a egy [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b03ac-104">Learn how toouse HDInsight with an [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="b03ac-105">Azure virtuális hálózat használatával lehetővé teszi, hogy a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-105">Using an Azure Virtual Network enables hello following scenarios:</span></span>

* <span data-ttu-id="b03ac-106">Csatlakozás tooHDInsight közvetlenül a helyi hálózatról.</span><span class="sxs-lookup"><span data-stu-id="b03ac-106">Connecting tooHDInsight directly from an on-premises network.</span></span>

* <span data-ttu-id="b03ac-107">Egy Azure virtuális hálózatra csatlakozó HDInsight toodata tárolja.</span><span class="sxs-lookup"><span data-stu-id="b03ac-107">Connecting HDInsight toodata stores in an Azure Virtual network.</span></span>

* <span data-ttu-id="b03ac-108">Közvetlen elérése során Hadoop-szolgáltatásokat nem érhető el több mint nyilvánosan hello internet.</span><span class="sxs-lookup"><span data-stu-id="b03ac-108">Directly accessing Hadoop services that are not available publicly over hello internet.</span></span> <span data-ttu-id="b03ac-109">Például Kafka API-k vagy hello HBase Java API.</span><span class="sxs-lookup"><span data-stu-id="b03ac-109">For example, Kafka APIs or hello HBase Java API.</span></span>

> [!WARNING]
> <span data-ttu-id="b03ac-110">a dokumentumban szereplő információk hello kell a TCP/IP-hálózat érteni.</span><span class="sxs-lookup"><span data-stu-id="b03ac-110">hello information in this document requires an understanding of TCP/IP networking.</span></span> <span data-ttu-id="b03ac-111">Ha nem ismeri a TCP/IP-hálózat, meg kell felkereshetők a személy, aki módosításainak tooproduction hálózatok elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="b03ac-111">If you are not familiar with TCP/IP networking, you should partner with someone who is before making modifications tooproduction networks.</span></span>

## <a name="planning"></a><span data-ttu-id="b03ac-112">Tervezés</span><span class="sxs-lookup"><span data-stu-id="b03ac-112">Planning</span></span>

<span data-ttu-id="b03ac-113">Az alábbiakban hello válaszolnia kell a virtuális hálózatban HDInsight tooinstall tervezésekor hello kérdésekre kaphat választ:</span><span class="sxs-lookup"><span data-stu-id="b03ac-113">hello following are hello questions that you must answer when planning tooinstall HDInsight in a virtual network:</span></span>

* <span data-ttu-id="b03ac-114">HDInsight tooinstall létrehozni meglévő virtuális hálózatban kell?</span><span class="sxs-lookup"><span data-stu-id="b03ac-114">Do you need tooinstall HDInsight into an existing virtual network?</span></span> <span data-ttu-id="b03ac-115">Új hálózat létrehozása vagy?</span><span class="sxs-lookup"><span data-stu-id="b03ac-115">Or are you creating a new network?</span></span>

    <span data-ttu-id="b03ac-116">Ha egy meglévő virtuális hálózatot használ, szükség lehet toomodify hello hálózati konfiguráció HDInsight telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="b03ac-116">If you are using an existing virtual network, you may need toomodify hello network configuration before you can install HDInsight.</span></span> <span data-ttu-id="b03ac-117">További információkért lásd: hello [HDInsight tooan meglévő virtuális hálózat hozzáadása](#existingvnet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-117">For more information, see hello [add HDInsight tooan existing virtual network](#existingvnet) section.</span></span>

* <span data-ttu-id="b03ac-118">Tooconnect hello virtuális hálózati tartalmazó HDInsight tooanother virtuális hálózat vagy a helyszíni hálózat van szüksége?</span><span class="sxs-lookup"><span data-stu-id="b03ac-118">Do you want tooconnect hello virtual network containing HDInsight tooanother virtual network or your on-premises network?</span></span>

    <span data-ttu-id="b03ac-119">tooeasily munkahelyi erőforrások hálózatok között, előfordulhat, hogy egy egyéni DNS toocreate kell és DNS-továbbító konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b03ac-119">tooeasily work with resources across networks, you may need toocreate a custom DNS and configure DNS forwarding.</span></span> <span data-ttu-id="b03ac-120">További információkért lásd: hello [kapcsolódó több hálózatok](#multinet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-120">For more information, see hello [connecting multiple networks](#multinet) section.</span></span>

* <span data-ttu-id="b03ac-121">Szeretné, hogy toorestrict/átirányítás bejövő vagy kimenő forgalom tooHDInsight?</span><span class="sxs-lookup"><span data-stu-id="b03ac-121">Do you want toorestrict/redirect inbound or outbound traffic tooHDInsight?</span></span>

    <span data-ttu-id="b03ac-122">A HDInsight az IP-címek hello Azure-adatközpontban kommunikálni kell üzemmódban.</span><span class="sxs-lookup"><span data-stu-id="b03ac-122">HDInsight must have unrestricted communication with specific IP addresses in hello Azure data center.</span></span> <span data-ttu-id="b03ac-123">Van még számos portot, az ügyfél-kommunikációhoz tűzfalon keresztül kell engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="b03ac-123">There are also several ports that must be allowed through firewalls for client communication.</span></span> <span data-ttu-id="b03ac-124">További információkért lásd: hello [hálózati forgalom vezérlése](#networktraffic) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-124">For more information, see hello [controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="b03ac-125"><a id="existingvnet"></a>HDInsight tooan meglévő virtuális hálózat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b03ac-125"><a id="existingvnet"></a>Add HDInsight tooan existing virtual network</span></span>

<span data-ttu-id="b03ac-126">Ez a szakasz toodiscover hello lépéseket használata útmutató tooadd egy új HDInsight tooan meglévő Azure-beli virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-126">Use hello steps in this section toodiscover how tooadd a new HDInsight tooan existing Azure Virtual Network.</span></span>

> [!NOTE]
> <span data-ttu-id="b03ac-127">Nem adhat hozzá egy meglévő HDInsight-fürt virtuális hálózatba.</span><span class="sxs-lookup"><span data-stu-id="b03ac-127">You cannot add an existing HDInsight cluster into a virtual network.</span></span>

1. <span data-ttu-id="b03ac-128">Használja a klasszikus és Resource Manager üzembe helyezési modellben hello virtuális hálózat?</span><span class="sxs-lookup"><span data-stu-id="b03ac-128">Are you using a classic or Resource Manager deployment model for hello virtual network?</span></span>

    <span data-ttu-id="b03ac-129">HDInsight 3.4 és nagyobb erőforrás-kezelő virtuális hálózat szükséges.</span><span class="sxs-lookup"><span data-stu-id="b03ac-129">HDInsight 3.4 and greater requires a Resource Manager virtual network.</span></span> <span data-ttu-id="b03ac-130">A HDInsight korábbi verzióiban a klasszikus virtuális hálózatot szükséges.</span><span class="sxs-lookup"><span data-stu-id="b03ac-130">Earlier versions of HDInsight required a classic virtual network.</span></span>

    <span data-ttu-id="b03ac-131">Ha a meglévő hálózat a klasszikus virtuális hálózatot, majd kell létrehozni egy erőforrás-kezelő virtuális hálózat és két hello csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="b03ac-131">If your existing network is a classic virtual network, then you must create a Resource Manager virtual network and then connect hello two.</span></span> <span data-ttu-id="b03ac-132">[Csatlakozás a hagyományos Vneteket toonew Vnetek](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b03ac-132">[Connecting classic VNets toonew VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span></span>

    <span data-ttu-id="b03ac-133">Miután csatlakozott, HDInsight hello erőforrás-kezelő hálózatán telepített kezelheti hello hagyományos hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-133">Once joined, HDInsight installed in hello Resource Manager network can interact with resources in hello classic network.</span></span>

2. <span data-ttu-id="b03ac-134">Használja a kényszerített bújtatás?</span><span class="sxs-lookup"><span data-stu-id="b03ac-134">Do you use forced tunneling?</span></span> <span data-ttu-id="b03ac-135">A kényszerített bújtatás az alhálózat-beállítással, amely kényszeríti a kimenő Internet forgalom tooa eszköz a vizsgálat és naplózás.</span><span class="sxs-lookup"><span data-stu-id="b03ac-135">Forced tunneling is a subnet setting that forces outbound Internet traffic tooa device for inspection and logging.</span></span> <span data-ttu-id="b03ac-136">A HDInsight nem támogatja a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="b03ac-136">HDInsight does not support forced tunneling.</span></span> <span data-ttu-id="b03ac-137">Távolítsa el a kényszerített bújtatás HDInsight egy alhálózatba telepítése előtt, vagy hozzon létre egy új alhálózatot a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b03ac-137">Either remove forced tunneling before installing HDInsight into a subnet, or create a new subnet for HDInsight.</span></span>

3. <span data-ttu-id="b03ac-138">Használhatók a hálózati biztonsági csoportok, felhasználó által definiált útvonalak és virtuális hálózati berendezések toorestrict forgalom virtuális gépbe vagy onnan hello virtuális hálózatot?</span><span class="sxs-lookup"><span data-stu-id="b03ac-138">Do you use network security groups, user-defined routes, or Virtual Network Appliances toorestrict traffic into or out of hello virtual network?</span></span>

    <span data-ttu-id="b03ac-139">Felügyelt szolgáltatásként HDInsight korlátlan hozzáférést tooseveral IP-címek hello Azure-adatközpontban van szükség.</span><span class="sxs-lookup"><span data-stu-id="b03ac-139">As a managed service, HDInsight requires unrestricted access tooseveral IP addresses in hello Azure data center.</span></span> <span data-ttu-id="b03ac-140">tooallow kommunikációt a következő IP-címek, a meglévő hálózati biztonsági csoport vagy felhasználó által definiált útvonalak frissítése.</span><span class="sxs-lookup"><span data-stu-id="b03ac-140">tooallow communication with these IP addresses, update any existing network security groups or user-defined routes.</span></span>

    <span data-ttu-id="b03ac-141">HDInsight tároló több szolgáltatásokat, amelyek különféle portokat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-141">HDInsight  hosts multiple services, which use a variety of ports.</span></span> <span data-ttu-id="b03ac-142">Nem forgalom toothese portok blokkolása.</span><span class="sxs-lookup"><span data-stu-id="b03ac-142">Do not block traffic toothese ports.</span></span> <span data-ttu-id="b03ac-143">Virtuális készülék tűzfalon keresztüli portok tooallow listáját lásd: hello [biztonsági](#security) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-143">For a list of ports tooallow through virtual appliance firewalls, see hello [Security](#security) section.</span></span>

    <span data-ttu-id="b03ac-144">toofind a meglévő biztonsági konfiguráció, a következő Azure PowerShell vagy Azure CLI parancsok használata hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-144">toofind your existing security configuration, use hello following Azure PowerShell or Azure CLI commands:</span></span>

    * <span data-ttu-id="b03ac-145">Network security groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="b03ac-145">Network security groups</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="b03ac-146">További információkért lásd: hello [hibaelhárítása a hálózati biztonsági csoportok](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-146">For more information, see hello [Troubleshoot network security groups](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) document.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="b03ac-147">Hálózati biztonsági csoportszabályok szabály prioritási sorrendben alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="b03ac-147">Network security group rules are applied in order based on rule priority.</span></span> <span data-ttu-id="b03ac-148">hello első szabály hello forgalom bizonyos mintázatnak megfelelő érvényes, és nincs más erre a forgalomra alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="b03ac-148">hello first rule that matches hello traffic pattern is applied, and no others are applied for that traffic.</span></span> <span data-ttu-id="b03ac-149">A leghatékonyabb tooleast megengedő szabályok.</span><span class="sxs-lookup"><span data-stu-id="b03ac-149">Order rules from most permissive tooleast permissive.</span></span> <span data-ttu-id="b03ac-150">További információkért lásd: hello [hálózati forgalmat hálózati biztonsági csoportokkal](../virtual-network/virtual-networks-nsg.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-150">For more information, see hello [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    * <span data-ttu-id="b03ac-151">Felhasználó által megadott útvonalak</span><span class="sxs-lookup"><span data-stu-id="b03ac-151">User-defined routes</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="b03ac-152">További információkért lásd: hello [útvonalak hibaelhárítása](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-152">For more information, see hello [Troubleshoot routes](../virtual-network/virtual-network-routes-troubleshoot-portal.md) document.</span></span>

4. <span data-ttu-id="b03ac-153">HDInsight-fürtök létrehozása és hello Azure virtuális hálózat konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="b03ac-153">Create an HDInsight cluster and select hello Azure Virtual Network during configuration.</span></span> <span data-ttu-id="b03ac-154">Kövesse a következő dokumentumok toounderstand hello fürtlétrehozás hello hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b03ac-154">Use hello steps in hello following documents toounderstand hello cluster creation process:</span></span>

    * [<span data-ttu-id="b03ac-155">Hozzon létre a HDInsight a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b03ac-155">Create HDInsight using hello Azure portal</span></span>](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [<span data-ttu-id="b03ac-156">HDInsight létrehozása az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="b03ac-156">Create HDInsight using Azure PowerShell</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [<span data-ttu-id="b03ac-157">A HDInsight használata az Azure CLI 1.0 létrehozása</span><span class="sxs-lookup"><span data-stu-id="b03ac-157">Create HDInsight using Azure CLI 1.0</span></span>](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [<span data-ttu-id="b03ac-158">HDInsight használata az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="b03ac-158">Create HDInsight using an Azure Resource Manager template</span></span>](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > <span data-ttu-id="b03ac-159">HDInsight hozzáadása tooa virtuális hálózat egy opcionális konfigurációs lépésre.</span><span class="sxs-lookup"><span data-stu-id="b03ac-159">Adding HDInsight tooa virtual network is an optional configuration step.</span></span> <span data-ttu-id="b03ac-160">Lehet, hogy tooselect hello virtuális hálózati hello fürt konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="b03ac-160">Be sure tooselect hello virtual network when configuring hello cluster.</span></span>

## <span data-ttu-id="b03ac-161"><a id="multinet"></a>Több hálózatok csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="b03ac-161"><a id="multinet"></a>Connecting multiple networks</span></span>

<span data-ttu-id="b03ac-162">hello legnagyobb kihívás a több hálózati konfiguráció névfeloldás hello hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="b03ac-162">hello biggest challenge with a multi-network configuration is name resolution between hello networks.</span></span>

<span data-ttu-id="b03ac-163">Azure névfeloldást biztosít az Azure virtuális hálózatban telepített szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b03ac-163">Azure provides name resolution for Azure services that are installed in a virtual network.</span></span> <span data-ttu-id="b03ac-164">A beépített névfeloldás lehetővé teszi, hogy a HDInsight tooconnect toohello erőforrások egy teljesen minősített tartománynevét (FQDN) használatával a következő:</span><span class="sxs-lookup"><span data-stu-id="b03ac-164">This built-in name resolution allows HDInsight tooconnect toohello following resources by using a fully qualified domain name (FQDN):</span></span>

* <span data-ttu-id="b03ac-165">Minden erőforrás elérhető internet hello.</span><span class="sxs-lookup"><span data-stu-id="b03ac-165">Any resource that is available on hello internet.</span></span> <span data-ttu-id="b03ac-166">Ha például a Microsoft.com webhelyre mutat, google.com.</span><span class="sxs-lookup"><span data-stu-id="b03ac-166">For example, microsoft.com, google.com.</span></span>

* <span data-ttu-id="b03ac-167">Bármely erőforrása, amely a rendszer a hello azonos Azure virtuális hálózatban, hello segítségével __belső DNS-név__ hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b03ac-167">Any resource that is in hello same Azure Virtual Network, by using hello __internal DNS name__ of hello resource.</span></span> <span data-ttu-id="b03ac-168">Például hello alapértelmezett névhozzárendelés használatakor hello a következők példa belső DNS nevek hozzárendelt tooHDInsight feldolgozó csomópontok:</span><span class="sxs-lookup"><span data-stu-id="b03ac-168">For example, when using hello default name resolution, hello following are example internal DNS names assigned tooHDInsight worker nodes:</span></span>

    * <span data-ttu-id="b03ac-169">wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="b03ac-169">wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>
    * <span data-ttu-id="b03ac-170">wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="b03ac-170">wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>

    <span data-ttu-id="b03ac-171">Mindkét ezeket a csomópontokat közvetlenül kommunikálhatnak egymással, és a Hdinsightban, más csomópontok belső DNS-nevek használatával.</span><span class="sxs-lookup"><span data-stu-id="b03ac-171">Both these nodes can communicate directly with each other, and other nodes in HDInsight, by using internal DNS names.</span></span>

<span data-ttu-id="b03ac-172">hello alapértelmezett névhozzárendelés does __nem__ illesztett toohello virtuális hálózatokon lévő erőforrások hello nevének HDInsight tooresolve engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b03ac-172">hello default name resolution does __not__ allow HDInsight tooresolve hello names of resources in networks that are joined toohello virtual network.</span></span> <span data-ttu-id="b03ac-173">Például közös toojoin a helyszíni hálózati toohello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-173">For example, it is common toojoin your on-premises network toohello virtual network.</span></span> <span data-ttu-id="b03ac-174">Csak hello alapértelmezett a névfeloldás HDInsight nem tud hozzáférni a hello a helyszíni hálózati erőforrások neve.</span><span class="sxs-lookup"><span data-stu-id="b03ac-174">With only hello default name resolution, HDInsight cannot access resources in hello on-premises network by name.</span></span> <span data-ttu-id="b03ac-175">Ellenkező hello igaz is, a helyszíni hálózati erőforrások neve nem tud hozzáférni hello virtuális hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-175">hello opposite is also true, resources in your on-premises network cannot access resources in hello virtual network by name.</span></span>

> [!WARNING]
> <span data-ttu-id="b03ac-176">Hello egyéni DNS-kiszolgáló létrehozása és hello virtuális hálózati toouse konfigurálnia kell, mielőtt létrehozná hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-176">You must create hello custom DNS server and configure hello virtual network toouse it before creating hello HDInsight cluster.</span></span>

<span data-ttu-id="b03ac-177">a névfeloldás tooenable hello virtuális hálózat és a csatlakoztatott hálózatokon lévő erőforrások között, végezze el a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-177">tooenable name resolution between hello virtual network and resources in joined networks, you must perform hello following actions:</span></span>

1. <span data-ttu-id="b03ac-178">Hozzon létre egy egyéni DNS-kiszolgáló hello Azure Virtual Network tooinstall HDInsight tervezett.</span><span class="sxs-lookup"><span data-stu-id="b03ac-178">Create a custom DNS server in hello Azure Virtual Network where you plan tooinstall HDInsight.</span></span>

2. <span data-ttu-id="b03ac-179">Hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b03ac-179">Configure hello virtual network toouse hello custom DNS server.</span></span>

3. <span data-ttu-id="b03ac-180">Megkeresi a hello Azure hozzárendelése a virtuális hálózat DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="b03ac-180">Find hello Azure assigned DNS suffix for your virtual network.</span></span> <span data-ttu-id="b03ac-181">Ez az érték túl hasonló`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="b03ac-181">This value is similar too`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span></span> <span data-ttu-id="b03ac-182">Az hello DNS-utótag megkereséséről további információkért lásd: hello [példa: egyéni DNS](#example-dns) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-182">For information on finding hello DNS suffix, see hello [Example: Custom DNS](#example-dns) section.</span></span>

4. <span data-ttu-id="b03ac-183">Konfigurálja a továbbítási hello DNS-kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="b03ac-183">Configure forwarding between hello DNS servers.</span></span> <span data-ttu-id="b03ac-184">hello konfigurációs távoli hálózati hello típusától függ.</span><span class="sxs-lookup"><span data-stu-id="b03ac-184">hello configuration depends on hello type of remote network.</span></span>

    * <span data-ttu-id="b03ac-185">Ha hello távoli hálózati egy a helyszíni hálózat, konfigurálja a DNS az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b03ac-185">If hello remote network is an on-premises network, configure DNS as follows:</span></span>
        
        * <span data-ttu-id="b03ac-186">__Egyéni DNS__ (a virtuális hálózati hello):</span><span class="sxs-lookup"><span data-stu-id="b03ac-186">__Custom DNS__ (in hello virtual network):</span></span>

            * <span data-ttu-id="b03ac-187">Továbbítási kérelmek hello DNS-utótag az hello virtuális hálózati toohello Azure rekurzív feloldó (168.63.129.16).</span><span class="sxs-lookup"><span data-stu-id="b03ac-187">Forward requests for hello DNS suffix of hello virtual network toohello Azure recursive resolver (168.63.129.16).</span></span> <span data-ttu-id="b03ac-188">Azure virtuális hálózat hello erőforrásokra vonatkozó kéréseket kezeli</span><span class="sxs-lookup"><span data-stu-id="b03ac-188">Azure handles requests for resources in hello virtual network</span></span>

            * <span data-ttu-id="b03ac-189">Továbbítsa az összes többi kérelmek toohello a helyi DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b03ac-189">Forward all other requests toohello on-premises DNS server.</span></span> <span data-ttu-id="b03ac-190">hello helyszíni DNS kezeli az összes többi feloldási kérések, még akkor is, kérelmek az internetes erőforrásokhoz, például a Microsoft.com webhelyre mutat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-190">hello on-premises DNS handles all other name resolution requests, even requests for internet resources such as Microsoft.com.</span></span>

        * <span data-ttu-id="b03ac-191">__A helyi DNS__: hello virtuális hálózat DNS-utótag toohello egyéni DNS kiszolgáló kérelmeket továbbítja.</span><span class="sxs-lookup"><span data-stu-id="b03ac-191">__On-premises DNS__: Forward requests for hello virtual network DNS suffix toohello custom DNS server.</span></span> <span data-ttu-id="b03ac-192">hello egyéni DNS-kiszolgáló majd toohello Azure rekurzív feloldó továbbítja.</span><span class="sxs-lookup"><span data-stu-id="b03ac-192">hello custom DNS server then forwards toohello Azure recursive resolver.</span></span>

        <span data-ttu-id="b03ac-193">A konfigurációs útvonalak kérelmek teljes tartománynevek DNS-utótagja hello hello virtuális hálózati toohello egyéni DNS-kiszolgáló tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="b03ac-193">This configuration routes requests for fully qualified domain names that contain hello DNS suffix of hello virtual network toohello custom DNS server.</span></span> <span data-ttu-id="b03ac-194">(Még a nyilvános internet címek) minden más kérelemhez hello a helyi DNS-kiszolgáló kezeli.</span><span class="sxs-lookup"><span data-stu-id="b03ac-194">All other requests (even for public internet addresses) are handled by hello on-premises DNS server.</span></span>

    * <span data-ttu-id="b03ac-195">Ha hello távoli hálózat egy másik Azure virtuális hálózatnak, konfigurálja a DNS az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b03ac-195">If hello remote network is another Azure Virtual Network, configure DNS as follows:</span></span>

        * <span data-ttu-id="b03ac-196">__Egyéni DNS__ (az egyes virtuális hálózati):</span><span class="sxs-lookup"><span data-stu-id="b03ac-196">__Custom DNS__ (in each virtual network):</span></span>

            * <span data-ttu-id="b03ac-197">Hello DNS-utótag hello virtuális hálózatok kérelmeket a rendszer továbbítja toohello egyéni DNS-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="b03ac-197">Requests for hello DNS suffix of hello virtual networks are forwarded toohello custom DNS servers.</span></span> <span data-ttu-id="b03ac-198">az egyes virtuális hálózati DNS hello felelős megoldása érdekében a hálózati erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-198">hello DNS in each virtual network is responsible for resolving resources within its network.</span></span>

            * <span data-ttu-id="b03ac-199">Minden más kérelmek toohello Azure rekurzív feloldó továbbítja.</span><span class="sxs-lookup"><span data-stu-id="b03ac-199">Forward all other requests toohello Azure recursive resolver.</span></span> <span data-ttu-id="b03ac-200">hello rekurzív feloldó felelős helyi feloldására és az internetes erőforrásokban.</span><span class="sxs-lookup"><span data-stu-id="b03ac-200">hello recursive resolver is responsible for resolving local and internet resources.</span></span>

        <span data-ttu-id="b03ac-201">hello DNS-kiszolgáló minden hálózat továbbítja a kérelmek toohello más, a DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="b03ac-201">hello DNS server for each network forwards requests toohello other, based on DNS suffix.</span></span> <span data-ttu-id="b03ac-202">Küldött egyéb kérések hello Azure rekurzív feloldó használatával oldja fel.</span><span class="sxs-lookup"><span data-stu-id="b03ac-202">Other requests are resolved using hello Azure recursive resolver.</span></span>

    <span data-ttu-id="b03ac-203">Minden egyes konfigurációs példát lásd: hello [példa: egyéni DNS](#example-dns) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-203">For an example of each configuration, see hello [Example: Custom DNS](#example-dns) section.</span></span>

<span data-ttu-id="b03ac-204">További információkért lásd: hello [névfeloldás virtuális gépek és a Szerepkörpéldányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-204">For more information, see hello [Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) document.</span></span>

## <a name="directly-connect-toohadoop-services"></a><span data-ttu-id="b03ac-205">Közvetlen csatlakozás tooHadoop szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b03ac-205">Directly connect tooHadoop services</span></span>

<span data-ttu-id="b03ac-206">A legtöbb dokumentáció a HDInsight feltételezi, hogy rendelkezik-e hozzáférési toohello fürt keresztül hello internet.</span><span class="sxs-lookup"><span data-stu-id="b03ac-206">Most documentation on HDInsight assumes that you have access toohello cluster over hello internet.</span></span> <span data-ttu-id="b03ac-207">Például, hogy csatlakozhasson https://CLUSTERNAME.azurehdinsight.net toohello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-207">For example, that you can connect toohello cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="b03ac-208">A cím hello nyilvános átjárón, amely esetén nem érhető el, használja az NSG-k vagy udr-EK toorestrict a hozzáférést a hello internet használja.</span><span class="sxs-lookup"><span data-stu-id="b03ac-208">This address uses hello public gateway, which is not available if you have used NSGs or UDRs toorestrict access from hello internet.</span></span>

<span data-ttu-id="b03ac-209">tooconnect tooAmbari és más weblapok hello virtuális hálózaton keresztül, használja a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-209">tooconnect tooAmbari and other web pages through hello virtual network, use hello following steps:</span></span>

1. <span data-ttu-id="b03ac-210">toodiscover hello belső teljes tartománynév (FQDN) hello HDInsight fürtcsomópont nevét, a hello a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="b03ac-210">toodiscover hello internal fully qualified domain names (FQDN) of hello HDInsight cluster nodes, use one of hello following methods:</span></span>

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

    <span data-ttu-id="b03ac-211">Hello csomópontlista visszaadott hello FQDN található hello átjárócsomópontokkal és használja a hello teljes tartománynevek tooconnect tooAmbari és egyéb webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b03ac-211">In hello list of nodes returned, find hello FQDN for hello head nodes and use hello FQDNs tooconnect tooAmbari and other web services.</span></span> <span data-ttu-id="b03ac-212">Tegyük fel például, `http://<headnode-fqdn>:8080` tooaccess Ambari.</span><span class="sxs-lookup"><span data-stu-id="b03ac-212">For example, use `http://<headnode-fqdn>:8080` tooaccess Ambari.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b03ac-213">Egyes hello átjárócsomópontokkal üzemeltetett szolgáltatások aktívak csak az egyik csomópont egyszerre.</span><span class="sxs-lookup"><span data-stu-id="b03ac-213">Some services hosted on hello head nodes are only active on one node at a time.</span></span> <span data-ttu-id="b03ac-214">Próbálja meg egy központi csomóponton szolgáltatások elérésére és a 404-es hibaüzenetet ad vissza, ha Váltás más átjárócsomópont toohello.</span><span class="sxs-lookup"><span data-stu-id="b03ac-214">If you try accessing a service on one head node and it returns a 404 error, switch toohello other head node.</span></span>

2. <span data-ttu-id="b03ac-215">toodetermine hello csomópont és a portot, amelyet a szolgáltatás érhető el, lásd: hello [a HDInsight Hadoop-szolgáltatás által használt portok](./hdinsight-hadoop-port-settings-for-services.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-215">toodetermine hello node and port that a service is available on, see hello [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

## <span data-ttu-id="b03ac-216"><a id="networktraffic"></a>Hálózati forgalom vezérlése</span><span class="sxs-lookup"><span data-stu-id="b03ac-216"><a id="networktraffic"></a> Controlling network traffic</span></span>

<span data-ttu-id="b03ac-217">Egy Azure virtuális hálózatot a hálózati forgalmat a következő módszerek hello vezérelhető:</span><span class="sxs-lookup"><span data-stu-id="b03ac-217">Network traffic in an Azure Virtual Networks can be controlled using hello following methods:</span></span>

* <span data-ttu-id="b03ac-218">**Hálózati biztonsági csoportok** (NSG) lehetővé teszik toofilter bejövő és kimenő forgalom toohello hálózati.</span><span class="sxs-lookup"><span data-stu-id="b03ac-218">**Network security groups** (NSG) allow you toofilter inbound and outbound traffic toohello network.</span></span> <span data-ttu-id="b03ac-219">További információkért lásd: hello [hálózati forgalmat hálózati biztonsági csoportokkal](../virtual-network/virtual-networks-nsg.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-219">For more information, see hello [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    > [!WARNING]
    > <span data-ttu-id="b03ac-220">A HDInsight nem támogatja a kimenő forgalom korlátozása.</span><span class="sxs-lookup"><span data-stu-id="b03ac-220">HDInsight does not support restricting outbound traffic.</span></span>

* <span data-ttu-id="b03ac-221">**Felhasználó által definiált útvonalak** (UDR) határozza meg, hogyan a forgalom között hello hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-221">**User-defined routes** (UDR) define how traffic flows between resources in hello network.</span></span> <span data-ttu-id="b03ac-222">További információkért lásd: hello [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-222">For more information, see hello [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md) document.</span></span>

* <span data-ttu-id="b03ac-223">**Virtuális készülékekre** replikálja az eszközök, például a tűzfalak és az útválasztók hello funkcióit.</span><span class="sxs-lookup"><span data-stu-id="b03ac-223">**Network virtual appliances** replicate hello functionality of devices such as firewalls and routers.</span></span> <span data-ttu-id="b03ac-224">További információkért lásd: hello [hálózati berendezések](https://azure.microsoft.com/solutions/network-appliances) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-224">For more information, see hello [Network Appliances](https://azure.microsoft.com/solutions/network-appliances) document.</span></span>

<span data-ttu-id="b03ac-225">Felügyelt szolgáltatásként HDInsight korlátlan hozzáférést tooAzure állapotát és a felügyeleti szolgáltatások hello Azure felhőben igényel.</span><span class="sxs-lookup"><span data-stu-id="b03ac-225">As a managed service, HDInsight requires unrestricted access tooAzure health and management services in hello Azure cloud.</span></span> <span data-ttu-id="b03ac-226">Az NSG-k és udr-EK használata esetén győződjön meg róla, hogy a HDInsight ezen szolgáltatások továbbra is kommunikál a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b03ac-226">When using NSGs and UDRs, you must ensure that HDInsight these services can still communicate with HDInsight.</span></span>

<span data-ttu-id="b03ac-227">HDInsight szolgáltatások számos portot teszi elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="b03ac-227">HDInsight exposes services on several ports.</span></span> <span data-ttu-id="b03ac-228">Ha egy virtuális készülékre tűzfalat használ, engedélyeznie kell a hello forgalmat mely portokon folyik a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b03ac-228">When using a virtual appliance firewall, you must allow traffic on hello ports used for these services.</span></span> <span data-ttu-id="b03ac-229">További információkért lásd: hello [szükséges portok] szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-229">For more information, see hello [Required ports] section.</span></span>

### <span data-ttu-id="b03ac-230"><a id="hdinsight-ip"></a>A hálózati biztonsági csoportok és a felhasználó által definiált útvonalak HDInsight</span><span class="sxs-lookup"><span data-stu-id="b03ac-230"><a id="hdinsight-ip"></a> HDInsight with network security groups and user-defined routes</span></span>

<span data-ttu-id="b03ac-231">Ha a kíván használni **hálózati biztonsági csoportok** vagy **felhasználó által definiált útvonalak** toocontrol hálózati forgalom, hajtsa végre a hello műveletek HDInsight telepítése előtt a következő:</span><span class="sxs-lookup"><span data-stu-id="b03ac-231">If you plan on using **network security groups** or **user-defined routes** toocontrol network traffic, perform hello following actions before installing HDInsight:</span></span>

1. <span data-ttu-id="b03ac-232">Azonosítsa a hello toouse tervezi a HDInsight az Azure-régió.</span><span class="sxs-lookup"><span data-stu-id="b03ac-232">Identify hello Azure region that you plan toouse for HDInsight.</span></span>

2. <span data-ttu-id="b03ac-233">Azonosítsa a HDInsight által igényelt hello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="b03ac-233">Identify hello IP addresses required by HDInsight.</span></span> <span data-ttu-id="b03ac-234">További információkért lásd: hello [HDInsight által megkövetelt IP-címek](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-234">For more information, see hello [IP Addresses required by HDInsight](#hdinsight-ip) section.</span></span>

3. <span data-ttu-id="b03ac-235">Létrehozhat vagy módosíthat hello hálózati biztonsági csoport vagy felhasználó által definiált útvonalak tooinstall HDInsight megtervezni hello alhálózat be.</span><span class="sxs-lookup"><span data-stu-id="b03ac-235">Create or modify hello network security groups or user-defined routes for hello subnet that you plan tooinstall HDInsight into.</span></span>

    * <span data-ttu-id="b03ac-236">__Hálózati biztonsági csoportok__: engedélyezése __bejövő__ porton forgalom __443-as__ hello IP-címről.</span><span class="sxs-lookup"><span data-stu-id="b03ac-236">__Network security groups__: allow __inbound__ traffic on port __443__ from hello IP addresses.</span></span>
    * <span data-ttu-id="b03ac-237">__Felhasználó által definiált útvonalak__: hozzon létre egy útvonal tooeach IP-címet, és állítsa be a hello __a következő ugrás típusa__ too__Internet__.</span><span class="sxs-lookup"><span data-stu-id="b03ac-237">__User-defined routes__: create a route tooeach IP address and set hello __Next hop type__ too__Internet__.</span></span>

<span data-ttu-id="b03ac-238">A hálózati biztonsági csoport vagy felhasználó által definiált útvonalak további információkért lásd: a következő dokumentáció hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-238">For more information on network security groups or user-defined routes, see hello following documentation:</span></span>

* [<span data-ttu-id="b03ac-239">Hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="b03ac-239">Network security group</span></span>](../virtual-network/virtual-networks-nsg.md)

* [<span data-ttu-id="b03ac-240">Felhasználó által definiált útvonalak</span><span class="sxs-lookup"><span data-stu-id="b03ac-240">User-defined routes</span></span>](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a><span data-ttu-id="b03ac-241">Alagúthasználat kényszerítése</span><span class="sxs-lookup"><span data-stu-id="b03ac-241">Forced tunneling</span></span>

<span data-ttu-id="b03ac-242">Kényszerített bújtatás a beállítás a felhasználói útválasztási ahol alhálózatból származó összes forgalmat, de kényszerített tooa adott hálózati helyre, például a helyszíni hálózat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-242">Forced tunneling is a user-defined routing configuration where all traffic from a subnet is forced tooa specific network or location, such as your on-premises network.</span></span> <span data-ttu-id="b03ac-243">HDInsight does __nem__ támogatási kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="b03ac-243">HDInsight does __not__ support forced tunneling.</span></span>

## <span data-ttu-id="b03ac-244"><a id="hdinsight-ip"></a>Szükséges IP-címek</span><span class="sxs-lookup"><span data-stu-id="b03ac-244"><a id="hdinsight-ip"></a> Required IP addresses</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b03ac-245">hello Azure állapotát, és szolgáltatások a hdinsight eszközzel képes toocommunicate kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b03ac-245">hello Azure health and management services must be able toocommunicate with HDInsight.</span></span> <span data-ttu-id="b03ac-246">Ha hálózati biztonsági csoport vagy felhasználó által definiált útvonalak, hello érkező forgalom engedélyezése ezen szolgáltatások tooreach HDInsight IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b03ac-246">If you use network security groups or user-defined routes, allow traffic from hello IP addresses for these services tooreach HDInsight.</span></span>
>
> <span data-ttu-id="b03ac-247">Ha hálózati biztonsági csoport vagy felhasználó által definiált útvonalak toocontrol forgalom nem használ, figyelmen kívül hagyhatja ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b03ac-247">If you do not use network security groups or user-defined routes toocontrol traffic, you can ignore this section.</span></span>

<span data-ttu-id="b03ac-248">Ha a hálózati biztonsági csoport vagy felhasználó által definiált útvonalak, engedélyeznie kell a hello Azure állapotát és a felügyeleti szolgáltatások tooreach HDInsight-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-248">If you use network security groups or user-defined routes, you must allow traffic from hello Azure health and management services tooreach HDInsight.</span></span> <span data-ttu-id="b03ac-249">Használja a következő lépéseket toofind hello IP-címek, engedélyezni kell az hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-249">Use hello following steps toofind hello IP addresses that must be allowed:</span></span>

1. <span data-ttu-id="b03ac-250">Mindig engedélyeznie kell a következő IP-címek hello érkező forgalmat:</span><span class="sxs-lookup"><span data-stu-id="b03ac-250">You must always allow traffic from hello following IP addresses:</span></span>

    | <span data-ttu-id="b03ac-251">IP-cím</span><span class="sxs-lookup"><span data-stu-id="b03ac-251">IP address</span></span> | <span data-ttu-id="b03ac-252">Engedélyezett port</span><span class="sxs-lookup"><span data-stu-id="b03ac-252">Allowed port</span></span> | <span data-ttu-id="b03ac-253">Irány</span><span class="sxs-lookup"><span data-stu-id="b03ac-253">Direction</span></span> |
    | ---- | ----- | ----- |
    | <span data-ttu-id="b03ac-254">168.61.49.99</span><span class="sxs-lookup"><span data-stu-id="b03ac-254">168.61.49.99</span></span> | <span data-ttu-id="b03ac-255">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-255">443</span></span> | <span data-ttu-id="b03ac-256">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-256">Inbound</span></span> |
    | <span data-ttu-id="b03ac-257">23.99.5.239</span><span class="sxs-lookup"><span data-stu-id="b03ac-257">23.99.5.239</span></span> | <span data-ttu-id="b03ac-258">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-258">443</span></span> | <span data-ttu-id="b03ac-259">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-259">Inbound</span></span> |
    | <span data-ttu-id="b03ac-260">168.61.48.131</span><span class="sxs-lookup"><span data-stu-id="b03ac-260">168.61.48.131</span></span> | <span data-ttu-id="b03ac-261">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-261">443</span></span> | <span data-ttu-id="b03ac-262">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-262">Inbound</span></span> |
    | <span data-ttu-id="b03ac-263">138.91.141.162</span><span class="sxs-lookup"><span data-stu-id="b03ac-263">138.91.141.162</span></span> | <span data-ttu-id="b03ac-264">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-264">443</span></span> | <span data-ttu-id="b03ac-265">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-265">Inbound</span></span> |

2. <span data-ttu-id="b03ac-266">Ha a HDInsight-fürt hello a következő területek közül, majd engedélyeznie kell a felsorolt hello régió hello IP-címekről érkező forgalmat:</span><span class="sxs-lookup"><span data-stu-id="b03ac-266">If your HDInsight cluster is in one of hello following regions, then you must allow traffic from hello IP addresses listed for hello region:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b03ac-267">Ha hello használata az Azure-régió nem szerepel a listában, majd csak hello négy IP-címek használatára 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="b03ac-267">If hello Azure region you are using is not listed, then only use hello four IP addresses from step 1.</span></span>

    | <span data-ttu-id="b03ac-268">Ország</span><span class="sxs-lookup"><span data-stu-id="b03ac-268">Country</span></span> | <span data-ttu-id="b03ac-269">Régió</span><span class="sxs-lookup"><span data-stu-id="b03ac-269">Region</span></span> | <span data-ttu-id="b03ac-270">Engedélyezett IP-címek</span><span class="sxs-lookup"><span data-stu-id="b03ac-270">Allowed IP addresses</span></span> | <span data-ttu-id="b03ac-271">Engedélyezett port</span><span class="sxs-lookup"><span data-stu-id="b03ac-271">Allowed port</span></span> | <span data-ttu-id="b03ac-272">Irány</span><span class="sxs-lookup"><span data-stu-id="b03ac-272">Direction</span></span> |
    | ---- | ---- | ---- | ---- | ----- |
    | <span data-ttu-id="b03ac-273">Ázsia</span><span class="sxs-lookup"><span data-stu-id="b03ac-273">Asia</span></span> | <span data-ttu-id="b03ac-274">Kelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="b03ac-274">East Asia</span></span> | <span data-ttu-id="b03ac-275">23.102.235.122</span><span class="sxs-lookup"><span data-stu-id="b03ac-275">23.102.235.122</span></span></br><span data-ttu-id="b03ac-276">52.175.38.134</span><span class="sxs-lookup"><span data-stu-id="b03ac-276">52.175.38.134</span></span> | <span data-ttu-id="b03ac-277">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-277">443</span></span> | <span data-ttu-id="b03ac-278">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-278">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-279">Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="b03ac-279">Southeast Asia</span></span> | <span data-ttu-id="b03ac-280">13.76.245.160</span><span class="sxs-lookup"><span data-stu-id="b03ac-280">13.76.245.160</span></span></br><span data-ttu-id="b03ac-281">13.76.136.249</span><span class="sxs-lookup"><span data-stu-id="b03ac-281">13.76.136.249</span></span> | <span data-ttu-id="b03ac-282">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-282">443</span></span> | <span data-ttu-id="b03ac-283">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-283">Inbound</span></span> |
    | <span data-ttu-id="b03ac-284">Ausztrália</span><span class="sxs-lookup"><span data-stu-id="b03ac-284">Australia</span></span> | <span data-ttu-id="b03ac-285">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="b03ac-285">Australia East</span></span> | <span data-ttu-id="b03ac-286">104.210.84.115</span><span class="sxs-lookup"><span data-stu-id="b03ac-286">104.210.84.115</span></span></br><span data-ttu-id="b03ac-287">13.75.152.195</span><span class="sxs-lookup"><span data-stu-id="b03ac-287">13.75.152.195</span></span> | <span data-ttu-id="b03ac-288">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-288">443</span></span> | <span data-ttu-id="b03ac-289">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-289">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-290">Délkelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="b03ac-290">Australia Southeast</span></span> | <span data-ttu-id="b03ac-291">13.77.2.56</span><span class="sxs-lookup"><span data-stu-id="b03ac-291">13.77.2.56</span></span></br><span data-ttu-id="b03ac-292">13.77.2.94</span><span class="sxs-lookup"><span data-stu-id="b03ac-292">13.77.2.94</span></span> | <span data-ttu-id="b03ac-293">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-293">443</span></span> | <span data-ttu-id="b03ac-294">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-294">Inbound</span></span> |
    | <span data-ttu-id="b03ac-295">Brazília</span><span class="sxs-lookup"><span data-stu-id="b03ac-295">Brazil</span></span> | <span data-ttu-id="b03ac-296">Dél-Brazília</span><span class="sxs-lookup"><span data-stu-id="b03ac-296">Brazil South</span></span> | <span data-ttu-id="b03ac-297">191.235.84.104</span><span class="sxs-lookup"><span data-stu-id="b03ac-297">191.235.84.104</span></span></br><span data-ttu-id="b03ac-298">191.235.87.113</span><span class="sxs-lookup"><span data-stu-id="b03ac-298">191.235.87.113</span></span> | <span data-ttu-id="b03ac-299">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-299">443</span></span> | <span data-ttu-id="b03ac-300">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-300">Inbound</span></span> |
    | <span data-ttu-id="b03ac-301">Kanada</span><span class="sxs-lookup"><span data-stu-id="b03ac-301">Canada</span></span> | <span data-ttu-id="b03ac-302">Kelet-Kanada</span><span class="sxs-lookup"><span data-stu-id="b03ac-302">Canada East</span></span> | <span data-ttu-id="b03ac-303">52.229.127.96</span><span class="sxs-lookup"><span data-stu-id="b03ac-303">52.229.127.96</span></span></br><span data-ttu-id="b03ac-304">52.229.123.172</span><span class="sxs-lookup"><span data-stu-id="b03ac-304">52.229.123.172</span></span> | <span data-ttu-id="b03ac-305">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-305">443</span></span> | <span data-ttu-id="b03ac-306">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-306">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-307">Közép-Kanada</span><span class="sxs-lookup"><span data-stu-id="b03ac-307">Canada Central</span></span> | <span data-ttu-id="b03ac-308">52.228.37.66</span><span class="sxs-lookup"><span data-stu-id="b03ac-308">52.228.37.66</span></span></br><span data-ttu-id="b03ac-309">52.228.45.222</span><span class="sxs-lookup"><span data-stu-id="b03ac-309">52.228.45.222</span></span> | <span data-ttu-id="b03ac-310">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-310">443</span></span> | <span data-ttu-id="b03ac-311">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-311">Inbound</span></span> |
    | <span data-ttu-id="b03ac-312">Kína</span><span class="sxs-lookup"><span data-stu-id="b03ac-312">China</span></span> | <span data-ttu-id="b03ac-313">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="b03ac-313">China North</span></span> | <span data-ttu-id="b03ac-314">42.159.96.170</span><span class="sxs-lookup"><span data-stu-id="b03ac-314">42.159.96.170</span></span></br><span data-ttu-id="b03ac-315">139.217.2.219</span><span class="sxs-lookup"><span data-stu-id="b03ac-315">139.217.2.219</span></span> | <span data-ttu-id="b03ac-316">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-316">443</span></span> | <span data-ttu-id="b03ac-317">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-317">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-318">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="b03ac-318">China East</span></span> | <span data-ttu-id="b03ac-319">42.159.198.178</span><span class="sxs-lookup"><span data-stu-id="b03ac-319">42.159.198.178</span></span></br><span data-ttu-id="b03ac-320">42.159.234.157</span><span class="sxs-lookup"><span data-stu-id="b03ac-320">42.159.234.157</span></span> | <span data-ttu-id="b03ac-321">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-321">443</span></span> | <span data-ttu-id="b03ac-322">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-322">Inbound</span></span> |
    | <span data-ttu-id="b03ac-323">Európa</span><span class="sxs-lookup"><span data-stu-id="b03ac-323">Europe</span></span> | <span data-ttu-id="b03ac-324">Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="b03ac-324">North Europe</span></span> | <span data-ttu-id="b03ac-325">52.164.210.96</span><span class="sxs-lookup"><span data-stu-id="b03ac-325">52.164.210.96</span></span></br><span data-ttu-id="b03ac-326">13.74.153.132</span><span class="sxs-lookup"><span data-stu-id="b03ac-326">13.74.153.132</span></span> | <span data-ttu-id="b03ac-327">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-327">443</span></span> | <span data-ttu-id="b03ac-328">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-328">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-329">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="b03ac-329">West Europe</span></span>| <span data-ttu-id="b03ac-330">52.166.243.90</span><span class="sxs-lookup"><span data-stu-id="b03ac-330">52.166.243.90</span></span></br><span data-ttu-id="b03ac-331">52.174.36.244</span><span class="sxs-lookup"><span data-stu-id="b03ac-331">52.174.36.244</span></span> | <span data-ttu-id="b03ac-332">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-332">443</span></span> | <span data-ttu-id="b03ac-333">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-333">Inbound</span></span> |
    | <span data-ttu-id="b03ac-334">Németország</span><span class="sxs-lookup"><span data-stu-id="b03ac-334">Germany</span></span> | <span data-ttu-id="b03ac-335">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="b03ac-335">Germany Central</span></span> | <span data-ttu-id="b03ac-336">51.4.146.68</span><span class="sxs-lookup"><span data-stu-id="b03ac-336">51.4.146.68</span></span></br><span data-ttu-id="b03ac-337">51.4.146.80</span><span class="sxs-lookup"><span data-stu-id="b03ac-337">51.4.146.80</span></span> | <span data-ttu-id="b03ac-338">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-338">443</span></span> | <span data-ttu-id="b03ac-339">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-339">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-340">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="b03ac-340">Germany Northeast</span></span> | <span data-ttu-id="b03ac-341">51.5.150.132</span><span class="sxs-lookup"><span data-stu-id="b03ac-341">51.5.150.132</span></span></br><span data-ttu-id="b03ac-342">51.5.144.101</span><span class="sxs-lookup"><span data-stu-id="b03ac-342">51.5.144.101</span></span> | <span data-ttu-id="b03ac-343">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-343">443</span></span> | <span data-ttu-id="b03ac-344">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-344">Inbound</span></span> |
    | <span data-ttu-id="b03ac-345">India</span><span class="sxs-lookup"><span data-stu-id="b03ac-345">India</span></span> | <span data-ttu-id="b03ac-346">Közép-India</span><span class="sxs-lookup"><span data-stu-id="b03ac-346">Central India</span></span> | <span data-ttu-id="b03ac-347">52.172.153.209</span><span class="sxs-lookup"><span data-stu-id="b03ac-347">52.172.153.209</span></span></br><span data-ttu-id="b03ac-348">52.172.152.49</span><span class="sxs-lookup"><span data-stu-id="b03ac-348">52.172.152.49</span></span> | <span data-ttu-id="b03ac-349">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-349">443</span></span> | <span data-ttu-id="b03ac-350">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-350">Inbound</span></span> |
    | <span data-ttu-id="b03ac-351">Japán</span><span class="sxs-lookup"><span data-stu-id="b03ac-351">Japan</span></span> | <span data-ttu-id="b03ac-352">Kelet-Japán</span><span class="sxs-lookup"><span data-stu-id="b03ac-352">Japan East</span></span> | <span data-ttu-id="b03ac-353">13.78.125.90</span><span class="sxs-lookup"><span data-stu-id="b03ac-353">13.78.125.90</span></span></br><span data-ttu-id="b03ac-354">13.78.89.60</span><span class="sxs-lookup"><span data-stu-id="b03ac-354">13.78.89.60</span></span> | <span data-ttu-id="b03ac-355">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-355">443</span></span> | <span data-ttu-id="b03ac-356">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-356">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-357">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="b03ac-357">Japan West</span></span> | <span data-ttu-id="b03ac-358">40.74.125.69</span><span class="sxs-lookup"><span data-stu-id="b03ac-358">40.74.125.69</span></span></br><span data-ttu-id="b03ac-359">138.91.29.150</span><span class="sxs-lookup"><span data-stu-id="b03ac-359">138.91.29.150</span></span> | <span data-ttu-id="b03ac-360">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-360">443</span></span> | <span data-ttu-id="b03ac-361">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-361">Inbound</span></span> |
    | <span data-ttu-id="b03ac-362">Korea</span><span class="sxs-lookup"><span data-stu-id="b03ac-362">Korea</span></span> | <span data-ttu-id="b03ac-363">Korea középső régiója</span><span class="sxs-lookup"><span data-stu-id="b03ac-363">Korea Central</span></span> | <span data-ttu-id="b03ac-364">52.231.39.142</span><span class="sxs-lookup"><span data-stu-id="b03ac-364">52.231.39.142</span></span></br><span data-ttu-id="b03ac-365">52.231.36.209</span><span class="sxs-lookup"><span data-stu-id="b03ac-365">52.231.36.209</span></span> | <span data-ttu-id="b03ac-366">433</span><span class="sxs-lookup"><span data-stu-id="b03ac-366">433</span></span> | <span data-ttu-id="b03ac-367">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-367">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-368">Korea déli régiója</span><span class="sxs-lookup"><span data-stu-id="b03ac-368">Korea South</span></span> | <span data-ttu-id="b03ac-369">52.231.203.16</span><span class="sxs-lookup"><span data-stu-id="b03ac-369">52.231.203.16</span></span></br><span data-ttu-id="b03ac-370">52.231.205.214</span><span class="sxs-lookup"><span data-stu-id="b03ac-370">52.231.205.214</span></span> | <span data-ttu-id="b03ac-371">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-371">443</span></span> | <span data-ttu-id="b03ac-372">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-372">Inbound</span></span>
    | <span data-ttu-id="b03ac-373">Egyesült Királyság</span><span class="sxs-lookup"><span data-stu-id="b03ac-373">United Kingdom</span></span> | <span data-ttu-id="b03ac-374">Az Egyesült Királyság nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="b03ac-374">UK West</span></span> | <span data-ttu-id="b03ac-375">51.141.13.110</span><span class="sxs-lookup"><span data-stu-id="b03ac-375">51.141.13.110</span></span></br><span data-ttu-id="b03ac-376">51.141.7.20</span><span class="sxs-lookup"><span data-stu-id="b03ac-376">51.141.7.20</span></span> | <span data-ttu-id="b03ac-377">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-377">443</span></span> | <span data-ttu-id="b03ac-378">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-378">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-379">Az Egyesült Királyság déli régiója</span><span class="sxs-lookup"><span data-stu-id="b03ac-379">UK South</span></span> | <span data-ttu-id="b03ac-380">51.140.47.39</span><span class="sxs-lookup"><span data-stu-id="b03ac-380">51.140.47.39</span></span></br><span data-ttu-id="b03ac-381">51.140.52.16</span><span class="sxs-lookup"><span data-stu-id="b03ac-381">51.140.52.16</span></span> | <span data-ttu-id="b03ac-382">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-382">443</span></span> | <span data-ttu-id="b03ac-383">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-383">Inbound</span></span> |
    | <span data-ttu-id="b03ac-384">Egyesült Államok</span><span class="sxs-lookup"><span data-stu-id="b03ac-384">United States</span></span> | <span data-ttu-id="b03ac-385">USA középső régiója</span><span class="sxs-lookup"><span data-stu-id="b03ac-385">Central US</span></span> | <span data-ttu-id="b03ac-386">13.67.223.215</span><span class="sxs-lookup"><span data-stu-id="b03ac-386">13.67.223.215</span></span></br><span data-ttu-id="b03ac-387">40.86.83.253</span><span class="sxs-lookup"><span data-stu-id="b03ac-387">40.86.83.253</span></span> | <span data-ttu-id="b03ac-388">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-388">443</span></span> | <span data-ttu-id="b03ac-389">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-389">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-390">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="b03ac-390">North Central US</span></span> | <span data-ttu-id="b03ac-391">157.56.8.38</span><span class="sxs-lookup"><span data-stu-id="b03ac-391">157.56.8.38</span></span></br><span data-ttu-id="b03ac-392">157.55.213.99</span><span class="sxs-lookup"><span data-stu-id="b03ac-392">157.55.213.99</span></span> | <span data-ttu-id="b03ac-393">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-393">443</span></span> | <span data-ttu-id="b03ac-394">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-394">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-395">USA nyugati középső régiója</span><span class="sxs-lookup"><span data-stu-id="b03ac-395">West Central US</span></span> | <span data-ttu-id="b03ac-396">52.161.23.15</span><span class="sxs-lookup"><span data-stu-id="b03ac-396">52.161.23.15</span></span></br><span data-ttu-id="b03ac-397">52.161.10.167</span><span class="sxs-lookup"><span data-stu-id="b03ac-397">52.161.10.167</span></span> | <span data-ttu-id="b03ac-398">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-398">443</span></span> | <span data-ttu-id="b03ac-399">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-399">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="b03ac-400">USA nyugati régiója, 2.</span><span class="sxs-lookup"><span data-stu-id="b03ac-400">West US 2</span></span> | <span data-ttu-id="b03ac-401">52.175.211.210</span><span class="sxs-lookup"><span data-stu-id="b03ac-401">52.175.211.210</span></span></br><span data-ttu-id="b03ac-402">52.175.222.222</span><span class="sxs-lookup"><span data-stu-id="b03ac-402">52.175.222.222</span></span> | <span data-ttu-id="b03ac-403">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-403">443</span></span> | <span data-ttu-id="b03ac-404">Bejövő</span><span class="sxs-lookup"><span data-stu-id="b03ac-404">Inbound</span></span> |

    <span data-ttu-id="b03ac-405">Információk hello IP-címek toouse az Azure Government, lásd: hello [Azure Government Eszközintelligencia + analitika](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-405">For information on hello IP addresses toouse for Azure Government, see hello [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) document.</span></span>

3. <span data-ttu-id="b03ac-406">Ha egy egyéni DNS-kiszolgáló használ a virtuális hálózat, is engedélyeznie kell a hozzáférést a __168.63.129.16__.</span><span class="sxs-lookup"><span data-stu-id="b03ac-406">If you use a custom DNS server with your virtual network, you must also allow access from __168.63.129.16__.</span></span> <span data-ttu-id="b03ac-407">Ez a cím az Azure rekurzív feloldó.</span><span class="sxs-lookup"><span data-stu-id="b03ac-407">This address is Azure's recursive resolver.</span></span> <span data-ttu-id="b03ac-408">További információkért lásd: hello [virtuális gépek és a szerepkör névfeloldását példányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-408">For more information, see hello [Name resolution for VMs and Role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.</span></span>

<span data-ttu-id="b03ac-409">További információkért lásd: hello [hálózati forgalom vezérlése](#networktraffic) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-409">For more information, see hello [Controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="b03ac-410"><a id="hdinsight-ports"></a>Szükséges portok</span><span class="sxs-lookup"><span data-stu-id="b03ac-410"><a id="hdinsight-ports"></a> Required ports</span></span>

<span data-ttu-id="b03ac-411">Ha a hálózat használatával kíván **virtuális készülék tűzfal** toosecure hello virtuális hálózat, engedélyeznie kell a kimenő forgalmat a következő portok hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-411">If you plan on using a network **virtual appliance firewall** toosecure hello virtual network, you must allow outbound traffic on hello following ports:</span></span>

* <span data-ttu-id="b03ac-412">53</span><span class="sxs-lookup"><span data-stu-id="b03ac-412">53</span></span>
* <span data-ttu-id="b03ac-413">443</span><span class="sxs-lookup"><span data-stu-id="b03ac-413">443</span></span>
* <span data-ttu-id="b03ac-414">1433</span><span class="sxs-lookup"><span data-stu-id="b03ac-414">1433</span></span>
* <span data-ttu-id="b03ac-415">11000-11999</span><span class="sxs-lookup"><span data-stu-id="b03ac-415">11000-11999</span></span>
* <span data-ttu-id="b03ac-416">14000-14999</span><span class="sxs-lookup"><span data-stu-id="b03ac-416">14000-14999</span></span>

<span data-ttu-id="b03ac-417">A portok adott szolgáltatások listájáért lásd: hello [a HDInsight Hadoop-szolgáltatás által használt portok](hdinsight-hadoop-port-settings-for-services.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-417">For a list of ports for specific services, see hello [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

<span data-ttu-id="b03ac-418">A virtuális készülékek vonatkozó tűzfalszabályok további információkért lásd: hello [virtuális berendezésre telepítik](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b03ac-418">For more information on firewall rules for virtual appliances, see hello [virtual appliance scenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) document.</span></span>

## <span data-ttu-id="b03ac-419"><a id="hdinsight-nsg"></a>Példa: hálózati biztonsági csoportok a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="b03ac-419"><a id="hdinsight-nsg"></a>Example: network security groups with HDInsight</span></span>

<span data-ttu-id="b03ac-420">Ebben a szakaszban hello példák bemutatják, hogyan toocreate hálózati biztonsági csoport szabályokat, amelyek engedélyezik a HDInsight toocommunicate hello az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b03ac-420">hello examples in this section demonstrate how toocreate network security group rules that allow HDInsight toocommunicate with hello Azure management services.</span></span> <span data-ttu-id="b03ac-421">Példák hello használatához állítsa be úgy a hello IP címek toomatch hello néhányat a meglévők közül hello használata Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="b03ac-421">Before using hello examples, adjust hello IP addresses toomatch hello ones for hello Azure region you are using.</span></span> <span data-ttu-id="b03ac-422">Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-422">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-resource-management-template"></a><span data-ttu-id="b03ac-423">Az Azure erőforrás-kezelés sablon</span><span class="sxs-lookup"><span data-stu-id="b03ac-423">Azure Resource Management template</span></span>

<span data-ttu-id="b03ac-424">hello következő erőforrás-kezelés sablonnal hoz létre egy virtuális hálózatot, amely korlátozza a bejövő forgalmat, de lehetővé teszi, hogy a HDInsight által igényelt hello IP-címekről érkező forgalom.</span><span class="sxs-lookup"><span data-stu-id="b03ac-424">hello following Resource Management template creates a virtual network that restricts inbound traffic, but allows traffic from hello IP addresses required by HDInsight.</span></span> <span data-ttu-id="b03ac-425">Ez a sablon is létrehoz egy HDInsight-fürt hello virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="b03ac-425">This template also creates an HDInsight cluster in hello virtual network.</span></span>

* [<span data-ttu-id="b03ac-426">A biztonságos Azure virtuális hálózat és egy HDInsight Hadoop-fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="b03ac-426">Deploy a secured Azure Virtual Network and an HDInsight Hadoop cluster</span></span>](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> <span data-ttu-id="b03ac-427">A példa toomatch hello Azure-régió, használja a használt hello IP-címek módosítása</span><span class="sxs-lookup"><span data-stu-id="b03ac-427">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="b03ac-428">Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-428">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="b03ac-429">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b03ac-429">Azure PowerShell</span></span>

<span data-ttu-id="b03ac-430">A következő PowerShell-parancsfájl toocreate bejövő forgalmát, és lehetővé teszi, hogy a forgalom hello IP-címek hello Észak-Európa régió virtuális hálózat hello használata.</span><span class="sxs-lookup"><span data-stu-id="b03ac-430">Use hello following PowerShell script toocreate a virtual network that restricts inbound traffic and allows traffic from hello IP addresses for hello North Europe region.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b03ac-431">A példa toomatch hello Azure-régió, használja a használt hello IP-címek módosítása</span><span class="sxs-lookup"><span data-stu-id="b03ac-431">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="b03ac-432">Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-432">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with hello resource group hello virtual network is in"
$subnetName = "Replace with hello name of hello subnet that you plan toouse for HDInsight"
# Get hello Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get hello region hello Virtual network is in.
$location = $vnet.Location
# Get hello subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for hello HDInsight health and management services.
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
# Set hello changes toohello security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply hello NSG toohello subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> <span data-ttu-id="b03ac-433">Ez a példa bemutatja, hogyan tooadd szabályok tooallow bejövő adatforgalmat a szükséges hello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="b03ac-433">This example demonstrates how tooadd rules tooallow inbound traffic on hello required IP addresses.</span></span> <span data-ttu-id="b03ac-434">A szabály toorestrict nem tartalmaz más forrásokból hozzáférés bejövő.</span><span class="sxs-lookup"><span data-stu-id="b03ac-434">It does not contain a rule toorestrict inbound access from other sources.</span></span>
>
> <span data-ttu-id="b03ac-435">hello a következő példa bemutatja, hogy tooenable SSH hogyan férhetnek hozzá az internethez hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-435">hello following example demonstrates how tooenable SSH access from hello Internet:</span></span>
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a><span data-ttu-id="b03ac-436">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b03ac-436">Azure CLI</span></span>

<span data-ttu-id="b03ac-437">A következő lépéseket toocreate bejövő forgalmát, de lehetővé teszi, hogy a HDInsight által igényelt hello IP-címekről érkező forgalom virtuális hálózat hello használata.</span><span class="sxs-lookup"><span data-stu-id="b03ac-437">Use hello following steps toocreate a virtual network that restricts inbound traffic, but allows traffic from hello IP addresses required by HDInsight.</span></span>

1. <span data-ttu-id="b03ac-438">A következő parancs toocreate új hálózati biztonsági csoport nevű használata hello `hdisecure`.</span><span class="sxs-lookup"><span data-stu-id="b03ac-438">Use hello following command toocreate a new network security group named `hdisecure`.</span></span> <span data-ttu-id="b03ac-439">Cserélje le **RESOURCEGROUPNAME** hello erőforráscsoporttal, amely tartalmazza a hello Azure-beli virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-439">Replace **RESOURCEGROUPNAME** with hello resource group that contains hello Azure Virtual Network.</span></span> <span data-ttu-id="b03ac-440">Cserélje le **hely** hello helyen lévő (régió) hello csoport jött létre.</span><span class="sxs-lookup"><span data-stu-id="b03ac-440">Replace **LOCATION** with hello location (region) that hello group was created in.</span></span>

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    <span data-ttu-id="b03ac-441">Hello csoport létrehozása után megjelenik a hello új csoport adatai.</span><span class="sxs-lookup"><span data-stu-id="b03ac-441">Once hello group has been created, you receive information on hello new group.</span></span>

2. <span data-ttu-id="b03ac-442">A következő tooadd szabályok toohello új hálózati biztonsági csoportot, amely hello Azure HDInsight állapotát és a felügyeleti szolgáltatás a 443-as portot a bejövő kommunikáció hello használata.</span><span class="sxs-lookup"><span data-stu-id="b03ac-442">Use hello following tooadd rules toohello new network security group that allow inbound communication on port 443 from hello Azure HDInsight health and management service.</span></span> <span data-ttu-id="b03ac-443">Cserélje le **RESOURCEGROUPNAME** hello Azure Virtual Network tartalmazó erőforráscsoport hello hello névvel.</span><span class="sxs-lookup"><span data-stu-id="b03ac-443">Replace **RESOURCEGROUPNAME** with hello name of hello resource group that contains hello Azure Virtual Network.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b03ac-444">A példa toomatch hello Azure-régió, használja a használt hello IP-címek módosítása</span><span class="sxs-lookup"><span data-stu-id="b03ac-444">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="b03ac-445">Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-445">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. <span data-ttu-id="b03ac-446">tooretrieve hello a hálózati biztonsági csoport egyedi azonosítója, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="b03ac-446">tooretrieve hello unique identifier for this network security group, use hello following command:</span></span>

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    <span data-ttu-id="b03ac-447">Ez a parancs visszaadja a szöveg a következő érték hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-447">This command returns a value similar toohello following text:</span></span>

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    <span data-ttu-id="b03ac-448">Használjon a hello parancs azonosítója dupla idézőjelbe, ha a várt hello eredmények nem jelenik.</span><span class="sxs-lookup"><span data-stu-id="b03ac-448">Use double-quotes around id in hello command if you don't get hello expected results.</span></span>

4. <span data-ttu-id="b03ac-449">A következő parancs tooapply hello hálózati biztonsági csoport tooa alhálózati hello használata.</span><span class="sxs-lookup"><span data-stu-id="b03ac-449">Use hello following command tooapply hello network security group tooa subnet.</span></span> <span data-ttu-id="b03ac-450">Cserélje le a hello __GUID__ és __RESOURCEGROUPNAME__ hello előző lépésben által visszaadott értékek hello néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="b03ac-450">Replace hello __GUID__ and __RESOURCEGROUPNAME__ values with hello ones returned from hello previous step.</span></span> <span data-ttu-id="b03ac-451">Cserélje le __VNETNAME__ és __SUBNETNAME__ hello virtuálishálózat-névnek és alhálózat neve, amelyet az toocreate.</span><span class="sxs-lookup"><span data-stu-id="b03ac-451">Replace __VNETNAME__ and __SUBNETNAME__ with hello virtual network name and subnet name that you want toocreate.</span></span>

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    <span data-ttu-id="b03ac-452">Ez a parancs után a HDInsight a virtuális hálózati hello is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="b03ac-452">Once this command completes, you can install HDInsight into hello Virtual Network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b03ac-453">Ezeket a lépéseket csak nyissa meg a toohello HDInsight állapotát és a felügyeleti szolgáltatás a hello Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="b03ac-453">These steps only open access toohello HDInsight health and management service on hello Azure cloud.</span></span> <span data-ttu-id="b03ac-454">Bármely más hozzáférési toohello HDInsight-fürt a virtuális hálózaton kívül hello le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="b03ac-454">Any other access toohello HDInsight cluster from outside hello Virtual Network is blocked.</span></span> <span data-ttu-id="b03ac-455">tooenable hozzáférés a külső hello virtuális hálózatról, hozzá kell adnia a további hálózati biztonsági csoportszabályok.</span><span class="sxs-lookup"><span data-stu-id="b03ac-455">tooenable access from outside hello virtual network, you must add additional Network Security Group rules.</span></span>
>
> <span data-ttu-id="b03ac-456">hello a következő példa bemutatja, hogy tooenable SSH hogyan férhetnek hozzá az internethez hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-456">hello following example demonstrates how tooenable SSH access from hello Internet:</span></span>
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <span data-ttu-id="b03ac-457"><a id="example-dns"></a>Példa: DNS-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b03ac-457"><a id="example-dns"></a> Example: DNS configuration</span></span>

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a><span data-ttu-id="b03ac-458">Névfeloldás egy virtuális és a csatlakoztatott helyszíni hálózat között</span><span class="sxs-lookup"><span data-stu-id="b03ac-458">Name resolution between a virtual network and a connected on-premises network</span></span>

<span data-ttu-id="b03ac-459">Ebben a példában a következő feltételek hello teszi:</span><span class="sxs-lookup"><span data-stu-id="b03ac-459">This example makes hello following assumptions:</span></span>

* <span data-ttu-id="b03ac-460">Rendelkezik egy Azure virtuális hálózatot, amely csatlakoztatott tooan a helyszíni hálózat VPN-átjáró használatával.</span><span class="sxs-lookup"><span data-stu-id="b03ac-460">You have an Azure Virtual Network that is connected tooan on-premises network using a VPN gateway.</span></span>

* <span data-ttu-id="b03ac-461">hello egyéni DNS-kiszolgáló hello virtuális hálózat futtató Linux vagy Unix hello operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="b03ac-461">hello custom DNS server in hello virtual network is running Linux or Unix as hello operating system.</span></span>

* <span data-ttu-id="b03ac-462">[Kötési](https://www.isc.org/downloads/bind/) hello egyéni DNS-kiszolgálón telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b03ac-462">[Bind](https://www.isc.org/downloads/bind/) is installed on hello custom DNS server.</span></span>

<span data-ttu-id="b03ac-463">Hello egyéni DNS-kiszolgálón a virtuális hálózati hello:</span><span class="sxs-lookup"><span data-stu-id="b03ac-463">On hello custom DNS server in hello virtual network:</span></span>

1. <span data-ttu-id="b03ac-464">Azure PowerShell vagy az Azure CLI toofind hello DNS-utótagjának használata hello virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="b03ac-464">Use either Azure PowerShell or Azure CLI toofind hello DNS suffix of hello virtual network:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="b03ac-465">Hello egyéni DNS-kiszolgálón hello virtuális hálózat, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.local` fájlt:</span><span class="sxs-lookup"><span data-stu-id="b03ac-465">On hello custom DNS server for hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.local` file:</span></span>

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    <span data-ttu-id="b03ac-466">Cserélje le a hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hello DNS-utótagját, a virtuális hálózat értéket.</span><span class="sxs-lookup"><span data-stu-id="b03ac-466">Replace hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with hello DNS suffix of your virtual network.</span></span>

    <span data-ttu-id="b03ac-467">Ez a konfiguráció hello DNS-utótag hello virtuális hálózati toohello Azure rekurzív feloldó az összes DNS-kérelmek irányítja.</span><span class="sxs-lookup"><span data-stu-id="b03ac-467">This configuration routes all DNS requests for hello DNS suffix of hello virtual network toohello Azure recursive resolver.</span></span>

2. <span data-ttu-id="b03ac-468">Hello egyéni DNS-kiszolgálón hello virtuális hálózat, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.options` fájlt:</span><span class="sxs-lookup"><span data-stu-id="b03ac-468">On hello custom DNS server for hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients tooaccept requests from
    // TODO: Add hello IP range of hello joined network toothis list
    acl goodclients {
        10.0.0.0/16; # IP address range of hello virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent toohello following
            forwarders {
                192.168.0.1; # Replace with hello IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="b03ac-469">Cserélje le a hello `10.0.0.0/16` hello IP-címtartomány a virtuális hálózat értéket.</span><span class="sxs-lookup"><span data-stu-id="b03ac-469">Replace hello `10.0.0.0/16` value with hello IP address range of your virtual network.</span></span> <span data-ttu-id="b03ac-470">Ez a bejegyzés lehetővé teszi, hogy a név feloldása kérelmek címek ebbe a tartományba.</span><span class="sxs-lookup"><span data-stu-id="b03ac-470">This entry allows name resolution requests addresses within this range.</span></span>

    * <span data-ttu-id="b03ac-471">Hello IP-címtartomány a hello a helyszíni hálózati toohello hozzáadása `acl goodclients { ... }` szakasz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-471">Add hello IP address range of hello on-premises network toohello `acl goodclients { ... }` section.</span></span>  <span data-ttu-id="b03ac-472">bejegyzés lehetővé teszi, hogy a források névfeloldási hello a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="b03ac-472">entry allows name resolution requests from resources in hello on-premises network.</span></span>
    
    * <span data-ttu-id="b03ac-473">Cserélje le a hello érték `192.168.0.1` a helyi DNS-kiszolgáló hello IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="b03ac-473">Replace hello value `192.168.0.1` with hello IP address of your on-premises DNS server.</span></span> <span data-ttu-id="b03ac-474">Ez a bejegyzés minden más DNS kérelmek toohello a helyi DNS-kiszolgáló irányítja.</span><span class="sxs-lookup"><span data-stu-id="b03ac-474">This entry routes all other DNS requests toohello on-premises DNS server.</span></span>

3. <span data-ttu-id="b03ac-475">toouse hello konfigurációs, indítsa újra a kötés.</span><span class="sxs-lookup"><span data-stu-id="b03ac-475">toouse hello configuration, restart Bind.</span></span> <span data-ttu-id="b03ac-476">Például: `sudo service bind9 restart`.</span><span class="sxs-lookup"><span data-stu-id="b03ac-476">For example, `sudo service bind9 restart`.</span></span>

4. <span data-ttu-id="b03ac-477">Feltételes továbbítók toohello a helyi DNS-kiszolgáló hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b03ac-477">Add a conditional forwarder toohello on-premises DNS server.</span></span> <span data-ttu-id="b03ac-478">Konfigurálhatja a hello feltételes továbbítók toosend kérések hello DNS-utótag lépés 1 toohello egyéni DNS-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="b03ac-478">Configure hello conditional forwarder toosend requests for hello DNS suffix from step 1 toohello custom DNS server.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b03ac-479">Hello dokumentációjából tájékozódhat arról, hogy miként a DNS-szoftver tooadd feltételes továbbítót.</span><span class="sxs-lookup"><span data-stu-id="b03ac-479">Consult hello documentation for your DNS software for specifics on how tooadd a conditional forwarder.</span></span>

<span data-ttu-id="b03ac-480">A lépések elvégzése után tooresources vagy a hálózat teljes tartománynevek (FQDN) használatával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="b03ac-480">After completing these steps, you can connect tooresources in either network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="b03ac-481">Most már telepítheti HDInsight hello virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-481">You can now install HDInsight into hello virtual network.</span></span>

### <a name="name-resolution-between-two-connected-virtual-networks"></a><span data-ttu-id="b03ac-482">Két csatlakoztatott virtuális hálózatok közötti névfeloldás</span><span class="sxs-lookup"><span data-stu-id="b03ac-482">Name resolution between two connected virtual networks</span></span>

<span data-ttu-id="b03ac-483">Ebben a példában a következő feltételek hello teszi:</span><span class="sxs-lookup"><span data-stu-id="b03ac-483">This example makes hello following assumptions:</span></span>

* <span data-ttu-id="b03ac-484">VPN-átjáró használatával, vagy a társviszony-létesítés csatlakoztatott két Azure virtuális hálózat rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b03ac-484">You have two Azure Virtual Networks that are connected using either a VPN gateway or peering.</span></span>

* <span data-ttu-id="b03ac-485">mindkét hálózat hello egyéni DNS-kiszolgáló fut. Linux vagy Unix hello operációs rendszert.</span><span class="sxs-lookup"><span data-stu-id="b03ac-485">hello custom DNS server in both networks is running Linux or Unix as hello operating system.</span></span>

* <span data-ttu-id="b03ac-486">[Kötési](https://www.isc.org/downloads/bind/) hello egyéni DNS-kiszolgálókon telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b03ac-486">[Bind](https://www.isc.org/downloads/bind/) is installed on hello custom DNS servers.</span></span>

1. <span data-ttu-id="b03ac-487">Azure PowerShell vagy az Azure CLI toofind hello DNS-utótagját, mindkét virtuális hálózat használata:</span><span class="sxs-lookup"><span data-stu-id="b03ac-487">Use either Azure PowerShell or Azure CLI toofind hello DNS suffix of both virtual networks:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="b03ac-488">Szöveg hello hello tartalmát, a következő használatát hello `/etc/bind/named.config.local` fájl hello egyéni DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b03ac-488">Use hello following text as hello contents of hello `/etc/bind/named.config.local` file on hello custom DNS server.</span></span> <span data-ttu-id="b03ac-489">Adja meg ezt a módosítást az egyéni DNS-kiszolgáló hello mindkét virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-489">Make this change on hello custom DNS server in both virtual networks.</span></span>

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello IP address of hello DNS server in hello other virtual network
    };
    ```

    <span data-ttu-id="b03ac-490">Cserélje le a hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hello DNS-utótagja hello értékének __más__ virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b03ac-490">Replace hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with hello DNS suffix of hello __other__ virtual network.</span></span> <span data-ttu-id="b03ac-491">Ez a bejegyzés irányítja a DNS-utótagja hello hello távoli hálózati toohello vonatkozó egyéni DNS-ét erre a hálózatra.</span><span class="sxs-lookup"><span data-stu-id="b03ac-491">This entry routes requests for hello DNS suffix of hello remote network toohello custom DNS in that network.</span></span>

3. <span data-ttu-id="b03ac-492">Hello egyéni DNS-kiszolgálókon mindkét virtuális hálózatban, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.options` fájlt:</span><span class="sxs-lookup"><span data-stu-id="b03ac-492">On hello custom DNS servers in both virtual networks, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients tooaccept requests from
    acl goodclients {
        10.1.0.0/16; # hello IP address range of one virtual network
        10.0.0.0/16; # hello IP address range of hello other virtual network
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

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="b03ac-493">Cserélje le a hello `10.0.0.0/16` és `10.1.0.0/16` értékek hello IP-címtartományok a virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="b03ac-493">Replace hello `10.0.0.0/16` and `10.1.0.0/16` values with hello IP address ranges of your virtual networks.</span></span> <span data-ttu-id="b03ac-494">Ez a bejegyzés lehetővé teszi, hogy minden hálózati erőforrások toomake kérelmek hello DNS-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="b03ac-494">This entry allows resources in each network toomake requests of hello DNS servers.</span></span>

    <span data-ttu-id="b03ac-495">Minden kérést, amelyek nincsenek a hello DNS-utótagok hello virtuális hálózatok (Fontos) hello Azure rekurzív feloldó kezeli.</span><span class="sxs-lookup"><span data-stu-id="b03ac-495">Any requests that are not for hello DNS suffixes of hello virtual networks (for example, microsoft.com) is handled by hello Azure recursive resolver.</span></span>

4. <span data-ttu-id="b03ac-496">toouse hello konfigurációs, indítsa újra a kötés.</span><span class="sxs-lookup"><span data-stu-id="b03ac-496">toouse hello configuration, restart Bind.</span></span> <span data-ttu-id="b03ac-497">Például `sudo service bind9 restart` mindkét DNS-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="b03ac-497">For example, `sudo service bind9 restart` on both DNS servers.</span></span>

<span data-ttu-id="b03ac-498">A lépések elvégzése után tooresources hello virtuális hálózatban, teljes tartománynevek (FQDN) használatával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="b03ac-498">After completing these steps, you can connect tooresources in hello virtual network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="b03ac-499">Most már telepítheti HDInsight hello virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b03ac-499">You can now install HDInsight into hello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b03ac-500">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b03ac-500">Next steps</span></span>

* <span data-ttu-id="b03ac-501">Például egy végpontok közötti HDInsight tooconnect tooan a helyszíni hálózat konfigurálása, lásd: [csatlakozás HDInsight tooan a helyszíni hálózat](./connect-on-premises-network.md).</span><span class="sxs-lookup"><span data-stu-id="b03ac-501">For an end-to-end example of configuring HDInsight tooconnect tooan on-premises network, see [Connect HDInsight tooan on-premises network](./connect-on-premises-network.md).</span></span>

* <span data-ttu-id="b03ac-502">Egy Azure virtuális hálózatot további információkért lásd: hello [Azure Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b03ac-502">For more information on Azure virtual networks, see hello [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="b03ac-503">Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="b03ac-503">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="b03ac-504">A felhasználó által definiált útvonalak további információkért lásd: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b03ac-504">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>