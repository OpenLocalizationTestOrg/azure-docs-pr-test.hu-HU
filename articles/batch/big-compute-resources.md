---
title: "az Azure-felhőbe hello kötegelt és HPC aaaResources |} Microsoft Docs"
description: "Futtatja a felügyeleti teendők központjaként párhuzamos kötegelt és nagy teljesítményű számítástechnikai (HPC) munkaterhelések az Azure technikai erőforrások toohelp sorolja fel."
services: batch, cloud-services, virtual-machines
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 6f8be911-c841-41ae-88d3-3bcfc029eb7f
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 03/17/2017
ms.author: danlep
ms.openlocfilehash: ab8ba24678bd7ec090306b501d29ca63c4fb83ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a><span data-ttu-id="43d44-103">Az Azure-ban nagy számítási: a kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások</span><span class="sxs-lookup"><span data-stu-id="43d44-103">Big Compute in Azure: Technical resources for batch and high-performance computing</span></span>
<span data-ttu-id="43d44-104">Ez az egy útmutató tootechnical erőforrások toohelp futtatja a felügyeleti teendők központjaként párhuzamos kötegelt és nagy teljesítményű számítástechnikai rendszerek (HPC) munkaterhelések az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="43d44-104">This is a guide tootechnical resources toohelp you run your large-scale parallel, batch, and high-performance computing (HPC) workloads in Azure.</span></span> <span data-ttu-id="43d44-105">A meglévő kötegelt vagy HPC-munkaterhelések toohello Azure felhőben, vagy új nagy számítási megoldások egy tartományt az Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="43d44-105">Extend your existing batch or HPC workloads toohello Azure cloud, or build new Big Compute solutions using a range of Azure services.</span></span>

## <a name="solutions-options"></a><span data-ttu-id="43d44-106">Megoldás beállításai</span><span class="sxs-lookup"><span data-stu-id="43d44-106">Solutions options</span></span>
<span data-ttu-id="43d44-107">További tudnivalók az Azure-ban nagy számítási beállítások, és válassza a hello megfelelő módszer az alkalmazások és szolgáltatások és az üzleti igényeknek.</span><span class="sxs-lookup"><span data-stu-id="43d44-107">Learn about Big Compute options in Azure, and choose hello right approach for your workload and business need.</span></span>

* [<span data-ttu-id="43d44-108">Kötegelt vagy HPC-megoldások</span><span class="sxs-lookup"><span data-stu-id="43d44-108">Batch and HPC solutions</span></span>](batch-hpc-solutions.md)
* [<span data-ttu-id="43d44-109">Videó: Nagy számítási az Azure és a HPC hello felhőben</span><span class="sxs-lookup"><span data-stu-id="43d44-109">Video: Big Compute in hello cloud with Azure and HPC</span></span>](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)

