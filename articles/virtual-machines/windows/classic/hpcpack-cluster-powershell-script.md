---
title: "PowerShell parancsfájl központi telepítése a Windows HPC-fürt |} Microsoft Docs"
description: "Az Azure virtuális gépeken a Windows HPC Pack 2012 R2-fürt üzembe PowerShell parancsfájl futtatása"
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
ms.openlocfilehash: 85b125ab19671b61d2541af6378c95feb88bf952
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="2fb5a-103">Hozzon létre egy Windows-fürt nagy teljesítményű számítástechnikai rendszerek (HPC) a HPC Pack IaaS telepítési parancsfájl</span><span class="sxs-lookup"><span data-stu-id="2fb5a-103">Create a Windows high-performance computing (HPC) cluster with the HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="2fb5a-104">A HPC Pack IaaS központi telepítés központi telepítése az Azure virtuális gépeken a Windows-munkaterhelések teljes HPC Pack 2012 R2 fürt PowerShell-parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-104">Run the HPC Pack IaaS deployment PowerShell script to deploy a complete HPC Pack 2012 R2 cluster for Windows workloads in Azure virtual machines.</span></span> <span data-ttu-id="2fb5a-105">A fürt tartalmaz egy Active Directory-tartományhoz átjárócsomópont Windows Server és a Microsoft HPC Pack fut, és további Windows számítási erőforrásokat, akkor adja meg.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-105">The cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and additional Windows compute resources you specify.</span></span> <span data-ttu-id="2fb5a-106">Ha szeretné HPC Pack-fürt üzembe helyezése az Azure Linux munkaterhelésekhez, lásd: [hozzon létre egy Linux HPC-fürtöt a HPC Pack IaaS telepítési parancsfájl](../../linux/classic/hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="2fb5a-106">If you want to deploy an HPC Pack cluster in Azure for Linux workloads, see [Create a Linux HPC cluster with the HPC Pack IaaS deployment script](../../linux/classic/hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="2fb5a-107">Az Azure Resource Manager-sablon segítségével is HPC Pack-fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-107">You can also use an Azure Resource Manager template to deploy an HPC Pack cluster.</span></span> <span data-ttu-id="2fb5a-108">Tekintse meg a [HPC-fürt létrehozása](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) és [HPC-fürt létrehozása egyéni számítási csomópont képének](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span><span class="sxs-lookup"><span data-stu-id="2fb5a-108">For examples, see [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) and [Create an HPC cluster with a custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="2fb5a-109">A jelen cikkben ismertetett PowerShell-parancsfájl az Azure-ban a klasszikus üzembe helyezési modellel hoz létre a Microsoft HPC Pack 2012 R2-fürt.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-109">The PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using the classic deployment model.</span></span> <span data-ttu-id="2fb5a-110">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> <span data-ttu-id="2fb5a-111">A jelen cikkben ismertetett parancsfájl emellett nem támogatja a HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-111">In addition, the script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a><span data-ttu-id="2fb5a-112">Példa konfigurációs fájlok</span><span class="sxs-lookup"><span data-stu-id="2fb5a-112">Example configuration files</span></span>
<span data-ttu-id="2fb5a-113">A következő példákban helyettesítse a saját értékeit az előfizetés-azonosítóval vagy és a fiók és a szolgáltatás nevének.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-113">In the following examples, substitute your own values for your subscription Id or name and the account and service names.</span></span>

### <a name="example-1"></a><span data-ttu-id="2fb5a-114">1. példa</span><span class="sxs-lookup"><span data-stu-id="2fb5a-114">Example 1</span></span>
<span data-ttu-id="2fb5a-115">A következő konfigurációs fájl egy HPC Pack fürt, amelyen egy helyi adatbázisok átjárócsomópont telepíti, és öt számítási csomópontok a Windows Server 2012 R2 operációs rendszert futtat.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-115">The following configuration file deploys an HPC Pack cluster that has a head node with local databases and five compute nodes running the Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="2fb5a-116">A felhőszolgáltatások közvetlenül az USA nyugati régiója hely jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-116">All the cloud services are created directly in the West US location.</span></span> <span data-ttu-id="2fb5a-117">Az átjárócsomópont úgy működik, mint a tartomány erdő tartományvezérlőjévé.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-117">The head node acts as domain controller of the domain forest.</span></span>

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

### <a name="example-2"></a><span data-ttu-id="2fb5a-118">2. példa</span><span class="sxs-lookup"><span data-stu-id="2fb5a-118">Example 2</span></span>
<span data-ttu-id="2fb5a-119">A következő konfigurációs fájl telepíti egy meglévő tartomány erdőben HPC Pack fürt.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-119">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="2fb5a-120">Helyi adatbázisok 1 központi csomóponttal rendelkező fürt, és 12 számítási csomópontjain alkalmazott BGInfo Virtuálisgép-bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-120">The cluster has 1 head node with local databases and 12 compute nodes with the BGInfo VM extension applied.</span></span>
<span data-ttu-id="2fb5a-121">A Windows-frissítések automatikus telepítését a virtuális gép esetében a tartomány erdő le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-121">Automatic installation of Windows updates is disabled for all the VMs in the domain forest.</span></span> <span data-ttu-id="2fb5a-122">A felhőszolgáltatások közvetlenül a Kelet-Ázsia hely jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-122">All the cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="2fb5a-123">A számítási csomópontok létrehozott három felhőszolgáltatások és három storage-fiókok: *MyHPCCN-0001* való *MyHPCCN-0005* a *MyHPCCNService01* és *mycnstorage01*; *MyHPCCN-0006* való *MyHPCCN0010* a *MyHPCCNService02* és *mycnstorage02*; és *MyHPCCN-0011* való *MyHPCCN-0012* a *MyHPCCNService03* és *mycnstorage03*).</span><span class="sxs-lookup"><span data-stu-id="2fb5a-123">The compute nodes are created in three cloud services and three storage accounts: *MyHPCCN-0001* to *MyHPCCN-0005* in *MyHPCCNService01* and *mycnstorage01*; *MyHPCCN-0006* to *MyHPCCN0010* in *MyHPCCNService02* and *mycnstorage02*; and *MyHPCCN-0011* to *MyHPCCN-0012* in *MyHPCCNService03* and *mycnstorage03*).</span></span> <span data-ttu-id="2fb5a-124">A számítási csomópontok jönnek létre, a számítási csomópont rögzítése meglévő titkos lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-124">The compute nodes are created from an existing private image captured from a compute node.</span></span> <span data-ttu-id="2fb5a-125">Az automatikus növelhető vagy csökkenthető az engedélyezve van az alapértelmezett növelhető vagy csökkenthető a időközönként.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-125">The auto grow and shrink service is enabled with default grow and shrink intervals.</span></span>

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

