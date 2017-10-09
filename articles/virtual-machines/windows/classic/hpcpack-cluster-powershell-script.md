---
title: "aaaPowerShell parancsfájl toodeploy Windows HPC-fürt |} Microsoft Docs"
description: "Az Azure virtuális gépeken a Windows HPC Pack 2012 R2-fürt futtatására a egy PowerShell-parancsfájl toodeploy"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 286b2be8-2533-40df-b02a-26156b1f1133
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 10ce1e9bc4e98954b955549bd72aaaf6106c69fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="9e0fb-103">Hozzon létre egy Windows nagy teljesítményű számítástechnikai rendszerek (HPC) fürtöt hello HPC Pack IaaS telepítési parancsfájl</span><span class="sxs-lookup"><span data-stu-id="9e0fb-103">Create a Windows high-performance computing (HPC) cluster with hello HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="9e0fb-104">Egy teljes HPC Pack 2012 R2-fürt Windows munkaterhelések az hello HPC Pack IaaS telepítési PowerShell parancsfájl toodeploy futtatása az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-104">Run hello HPC Pack IaaS deployment PowerShell script toodeploy a complete HPC Pack 2012 R2 cluster for Windows workloads in Azure virtual machines.</span></span> <span data-ttu-id="9e0fb-105">hello fürt tartalmaz egy Active Directory-tartományhoz átjárócsomópont Windows Server és a Microsoft HPC Pack fut, és további Windows számítási erőforrásokat, akkor adja meg.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-105">hello cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and additional Windows compute resources you specify.</span></span> <span data-ttu-id="9e0fb-106">Ha az Azure HPC Pack fürtöt toodeploy Linux munkaterhelésekhez, lásd: [hozzon létre egy Linux HPC-fürtöt hello HPC Pack IaaS telepítési parancsfájl](../../linux/classic/hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="9e0fb-106">If you want toodeploy an HPC Pack cluster in Azure for Linux workloads, see [Create a Linux HPC cluster with hello HPC Pack IaaS deployment script](../../linux/classic/hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="9e0fb-107">Az Azure Resource Manager sablon toodeploy egy HPC Pack fürthöz is használható.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-107">You can also use an Azure Resource Manager template toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="9e0fb-108">Tekintse meg a [HPC-fürt létrehozása](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) és [HPC-fürt létrehozása egyéni számítási csomópont képének](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span><span class="sxs-lookup"><span data-stu-id="9e0fb-108">For examples, see [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) and [Create an HPC cluster with a custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="9e0fb-109">hello ebben a cikkben ismertetett PowerShell-parancsfájl a klasszikus üzembe helyezési modellel hello Azure-ban létrehoz egy Microsoft HPC Pack 2012 R2 fürtöt.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-109">hello PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using hello classic deployment model.</span></span> <span data-ttu-id="9e0fb-110">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="9e0fb-111">Ebben a cikkben ismertetett hello parancsfájl emellett nem támogatja a HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-111">In addition, hello script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a><span data-ttu-id="9e0fb-112">Példa konfigurációs fájlok</span><span class="sxs-lookup"><span data-stu-id="9e0fb-112">Example configuration files</span></span>
<span data-ttu-id="9e0fb-113">Hello az alábbi példák a saját értékeit az előfizetés-azonosítóval vagy és hello fiók és a szolgáltatás nevének helyettesítse be.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-113">In hello following examples, substitute your own values for your subscription Id or name and hello account and service names.</span></span>

### <a name="example-1"></a><span data-ttu-id="9e0fb-114">1. példa</span><span class="sxs-lookup"><span data-stu-id="9e0fb-114">Example 1</span></span>
<span data-ttu-id="9e0fb-115">hello következő konfigurációs fájl telepíti egy HPC Pack fürt, amelyen egy helyi adatbázisok átjárócsomópont és öt számítási csomópontok hello Windows Server 2012 R2 operációs rendszert futtat.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-115">hello following configuration file deploys an HPC Pack cluster that has a head node with local databases and five compute nodes running hello Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="9e0fb-116">Összes hello felhőszolgáltatás közvetlenül hello USA nyugati régiója hely jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-116">All hello cloud services are created directly in hello West US location.</span></span> <span data-ttu-id="9e0fb-117">hello átjárócsomópont hello tartomány erdő tartományvezérlőjévé funkcionál.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-117">hello head node acts as domain controller of hello domain forest.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a><span data-ttu-id="9e0fb-118">2. példa</span><span class="sxs-lookup"><span data-stu-id="9e0fb-118">Example 2</span></span>
<span data-ttu-id="9e0fb-119">a következő konfigurációs fájl hello telepíti egy meglévő tartomány erdőben HPC Pack fürt.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-119">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="9e0fb-120">hello fürt 1 átjárócsomópont helyi adatbázisokkal rendelkezik, és 12 számítási csomópontokat a hello alkalmazott BGInfo Virtuálisgép-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-120">hello cluster has 1 head node with local databases and 12 compute nodes with hello BGInfo VM extension applied.</span></span>
<span data-ttu-id="9e0fb-121">Windows-frissítések automatikus telepítése a virtuális gépek hello tartomány erdő összes hello le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-121">Automatic installation of Windows updates is disabled for all hello VMs in hello domain forest.</span></span> <span data-ttu-id="9e0fb-122">Összes hello felhőszolgáltatás közvetlenül a Kelet-Ázsia hely jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-122">All hello cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="9e0fb-123">hello számítási csomópontok létrehozott három felhőszolgáltatások és három storage-fiókok: *MyHPCCN-0001* túl*MyHPCCN-0005* a *MyHPCCNService01* és  *mycnstorage01*; *MyHPCCN-0006* túl*MyHPCCN0010* a *MyHPCCNService02* és *mycnstorage02*; és  *MyHPCCN-0011* túl*MyHPCCN-0012* a *MyHPCCNService03* és *mycnstorage03*).</span><span class="sxs-lookup"><span data-stu-id="9e0fb-123">hello compute nodes are created in three cloud services and three storage accounts: *MyHPCCN-0001* too*MyHPCCN-0005* in *MyHPCCNService01* and *mycnstorage01*; *MyHPCCN-0006* too*MyHPCCN0010* in *MyHPCCNService02* and *mycnstorage02*; and *MyHPCCN-0011* too*MyHPCCN-0012* in *MyHPCCNService03* and *mycnstorage03*).</span></span> <span data-ttu-id="9e0fb-124">hello számítási csomópontokat, a számítási csomópont rögzítése meglévő titkos lemezképpel jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-124">hello compute nodes are created from an existing private image captured from a compute node.</span></span> <span data-ttu-id="9e0fb-125">hello automatikus növelhető vagy csökkenthető az engedélyezve van az alapértelmezett növelhető vagy csökkenthető a időközönként.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-125">hello auto grow and shrink service is enabled with default grow and shrink intervals.</span></span>

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a><span data-ttu-id="9e0fb-126">3. példa</span><span class="sxs-lookup"><span data-stu-id="9e0fb-126">Example 3</span></span>
<span data-ttu-id="9e0fb-127">a következő konfigurációs fájl hello telepíti egy meglévő tartomány erdőben HPC Pack fürt.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-127">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="9e0fb-128">hello fürt tartalmaz egy átjárócsomópont, egy adatbázis-kiszolgálón, 500 GB adatlemezt, hello Windows Server 2012 R2 operációs rendszer, és öt számítási csomópontok hello Windows Server 2012 R2 operációs rendszert futtató két csomópontok.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-128">hello cluster contains one head node, one database server with a 500 GB data disk, two broker nodes running hello Windows Server 2012 R2 operating system, and five compute nodes running hello Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="9e0fb-129">hello MyHPCCNService hello affinitáscsoportban jön létre a felhőalapú szolgáltatás *MyIBAffinityGroup*, és hello felhőszolgáltatásokra jönnek létre hello affinitáscsoportban *MyAffinityGroup*.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-129">hello cloud service MyHPCCNService is created in hello affinity group *MyIBAffinityGroup*, and hello other cloud services are created in hello affinity group *MyAffinityGroup*.</span></span> <span data-ttu-id="9e0fb-130">hello HPC feladat ütemezési REST API-t és a HPC-webportál hello átjárócsomópont engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-130">hello HPC Job Scheduler REST API and HPC web portal are enabled on hello head node.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a><span data-ttu-id="9e0fb-131">4. példa</span><span class="sxs-lookup"><span data-stu-id="9e0fb-131">Example 4</span></span>
<span data-ttu-id="9e0fb-132">a következő konfigurációs fájl hello telepíti egy meglévő tartomány erdőben HPC Pack fürt.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-132">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="9e0fb-133">helyi adatbázisok két központi csomóponttal rendelkező hello fürt, két Azure csomópont sablonok létrehozása és három mérete közepes Azure csomópontot hoz létre Azure csomópontsablonhoz *AzureTemplate1*.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-133">hello cluster has two head node with local databases, two Azure node templates are created, and three size Medium Azure nodes are created for Azure node template *AzureTemplate1*.</span></span> <span data-ttu-id="9e0fb-134">A parancsfájl futó hello átjárócsomópont hello átjárócsomópont konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-134">A script file runs on hello head node after hello head node is configured.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a><span data-ttu-id="9e0fb-135">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9e0fb-135">Troubleshooting</span></span>
* <span data-ttu-id="9e0fb-136">**"A virtuális hálózat nem létezik" hiba** -futtatásakor hello parancsfájl toodeploy több fürt az Azure-ban egyszerre egy előfizetéshez tartozó, egy vagy több üzemelő példány hello hiba miatt sikertelen lehet "VNet *VNet\_neve* nem a létező".</span><span class="sxs-lookup"><span data-stu-id="9e0fb-136">**“VNet doesn’t exist” error** - If you run hello script toodeploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with hello error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="9e0fb-137">Ez a hiba akkor fordul elő, ha parancsprogrammal hello újra a sikertelen hello telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-137">If this error occurs, run hello script again for hello failed deployment.</span></span>
* <span data-ttu-id="9e0fb-138">**Az Azure-beli virtuális hálózat hello Internet probléma elérése hello** – Ha hoz létre egy fürtöt az új tartományvezérlő hello telepítési parancsfájlt, vagy manuálisan főkiszolgálóvá előléptetni egy átjárócsomóponttal VM toodomain vezérlő, problémák hello virtuális gépek toohello internethez kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-138">**Problem accessing hello Internet from hello Azure virtual network** - If you create a cluster with a new domain controller by using hello deployment script, or you manually promote a head node VM toodomain controller, you may experience problems connecting hello VMs toohello Internet.</span></span> <span data-ttu-id="9e0fb-139">Ez a probléma akkor fordulhat elő, ha továbbító DNS-kiszolgáló automatikusan hello tartományvezérlőn van konfigurálva, és a továbbító DNS-kiszolgáló nem oldja meg megfelelően.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-139">This problem can occur if a forwarder DNS server is automatically configured on hello domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="9e0fb-140">a probléma megoldásához toowork jelentkezzen be a toohello tartományvezérlő és vagy eltávolítása hello továbbító konfigurációs beállítás, vagy egy érvényes továbbító DNS-kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-140">toowork around this problem, log on toohello domain controller and either remove hello forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="9e0fb-141">Ezt a beállítást, a Kiszolgálókezelőben kattintson tooconfigure **eszközök** >
    **DNS** tooopen DNS-kezelőben, majd kattintson duplán **továbbítók**.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-141">tooconfigure this setting, in Server Manager click **Tools** >
    **DNS** tooopen DNS Manager, and then double-click **Forwarders**.</span></span>
