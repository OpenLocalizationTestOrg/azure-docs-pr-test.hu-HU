---
title: "Egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-ban |} Microsoft Docs"
description: "Ez az oktatóanyag a klasszikus üzembe helyezési modellel létrehozott erőforrást használ, és a belső terheléselosztót használó Azure-ban létrehoz egy Always On rendelkezésre állási csoport figyelőjének."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: fea70b389b1f1d6af963e3f14fdc48e8d857dd53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a><span data-ttu-id="49968-103">Egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="49968-103">Configure an ILB listener for Always On availability groups in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="49968-104">Belső figyelő</span><span class="sxs-lookup"><span data-stu-id="49968-104">Internal listener</span></span>](../classic/ps-sql-int-listener.md)
> * [<span data-ttu-id="49968-105">Külső figyelő</span><span class="sxs-lookup"><span data-stu-id="49968-105">External listener</span></span>](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a><span data-ttu-id="49968-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="49968-106">Overview</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49968-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="49968-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="49968-108">Ez a cikk ismerteti a klasszikus üzembe helyezési modellel használatát.</span><span class="sxs-lookup"><span data-stu-id="49968-108">This article covers the use of the classic deployment model.</span></span> <span data-ttu-id="49968-109">Azt javasoljuk, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="49968-109">We recommend that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="49968-110">A Resource Manager modellt egy figyelőt a következő Always On rendelkezésre állási csoport konfigurálásához lásd: [egy terheléselosztót egy Always On rendelkezésre állási csoport konfigurálása az Azure-](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="49968-110">To configure a listener for an Always On availability group in the Resource Manager model, see [Configure a load balancer for an Always On availability group in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

<span data-ttu-id="49968-111">A rendelkezésre állási csoport replikák, amelyek csak a helyszíni vagy Azure csak, vagy a helyszíni és az Azure hibrid konfigurációk átnyúló tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="49968-111">Your availability group can contain replicas that are on-premises only or Azure only, or that span both on-premises and Azure for hybrid configurations.</span></span> <span data-ttu-id="49968-112">Az Azure replikák a régión belül, vagy több virtuális hálózat használó különféle régiókban találhatók.</span><span class="sxs-lookup"><span data-stu-id="49968-112">Azure replicas can reside within the same region or across multiple regions that use multiple virtual networks.</span></span> <span data-ttu-id="49968-113">Ebben a cikkben szereplő eljárások azt feltételezik, hogy már rendelkezik [konfigurált rendelkezésre állási csoport](../classic/portal-sql-alwayson-availability-groups.md) , de még nincs konfigurálva egy figyelő.</span><span class="sxs-lookup"><span data-stu-id="49968-113">The procedures in this article assume that you have already [configured an availability group](../classic/portal-sql-alwayson-availability-groups.md) but have not yet configured a listener.</span></span>

## <a name="guidelines-and-limitations-for-internal-listeners"></a><span data-ttu-id="49968-114">Tudnivalók és korlátozások belső figyelők</span><span class="sxs-lookup"><span data-stu-id="49968-114">Guidelines and limitations for internal listeners</span></span>
<span data-ttu-id="49968-115">A használata egy belső terheléselosztón (ILB) egy rendelkezésre állási csoport figyelőjével, az Azure rendszerben a következő irányelveket:</span><span class="sxs-lookup"><span data-stu-id="49968-115">The use of an internal load balancer (ILB) with an availability group listener in Azure is subject to the following guidelines:</span></span>

* <span data-ttu-id="49968-116">A rendelkezésre állási csoport figyelőjének Windows Server 2008 R2, Windows Server 2012 és Windows Server 2012 R2 esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="49968-116">The availability group listener is supported on Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span>
* <span data-ttu-id="49968-117">Csak egy belső rendelkezésre állási csoport figyelőjének minden felhőalapú szolgáltatás, mert a figyelő a Példánynak van konfigurálva, és csak egy ILB minden felhőszolgáltatás esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="49968-117">Only one internal availability group listener is supported for each cloud service, because the listener is configured to the ILB, and there is only one ILB for each cloud service.</span></span> <span data-ttu-id="49968-118">Azonban azt is létrehozhat több külső figyelők.</span><span class="sxs-lookup"><span data-stu-id="49968-118">However, it is possible to create multiple external listeners.</span></span> <span data-ttu-id="49968-119">További információkért lásd: [egy külső figyelőt a Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-ext-listener.md).</span><span class="sxs-lookup"><span data-stu-id="49968-119">For more information, see [Configure an external listener for Always On availability groups in Azure](../classic/ps-sql-ext-listener.md).</span></span>

## <a name="determine-the-accessibility-of-the-listener"></a><span data-ttu-id="49968-120">A figyelő a kisegítő lehetőségek meghatározása</span><span class="sxs-lookup"><span data-stu-id="49968-120">Determine the accessibility of the listener</span></span>
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

<span data-ttu-id="49968-121">Ez a cikk foglalkozik, amely egy ILB használja egy figyelő létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="49968-121">This article focuses on creating a listener that uses an ILB.</span></span> <span data-ttu-id="49968-122">Ha egy nyilvános vagy külső figyelőt, lásd: Ez a cikk ismerteti, amelyek a beállítás mentése verziója egy [külső figyelő](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49968-122">If you need an public or external listener, see the version of this article that discusses setting up an [external listener](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a><span data-ttu-id="49968-123">Hozzon létre virtuális gép elosztott terhelésű végpontok közvetlen kiszolgálói visszatérési</span><span class="sxs-lookup"><span data-stu-id="49968-123">Create load-balanced VM endpoints with direct server return</span></span>
<span data-ttu-id="49968-124">Először hozzon létre egy ILB a parancsfájl futtatásával később ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="49968-124">You first create an ILB by running the script later in this section.</span></span>

<span data-ttu-id="49968-125">Az egyes virtuális gépek az Azure-replikát tartalmazó elosztott terhelésű végpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="49968-125">Create a load-balanced endpoint for each VM that hosts an Azure replica.</span></span> <span data-ttu-id="49968-126">Ha több régióba replikákat, az adott régió minden egyes replikának a azonos felhőszolgáltatásban azonos Azure virtuális hálózatban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="49968-126">If you have replicas in multiple regions, each replica for that region must be in the same cloud service in the same Azure virtual network.</span></span> <span data-ttu-id="49968-127">Rendelkezésre állási csoport replikái, amelyek több Azure-régiók több létrehozása több virtuális hálózat beállítását igényli.</span><span class="sxs-lookup"><span data-stu-id="49968-127">Creating availability group replicas that span multiple Azure regions requires configuring multiple virtual networks.</span></span> <span data-ttu-id="49968-128">Több virtuális hálózati kapcsolat konfigurálásával kapcsolatos további információkért lásd: [virtuális hálózat konfigurálása a virtuális hálózati kapcsolat](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="49968-128">For more information on configuring cross virtual network connectivity, see [Configure virtual network to virtual network connectivity](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span>

1. <span data-ttu-id="49968-129">Nyissa meg az Azure portálon, minden egyes virtuális géphez, amelyen a replika adatait tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="49968-129">In the Azure portal, go to each VM that hosts a replica to view the details.</span></span>

2. <span data-ttu-id="49968-130">Kattintson a **végpontok** lapon az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="49968-130">Click the **Endpoints** tab for each VM.</span></span>

3. <span data-ttu-id="49968-131">Ellenőrizze, hogy a **neve** és **nyilvános Port** a figyelő végponttal, amely a használni kívánt nem már használatban van.</span><span class="sxs-lookup"><span data-stu-id="49968-131">Verify that the **Name** and **Public Port** of the listener endpoint that you want to use are not already in use.</span></span> <span data-ttu-id="49968-132">A jelen szakaszban ismertetett példa, a neve: *MyEndpoint*, és a portot *1433*.</span><span class="sxs-lookup"><span data-stu-id="49968-132">In the example in this section, the name is *MyEndpoint*, and the port is *1433*.</span></span>

4. <span data-ttu-id="49968-133">A helyi ügyfél, töltse le és telepítse a legújabb [PowerShell modul](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="49968-133">On your local client, download and install the latest [PowerShell module](https://azure.microsoft.com/downloads/).</span></span>

5. <span data-ttu-id="49968-134">Indítsa el az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49968-134">Start Azure PowerShell.</span></span>  
    <span data-ttu-id="49968-135">Egy új PowerShell-munkamenetet nyit meg, az Azure felügyeleti modulok terhelését.</span><span class="sxs-lookup"><span data-stu-id="49968-135">A new PowerShell session opens, with the Azure administrative modules loaded.</span></span>

6. <span data-ttu-id="49968-136">Futtassa az `Get-AzurePublishSettingsFile` parancsot.</span><span class="sxs-lookup"><span data-stu-id="49968-136">Run `Get-AzurePublishSettingsFile`.</span></span> <span data-ttu-id="49968-137">Ez a parancsmag egy böngészőt, és töltse le a közzétételi beállítások fájlja egy helyi könyvtárba irányítja.</span><span class="sxs-lookup"><span data-stu-id="49968-137">This cmdlet directs you to a browser to download a publish settings file to a local directory.</span></span> <span data-ttu-id="49968-138">Rendszer felajánlhatja a bejelentkezési hitelesítő adatok Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="49968-138">You might be prompted for your sign-in credentials for your Azure subscription.</span></span>

7. <span data-ttu-id="49968-139">Futtassa a következő `Import-AzurePublishSettingsFile` elérési útját a letöltött közzétételi beállítások fájlja parancsot:</span><span class="sxs-lookup"><span data-stu-id="49968-139">Run the following `Import-AzurePublishSettingsFile` command with the path of the publish settings file that you downloaded:</span></span>

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    <span data-ttu-id="49968-140">A közzétételi beállítások fájlja az importálása után kezelheti az Azure-előfizetéshez a PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="49968-140">After the publish settings file is imported, you can manage your Azure subscription in the PowerShell session.</span></span>

8. <span data-ttu-id="49968-141">A *ILB*, statikus IP-cím.</span><span class="sxs-lookup"><span data-stu-id="49968-141">For *ILB*, assign a static IP address.</span></span> <span data-ttu-id="49968-142">Vizsgálja meg az aktuális virtuális hálózati konfigurációt a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="49968-142">Examine the current virtual network configuration by running the following command:</span></span>

        (Get-AzureVNetConfig).XMLConfiguration
9. <span data-ttu-id="49968-143">Megjegyzés: a *alhálózati* nevet az alhálózatot, amely tartalmazza a virtuális gépeket üzemeltető a replikákat.</span><span class="sxs-lookup"><span data-stu-id="49968-143">Note the *Subnet* name for the subnet that contains the VMs that host the replicas.</span></span> <span data-ttu-id="49968-144">Ez a név a parancsfájl a $SubnetName paraméter szerepel.</span><span class="sxs-lookup"><span data-stu-id="49968-144">This name is used in the $SubnetName parameter in the script.</span></span>

10. <span data-ttu-id="49968-145">Megjegyzés: a *VirtualNetworkSite* nevét és a kezdő *címelőtagja* az alhálózat, amely tartalmazza az, hogy a replika virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="49968-145">Note the *VirtualNetworkSite* name and the starting *AddressPrefix* for the subnet that contains the VMs that host the replicas.</span></span> <span data-ttu-id="49968-146">Elérhető IP-cím úgy, hogy két keresse meg a `Test-AzureStaticVNetIP` parancsot, és az megvizsgálta a *AvailableAddresses*.</span><span class="sxs-lookup"><span data-stu-id="49968-146">Look for an available IP address by passing both values to the `Test-AzureStaticVNetIP` command and by examining the *AvailableAddresses*.</span></span> <span data-ttu-id="49968-147">Például, ha a virtuális hálózat neve *MyVNet* , és a alhálózati címtartományt kezdődő *172.16.0.128*, a következő parancsot kellene listában elérhető címek:</span><span class="sxs-lookup"><span data-stu-id="49968-147">For example, if the virtual network is named *MyVNet* and has a subnet address range that starts at *172.16.0.128*, the following command would list available addresses:</span></span>

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. <span data-ttu-id="49968-148">Válassza ki a rendelkezésre álló címek egyikét, és használja azt a parancsfájlt a következő lépésben $ILBStaticIP paraméterében.</span><span class="sxs-lookup"><span data-stu-id="49968-148">Select one of the available addresses, and use it in the $ILBStaticIP parameter of the script in the next step.</span></span>

12. <span data-ttu-id="49968-149">Másolja a következő PowerShell-parancsfájl egy szövegszerkesztőben, és állítsa be a változó értéke a környezetnek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="49968-149">Copy the following PowerShell script to a text editor, and set the variable values to suit your environment.</span></span> <span data-ttu-id="49968-150">Egyes paraméterek alapértelmezett adtak ki.</span><span class="sxs-lookup"><span data-stu-id="49968-150">Defaults have been provided for some parameters.</span></span>  

    <span data-ttu-id="49968-151">Meglévő affinitáscsoportok használó központi telepítések nem adható hozzá egy Példánynak.</span><span class="sxs-lookup"><span data-stu-id="49968-151">Existing deployments that use affinity groups cannot add an ILB.</span></span> <span data-ttu-id="49968-152">ILB követelményeivel kapcsolatos további információkért lásd: [belső load balancer áttekintése](../../../load-balancer/load-balancer-internal-overview.md).</span><span class="sxs-lookup"><span data-stu-id="49968-152">For more information about ILB requirements, see [Internal load balancer overview](../../../load-balancer/load-balancer-internal-overview.md).</span></span>

    <span data-ttu-id="49968-153">Is ha a rendelkezésre állási csoport is az Azure-régiók, futtatnia kell a parancsfájl egyszer minden adatközpontban és a felhőalapú szolgáltatás, és azt az adatközpontot lévő csomópontok.</span><span class="sxs-lookup"><span data-stu-id="49968-153">Also, if your availability group spans Azure regions, you must run the script once in each datacenter for the cloud service and nodes that reside in that datacenter.</span></span>

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. <span data-ttu-id="49968-154">Miután beállította a változókat, másolja a parancsfájlt a szövegszerkesztőben az futtatni a PowerShell-munkamenethez.</span><span class="sxs-lookup"><span data-stu-id="49968-154">After you have set the variables, copy the script from the text editor to your PowerShell session to run it.</span></span> <span data-ttu-id="49968-155">Ha a parancssor még mindig  **>>** , nyomja le az Enter újra győződjön meg arról, hogy a parancsfájl futásának indításakor.</span><span class="sxs-lookup"><span data-stu-id="49968-155">If the prompt still shows **>>**, press Enter again to make sure the script starts running.</span></span>

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a><span data-ttu-id="49968-156">Győződjön meg arról, hogy KB2854082 telepítve van-e, ha szükséges</span><span class="sxs-lookup"><span data-stu-id="49968-156">Verify that KB2854082 is installed if necessary</span></span>
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a><span data-ttu-id="49968-157">Nyissa meg a tűzfal portjait a rendelkezésre állási csoport csomópontok</span><span class="sxs-lookup"><span data-stu-id="49968-157">Open the firewall ports in availability group nodes</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a><span data-ttu-id="49968-158">A rendelkezésre állási csoport figyelőjének létrehozása</span><span class="sxs-lookup"><span data-stu-id="49968-158">Create the availability group listener</span></span>

<span data-ttu-id="49968-159">A rendelkezésre állási csoport figyelőjének létrehozása két lépésben.</span><span class="sxs-lookup"><span data-stu-id="49968-159">Create the availability group listener in two steps.</span></span> <span data-ttu-id="49968-160">Először az ügyfél hozzáférési pont fürterőforrás létrehozása, és konfigurálja a függőségek.</span><span class="sxs-lookup"><span data-stu-id="49968-160">First, create the client access point cluster resource and configure  dependencies.</span></span> <span data-ttu-id="49968-161">Ezután adja meg a fürt erőforrásait PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49968-161">Second, configure the cluster resources in PowerShell.</span></span>

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a><span data-ttu-id="49968-162">Az ügyfél hozzáférési pontjának létrehozása és konfigurálása a fürt függőségek</span><span class="sxs-lookup"><span data-stu-id="49968-162">Create the client access point and configure the cluster dependencies</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a><span data-ttu-id="49968-163">Konfigurálja a fürt erőforrásait a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="49968-163">Configure the cluster resources in PowerShell</span></span>
1. <span data-ttu-id="49968-164">A Példánynak a korábban létrehozott ILB IP-címét kell használnia.</span><span class="sxs-lookup"><span data-stu-id="49968-164">For ILB, you must use the IP address of the ILB that was created earlier.</span></span> <span data-ttu-id="49968-165">Ez a PowerShell IP-cím beszerzéséhez használja a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="49968-165">To obtain this IP address in PowerShell, use the following script:</span></span>

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. <span data-ttu-id="49968-166">Válassza ki a virtuális gépek egyik másolja a PowerShell-parancsfájl az operációs rendszer egy szövegszerkesztőben, és akkor a változó értéke az értékeket, amelyet korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="49968-166">On one of the VMs, copy the PowerShell script for your operating system to a text editor, and then set the variables to the values you noted earlier.</span></span>

    <span data-ttu-id="49968-167">A Windows Server 2012 vagy újabb használja a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="49968-167">For Windows Server 2012 or later, use the following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    <span data-ttu-id="49968-168">A Windows Server 2008 R2 a következő parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="49968-168">For Windows Server 2008 R2, use the following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. <span data-ttu-id="49968-169">Miután beállította a változókat, nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban, a parancsfájl a szövegszerkesztőben illessze be a PowerShell-munkamenetet futtatni.</span><span class="sxs-lookup"><span data-stu-id="49968-169">After you have set the variables, open an elevated Windows PowerShell window, paste the script from the text editor into your PowerShell session to run it.</span></span> <span data-ttu-id="49968-170">Ha a parancssor még mindig  **>>** , nyomja le az Enter újra annak ellenőrzése, hogy a parancsfájl futásának indításakor.</span><span class="sxs-lookup"><span data-stu-id="49968-170">If the prompt still shows **>>**, Press Enter again to make sure that the script starts running.</span></span>

4. <span data-ttu-id="49968-171">Ismételje meg az előző lépést az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="49968-171">Repeat the preceding steps for each VM.</span></span>  
    <span data-ttu-id="49968-172">Ez a parancsfájl az IP-cím erőforrás a felhőalapú szolgáltatás IP-címmel konfigurálja, és paramétereinek más, mint a mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="49968-172">This script configures the IP address resource with the IP address of the cloud service and sets other parameters, such as the probe port.</span></span> <span data-ttu-id="49968-173">Az IP-cím erőforrás online állapotba kerül, ha azt is válaszol a mintavételi portot a lekérdezés a korábban létrehozott elosztott terhelésű végpont.</span><span class="sxs-lookup"><span data-stu-id="49968-173">When the IP address resource is brought online, it can respond to the polling on the probe port from the load-balanced endpoint that you created earlier.</span></span>

## <a name="bring-the-listener-online"></a><span data-ttu-id="49968-174">A figyelő hálózatra kapcsolása</span><span class="sxs-lookup"><span data-stu-id="49968-174">Bring the listener online</span></span>
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a><span data-ttu-id="49968-175">Követő műveletet elemek</span><span class="sxs-lookup"><span data-stu-id="49968-175">Follow-up items</span></span>
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-virtual-network"></a><span data-ttu-id="49968-176">A rendelkezésre állási csoport figyelőjét (belül az azonos virtuális hálózatban) tesztelése</span><span class="sxs-lookup"><span data-stu-id="49968-176">Test the availability group listener (within the same virtual network)</span></span>
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a><span data-ttu-id="49968-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49968-177">Next steps</span></span>
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