### <a name="example-3"></a><span data-ttu-id="2fb5a-126">3. példa</span><span class="sxs-lookup"><span data-stu-id="2fb5a-126">Example 3</span></span>
<span data-ttu-id="2fb5a-127">A következő konfigurációs fájl telepíti egy meglévő tartomány erdőben HPC Pack fürt.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-127">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="2fb5a-128">A fürt egy átjárócsomópont, egy adatbázis-kiszolgálón, 500 GB adatlemezt, a Windows Server 2012 R2 operációs rendszert futtató két csomópontok és a Windows Server 2012 R2 operációs rendszert futtató öt számítási csomópontokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-128">The cluster contains one head node, one database server with a 500 GB data disk, two broker nodes running the Windows Server 2012 R2 operating system, and five compute nodes running the Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="2fb5a-129">A felhőszolgáltatás MyHPCCNService létrejön az affinitáscsoport *MyIBAffinityGroup*, és a felhőszolgáltatásokra létrejönnek az affinitáscsoport *MyAffinityGroup*.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-129">The cloud service MyHPCCNService is created in the affinity group *MyIBAffinityGroup*, and the other cloud services are created in the affinity group *MyAffinityGroup*.</span></span> <span data-ttu-id="2fb5a-130">A HPC feladat ütemezési REST API-t és a HPC-webportál az átjárócsomópont engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-130">The HPC Job Scheduler REST API and HPC web portal are enabled on the head node.</span></span>

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



