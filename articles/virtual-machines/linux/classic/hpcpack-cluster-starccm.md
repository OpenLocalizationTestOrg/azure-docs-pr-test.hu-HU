---
title: "aaaRun csillag-CCM + HPC Pack Linux virtuális gépeken |} Microsoft Docs"
description: "Az Azure a Microsoft HPC Pack fürt központi telepítése és futtatása egy csillag-feladat CCM + több Linux a számítási csomópontok az RDMA-hálózaton."
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
ms.openlocfilehash: 8265013cb295f53d6d4354ab2f100ef20d9f4c8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a><span data-ttu-id="7db02-103">Futtassa a csillag-CCM + Microsoft HPC Pack egy Linux RDMA a fürt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="7db02-103">Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>
<span data-ttu-id="7db02-104">Ez a cikk bemutatja, hogyan toodeploy a Microsoft HPC Pack fürtön Azure, és futtassa a [CD-adapco csillag-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) feladat több Linux számítási csomópontokra, amelyeket kötik össze, az InfiniBand.</span><span class="sxs-lookup"><span data-stu-id="7db02-104">This article shows you how toodeploy a Microsoft HPC Pack cluster on Azure and run a [CD-adapco STAR-CCM+](http://www.cd-adapco.com/products/star-ccm%C2%AE) job on multiple Linux compute nodes that are interconnected with InfiniBand.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="7db02-105">A Microsoft HPC Pack számos szolgáltatások toorun nagyméretű HPC és párhuzamos alkalmazások, beleértve a MPI alkalmazásokat, a Microsoft Azure virtuális gépek fürtjein.</span><span class="sxs-lookup"><span data-stu-id="7db02-105">Microsoft HPC Pack provides features toorun a variety of large-scale HPC and parallel applications, including MPI applications, on clusters of Microsoft Azure virtual machines.</span></span> <span data-ttu-id="7db02-106">HPC Pack a telepített HPC Pack-fürtben lévő Linux számítási csomópont virtuális gépeken futó Linux HPC-alkalmazásokhoz is támogatja.</span><span class="sxs-lookup"><span data-stu-id="7db02-106">HPC Pack also supports running Linux HPC applications on Linux compute-node VMs that are deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="7db02-107">Egy bevezető toousing Linux számítási csomópontok HPC Pack, lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7db02-107">For an introduction toousing Linux compute nodes with HPC Pack, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="set-up-an-hpc-pack-cluster"></a><span data-ttu-id="7db02-108">Egy HPC Pack fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="7db02-108">Set up an HPC Pack cluster</span></span>
<span data-ttu-id="7db02-109">Hello HPC Pack IaaS üzembe helyezési parancsfájlok letöltését hello [letöltőközpontból](https://www.microsoft.com/en-us/download/details.aspx?id=44949) és helyileg bontsa ki.</span><span class="sxs-lookup"><span data-stu-id="7db02-109">Download hello HPC Pack IaaS deployment scripts from hello [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) and extract them locally.</span></span>

<span data-ttu-id="7db02-110">Az Azure PowerShell feltétele.</span><span class="sxs-lookup"><span data-stu-id="7db02-110">Azure PowerShell is a prerequisite.</span></span> <span data-ttu-id="7db02-111">Ha a PowerShell nincs konfigurálva a helyi gépén, olvassa el a hello cikk [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7db02-111">If PowerShell is not configured on your local machine, please read hello article [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="7db02-112">Írásának hello időpontban hello Linux képek hello Azure piactér (tartalmazó hello InfiniBand illesztőprogramokat az Azure-bA) a SLES 12 rendszert, a CentOS 6.5-ös és a CentOS 7.1 vannak.</span><span class="sxs-lookup"><span data-stu-id="7db02-112">At hello time of this writing, hello Linux images from hello Azure Marketplace (which contains hello InfiniBand drivers for Azure) are for SLES 12, CentOS 6.5, and CentOS 7.1.</span></span> <span data-ttu-id="7db02-113">Ez a cikk az SLES 12 hello használata alapul.</span><span class="sxs-lookup"><span data-stu-id="7db02-113">This article is based on hello usage of SLES 12.</span></span> <span data-ttu-id="7db02-114">tooretrieve hello nevét, amely támogatja az HPC hello piactér minden Linux-lemezképek, a következő PowerShell-paranccsal hello is futtathatja:</span><span class="sxs-lookup"><span data-stu-id="7db02-114">tooretrieve hello name of all Linux images that support HPC in hello Marketplace, you can run hello following PowerShell command:</span></span>

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

<span data-ttu-id="7db02-115">hello kimeneti tartalmazza ezeket a lemezképeket érhetők el és lemezképnév hello hello helye (**ImageName**) toobe később hello központi telepítési sablont használni.</span><span class="sxs-lookup"><span data-stu-id="7db02-115">hello output lists hello location in which these images are available and hello image name (**ImageName**) toobe used in hello deployment template later.</span></span>

<span data-ttu-id="7db02-116">Hello fürt központi telepítése érdekében, hogy toobuild egy HPC Pack telepítési sablon fájlt.</span><span class="sxs-lookup"><span data-stu-id="7db02-116">Before you deploy hello cluster, you have toobuild an HPC Pack deployment template file.</span></span> <span data-ttu-id="7db02-117">Mivel azt még célzó kis fürt, hello átjárócsomópont hello a tartományvezérlőn lennie, és egy helyi SQL-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="7db02-117">Because we're targeting a small cluster, hello head node will be hello domain controller and host a local SQL database.</span></span>

<span data-ttu-id="7db02-118">hello következő sablon fog átjárócsomópont ilyen egy, nevű XML-fájl létrehozása **MyCluster.xml**, és cserélje le a hello értékének **; előfizetés-azonosító**, **StorageAccount**,  **Hely**, **VMName**, és **szolgáltatásnév** legyen.</span><span class="sxs-lookup"><span data-stu-id="7db02-118">hello following template will deploy such a head node, create an XML file named **MyCluster.xml**, and replace hello values of **SubscriptionId**, **StorageAccount**, **Location**, **VMName**, and **ServiceName** with yours.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

<span data-ttu-id="7db02-119">Indítsa el a hello átjárócsomópont létrehozási hello PowerShell-parancs futtatásával egy rendszergazda jogú parancssorból:</span><span class="sxs-lookup"><span data-stu-id="7db02-119">Start hello head-node creation by running hello PowerShell command in an elevated command prompt:</span></span>

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

<span data-ttu-id="7db02-120">Too30 20 perc elteltével hello átjárócsomópont készen áll.</span><span class="sxs-lookup"><span data-stu-id="7db02-120">After 20 too30 minutes, hello head node should be ready.</span></span> <span data-ttu-id="7db02-121">Lehet csatlakoztatni tooit hello Azure-portálon hello kattintva **Connect** hello virtuális gép ikonja.</span><span class="sxs-lookup"><span data-stu-id="7db02-121">You can connect tooit from hello Azure portal by clicking hello **Connect** icon of hello virtual machine.</span></span>

<span data-ttu-id="7db02-122">Idővel előfordulhat, hogy toofix hello DNS-továbbító.</span><span class="sxs-lookup"><span data-stu-id="7db02-122">You might eventually have toofix hello DNS forwarder.</span></span> <span data-ttu-id="7db02-123">toodo Igen, a DNS-kezelő elindítása.</span><span class="sxs-lookup"><span data-stu-id="7db02-123">toodo so, start DNS Manager.</span></span>

1. <span data-ttu-id="7db02-124">Kattintson a jobb gombbal hello kiszolgálónév a DNS-kezelőben válassza **tulajdonságok**, majd kattintson a hello **továbbítók** lapon.</span><span class="sxs-lookup"><span data-stu-id="7db02-124">Right-click hello server name in DNS Manager, select **Properties**, and then click hello **Forwarders** tab.</span></span>
2. <span data-ttu-id="7db02-125">Kattintson a hello **szerkesztése** tooremove továbbítókat gombra, majd a **OK**.</span><span class="sxs-lookup"><span data-stu-id="7db02-125">Click hello **Edit** button tooremove any forwarders, and then click **OK**.</span></span>
3. <span data-ttu-id="7db02-126">Győződjön meg arról, hogy hello **gyökérmutató használatára, ha nincs továbbítók elérhető** jelölőnégyzet be van jelölve, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="7db02-126">Make sure that hello **Use root hints if no forwarders are available** check box is selected, and then click **OK**.</span></span>

## <a name="set-up-linux-compute-nodes"></a><span data-ttu-id="7db02-127">Linux számítási csomópontok beállítása</span><span class="sxs-lookup"><span data-stu-id="7db02-127">Set up Linux compute nodes</span></span>
<span data-ttu-id="7db02-128">Hello Linux számítási csomópontok hello segítségével telepítheti, hogy használja-e toocreate hello átjárócsomópont azonos központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="7db02-128">You deploy hello Linux compute nodes by using hello same deployment template that you used toocreate hello head node.</span></span>

<span data-ttu-id="7db02-129">Fájl másolása hello **MyCluster.xml** a helyi számítógép toohello átjárócsomópont, és a frissítés hello **NodeCount** hello számát, amelyet az toodeploy csomópontok kód (< = 20).</span><span class="sxs-lookup"><span data-stu-id="7db02-129">Copy hello file **MyCluster.xml** from your local machine toohello head node, and update hello **NodeCount** tag with hello number of nodes that you want toodeploy (<=20).</span></span> <span data-ttu-id="7db02-130">Lehet, gondos toohave elegendő rendelkezésre álló magot a Azure kvótájának A9 feltünteti az előfizetésében 16 mag fog használni.</span><span class="sxs-lookup"><span data-stu-id="7db02-130">Be careful toohave enough available cores in your Azure quota, because each A9 instance will consume 16 cores in your subscription.</span></span> <span data-ttu-id="7db02-131">A8 példányok (8 mag) is A9 helyett használja, ha szeretné toouse hello további virtuális gépek ugyanazt a költségvetést.</span><span class="sxs-lookup"><span data-stu-id="7db02-131">You can use A8 instances (8 cores) instead of A9 if you want toouse more VMs in hello same budget.</span></span>

<span data-ttu-id="7db02-132">Hello átjárócsomópont hello HPC Pack IaaS üzembe helyezési parancsfájlok másolja.</span><span class="sxs-lookup"><span data-stu-id="7db02-132">On hello head node, copy hello HPC Pack IaaS deployment scripts.</span></span>

<span data-ttu-id="7db02-133">Futtassa a következő Azure PowerShell-parancsok rendszergazda jogú parancssorból hello:</span><span class="sxs-lookup"><span data-stu-id="7db02-133">Run hello following Azure PowerShell commands in an elevated command prompt:</span></span>

1. <span data-ttu-id="7db02-134">Futtatás **Add-AzureAccount** tooconnect tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="7db02-134">Run **Add-AzureAccount** tooconnect tooyour Azure subscription.</span></span>
2. <span data-ttu-id="7db02-135">Ha több előfizetéssel rendelkezik, akkor futtassa **Get-AzureSubscription** toolist őket.</span><span class="sxs-lookup"><span data-stu-id="7db02-135">If you have multiple subscriptions, run **Get-AzureSubscription** toolist them.</span></span>
3. <span data-ttu-id="7db02-136">Állítsa az alapértelmezett előfizetés hello futtatásával **válasszon-AzureSubscription - SubscriptionName xxxx-alapértelmezett** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7db02-136">Set a default subscription by running hello **Select-AzureSubscription -SubscriptionName xxxx -Default** command.</span></span>
4. <span data-ttu-id="7db02-137">Futtatás **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** toostart Linux számítási csomópontok telepítésével.</span><span class="sxs-lookup"><span data-stu-id="7db02-137">Run **.\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml** toostart deploying Linux compute nodes.</span></span>
   
   ![Átjárócsomópont telepítési művelet:][hndeploy]

<span data-ttu-id="7db02-139">Hello HPC Pack Cluster Manager eszköz megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7db02-139">Open hello HPC Pack Cluster Manager tool.</span></span> <span data-ttu-id="7db02-140">Néhány perc múlva a Linux számítási csomópontok rendszeresen megjelennek a listában számítási csomópont.</span><span class="sxs-lookup"><span data-stu-id="7db02-140">After few minutes, Linux compute nodes will regularly appear in list of cluster compute nodes.</span></span> <span data-ttu-id="7db02-141">A klasszikus üzembe helyezési mód hello IaaS virtuális gépek jönnek létre egymás után.</span><span class="sxs-lookup"><span data-stu-id="7db02-141">With hello classic deployment mode, IaaS VMs are created sequentially.</span></span> <span data-ttu-id="7db02-142">Ezért ha a csomópontok számát hello fontos, rávenni a felhasználókat az összes telepített is igénybe vehet jelentős mennyiségű időt.</span><span class="sxs-lookup"><span data-stu-id="7db02-142">So if hello number of nodes is important, getting them all deployed can take a significant amount of time.</span></span>

![Linux-csomópontok HPC Pack-Fürtkezelőben][clustermanager]

<span data-ttu-id="7db02-144">Most, hogy minden csomópont működik, és hello fürtben, nincsenek további infrastruktúra beállítások toomake.</span><span class="sxs-lookup"><span data-stu-id="7db02-144">Now that all nodes are up and running in hello cluster, there are additional infrastructure settings toomake.</span></span>

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a><span data-ttu-id="7db02-145">Egy Azure fájlmegosztás Windows és Linux csomópontok beállítása</span><span class="sxs-lookup"><span data-stu-id="7db02-145">Set up an Azure File share for Windows and Linux nodes</span></span>
<span data-ttu-id="7db02-146">Hello Azure File service toostore parancsfájlok, alkalmazásokat és az adatfájlok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7db02-146">You can use hello Azure File service toostore scripts, application packages, and data files.</span></span> <span data-ttu-id="7db02-147">Az Azure File állandó tároló Azure Blob storage-CIFS képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="7db02-147">Azure File provides CIFS capabilities on top of Azure Blob storage as a persistent store.</span></span> <span data-ttu-id="7db02-148">Vegye figyelembe, hogy ez nem hello legjobban méretezhető megoldás, de hello legegyszerűbb egy és nem igényel dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="7db02-148">Be aware that this is not hello most scalable solution, but it is hello simplest one and doesn’t require dedicated VMs.</span></span>

<span data-ttu-id="7db02-149">Hozzon létre egy Azure fájlmegosztás hello hello cikk utasításait követve [Ismerkedés az Azure File storage on Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="7db02-149">Create an Azure File share by following hello instructions in hello article [Get started with Azure File storage on Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>

<span data-ttu-id="7db02-150">Hello neve a tárfiókja, továbbra is **saname**, hello fájlmegosztás neve szerint **megosztásnév**, és hello tárfiók hívóbetűjét, **sakey**.</span><span class="sxs-lookup"><span data-stu-id="7db02-150">Keep hello name of your storage account as **saname**, hello file share name as **sharename**, and hello storage account key as **sakey**.</span></span>

### <a name="mount-hello-azure-file-share-on-hello-head-node"></a><span data-ttu-id="7db02-151">Hello átjárócsomópont hello Azure fájlmegosztás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="7db02-151">Mount hello Azure File share on hello head node</span></span>
<span data-ttu-id="7db02-152">Nyisson meg egy rendszergazda jogú parancssort, és futtassa a következő parancs toostore hello hitelesítő hello helyi számítógép tárolóban hello:</span><span class="sxs-lookup"><span data-stu-id="7db02-152">Open an elevated command prompt and run hello following command toostore hello credentials in hello local machine vault:</span></span>

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

<span data-ttu-id="7db02-153">Ezt követően toomount hello Azure fájlmegosztás, futtassa:</span><span class="sxs-lookup"><span data-stu-id="7db02-153">Then, toomount hello Azure File share, run:</span></span>

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-hello-azure-file-share-on-linux-compute-nodes"></a><span data-ttu-id="7db02-154">A Linux számítási csomópontok hello Azure fájlmegosztás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="7db02-154">Mount hello Azure File share on Linux compute nodes</span></span>
<span data-ttu-id="7db02-155">HPC Pack részeként elérhető egy hasznos eszköz egy hello clusrun eszköz.</span><span class="sxs-lookup"><span data-stu-id="7db02-155">One useful tool that comes with HPC Pack is hello clusrun tool.</span></span> <span data-ttu-id="7db02-156">Ez ugyanaz a parancs parancssori eszköz toorun hello egyidejűleg is használhat a számítási csomópontok készlete.</span><span class="sxs-lookup"><span data-stu-id="7db02-156">You can use this command-line tool toorun hello same command simultaneously on a set of compute nodes.</span></span> <span data-ttu-id="7db02-157">Ebben az esetben ez toomount hello Azure fájlmegosztás által használt, és továbbra is fennáll, toosurvive újraindítások.</span><span class="sxs-lookup"><span data-stu-id="7db02-157">In our case, it's used toomount hello Azure File share and persist it toosurvive reboots.</span></span>
<span data-ttu-id="7db02-158">Egy rendszergazda jogú parancssort a hello átjárócsomópont futtassa a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="7db02-158">In an elevated command prompt on hello head node, run hello following commands.</span></span>

<span data-ttu-id="7db02-159">toocreate hello csatlakoztatási könyvtár:</span><span class="sxs-lookup"><span data-stu-id="7db02-159">toocreate hello mount directory:</span></span>

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

<span data-ttu-id="7db02-160">toomount hello Azure fájlmegosztás:</span><span class="sxs-lookup"><span data-stu-id="7db02-160">toomount hello Azure File share:</span></span>

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

<span data-ttu-id="7db02-161">toopersist hello csatlakoztatási megosztás:</span><span class="sxs-lookup"><span data-stu-id="7db02-161">toopersist hello mount share:</span></span>

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a><span data-ttu-id="7db02-162">Telepítse a csillag-CCM +</span><span class="sxs-lookup"><span data-stu-id="7db02-162">Install STAR-CCM+</span></span>
<span data-ttu-id="7db02-163">Azure Virtuálisgép-példányok A8 és A9 InfiniBand támogatási és RDMA-képességeinek biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="7db02-163">Azure VM instances A8 and A9 provide InfiniBand support and RDMA capabilities.</span></span> <span data-ttu-id="7db02-164">hello kernel illesztőprogramokat, amelyek lehetővé teszik a szolgáltatásokat a Windows Server 2012 R2, SUSE 12, CentOS 6.5, valamint CentOS 7.1 hello Azure piactér érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7db02-164">hello kernel drivers that enable those capabilities are available for Windows Server 2012 R2, SUSE 12, CentOS 6.5, and CentOS 7.1 images in hello Azure Marketplace.</span></span> <span data-ttu-id="7db02-165">Microsoft MPI és Intel MPI (5.x) olyan hello két MPI könyvtárak, amelyek támogatják a kívánt illesztőprogramokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7db02-165">Microsoft MPI and Intel MPI (release 5.x) are hello two MPI libraries that support those drivers in Azure.</span></span>

<span data-ttu-id="7db02-166">CD-adapco csillag-CCM + 11.x kiadási és újabb rendszer kötegelt Intel MPI verziójával 5.x, így Azure InfiniBand támogatása is megtalálható.</span><span class="sxs-lookup"><span data-stu-id="7db02-166">CD-adapco STAR-CCM+ release 11.x and later is bundled with Intel MPI version 5.x, so InfiniBand support for Azure is included.</span></span>

<span data-ttu-id="7db02-167">Első hello Linux64 csillag-hello csomagot CCM + [CD-adapco portal](https://steve.cd-adapco.com).</span><span class="sxs-lookup"><span data-stu-id="7db02-167">Get hello Linux64 STAR-CCM+ package from hello [CD-adapco portal](https://steve.cd-adapco.com).</span></span> <span data-ttu-id="7db02-168">Ebben az esetben vegyes pontosság 11.02.010 verziót használtuk.</span><span class="sxs-lookup"><span data-stu-id="7db02-168">In our case, we used version 11.02.010 in mixed precision.</span></span>

<span data-ttu-id="7db02-169">A hello átjárócsomópont, hello **/hpcdata** Azure fájl megosztásához hozzon létre egy héjparancsfájlt nevű **setupstarccm.sh** a tartalom a következő hello.</span><span class="sxs-lookup"><span data-stu-id="7db02-169">On hello head node, in hello **/hpcdata** Azure File share, create a shell script named **setupstarccm.sh** with hello following content.</span></span> <span data-ttu-id="7db02-170">Ez a parancsfájl fog futni minden számítási csomópont tooset be csillag a-CCM + helyileg.</span><span class="sxs-lookup"><span data-stu-id="7db02-170">This script will be run on each compute node tooset up STAR-CCM+ locally.</span></span>

#### <a name="sample-setupstarcmsh-script"></a><span data-ttu-id="7db02-171">Mintaparancsfájl setupstarcm.sh</span><span class="sxs-lookup"><span data-stu-id="7db02-171">Sample setupstarcm.sh script</span></span>
```
    #!/bin/bash
    # setupstarcm.sh tooset up STAR-CCM+ locally

    # Create hello CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy hello STAR-CCM package from hello file share toohello local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract hello package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without hello FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
<span data-ttu-id="7db02-172">Most, tooset be csillag-CCM + a Linux számítási csomópontok, nyisson meg egy rendszergazda jogú parancssort és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="7db02-172">Now, tooset up STAR-CCM+ on all your Linux compute nodes, open an elevated command prompt and run hello following command:</span></span>

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

<span data-ttu-id="7db02-173">Hello parancs futása közben, a figyelheti hello CPU-használat hello hőtérkép a fürtkezelő használatával.</span><span class="sxs-lookup"><span data-stu-id="7db02-173">While hello command is running, you can monitor hello CPU usage by using hello heat map of Cluster Manager.</span></span> <span data-ttu-id="7db02-174">Néhány perc múlva minden csomópontja megfelelően létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="7db02-174">After few minutes, all nodes should be correctly set up.</span></span>

## <a name="run-star-ccm-jobs"></a><span data-ttu-id="7db02-175">Futtassa a csillag-CCM + feladatok</span><span class="sxs-lookup"><span data-stu-id="7db02-175">Run STAR-CCM+ jobs</span></span>
<span data-ttu-id="7db02-176">HPC Pack használt rendelés toorun csillag feladat ütemezési platformképességei-CCM + feladatok.</span><span class="sxs-lookup"><span data-stu-id="7db02-176">HPC Pack is used for its job scheduler capabilities in order toorun STAR-CCM+ jobs.</span></span> <span data-ttu-id="7db02-177">toodo esetben azt kell hello támogatási néhány olyan parancsfájlok, amelyek használt toostart hello feladatot, és futtassa a csillag-CCM +.</span><span class="sxs-lookup"><span data-stu-id="7db02-177">toodo so, we need hello support of a few scripts that are used toostart hello job and run STAR-CCM+.</span></span> <span data-ttu-id="7db02-178">hello bemeneti adatok másolatok hello Azure fájlmegosztás első az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="7db02-178">hello input data is kept on hello Azure File share first for simplicity.</span></span>

<span data-ttu-id="7db02-179">a következő PowerShell-parancsfájl hello használt tooqueue csillag-CCM + feladat.</span><span class="sxs-lookup"><span data-stu-id="7db02-179">hello following PowerShell script is used tooqueue a STAR-CCM+ job.</span></span> <span data-ttu-id="7db02-180">Három argumentummal van:</span><span class="sxs-lookup"><span data-stu-id="7db02-180">It takes three arguments:</span></span>

* <span data-ttu-id="7db02-181">hello modell neve</span><span class="sxs-lookup"><span data-stu-id="7db02-181">hello model name</span></span>
* <span data-ttu-id="7db02-182">használt csomópontok toobe hello száma</span><span class="sxs-lookup"><span data-stu-id="7db02-182">hello number of nodes toobe used</span></span>
* <span data-ttu-id="7db02-183">a minden egyes csomópont toobe használt magok hello száma</span><span class="sxs-lookup"><span data-stu-id="7db02-183">hello number of cores on each node toobe used</span></span>

<span data-ttu-id="7db02-184">Mivel a csillag-CCM + töltheti hello memória sávszélesség, az általában jobb toouse kevesebb processzormag, egy számítási csomópontok és új csomópontok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7db02-184">Because STAR-CCM+ can fill hello memory bandwidth, it's usually better toouse fewer cores per compute nodes and add new nodes.</span></span> <span data-ttu-id="7db02-185">hello pontos csomópont magok számát hello processzorcsaládját és hello memóriamodulok közötti sebességétől függ.</span><span class="sxs-lookup"><span data-stu-id="7db02-185">hello exact number of cores per node will depend on hello processor family and hello interconnect speed.</span></span>

<span data-ttu-id="7db02-186">hello csomópontok kizárólag hello feladathoz kiosztott, és nem osztható meg más feladatok.</span><span class="sxs-lookup"><span data-stu-id="7db02-186">hello nodes are allocated exclusively for hello job and can’t be shared with other jobs.</span></span> <span data-ttu-id="7db02-187">hello feladat nem indult el MPI feladatként közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="7db02-187">hello job is not started as an MPI job directly.</span></span> <span data-ttu-id="7db02-188">Hello **runstarccm.sh** héjparancsfájlt hello MPI indítója elindul.</span><span class="sxs-lookup"><span data-stu-id="7db02-188">hello **runstarccm.sh** shell script will start hello MPI launcher.</span></span>

<span data-ttu-id="7db02-189">hello bemeneti modell és hello **runstarccm.sh** parancsfájl tárolódnak hello **/hpcdata** korábban csatlakoztatott megosztást.</span><span class="sxs-lookup"><span data-stu-id="7db02-189">hello input model and hello **runstarccm.sh** script are stored in hello **/hpcdata** share that was previously mounted.</span></span>

<span data-ttu-id="7db02-190">Naplófájlok hello feladatazonosítóval vannak nevezve, és vannak tárolva hello **/hpcdata megosztás**, együtt hello csillag-CCM + kimeneti fájlok.</span><span class="sxs-lookup"><span data-stu-id="7db02-190">Log files are named with hello job ID and are stored in hello **/hpcdata share**, along with hello STAR-CCM+ output files.</span></span>

#### <a name="sample-submitstarccmjobps1-script"></a><span data-ttu-id="7db02-191">Mintaparancsfájl SubmitStarccmJob.ps1</span><span class="sxs-lookup"><span data-stu-id="7db02-191">Sample SubmitStarccmJob.ps1 script</span></span>
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us hello job ID that's used tooidentify hello name of hello uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit hello job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
<span data-ttu-id="7db02-192">Cserélje le **runner.java** az előnyben részesített csillag-CCM + Java modell indítója és naplózási kódot.</span><span class="sxs-lookup"><span data-stu-id="7db02-192">Replace **runner.java** with your preferred STAR-CCM+ Java model launcher and logging code.</span></span>

#### <a name="sample-runstarccmsh-script"></a><span data-ttu-id="7db02-193">Mintaparancsfájl runstarccm.sh</span><span class="sxs-lookup"><span data-stu-id="7db02-193">Sample runstarccm.sh script</span></span>
```
    #!/bin/bash
    echo "start"
    # hello path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set hello mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into hello hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with hello hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

<span data-ttu-id="7db02-194">Ebben a tesztben használtuk egy igény szerinti Power licenc token.</span><span class="sxs-lookup"><span data-stu-id="7db02-194">In our test, we used a Power-On-Demand license token.</span></span> <span data-ttu-id="7db02-195">A token tooset hello rendelkezik **$CDLMD_LICENSE_FILE** környezeti változó túl **1999@flex.cd-adapco.com**  és hello hello kulcs **- podkey** hello parancssor beállítás .</span><span class="sxs-lookup"><span data-stu-id="7db02-195">For that token, you have tooset hello **$CDLMD_LICENSE_FILE** environment variable too**1999@flex.cd-adapco.com** and hello key in hello **-podkey** option of hello command line.</span></span>

<span data-ttu-id="7db02-196">Néhány inicializálás után hello parancsfájl kivonatok – hello **$CCP_NODES_CORES** környezeti változókat, amelyek beállítása HPC Pack – hello csomópontok toobuild egy hostfile, amely MPI indítója hello használja listáját.</span><span class="sxs-lookup"><span data-stu-id="7db02-196">After some initialization, hello script extracts--from hello **$CCP_NODES_CORES** environment variables that HPC Pack set--hello list of nodes toobuild a hostfile that hello MPI launcher uses.</span></span> <span data-ttu-id="7db02-197">A hostfile számítási csomópont hello feladat, soronként egy név használt nevek hello listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7db02-197">This hostfile will contain hello list of compute node names that are used for hello job, one name per line.</span></span>

<span data-ttu-id="7db02-198">hello formátuma **$CCP_NODES_CORES** ezt a mintát követi:</span><span class="sxs-lookup"><span data-stu-id="7db02-198">hello format of **$CCP_NODES_CORES** follows this pattern:</span></span>

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

<span data-ttu-id="7db02-199">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="7db02-199">Where:</span></span>

* <span data-ttu-id="7db02-200">`<Number of nodes>`nem lefoglalt toothis feladat csomópontok hello száma.</span><span class="sxs-lookup"><span data-stu-id="7db02-200">`<Number of nodes>` is hello number of nodes allocated toothis job.</span></span>
* <span data-ttu-id="7db02-201">`<Name of node_n_...>`az egyes csomópontok toothis feladat lefoglalt hello név.</span><span class="sxs-lookup"><span data-stu-id="7db02-201">`<Name of node_n_...>` is hello name of each node allocated toothis job.</span></span>
* <span data-ttu-id="7db02-202">`<Cores of node_n_...>`hello csomóponton toothis feladat lefoglalt magok száma hello van.</span><span class="sxs-lookup"><span data-stu-id="7db02-202">`<Cores of node_n_...>` is hello number of cores on hello node allocated toothis job.</span></span>

<span data-ttu-id="7db02-203">magok száma hello (**$NBCORES**) egyben csomópontok száma alapján számított hello (**$NBNODES**) és a csomópont magok számát hello (paraméterként megadott **$NBCORESPERNODE**).</span><span class="sxs-lookup"><span data-stu-id="7db02-203">hello number of cores (**$NBCORES**) is also calculated based on hello number of nodes (**$NBNODES**) and hello number of cores per node (provided as parameter **$NBCORESPERNODE**).</span></span>

<span data-ttu-id="7db02-204">Hello MPI beállítások hello Azure Intel MPI használt állók közül a következők:</span><span class="sxs-lookup"><span data-stu-id="7db02-204">For hello MPI options, hello ones that are used with Intel MPI on Azure are:</span></span>

* <span data-ttu-id="7db02-205">`-mpi intel`Intel MPI toospecify.</span><span class="sxs-lookup"><span data-stu-id="7db02-205">`-mpi intel` toospecify Intel MPI.</span></span>
* <span data-ttu-id="7db02-206">`-fabric UDAPL`toouse Azure InfiniBand műveletek.</span><span class="sxs-lookup"><span data-stu-id="7db02-206">`-fabric UDAPL` toouse Azure InfiniBand verbs.</span></span>
* <span data-ttu-id="7db02-207">`-cpubind bandwidth,v`a csillag MPI toooptimize sávszélesség-CCM +.</span><span class="sxs-lookup"><span data-stu-id="7db02-207">`-cpubind bandwidth,v` toooptimize bandwidth for MPI with STAR-CCM+.</span></span>
* <span data-ttu-id="7db02-208">`-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Intel MPI toomake Azure InfiniBand dolgozni, és tooset hello szükséges csomópont magok számát.</span><span class="sxs-lookup"><span data-stu-id="7db02-208">`-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"` toomake Intel MPI work with Azure InfiniBand, and tooset hello required number of cores per node.</span></span>
* <span data-ttu-id="7db02-209">`-batch`toostart csillag-CCM + kötegelt módban nincs felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="7db02-209">`-batch` toostart STAR-CCM+ in batch mode with no UI.</span></span>

<span data-ttu-id="7db02-210">Végezetül toostart egy feladatot, győződjön meg arról, hogy a csomópontok megfelelően működik-e, és a kezelő online állapotban.</span><span class="sxs-lookup"><span data-stu-id="7db02-210">Finally, toostart a job, make sure that your nodes are up and running and are online in Cluster Manager.</span></span> <span data-ttu-id="7db02-211">A PowerShell-parancssorból futtassa ezt:</span><span class="sxs-lookup"><span data-stu-id="7db02-211">Then from a PowerShell command prompt, run this:</span></span>

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a><span data-ttu-id="7db02-212">Csomópont leállítása</span><span class="sxs-lookup"><span data-stu-id="7db02-212">Stop nodes</span></span>
<span data-ttu-id="7db02-213">Meg Miután befejezte a teszteket, használhatja a következő HPC Pack PowerShell-parancsok toostop hello és indítsa el a csomópontok:</span><span class="sxs-lookup"><span data-stu-id="7db02-213">Later on, after you're done with your tests, you can use hello following HPC Pack PowerShell commands toostop and start nodes:</span></span>

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a><span data-ttu-id="7db02-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7db02-214">Next steps</span></span>
<span data-ttu-id="7db02-215">Próbálja más Linux feladatokat futtatni.</span><span class="sxs-lookup"><span data-stu-id="7db02-215">Try running other Linux workloads.</span></span> <span data-ttu-id="7db02-216">Például lásd:</span><span class="sxs-lookup"><span data-stu-id="7db02-216">For example, see:</span></span>

* [<span data-ttu-id="7db02-217">A Microsoft HPC Pack NAMD futó Linux számítási csomópontok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="7db02-217">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
* [<span data-ttu-id="7db02-218">Microsoft HPC Pack OpenFOAM futtassa az Azure Linux RDMA fürtön</span><span class="sxs-lookup"><span data-stu-id="7db02-218">Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