* <span data-ttu-id="9e0fb-142">**RDMA hálózati elérésekor számítási igényű virtuális gépek** –, ha ad hozzá a Windows Server számítási csomópont használja az RDMA-kompatibilisek-e virtuális gépek méretezés például A8 és A9, vagy e virtuális gépek toohello RDMA alkalmazás hálózati csatlakozás problémák.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-142">**Problem accessing RDMA network from compute-intensive VMs** - If you add Windows Server compute or broker node VMs using an RDMA-capable size such as A8 or A9, you may experience problems connecting those VMs toohello RDMA application network.</span></span> <span data-ttu-id="9e0fb-143">Ez a probléma akkor fordul elő, akkor ha hello HpcVmDrivers kiterjesztése nincs megfelelően telepítve hello virtuális gépek hozzáadásakor toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-143">One reason this problem occurs is if hello HpcVmDrivers extension is not properly installed when hello VMs are added toohello cluster.</span></span> <span data-ttu-id="9e0fb-144">Például a bővítmény előfordulhat, hogy ragadt; ebben az állapotban telepítése hello.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-144">For example, the extension might be stuck in hello installing state.</span></span>
  
    <span data-ttu-id="9e0fb-145">a probléma megoldásához, első hello állapotának hello virtuális gépek hello bővítmény toowork.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-145">toowork around this problem, first check hello state of hello extension in hello VMs.</span></span> <span data-ttu-id="9e0fb-146">Hello bővítmény nem megfelelően van telepítve, ha próbálja meg eltávolítani a hello csomópontok hello HPC-fürtből, és majd újból vegye fel hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-146">If hello extension is not properly installed, try removing hello nodes from hello HPC cluster and then add hello nodes again.</span></span> <span data-ttu-id="9e0fb-147">Számítási csomópont virtuális gépek például hello átjárócsomópont hello Add-HpcIaaSNode.ps1 parancsfájl futtatásával is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-147">For example, you can add compute node VMs by running hello Add-HpcIaaSNode.ps1 script on hello head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e0fb-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9e0fb-148">Next steps</span></span>