### <a name="example-4"></a><span data-ttu-id="2fb5a-131">4. példa</span><span class="sxs-lookup"><span data-stu-id="2fb5a-131">Example 4</span></span>
<span data-ttu-id="2fb5a-132">A következő konfigurációs fájl telepíti egy meglévő tartomány erdőben HPC Pack fürt.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-132">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="2fb5a-133">Helyi adatbázisok két központi csomóponttal rendelkező fürt, két Azure csomópont sablonok létrehozása és három mérete közepes Azure csomópontot hoz létre Azure csomópontsablonhoz *AzureTemplate1*.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-133">The cluster has two head node with local databases, two Azure node templates are created, and three size Medium Azure nodes are created for Azure node template *AzureTemplate1*.</span></span> <span data-ttu-id="2fb5a-134">Az átjárócsomópont konfigurálása után az átjárócsomópont fut egy parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-134">A script file runs on the head node after the head node is configured.</span></span>

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

## <a name="troubleshooting"></a><span data-ttu-id="2fb5a-135">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="2fb5a-135">Troubleshooting</span></span>
* <span data-ttu-id="2fb5a-136">**"A virtuális hálózat nem létezik" hiba** -a parancsfájl futtatása az Azure-ban egy előfizetéshez tartozó egyidejűleg több fürtök üzembe helyezésekor, ha egy vagy több üzemelő példány a hiba miatt sikertelen lehet "VNet *VNet\_neve* nem létezik".</span><span class="sxs-lookup"><span data-stu-id="2fb5a-136">**“VNet doesn’t exist” error** - If you run the script to deploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with the error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="2fb5a-137">Ha ez a hiba akkor fordul elő, futtassa a parancsfájlt újra a sikertelen központi telepítésnél.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-137">If this error occurs, run the script again for the failed deployment.</span></span>
* <span data-ttu-id="2fb5a-138">**Az Internet elérése az Azure virtuális hálózat a probléma** –, ha akkor hozzon létre egy fürtöt egy új tartományvezérlő a telepítési parancsfájl használatával manuálisan főkiszolgálóvá előléptetni egy átjárócsomópont VM tartományvezérlőre, vagy a virtuális gépek csatlakozik az internetre problémák.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-138">**Problem accessing the Internet from the Azure virtual network** - If you create a cluster with a new domain controller by using the deployment script, or you manually promote a head node VM to domain controller, you may experience problems connecting the VMs to the Internet.</span></span> <span data-ttu-id="2fb5a-139">Ez a probléma akkor fordulhat elő, ha továbbító DNS-kiszolgáló automatikusan konfigurálja a tartományvezérlőn, és a továbbító DNS-kiszolgáló nem oldja meg megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-139">This problem can occur if a forwarder DNS server is automatically configured on the domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="2fb5a-140">Ez a probléma, jelentkezzen be a tartományvezérlő, és vagy távolítsa el a továbbító konfigurációs beállítás, vagy egy érvényes továbbító DNS-kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-140">To work around this problem, log on to the domain controller and either remove the forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="2fb5a-141">Ez a beállítás konfigurálása a Kiszolgálókezelőben kattintson **eszközök** >
    **DNS** nyissa meg a DNS-kezelőben, és kattintson duplán a **továbbítók**.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-141">To configure this setting, in Server Manager click **Tools** >
    **DNS** to open DNS Manager, and then double-click **Forwarders**.</span></span>
