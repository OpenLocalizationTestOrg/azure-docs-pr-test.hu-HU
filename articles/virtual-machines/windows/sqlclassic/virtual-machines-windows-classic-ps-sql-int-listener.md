---
title: "az Always On rendelkezésre állási csoportok az Azure-ban egy ILB figyelő aaaConfigure |} Microsoft Docs"
description: "Ez az oktatóanyag hello klasszikus telepítési modellel létrehozott erőforrást használ, és a belső terheléselosztót használó Azure-ban létrehoz egy Always On rendelkezésre állási csoport figyelőjének."
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
ms.openlocfilehash: 2ce9b64fea491c945b58f7641e41fd39d90b078a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a><span data-ttu-id="3914a-103">Egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3914a-103">Configure an ILB listener for Always On availability groups in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3914a-104">Belső figyelő</span><span class="sxs-lookup"><span data-stu-id="3914a-104">Internal listener</span></span>](../classic/ps-sql-int-listener.md)
> * [<span data-ttu-id="3914a-105">Külső figyelő</span><span class="sxs-lookup"><span data-stu-id="3914a-105">External listener</span></span>](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a><span data-ttu-id="3914a-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3914a-106">Overview</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3914a-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3914a-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3914a-108">Ez a cikk hello klasszikus üzembe helyezési modellel hello használatát ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3914a-108">This article covers hello use of hello classic deployment model.</span></span> <span data-ttu-id="3914a-109">Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="3914a-109">We recommend that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="3914a-110">egy Always On rendelkezésre állási csoport hello Resource Manager modellt, a figyelő tooconfigure lásd [egy terheléselosztót egy Always On rendelkezésre állási csoport konfigurálása az Azure-](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="3914a-110">tooconfigure a listener for an Always On availability group in hello Resource Manager model, see [Configure a load balancer for an Always On availability group in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

<span data-ttu-id="3914a-111">A rendelkezésre állási csoport replikák, amelyek csak a helyszíni vagy Azure csak, vagy a helyszíni és az Azure hibrid konfigurációk átnyúló tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="3914a-111">Your availability group can contain replicas that are on-premises only or Azure only, or that span both on-premises and Azure for hybrid configurations.</span></span> <span data-ttu-id="3914a-112">Az Azure replikák elhelyezkedhetnek hello belül azonos régióban vagy több virtuális hálózat használó különféle régiókban.</span><span class="sxs-lookup"><span data-stu-id="3914a-112">Azure replicas can reside within hello same region or across multiple regions that use multiple virtual networks.</span></span> <span data-ttu-id="3914a-113">hello ebben a cikkben szereplő eljárások azt feltételezik, hogy már rendelkezik [konfigurált rendelkezésre állási csoport](../classic/portal-sql-alwayson-availability-groups.md) , de még nincs konfigurálva egy figyelő.</span><span class="sxs-lookup"><span data-stu-id="3914a-113">hello procedures in this article assume that you have already [configured an availability group](../classic/portal-sql-alwayson-availability-groups.md) but have not yet configured a listener.</span></span>

## <a name="guidelines-and-limitations-for-internal-listeners"></a><span data-ttu-id="3914a-114">Tudnivalók és korlátozások belső figyelők</span><span class="sxs-lookup"><span data-stu-id="3914a-114">Guidelines and limitations for internal listeners</span></span>
<span data-ttu-id="3914a-115">hello egy belső terheléselosztón (ILB) egy rendelkezésre állási csoport figyelőjével, az Azure-ban használata tulajdonos toohello a következő irányelveket:</span><span class="sxs-lookup"><span data-stu-id="3914a-115">hello use of an internal load balancer (ILB) with an availability group listener in Azure is subject toohello following guidelines:</span></span>

* <span data-ttu-id="3914a-116">hello rendelkezésre állási csoport figyelőjének Windows Server 2008 R2, Windows Server 2012 és Windows Server 2012 R2 esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="3914a-116">hello availability group listener is supported on Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span>
* <span data-ttu-id="3914a-117">Csak egy belső rendelkezésre állási csoport figyelőjének támogatott minden felhőalapú szolgáltatás, mert a hello figyelő toohello ILB konfigurálva, és csak egy ILB minden felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3914a-117">Only one internal availability group listener is supported for each cloud service, because hello listener is configured toohello ILB, and there is only one ILB for each cloud service.</span></span> <span data-ttu-id="3914a-118">Lehetséges toocreate azonban több külső figyelők.</span><span class="sxs-lookup"><span data-stu-id="3914a-118">However, it is possible toocreate multiple external listeners.</span></span> <span data-ttu-id="3914a-119">További információkért lásd: [egy külső figyelőt a Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-ext-listener.md).</span><span class="sxs-lookup"><span data-stu-id="3914a-119">For more information, see [Configure an external listener for Always On availability groups in Azure](../classic/ps-sql-ext-listener.md).</span></span>

## <a name="determine-hello-accessibility-of-hello-listener"></a><span data-ttu-id="3914a-120">Határozza meg a hello kisegítő hello-figyelő</span><span class="sxs-lookup"><span data-stu-id="3914a-120">Determine hello accessibility of hello listener</span></span>
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

<span data-ttu-id="3914a-121">Ez a cikk foglalkozik, amely egy ILB használja egy figyelő létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="3914a-121">This article focuses on creating a listener that uses an ILB.</span></span> <span data-ttu-id="3914a-122">Ha egy nyilvános vagy külső figyelőt, lásd: Ez a cikk ismerteti, amelyek a beállítás mentése hello verziója egy [külső figyelő](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3914a-122">If you need an public or external listener, see hello version of this article that discusses setting up an [external listener](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a><span data-ttu-id="3914a-123">Hozzon létre virtuális gép elosztott terhelésű végpontok közvetlen kiszolgálói visszatérési</span><span class="sxs-lookup"><span data-stu-id="3914a-123">Create load-balanced VM endpoints with direct server return</span></span>
<span data-ttu-id="3914a-124">Először hozzon létre egy ILB hello parancsfájl futtatásával később ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="3914a-124">You first create an ILB by running hello script later in this section.</span></span>

<span data-ttu-id="3914a-125">Az egyes virtuális gépek az Azure-replikát tartalmazó elosztott terhelésű végpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3914a-125">Create a load-balanced endpoint for each VM that hosts an Azure replica.</span></span> <span data-ttu-id="3914a-126">Ha több régióba replikákat, minden egyes replikának régió nem lehet azonos a felhőalapú szolgáltatás a hello a hello azonos Azure virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="3914a-126">If you have replicas in multiple regions, each replica for that region must be in hello same cloud service in hello same Azure virtual network.</span></span> <span data-ttu-id="3914a-127">Rendelkezésre állási csoport replikái, amelyek több Azure-régiók több létrehozása több virtuális hálózat beállítását igényli.</span><span class="sxs-lookup"><span data-stu-id="3914a-127">Creating availability group replicas that span multiple Azure regions requires configuring multiple virtual networks.</span></span> <span data-ttu-id="3914a-128">Több virtuális hálózati kapcsolat konfigurálásával kapcsolatos további információkért lásd: [konfigurálja a virtuális hálózati toovirtual hálózati kapcsolat](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="3914a-128">For more information on configuring cross virtual network connectivity, see [Configure virtual network toovirtual network connectivity](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span>

1. <span data-ttu-id="3914a-129">A hello Azure-portálon válassza a tooeach virtuális gép, amelyen a replika tooview hello részletei.</span><span class="sxs-lookup"><span data-stu-id="3914a-129">In hello Azure portal, go tooeach VM that hosts a replica tooview hello details.</span></span>

2. <span data-ttu-id="3914a-130">Kattintson a hello **végpontok** lapon az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3914a-130">Click hello **Endpoints** tab for each VM.</span></span>

3. <span data-ttu-id="3914a-131">Győződjön meg arról, hogy hello **neve** és **nyilvános Port** hello figyelő végpont használni kívánt toouse nem már használatban van.</span><span class="sxs-lookup"><span data-stu-id="3914a-131">Verify that hello **Name** and **Public Port** of hello listener endpoint that you want toouse are not already in use.</span></span> <span data-ttu-id="3914a-132">Ebben a szakaszban hello példában hello értéke *MyEndpoint*, és hello port *1433*.</span><span class="sxs-lookup"><span data-stu-id="3914a-132">In hello example in this section, hello name is *MyEndpoint*, and hello port is *1433*.</span></span>

4. <span data-ttu-id="3914a-133">A helyi ügyfélen, töltse le és telepítse legújabb a hello [PowerShell modul](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="3914a-133">On your local client, download and install hello latest [PowerShell module](https://azure.microsoft.com/downloads/).</span></span>

5. <span data-ttu-id="3914a-134">Indítsa el az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3914a-134">Start Azure PowerShell.</span></span>  
    <span data-ttu-id="3914a-135">Egy új PowerShell-munkamenetet nyit meg, a hello Azure felügyeleti modulok terhelését.</span><span class="sxs-lookup"><span data-stu-id="3914a-135">A new PowerShell session opens, with hello Azure administrative modules loaded.</span></span>

6. <span data-ttu-id="3914a-136">Futtassa az `Get-AzurePublishSettingsFile` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3914a-136">Run `Get-AzurePublishSettingsFile`.</span></span> <span data-ttu-id="3914a-137">Ez a parancsmag tooa böngésző toodownload közzétételi beállítások fájl tooa helyi könyvtár irányítja.</span><span class="sxs-lookup"><span data-stu-id="3914a-137">This cmdlet directs you tooa browser toodownload a publish settings file tooa local directory.</span></span> <span data-ttu-id="3914a-138">Rendszer felajánlhatja a bejelentkezési hitelesítő adatok Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="3914a-138">You might be prompted for your sign-in credentials for your Azure subscription.</span></span>

7. <span data-ttu-id="3914a-139">Futtassa a következő hello `Import-AzurePublishSettingsFile` hello hello elérési parancs közzététele beállításfájl letöltött:</span><span class="sxs-lookup"><span data-stu-id="3914a-139">Run hello following `Import-AzurePublishSettingsFile` command with hello path of hello publish settings file that you downloaded:</span></span>

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    <span data-ttu-id="3914a-140">Miután hello közzététele beállításfájl importálása, kezelheti az Azure-előfizetéshez hello PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="3914a-140">After hello publish settings file is imported, you can manage your Azure subscription in hello PowerShell session.</span></span>

8. <span data-ttu-id="3914a-141">A *ILB*, statikus IP-cím.</span><span class="sxs-lookup"><span data-stu-id="3914a-141">For *ILB*, assign a static IP address.</span></span> <span data-ttu-id="3914a-142">Vizsgálja meg a hello jelenlegi virtuális hálózati konfiguráció hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3914a-142">Examine hello current virtual network configuration by running hello following command:</span></span>

        (Get-AzureVNetConfig).XMLConfiguration
9. <span data-ttu-id="3914a-143">Megjegyzés: hello *alhálózati* hello replika hello virtuális gépeket tartalmazó hello alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="3914a-143">Note hello *Subnet* name for hello subnet that contains hello VMs that host hello replicas.</span></span> <span data-ttu-id="3914a-144">Ez a név hello parancsfájlban hello $SubnetName paraméter szerepel.</span><span class="sxs-lookup"><span data-stu-id="3914a-144">This name is used in hello $SubnetName parameter in hello script.</span></span>

10. <span data-ttu-id="3914a-145">Megjegyzés: hello *VirtualNetworkSite* és a kezdési hello *címelőtagja* hello replika hello virtuális gépeket tartalmazó hello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="3914a-145">Note hello *VirtualNetworkSite* name and hello starting *AddressPrefix* for hello subnet that contains hello VMs that host hello replicas.</span></span> <span data-ttu-id="3914a-146">Keresse meg az elérhető IP-cím úgy, hogy mindkét értékek toohello `Test-AzureStaticVNetIP` parancsot, és úgy hello *AvailableAddresses*.</span><span class="sxs-lookup"><span data-stu-id="3914a-146">Look for an available IP address by passing both values toohello `Test-AzureStaticVNetIP` command and by examining hello *AvailableAddresses*.</span></span> <span data-ttu-id="3914a-147">Például, ha hello virtuális hálózat neve *MyVNet* , és a alhálózati címtartományt kezdődő *172.16.0.128*, hello következő parancsot a rendelkezésre álló címek volna listában:</span><span class="sxs-lookup"><span data-stu-id="3914a-147">For example, if hello virtual network is named *MyVNet* and has a subnet address range that starts at *172.16.0.128*, hello following command would list available addresses:</span></span>

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. <span data-ttu-id="3914a-148">Válasszon egyet az elérhető címek hello, és felhasználhatja őket a hello $ILBStaticIP paraméter hello parancsfájl hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="3914a-148">Select one of hello available addresses, and use it in hello $ILBStaticIP parameter of hello script in hello next step.</span></span>

12. <span data-ttu-id="3914a-149">A következő PowerShell-parancsfájl tooa szövegszerkesztőben hello másolja, és állítsa be a hello változók értékeinek toosuit a környezetben.</span><span class="sxs-lookup"><span data-stu-id="3914a-149">Copy hello following PowerShell script tooa text editor, and set hello variable values toosuit your environment.</span></span> <span data-ttu-id="3914a-150">Egyes paraméterek alapértelmezett adtak ki.</span><span class="sxs-lookup"><span data-stu-id="3914a-150">Defaults have been provided for some parameters.</span></span>  

    <span data-ttu-id="3914a-151">Meglévő affinitáscsoportok használó központi telepítések nem adható hozzá egy Példánynak.</span><span class="sxs-lookup"><span data-stu-id="3914a-151">Existing deployments that use affinity groups cannot add an ILB.</span></span> <span data-ttu-id="3914a-152">ILB követelményeivel kapcsolatos további információkért lásd: [belső load balancer áttekintése](../../../load-balancer/load-balancer-internal-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3914a-152">For more information about ILB requirements, see [Internal load balancer overview](../../../load-balancer/load-balancer-internal-overview.md).</span></span>

    <span data-ttu-id="3914a-153">Is ha a rendelkezésre állási csoport Azure-régiók kiterjedő, futtatnia kell hello parancsfájl egyszer minden adatközpontban hello felhőalapú szolgáltatás, és azt az adatközpontot lévő csomópontok.</span><span class="sxs-lookup"><span data-stu-id="3914a-153">Also, if your availability group spans Azure regions, you must run hello script once in each datacenter for hello cloud service and nodes that reside in that datacenter.</span></span>

        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that hello replicas use in hello virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for hello ILB in hello subnet
        $ILBName = "AGListenerLB" # customize hello ILB name or use this default value

        # Create hello ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. <span data-ttu-id="3914a-154">Hello változók beállítása után másolási hello parancsfájl hello text editor tooyour PowerShell munkamenetben toorun azt.</span><span class="sxs-lookup"><span data-stu-id="3914a-154">After you have set hello variables, copy hello script from hello text editor tooyour PowerShell session toorun it.</span></span> <span data-ttu-id="3914a-155">Ha még mindig hello kérdés  **>>** , nyomja le az Enter újra toomake meg arról, hogy hello parancsfájl futásának indításakor-e.</span><span class="sxs-lookup"><span data-stu-id="3914a-155">If hello prompt still shows **>>**, press Enter again toomake sure hello script starts running.</span></span>

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a><span data-ttu-id="3914a-156">Győződjön meg arról, hogy KB2854082 telepítve van-e, ha szükséges</span><span class="sxs-lookup"><span data-stu-id="3914a-156">Verify that KB2854082 is installed if necessary</span></span>
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a><span data-ttu-id="3914a-157">Nyissa meg a rendelkezésre állási csoport csomópontok hello tűzfalportok</span><span class="sxs-lookup"><span data-stu-id="3914a-157">Open hello firewall ports in availability group nodes</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a><span data-ttu-id="3914a-158">Hello rendelkezésre állási csoport figyelőjének létrehozása</span><span class="sxs-lookup"><span data-stu-id="3914a-158">Create hello availability group listener</span></span>

<span data-ttu-id="3914a-159">Hello rendelkezésre állási csoport figyelőjének létrehozása két lépésben.</span><span class="sxs-lookup"><span data-stu-id="3914a-159">Create hello availability group listener in two steps.</span></span> <span data-ttu-id="3914a-160">Először hello ügyfél hozzáférési pont fürterőforrás létrehozása, és konfigurálja a függőségek.</span><span class="sxs-lookup"><span data-stu-id="3914a-160">First, create hello client access point cluster resource and configure  dependencies.</span></span> <span data-ttu-id="3914a-161">Második konfigurálja a PowerShell hello fürt erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="3914a-161">Second, configure hello cluster resources in PowerShell.</span></span>

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a><span data-ttu-id="3914a-162">Ügyfél-hozzáférési pont hello létrehozása és konfigurálása hello fürt függőségek</span><span class="sxs-lookup"><span data-stu-id="3914a-162">Create hello client access point and configure hello cluster dependencies</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a><span data-ttu-id="3914a-163">A PowerShell hello fürterőforrások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3914a-163">Configure hello cluster resources in PowerShell</span></span>
1. <span data-ttu-id="3914a-164">A Példánynak korábban létrehozott ILB hello hello IP-címét kell használnia.</span><span class="sxs-lookup"><span data-stu-id="3914a-164">For ILB, you must use hello IP address of hello ILB that was created earlier.</span></span> <span data-ttu-id="3914a-165">az IP-címe a PowerShell, a következő parancsfájl használata hello tooobtain:</span><span class="sxs-lookup"><span data-stu-id="3914a-165">tooobtain this IP address in PowerShell, use hello following script:</span></span>

        # Define variables
        $ServiceName="<MyServiceName>" # hello name of hello cloud service that contains hello AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. <span data-ttu-id="3914a-166">Hello virtuális gépek egyik hello PowerShell parancsfájl másolása az operációs rendszer tooa szövegszerkesztőben, és korábban feljegyzett toohello értékek hello változók adja.</span><span class="sxs-lookup"><span data-stu-id="3914a-166">On one of hello VMs, copy hello PowerShell script for your operating system tooa text editor, and then set hello variables toohello values you noted earlier.</span></span>

    <span data-ttu-id="3914a-167">A Windows Server 2012 vagy újabb használja a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="3914a-167">For Windows Server 2012 or later, use hello following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    <span data-ttu-id="3914a-168">A Windows Server 2008 R2 a következő parancsfájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="3914a-168">For Windows Server 2008 R2, use hello following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. <span data-ttu-id="3914a-169">Miután set hello változók, nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban, Beillesztés hello parancsfájl hello szövegszerkesztőben a PowerShell-munkamenet toorun be azt.</span><span class="sxs-lookup"><span data-stu-id="3914a-169">After you have set hello variables, open an elevated Windows PowerShell window, paste hello script from hello text editor into your PowerShell session toorun it.</span></span> <span data-ttu-id="3914a-170">Ha még mindig hello kérdés  **>>** , nyomja le az Enter újra toomake meg arról, hogy hello parancsfájl futásának indításakor.</span><span class="sxs-lookup"><span data-stu-id="3914a-170">If hello prompt still shows **>>**, Press Enter again toomake sure that hello script starts running.</span></span>

4. <span data-ttu-id="3914a-171">Ismételje meg az előző lépésekben az egyes virtuális gépek hello.</span><span class="sxs-lookup"><span data-stu-id="3914a-171">Repeat hello preceding steps for each VM.</span></span>  
    <span data-ttu-id="3914a-172">Ez a parancsfájl hello IP-cím erőforrás hello felhőszolgáltatás hello IP-címmel konfigurálja, és beállítja a más paramétereket, például hello mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="3914a-172">This script configures hello IP address resource with hello IP address of hello cloud service and sets other parameters, such as hello probe port.</span></span> <span data-ttu-id="3914a-173">Hello IP-cím erőforrás online állapotba kerül, ha azt válaszolhassanak a hello mintavételi portot a hello elosztott terhelésű végpont korábban létrehozott lekérdezési toohello.</span><span class="sxs-lookup"><span data-stu-id="3914a-173">When hello IP address resource is brought online, it can respond toohello polling on hello probe port from hello load-balanced endpoint that you created earlier.</span></span>

## <a name="bring-hello-listener-online"></a><span data-ttu-id="3914a-174">Kapcsolja a hálózatra hello figyelő</span><span class="sxs-lookup"><span data-stu-id="3914a-174">Bring hello listener online</span></span>
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a><span data-ttu-id="3914a-175">Követő műveletet elemek</span><span class="sxs-lookup"><span data-stu-id="3914a-175">Follow-up items</span></span>
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-virtual-network"></a><span data-ttu-id="3914a-176">Teszt hello rendelkezésre állási csoport figyelőjét (belül hello azonos virtuális hálózat)</span><span class="sxs-lookup"><span data-stu-id="3914a-176">Test hello availability group listener (within hello same virtual network)</span></span>
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a><span data-ttu-id="3914a-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3914a-177">Next steps</span></span>
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