## <a name="azure-batch"></a><span data-ttu-id="43d44-110">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="43d44-110">Azure Batch</span></span>
<span data-ttu-id="43d44-111">[Kötegelt](https://azure.microsoft.com/services/batch/) van, így könnyen toocloud-enable platform szolgáltatás, a Linux és Windows-alkalmazások és futtatási feladatok és a fürt és a feladat ütemezési irányítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="43d44-111">[Batch](https://azure.microsoft.com/services/batch/) is a platform service that makes it easy toocloud-enable your Linux and Windows applications and run jobs without setting up and managing a cluster and job scheduler.</span></span> <span data-ttu-id="43d44-112">Azure Batch hello SDK toointegrate ügyfélalkalmazások használjon különböző nyelveken szakasz adatok tooAzure, keresztül, és build feladat végrehajtási folyamatok.</span><span class="sxs-lookup"><span data-stu-id="43d44-112">Use hello SDK toointegrate client applications with Azure Batch through various languages, stage data tooAzure, and build job execution pipelines.</span></span>

* [<span data-ttu-id="43d44-113">Dokumentáció</span><span class="sxs-lookup"><span data-stu-id="43d44-113">Documentation</span></span>](https://azure.microsoft.com/documentation/services/batch/)
* <span data-ttu-id="43d44-114">[.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), és [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API-referencia</span><span class="sxs-lookup"><span data-stu-id="43d44-114">[.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), and [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API reference</span></span>
* <span data-ttu-id="43d44-115">[Batch management .NET kódtár](https://msdn.microsoft.com/library/mt463120.aspx) referencia</span><span class="sxs-lookup"><span data-stu-id="43d44-115">[Batch management .NET library](https://msdn.microsoft.com/library/mt463120.aspx) reference</span></span>
* <span data-ttu-id="43d44-116">Oktatóanyag: Ismerkedés a [Azure Batch .NET-keretrendszerhez készült](batch-dotnet-get-started.md) és [kötegelt Python-ügyfél](batch-python-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="43d44-116">Tutorials: Get started with [Azure Batch library for .NET](batch-dotnet-get-started.md) and [Batch Python client](batch-python-tutorial.md)</span></span>
* [<span data-ttu-id="43d44-117">Batch fórum</span><span class="sxs-lookup"><span data-stu-id="43d44-117">Batch forum</span></span>](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)
* [<span data-ttu-id="43d44-118">Kötegelt videók</span><span class="sxs-lookup"><span data-stu-id="43d44-118">Batch videos</span></span>](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a><span data-ttu-id="43d44-119">HPC-fürt megoldások</span><span class="sxs-lookup"><span data-stu-id="43d44-119">HPC cluster solutions</span></span>
<span data-ttu-id="43d44-120">Központi telepítése, illetve kibővíthetők a meglévő Windows vagy Linux HPC fürt tooAzure toorun a számítási igényes munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="43d44-120">Deploy or extend your existing Windows or Linux HPC cluster tooAzure toorun your compute intensive workloads.</span></span>  

### <a name="microsoft-hpc-pack"></a><span data-ttu-id="43d44-121">A Microsoft HPC csomag</span><span class="sxs-lookup"><span data-stu-id="43d44-121">Microsoft HPC Pack</span></span>
<span data-ttu-id="43d44-122">HPC Pack a Microsoft Windows és Linux HPC munkaterhelések futtatására képes, vagyis a Microsoft Azure és a Windows Server technológiáira szabad HPC megoldása.</span><span class="sxs-lookup"><span data-stu-id="43d44-122">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies, capable of running Windows and Linux HPC workloads.</span></span>  

* [<span data-ttu-id="43d44-123">HPC Pack 2016 letöltése</span><span class="sxs-lookup"><span data-stu-id="43d44-123">Download HPC Pack 2016</span></span>](https://www.microsoft.com/download/details.aspx?id=54507)
* [<span data-ttu-id="43d44-124">Töltse le a HPC Pack 2012 R2 Update 3</span><span class="sxs-lookup"><span data-stu-id="43d44-124">Download HPC Pack 2012 R2 Update 3</span></span>](https://www.microsoft.com/download/details.aspx?id=49922)
* [<span data-ttu-id="43d44-125">Dokumentáció</span><span class="sxs-lookup"><span data-stu-id="43d44-125">Documentation</span></span>](https://technet.microsoft.com/library/jj899572.aspx)
* <span data-ttu-id="43d44-126">HPC Pack fürt beállításai az Azure-ban: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="43d44-126">HPC Pack cluster options in Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) and [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span> 
* [<span data-ttu-id="43d44-127">Kapacitásnövelés a HPC Pack tooAzure worker-példányok</span><span class="sxs-lookup"><span data-stu-id="43d44-127">Burst tooAzure worker instances with HPC Pack</span></span>](https://technet.microsoft.com/library/gg481749.aspx)
* [<span data-ttu-id="43d44-128">Kapacitásnövelés a HPC Pack kötegelt tooAzure</span><span class="sxs-lookup"><span data-stu-id="43d44-128">Burst tooAzure  Batch with HPC Pack</span></span>](https://technet.microsoft.com/library/mt612877.aspx)
* [<span data-ttu-id="43d44-129">A Windows HPC-fórumok</span><span class="sxs-lookup"><span data-stu-id="43d44-129">Windows HPC forums</span></span>](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a><span data-ttu-id="43d44-130">Linux- és OSS fürtmegoldások</span><span class="sxs-lookup"><span data-stu-id="43d44-130">Linux and OSS cluster solutions</span></span>
<span data-ttu-id="43d44-131">Ezek Azure-sablonok toodeploy Linux HPC-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="43d44-131">Use these Azure templates toodeploy Linux HPC clusters.</span></span>

* <span data-ttu-id="43d44-132">[Léptetéses SLURM fürt](https://azure.microsoft.com/documentation/templates/slurm/) és [blogbejegyzés](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="43d44-132">[Spin up a SLURM cluster](https://azure.microsoft.com/documentation/templates/slurm/) and [blog post](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span></span>
* [<span data-ttu-id="43d44-133">Léptetéses nyomaték fürt</span><span class="sxs-lookup"><span data-stu-id="43d44-133">Spin up a Torque cluster</span></span>](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [<span data-ttu-id="43d44-134">Rács sablonok számítási PBS Professional</span><span class="sxs-lookup"><span data-stu-id="43d44-134">Compute grid templates with PBS Professional</span></span>](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

## <a name="hpc-storage"></a><span data-ttu-id="43d44-135">HPC-tároló</span><span class="sxs-lookup"><span data-stu-id="43d44-135">HPC storage</span></span>
* [<span data-ttu-id="43d44-136">Párhuzamos fájlrendszerek HPC tárolás az Azure-on</span><span class="sxs-lookup"><span data-stu-id="43d44-136">Parallel file systems for HPC storage on Azure</span></span>](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)
* [<span data-ttu-id="43d44-137">Intel felhő Edition fényesség szoftver - kiértékelés</span><span class="sxs-lookup"><span data-stu-id="43d44-137">Intel Cloud Edition for Lustre Software - Eval</span></span>](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)
* [<span data-ttu-id="43d44-138">CentOS 7.2 sablon BeeGFS</span><span class="sxs-lookup"><span data-stu-id="43d44-138">BeeGFS on CentOS 7.2 template</span></span>](https://github.com/smith1511/hpc/tree/master/beegfs-shared-on-centos7.2)




## <a name="microsoft-mpi"></a><span data-ttu-id="43d44-139">Microsoft MPI</span><span class="sxs-lookup"><span data-stu-id="43d44-139">Microsoft MPI</span></span>
<span data-ttu-id="43d44-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) rendszer hello üzenet átadásakor felület szabványos fejlesztéséhez és a Windows hello platformon futó párhuzamos alkalmazások a Microsoft általi implementációja.</span><span class="sxs-lookup"><span data-stu-id="43d44-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) is a Microsoft implementation of hello Message Passing Interface standard for developing and running parallel applications on hello Windows platform.</span></span>

* [<span data-ttu-id="43d44-141">MS-MPI letöltése</span><span class="sxs-lookup"><span data-stu-id="43d44-141">Download MS-MPI</span></span>](http://go.microsoft.com/FWLink/p/?LinkID=389556)
* [<span data-ttu-id="43d44-142">MS-MPI-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="43d44-142">MS-MPI reference</span></span>](https://msdn.microsoft.com/library/dn473458.aspx)
* [<span data-ttu-id="43d44-143">MPI-fórum</span><span class="sxs-lookup"><span data-stu-id="43d44-143">MPI forum</span></span>](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a><span data-ttu-id="43d44-144">Számításigényes példányok</span><span class="sxs-lookup"><span data-stu-id="43d44-144">Compute-intensive instances</span></span>
<span data-ttu-id="43d44-145">Az Azure-ajánlatok egy [tartomány a Virtuálisgép-méretek](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), beleértve [számítási igényű H-sorozat](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) csatlakozni tudnak tooa háttér-RDMA hálózati, toorun a Linux és a Windows HPC munkaterhelések példányok.</span><span class="sxs-lookup"><span data-stu-id="43d44-145">Azure offers a [range of VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), including [compute-intensive H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instances capable of connecting tooa back-end RDMA network, toorun your Linux and Windows HPC workloads.</span></span> 

* [<span data-ttu-id="43d44-146">A Linux RDMA fürt toorun MPI alkalmazások beállítása</span><span class="sxs-lookup"><span data-stu-id="43d44-146">Set up a Linux RDMA cluster toorun MPI applications</span></span>](../virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="43d44-147">A Microsoft HPC Pack toorun MPI alkalmazások Windows RDMA fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="43d44-147">Set up a Windows RDMA cluster with Microsoft HPC Pack toorun MPI applications</span></span>](../virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

<span data-ttu-id="43d44-148">GPU-igényes munkaterhelések, tekintse meg [NC és a portok HV](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span><span class="sxs-lookup"><span data-stu-id="43d44-148">For GPU-intensive workloads, check out [NC and NV sizes](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span></span>

## <a name="samples-and-demos"></a><span data-ttu-id="43d44-149">Kódminták és bemutatók</span><span class="sxs-lookup"><span data-stu-id="43d44-149">Samples and demos</span></span>
* [<span data-ttu-id="43d44-150">Azure Batch C# és Python-Kódminták</span><span class="sxs-lookup"><span data-stu-id="43d44-150">Azure Batch C# and Python code samples</span></span>](https://github.com/Azure/azure-batch-samples)
* <span data-ttu-id="43d44-151">[A Batch-hajógyárnak](https://azure.github.io/batch-shipyard/) kötegelt stílusú Dockerized munkaterhelések tooAzure kötegelt az egyszerű telepítés eszköztára</span><span class="sxs-lookup"><span data-stu-id="43d44-151">[Batch Shipyard](https://azure.github.io/batch-shipyard/) toolkit for easy deployment of batch-style Dockerized workloads tooAzure Batch</span></span>
* <span data-ttu-id="43d44-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) R csomagot épülő Azure Batch</span><span class="sxs-lookup"><span data-stu-id="43d44-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) R package, built on top of Azure Batch</span></span>
* [<span data-ttu-id="43d44-153">SUSE Linux Enterprise Server HPC tesztelése</span><span class="sxs-lookup"><span data-stu-id="43d44-153">Test drive SUSE Linux Enterprise Server for HPC</span></span>](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a><span data-ttu-id="43d44-154">Kapcsolódó Azure-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="43d44-154">Related Azure services</span></span>
* [<span data-ttu-id="43d44-155">Data Factory</span><span class="sxs-lookup"><span data-stu-id="43d44-155">Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)
* [<span data-ttu-id="43d44-156">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="43d44-156">Machine Learning</span></span>](https://azure.microsoft.com/documentation/services/machine-learning/)
* [<span data-ttu-id="43d44-157">HDInsight</span><span class="sxs-lookup"><span data-stu-id="43d44-157">HDInsight</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="43d44-158">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="43d44-158">Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="43d44-159">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="43d44-159">Virtual Machine Scale Sets</span></span>](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)
* [<span data-ttu-id="43d44-160">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="43d44-160">Cloud Services</span></span>](https://azure.microsoft.com/documentation/services/cloud-services/)
* [<span data-ttu-id="43d44-161">APP SERVICE</span><span class="sxs-lookup"><span data-stu-id="43d44-161">App Service</span></span>](https://azure.microsoft.com/documentation/services/app-service/)
* [<span data-ttu-id="43d44-162">Médiaszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="43d44-162">Media Services</span></span>](https://azure.microsoft.com/documentation/services/media-services/)
* [<span data-ttu-id="43d44-163">Functions</span><span class="sxs-lookup"><span data-stu-id="43d44-163">Functions</span></span>](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a><span data-ttu-id="43d44-164">Az architektúrát szemléltető ábrák</span><span class="sxs-lookup"><span data-stu-id="43d44-164">Architecture blueprints</span></span>
* <span data-ttu-id="43d44-165">[Azure Batch és az Azure Data Factory használatával HPC és az orchestration](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) és [cikk](../data-factory/data-factory-data-processing-using-batch.md)</span><span class="sxs-lookup"><span data-stu-id="43d44-165">[HPC and data orchestration using Azure Batch and Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) and [article](../data-factory/data-factory-data-processing-using-batch.md)</span></span>

## <a name="industry-solutions"></a><span data-ttu-id="43d44-166">Iparági megoldások</span><span class="sxs-lookup"><span data-stu-id="43d44-166">Industry solutions</span></span>
* [<span data-ttu-id="43d44-167">Banki és nagy piacok</span><span class="sxs-lookup"><span data-stu-id="43d44-167">Banking and capital markets</span></span>](https://finance.azure.com/)
* [<span data-ttu-id="43d44-168">Mérnöki csapathoz szimulációja</span><span class="sxs-lookup"><span data-stu-id="43d44-168">Engineering simulations</span></span>](https://simulation.azure.com/) 

## <a name="customer-stories"></a><span data-ttu-id="43d44-169">Ügyfelek történetei</span><span class="sxs-lookup"><span data-stu-id="43d44-169">Customer stories</span></span>
* [<span data-ttu-id="43d44-170">ANEO</span><span class="sxs-lookup"><span data-stu-id="43d44-170">ANEO</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 
* [<span data-ttu-id="43d44-171">d3View</span><span class="sxs-lookup"><span data-stu-id="43d44-171">d3View</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)
* [<span data-ttu-id="43d44-172">Ludwig Intézet kapcsolatos kutatás</span><span class="sxs-lookup"><span data-stu-id="43d44-172">Ludwig Institute of Cancer Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)
* [<span data-ttu-id="43d44-173">A Microsoft Research</span><span class="sxs-lookup"><span data-stu-id="43d44-173">Microsoft Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)
* [<span data-ttu-id="43d44-174">Milliman</span><span class="sxs-lookup"><span data-stu-id="43d44-174">Milliman</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)
* [<span data-ttu-id="43d44-175">Mitsubishi UFJ biztosítékok nemzetközi</span><span class="sxs-lookup"><span data-stu-id="43d44-175">Mitsubishi UFJ Securities International</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)
* [<span data-ttu-id="43d44-176">Schlumberger</span><span class="sxs-lookup"><span data-stu-id="43d44-176">Schlumberger</span></span>](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [<span data-ttu-id="43d44-177">Torony Watson</span><span class="sxs-lookup"><span data-stu-id="43d44-177">Towers Watson</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)
* [<span data-ttu-id="43d44-178">UberCloud</span><span class="sxs-lookup"><span data-stu-id="43d44-178">UberCloud</span></span>](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

## <a name="next-steps"></a><span data-ttu-id="43d44-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43d44-179">Next steps</span></span>
* <span data-ttu-id="43d44-180">Hello legújabb közlemények, lásd: hello [Microsoft HPC és kötegelt csapat blogja](http://blogs.technet.com/b/windowshpc/) és hello [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span><span class="sxs-lookup"><span data-stu-id="43d44-180">For hello latest announcements, see hello [Microsoft HPC and Batch team blog](http://blogs.technet.com/b/windowshpc/) and hello [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span></span>
* <span data-ttu-id="43d44-181">Lásd még: [what's new in kötegelt](https://azure.microsoft.com/updates/?service=batch) , illetve fizethetnek toohello [RSS-hírcsatorna](https://azure.microsoft.com/updates/feed/?service=batch).</span><span class="sxs-lookup"><span data-stu-id="43d44-181">Also see [what's new in Batch](https://azure.microsoft.com/updates/?service=batch) or subscribe toohello [RSS feed](https://azure.microsoft.com/updates/feed/?service=batch).</span></span>

