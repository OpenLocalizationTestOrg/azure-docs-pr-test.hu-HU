---
title: "aaaPowerShell parancsfájl toodeploy Linux HPC-fürt |} Microsoft Docs"
description: "Az Azure virtuális gépeken Linux HPC Pack 2012 R2-fürt futtatni egy PowerShell-parancsfájl toodeploy"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 73041960-58d3-4ecf-9540-d7e1a612c467
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 885b03fa2fd604827dc388803fc21debab730979
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="c2df4-103">Hozzon létre egy Linux nagy teljesítményű számítástechnikai rendszerek (HPC) fürtöt hello HPC Pack IaaS telepítési parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c2df4-103">Create a Linux high-performance computing (HPC) cluster with hello HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="c2df4-104">Egy teljes HPC Pack 2012 R2-fürt Linux munkaterhelések az hello HPC Pack IaaS telepítési PowerShell parancsfájl toodeploy futtassa az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="c2df4-104">Run hello HPC Pack IaaS deployment PowerShell script toodeploy a complete HPC Pack 2012 R2 cluster for Linux workloads in Azure virtual machines.</span></span> <span data-ttu-id="c2df4-105">hello fürt egy Active Directory-tartományhoz átjárócsomópont Windows Server és a Microsoft HPC Pack fut, és a számítási csomópontok valamelyikét futtató hello HPC Pack által támogatott Linux terjesztésekről áll.</span><span class="sxs-lookup"><span data-stu-id="c2df4-105">hello cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and compute nodes that run one of hello Linux distributions supported by HPC Pack.</span></span> <span data-ttu-id="c2df4-106">Ha azt szeretné, hogy az a Windows Azure munkaterhelések HPC Pack fürtöt toodeploy, [hozzon létre egy Windows HPC-fürtöt hello HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c2df4-106">If you want toodeploy an HPC Pack cluster in Azure for Windows workloads, see [Create a Windows HPC cluster with hello HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="c2df4-107">Az Azure Resource Manager sablon toodeploy egy HPC Pack fürthöz is használható.</span><span class="sxs-lookup"><span data-stu-id="c2df4-107">You can also use an Azure Resource Manager template toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="c2df4-108">Egy vonatkozó példáért lásd: [HPC-fürt létrehozása Linux számítási csomópontok](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span><span class="sxs-lookup"><span data-stu-id="c2df4-108">For an example, see [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c2df4-109">hello ebben a cikkben ismertetett PowerShell-parancsfájl a klasszikus üzembe helyezési modellel hello Azure-ban létrehoz egy Microsoft HPC Pack 2012 R2 fürtöt.</span><span class="sxs-lookup"><span data-stu-id="c2df4-109">hello PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using hello classic deployment model.</span></span> <span data-ttu-id="c2df4-110">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="c2df4-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="c2df4-111">Ebben a cikkben ismertetett hello parancsfájl emellett nem támogatja a HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="c2df4-111">In addition, hello script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a><span data-ttu-id="c2df4-112">Példa konfigurációs fájlt</span><span class="sxs-lookup"><span data-stu-id="c2df4-112">Example configuration file</span></span>
<span data-ttu-id="c2df4-113">hello következő konfigurációs fájl egy tartományvezérlő és a tartomány erdő hoz létre és telepít egy HPC Pack fürt, amelynek helyi adatbázisok egy átjárócsomópont és 10 Linux számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="c2df4-113">hello following configuration file creates a domain controller and domain forest and deploys an HPC Pack cluster which has one head node with local databases and 10 Linux compute nodes.</span></span> <span data-ttu-id="c2df4-114">Összes hello felhőszolgáltatás közvetlenül hello Kelet-Ázsia hely jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="c2df4-114">All hello cloud services are created directly in hello East Asia location.</span></span> <span data-ttu-id="c2df4-115">hello Linux számítási csomópontok jönnek létre két felhőalapú szolgáltatások és a két storage-fiókok (Ez azt jelenti, hogy *MyLnxCN-0001* való *MyLnxCN-0005* a *MyLnxCNService01* és *mylnxstorage01*, és *MyLnxCN-0006* való *MyLnxCN-0010* a *MyLnxCNService02* és *mylnxstorage02* ).</span><span class="sxs-lookup"><span data-stu-id="c2df4-115">hello Linux compute nodes are created in two cloud services and two storage accounts (that is, *MyLnxCN-0001* to *MyLnxCN-0005* in *MyLnxCNService01* and *mylnxstorage01*, and *MyLnxCN-0006* to *MyLnxCN-0010* in *MyLnxCNService02* and *mylnxstorage02*).</span></span> <span data-ttu-id="c2df4-116">OpenLogic CentOS 7.0 verzió Linux lemezképből hello számítási csomópontok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="c2df4-116">hello compute nodes are created from an OpenLogic CentOS version 7.0 Linux image.</span></span> 

<span data-ttu-id="c2df4-117">Helyettesítse a saját értékeit az előfizetés nevét és az hello fiók és a szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="c2df4-117">Substitute your own values for your subscription name and hello account and service names.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a><span data-ttu-id="c2df4-118">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="c2df4-118">Troubleshooting</span></span>
* <span data-ttu-id="c2df4-119">**"A virtuális hálózat nem létezik" hiba**.</span><span class="sxs-lookup"><span data-stu-id="c2df4-119">**“VNet doesn’t exist” error**.</span></span> <span data-ttu-id="c2df4-120">Ha hello HPC Pack IaaS telepítési parancsfájl toodeploy több fürt az Azure-ban egyszerre egy előfizetéshez tartozó, egy vagy több üzemelő példány hello hiba miatt sikertelen lehet "VNet *VNet\_neve* nem létezik".</span><span class="sxs-lookup"><span data-stu-id="c2df4-120">If you run hello HPC Pack IaaS deployment script toodeploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with hello error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="c2df4-121">Ha ez a hiba akkor fordul elő, futtassa újra a hello parancsfájl nem tudta hello központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="c2df4-121">If this error occurs, rerun hello script for hello failed deployment.</span></span>
* <span data-ttu-id="c2df4-122">**Az Azure-beli virtuális hálózat hello Internet probléma elérése hello**.</span><span class="sxs-lookup"><span data-stu-id="c2df4-122">**Problem accessing hello Internet from hello Azure virtual network**.</span></span> <span data-ttu-id="c2df4-123">HPC Pack-fürtöt hoz létre az új tartományvezérlő hello telepítési parancsfájl használatával, vagy manuálisan főkiszolgálóvá előléptetni egy átjárócsomóponttal VM toodomain vezérlő, ha problémába ütközik a virtuális gépek hello hello Azure-beli virtuális hálózat toohello Internet tapasztalhatja.</span><span class="sxs-lookup"><span data-stu-id="c2df4-123">If you create an HPC Pack cluster with a new domain controller by using hello deployment script, or you manually promote a head node VM toodomain controller, you may experience problems connecting hello VMs in hello Azure virtual network toohello Internet.</span></span> <span data-ttu-id="c2df4-124">Ez akkor fordulhat elő, ha egy továbbító DNS-kiszolgáló automatikusan hello tartományvezérlőn van konfigurálva, és a továbbító DNS-kiszolgáló nem oldja meg megfelelően.</span><span class="sxs-lookup"><span data-stu-id="c2df4-124">This can occur if a forwarder DNS server is automatically configured on hello domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="c2df4-125">a probléma megoldásához toowork jelentkezzen be a toohello tartományvezérlő és vagy eltávolítása hello továbbító konfigurációs beállítás, vagy egy érvényes továbbító DNS-kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c2df4-125">toowork around this problem, log on toohello domain controller and either remove hello forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="c2df4-126">Kattintson a Kiszolgálókezelő toodo **eszközök** > **DNS** tooopen DNS-kezelőben, majd kattintson duplán **továbbítók**.</span><span class="sxs-lookup"><span data-stu-id="c2df4-126">toodo this, in Server Manager click **Tools** > **DNS** tooopen DNS Manager, and then double-click **Forwarders**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2df4-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2df4-127">Next steps</span></span>
* <span data-ttu-id="c2df4-128">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) támogatott Linux disztribúciókról kapcsolatos információkért megköveteli az adatok, és a feladatok tooan HPC Pack fürt Linux elküldése számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="c2df4-128">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for information about supported Linux distributions, moving data, and submitting jobs tooan HPC Pack cluster with Linux compute nodes.</span></span>
* <span data-ttu-id="c2df4-129">Oktatóprogramot kínál, amelyek hello parancsfájl toocreate egy fürt használja, és futtassa a Linux HPC munkaterhelés lásd:</span><span class="sxs-lookup"><span data-stu-id="c2df4-129">For tutorials that use hello script toocreate a cluster and run a Linux HPC workload, see:</span></span>
  * [<span data-ttu-id="c2df4-130">A Microsoft HPC Pack NAMD futó Linux számítási csomópontok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c2df4-130">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
  * [<span data-ttu-id="c2df4-131">A Microsoft HPC Pack OpenFOAM futó Linux számítási csomópontok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c2df4-131">Run OpenFOAM with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-openfoam.md)
  * [<span data-ttu-id="c2df4-132">Futtassa a csillag-CCM + Microsoft HPC Pack Linux számítási csomópontok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c2df4-132">Run STAR-CCM+ with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-starccm.md)

