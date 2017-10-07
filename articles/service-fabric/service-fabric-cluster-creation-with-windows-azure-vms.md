---
title: "Azure virtuális gépeken futó Windows-fürtöt aaaCreate önálló |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és kezelése az Azure Service Fabric-fürt Windows Server rendszert futtató Azure virtuális gépeken."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 7eeb40d2-fb22-4a77-80ca-f1b46b22edbc
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: ryanwi;chackdan
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: 8900204fe69887a7a0ca54b06e0d32534421bcfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a><span data-ttu-id="e5dfb-103">A három csomópont különálló Service Fabric-fürt létrehozása a Windows Server rendszert futtató Azure virtuális gépekkel</span><span class="sxs-lookup"><span data-stu-id="e5dfb-103">Create a three node standalone Service Fabric cluster with Azure virtual machines running Windows Server</span></span>
<span data-ttu-id="e5dfb-104">Ez a cikk ismerteti, hogyan toocreate a fürtben, a Windows-alapú Azure virtuális gépek (VM), használatával hello a különálló Service Fabric telepítő a Windows Server.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-104">This article describes how toocreate a cluster on Windows-based Azure virtual machines (VMs), using hello standalone Service Fabric installer for Windows Server.</span></span> <span data-ttu-id="e5dfb-105">hello forgatókönyv egy különleges esetben a rendszer [létrehozása és kezelése Windows Server rendszert futtató fürtre](service-fabric-cluster-creation-for-windows-server.md) hello virtuális gépek esetén [Windows Server rendszert futtató Azure virtuális gépek](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), azonban nem hozza létre [egy Azure Service Fabric-fürt felhőalapú](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e5dfb-105">hello scenario is a special case of [Create and manage a cluster running on Windows Server](service-fabric-cluster-creation-for-windows-server.md) where hello VMs are [Azure VMs running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), however you are not creating [an Azure cloud-based Service Fabric cluster](service-fabric-cluster-creation-via-portal.md).</span></span> <span data-ttu-id="e5dfb-106">az ebben a mintában a következő hello különbség, hogy hello különálló Service Fabric-fürt hozta létre a következő lépéseket hello teljes mértékben Ön kezeli, mivel hello Azure felhőalapú Service Fabric-fürtök által kezelt és frissíteni a Service Fabric hello erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-106">hello distinction in following this pattern is that hello standalone Service Fabric cluster created by hello following steps is entirely managed by you, whereas hello Azure cloud-based Service Fabric clusters are managed and upgraded by hello Service Fabric resource provider.</span></span>