* <span data-ttu-id="2fb5a-142">**RDMA hálózati elérésekor számítási igényű virtuális gépek** –, ha ad hozzá a Windows Server számítási csomópont használja az RDMA-kompatibilisek-e virtuális gépek méretezés például A8 és A9, vagy virtuális gépek csatlakoztatása az RDMA-alkalmazás hálózati problémák.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-142">**Problem accessing RDMA network from compute-intensive VMs** - If you add Windows Server compute or broker node VMs using an RDMA-capable size such as A8 or A9, you may experience problems connecting those VMs to the RDMA application network.</span></span> <span data-ttu-id="2fb5a-143">Ez a probléma akkor fordul elő, akkor ha a HpcVmDrivers kiterjesztése nincs megfelelően telepítve a virtuális gépek a fürt való hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-143">One reason this problem occurs is if the HpcVmDrivers extension is not properly installed when the VMs are added to the cluster.</span></span> <span data-ttu-id="2fb5a-144">A bővítmény például Beragadt telepítése állapotban.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-144">For example, the extension might be stuck in the installing state.</span></span>
  
    <span data-ttu-id="2fb5a-145">Ez a probléma megoldása érdekében először ellenőrizze a bővítményt a virtuális gépek állapotát.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-145">To work around this problem, first check the state of the extension in the VMs.</span></span> <span data-ttu-id="2fb5a-146">Ha a kiterjesztés nem megfelelően van telepítve, távolítsa el a csomópontok a HPC-fürtből, és majd újból vegye fel a csomópontok.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-146">If the extension is not properly installed, try removing the nodes from the HPC cluster and then add the nodes again.</span></span> <span data-ttu-id="2fb5a-147">Például az Add-HpcIaaSNode.ps1 parancsfájl futtatásával az átjárócsomópont számítási csomópont virtuális gépek is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-147">For example, you can add compute node VMs by running the Add-HpcIaaSNode.ps1 script on the head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fb5a-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2fb5a-148">Next steps</span></span>
* <span data-ttu-id="2fb5a-149">Próbálja meg futtatni egy teszt munkaterhelés a fürtön.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-149">Try running a test workload on the cluster.</span></span> <span data-ttu-id="2fb5a-150">Egy vonatkozó példáért lásd: a HPC Pack [– első lépések útmutató](https://technet.microsoft.com/library/jj884144).</span><span class="sxs-lookup"><span data-stu-id="2fb5a-150">For an example, see the HPC Pack [getting started guide](https://technet.microsoft.com/library/jj884144).</span></span>
* <span data-ttu-id="2fb5a-151">Az oktatóanyagnak, amellyel a parancsfájl a fürtöt tartalmazó környezetben, és futtassa a HPC-munkaterhelés, lásd: [Ismerkedés az Azure-, Excel és SOA alkalmazásokat és szolgáltatásokat futtathatnak HPC Pack fürtöt a](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2fb5a-151">For a tutorial to script the cluster deployment and run an HPC workload, see [Get started with an HPC Pack cluster in Azure to run Excel and SOA workloads](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="2fb5a-152">Próbálja meg a HPC Pack eszközök elindítása, leállítása, adja hozzá, és a számítási csomópontok eltávolítása egy fürtről hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2fb5a-152">Try HPC Pack's tools to start, stop, add, and remove compute nodes from a cluster you create.</span></span> <span data-ttu-id="2fb5a-153">Lásd: [kezelése számítási csomópontok HPC csomagban fürtön, az Azure-ban](hpcpack-cluster-node-manage.md).</span><span class="sxs-lookup"><span data-stu-id="2fb5a-153">See [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md).</span></span>
* <span data-ttu-id="2fb5a-154">Első hozzon létre feladatokat küldhet a fürthöz egy helyi számítógépről, tekintse meg a [nyújt HPC feladatok egy helyi számítógépről egy HPC Pack fürtön, az Azure-ban](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2fb5a-154">To get set up to submit jobs to the cluster from a local computer, see [Submit HPC jobs from an on-premises computer to an HPC Pack cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