* <span data-ttu-id="9e0fb-149">Futtassa a test-munkaterhelések hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-149">Try running a test workload on hello cluster.</span></span> <span data-ttu-id="9e0fb-150">Egy vonatkozó példáért lásd: hello HPC Pack [– első lépések útmutató](https://technet.microsoft.com/library/jj884144).</span><span class="sxs-lookup"><span data-stu-id="9e0fb-150">For an example, see hello HPC Pack [getting started guide](https://technet.microsoft.com/library/jj884144).</span></span>
* <span data-ttu-id="9e0fb-151">Egy oktatóanyag tooscript hello fürttelepítést, és futtassa a HPC-munkaterhelés, a következő témakörben: [Ismerkedés az Azure toorun Excel és a SOA munkaterhelések HPC Pack fürtöt a](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9e0fb-151">For a tutorial tooscript hello cluster deployment and run an HPC workload, see [Get started with an HPC Pack cluster in Azure toorun Excel and SOA workloads](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="9e0fb-152">HPC Pack eszközök toostart próbálja, állítsa le, vegye fel és számítási csomópontok eltávolítása egy fürtről hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9e0fb-152">Try HPC Pack's tools toostart, stop, add, and remove compute nodes from a cluster you create.</span></span> <span data-ttu-id="9e0fb-153">Lásd: [kezelése számítási csomópontok HPC csomagban fürtön, az Azure-ban](hpcpack-cluster-node-manage.md).</span><span class="sxs-lookup"><span data-stu-id="9e0fb-153">See [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md).</span></span>
* <span data-ttu-id="9e0fb-154">tooget toosubmit feladatok toohello fürt beállításához a helyi számítógépről, tekintse meg [egy helyi számítógép tooan HPC Pack-feladatok HPC küldje el a fürt az Azure-ban](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9e0fb-154">tooget set up toosubmit jobs toohello cluster from a local computer, see [Submit HPC jobs from an on-premises computer tooan HPC Pack cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