## <a name="steps-toocreate-hello-standalone-cluster"></a><span data-ttu-id="e5dfb-107">Lépéseket toocreate hello önálló fürthöz</span><span class="sxs-lookup"><span data-stu-id="e5dfb-107">Steps toocreate hello standalone cluster</span></span>
1. <span data-ttu-id="e5dfb-108">Jelentkezzen be Azure-portálon toohello, és hozzon létre egy új Windows Server 2012 R2 Datacenter vagy a Windows Server 2016 Datacenter VM egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-108">Sign in toohello Azure portal and create a new Windows Server 2012 R2 Datacenter or Windows Server 2016 Datacenter VM in a resource group.</span></span> <span data-ttu-id="e5dfb-109">A cikk elolvasása hello [Windows virtuális gép létrehozása az Azure-portálon hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-109">Read hello article [Create a Windows VM in hello Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for more details.</span></span>
2. <span data-ttu-id="e5dfb-110">Vegye fel néhány további virtuális gépek toohello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-110">Add a couple more VMs toohello same resource group.</span></span> <span data-ttu-id="e5dfb-111">Győződjön meg arról, hogy egyes virtuális gépek hello rendelkezik hello azonos rendszergazdai felhasználónév és jelszó létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-111">Ensure that each of hello VMs has hello same administrator user name and password when created.</span></span> <span data-ttu-id="e5dfb-112">Létrehozás után megtekintheti az összes három virtuális hello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-112">Once created you should see all three VMs in hello same virtual network.</span></span>
3. <span data-ttu-id="e5dfb-113">Csatlakozás a virtuális gépek hello tooeach, és kikapcsolni hello hello használata a Windows tűzfal [Kiszolgálókezelő, a helyi kiszolgáló irányítópult](https://technet.microsoft.com/library/jj134147.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5dfb-113">Connect tooeach of hello VMs and turn off hello Windows Firewall using hello [Server Manager, Local Server dashboard](https://technet.microsoft.com/library/jj134147.aspx).</span></span> <span data-ttu-id="e5dfb-114">Ez biztosítja, hello hálózati forgalom képes kommunikálni a hello gépek között.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-114">This ensures that hello network traffic can communicate between hello machines.</span></span> <span data-ttu-id="e5dfb-115">Csatlakoztatott tooeach a számítógépen, miközben hello IP-cím beolvasása nyisson meg egy parancssort, és írja be `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-115">While connected tooeach machine, get hello IP address by opening a command prompt and typing `ipconfig`.</span></span> <span data-ttu-id="e5dfb-116">Másik lehetőségként láthatja hello IP cím az egyes gépek hello Portal hello virtuális hálózati erőforrás hello erőforráscsoport kiválasztásával, és létre minden egyes ezeknek a gépeknek hello hálózati kapcsolatok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-116">Alternatively you can see hello IP address of each machine on hello portal, by selecting hello virtual network resource for hello resource group and checking hello network interfaces created for each of these machines.</span></span>
4. <span data-ttu-id="e5dfb-117">Csatlakoztassa a virtuális gépek hello tooone, és tesztelje, hogy pingelhető hello a többi két virtuális gép sikeresen.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-117">Connect tooone of hello VMs and test that you can ping hello other two VMs successfully.</span></span>
5. <span data-ttu-id="e5dfb-118">Csatlakozás a virtuális gépek hello tooone és [hello különálló Service Fabric-csomag letöltése a Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) hello egy új mappájába számítógéphez, és bontsa ki a hello csomag.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-118">Connect tooone of hello VMs and [download hello standalone Service Fabric package for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) into a new folder on hello machine and extract hello package.</span></span>
6. <span data-ttu-id="e5dfb-119">Nyissa meg hello *ClusterConfig.Unsecure.MultiMachine.json* fájlt a Jegyzettömbben, és hello gépek hello három IP-címét az egyes csomópontok szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-119">Open hello *ClusterConfig.Unsecure.MultiMachine.json* file in Notepad and edit each node with hello three IP addresses of hello machines.</span></span> <span data-ttu-id="e5dfb-120">Módosítsa hello felső hello fürt nevét, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-120">Change hello cluster name at hello top and save hello file.</span></span>  <span data-ttu-id="e5dfb-121">Hello fürtjegyzékben részleges példát alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-121">A partial example of hello cluster manifest is shown below.</span></span>
   
    ```
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "01-2017",
        "nodes": [
        {
            "nodeName": "standalonenode0",
            "iPAddress": "10.1.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "standalonenode1",
            "iPAddress": "10.1.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "standalonenode2",
            "iPAddress": "10.1.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
        ],
    ```
7. <span data-ttu-id="e5dfb-122">Ha azt tervezi, hogy a biztonságos fürt toobe, döntse el, hello biztonsági intézkedés kívánja toouse, majd hajtsa végre a hello hello készítésével kapcsolatos hivatkozás: [X509 tanúsítvány](service-fabric-windows-cluster-x509-security.md) vagy [Windows biztonsági](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="e5dfb-122">If you intend this toobe a secure cluster, decide hello security measure you would like toouse and follow hello steps at hello associated link: [X509 Certificate](service-fabric-windows-cluster-x509-security.md) or [Windows Security](service-fabric-windows-cluster-windows-security.md).</span></span> <span data-ttu-id="e5dfb-123">Ha használja a Windows biztonsági hello fürt beállítása, szüksége lesz a tooset be egy tartomány tartományvezérlői toomanage Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-123">If setting up hello cluster using Windows Security, you will need tooset up a domain controller toomanage Active Directory.</span></span> <span data-ttu-id="e5dfb-124">Vegye figyelembe, hogy egy tartomány tartományvezérlői gép használja, mint a Service Fabric csomópont nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-124">Note that using a domain controller machine as a Service Fabric node is not supported.</span></span>
8. <span data-ttu-id="e5dfb-125">Nyissa meg a [a PowerShell ISE ablaka](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span><span class="sxs-lookup"><span data-stu-id="e5dfb-125">Open a [PowerShell ISE window](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span></span> <span data-ttu-id="e5dfb-126">Keresse meg a toohello mappát, amelyben kibontotta hello letöltött önálló telepítő csomag és hello fürt konfigurációs fájlt mentette.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-126">Navigate toohello folder where you extracted hello downloaded standalone installer package and saved hello cluster configuration file.</span></span> <span data-ttu-id="e5dfb-127">Futtassa a következő PowerShell parancs toodeploy hello fürt hello:</span><span class="sxs-lookup"><span data-stu-id="e5dfb-127">Run hello following PowerShell command toodeploy hello cluster:</span></span>
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

<span data-ttu-id="e5dfb-128">hello parancsfájl hello Service Fabric-fürt távolról konfigurálja, és kell jelentse az előrehaladást, a központi telepítés áthalad.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-128">hello script will remotely configure hello Service Fabric cluster and should report progress as deployment rolls through.</span></span>

9. <span data-ttu-id="e5dfb-129">Után körülbelül egy perce, ellenőrizheti, ha hello fürt működőképességét toohello csatlakoztatásával Service Fabric Explorer használatával hello gép IP-címek, például a `http://10.1.0.5:19080/Explorer/index.html`.</span><span class="sxs-lookup"><span data-stu-id="e5dfb-129">After about a minute, you can check if hello cluster is operational by connecting toohello Service Fabric Explorer using one of hello machine's IP addresses, for example by using `http://10.1.0.5:19080/Explorer/index.html`.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e5dfb-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5dfb-130">Next steps</span></span>
* [<span data-ttu-id="e5dfb-131">Önálló Service Fabric-fürtök létrehozása Windows Server vagy Linux rendszerű gépeken</span><span class="sxs-lookup"><span data-stu-id="e5dfb-131">Create standalone Service Fabric clusters on Windows Server or Linux</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="e5dfb-132">Hozzáadása vagy eltávolítása, csomópontok tooa különálló Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="e5dfb-132">Add or remove nodes tooa standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="e5dfb-133">Önálló Windows-fürt konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="e5dfb-133">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="e5dfb-134">Biztonságos Windows használja-e a Windows biztonsági önálló fürtben</span><span class="sxs-lookup"><span data-stu-id="e5dfb-134">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="e5dfb-135">Biztonságos Windows X509 használata önálló fürtben, tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="e5dfb-135">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

