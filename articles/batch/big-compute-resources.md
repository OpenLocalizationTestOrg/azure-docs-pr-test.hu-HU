---
title: "A kötegelt és Azure felhőben HPC erőforrások |} Microsoft Docs"
description: "Azok a műszaki források segítségével is futtatható, a nagyméretű párhuzamos köteg és nagy teljesítményű számítástechnikai (HPC) munkaterhelések az Azure-ban."
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
ms.openlocfilehash: 18be9f503b57117a7e8f5f0a4e9c93614cc7755b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a><span data-ttu-id="45fe4-103">Az Azure-ban nagy számítási: a kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások</span><span class="sxs-lookup"><span data-stu-id="45fe4-103">Big Compute in Azure: Technical resources for batch and high-performance computing</span></span>
<span data-ttu-id="45fe4-104">Ez a műszaki forrásanyagaihoz útmutatóját segítséget nyújt az Azure-ban a nagy méretű párhuzamos köteg és nagy teljesítményű számítástechnikai rendszerek (HPC) munkaterhelések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="45fe4-104">This is a guide to technical resources to help you run your large-scale parallel, batch, and high-performance computing (HPC) workloads in Azure.</span></span> <span data-ttu-id="45fe4-105">A meglévő kötegelt vagy HPC-munkaterhelések az Azure felhőbe bővítheti, vagy új nagy számítási megoldások egy tartományt az Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="45fe4-105">Extend your existing batch or HPC workloads to the Azure cloud, or build new Big Compute solutions using a range of Azure services.</span></span>

## <a name="solutions-options"></a><span data-ttu-id="45fe4-106">Megoldás beállításai</span><span class="sxs-lookup"><span data-stu-id="45fe4-106">Solutions options</span></span>
<span data-ttu-id="45fe4-107">További tudnivalók az Azure-ban nagy számítási beállítások, és válassza ki a megfelelő módszer az alkalmazások és szolgáltatások és az üzleti igényeknek.</span><span class="sxs-lookup"><span data-stu-id="45fe4-107">Learn about Big Compute options in Azure, and choose the right approach for your workload and business need.</span></span>

* [<span data-ttu-id="45fe4-108">Kötegelt vagy HPC-megoldások</span><span class="sxs-lookup"><span data-stu-id="45fe4-108">Batch and HPC solutions</span></span>](batch-hpc-solutions.md)
* [<span data-ttu-id="45fe4-109">Videó: Nagy számítási az Azure és a HPC a felhőben</span><span class="sxs-lookup"><span data-stu-id="45fe4-109">Video: Big Compute in the cloud with Azure and HPC</span></span>](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)

## <a name="azure-batch"></a><span data-ttu-id="45fe4-110">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="45fe4-110">Azure Batch</span></span>
<span data-ttu-id="45fe4-111">[Kötegelt](https://azure.microsoft.com/services/batch/) platform szolgáltatás, amely megkönnyíti a felhő-engedélyezése a Linux és Windows-alkalmazások és a feladatok futtatása és a fürt és a feladat ütemezési irányítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="45fe4-111">[Batch](https://azure.microsoft.com/services/batch/) is a platform service that makes it easy to cloud-enable your Linux and Windows applications and run jobs without setting up and managing a cluster and job scheduler.</span></span> <span data-ttu-id="45fe4-112">Az SDK-val ügyfél alkalmazások integrálása az Azure Batch keresztül különböző nyelveken szakasz adatokat az Azure-ba, és build feladat végrehajtási folyamatok.</span><span class="sxs-lookup"><span data-stu-id="45fe4-112">Use the SDK to integrate client applications with Azure Batch through various languages, stage data to Azure, and build job execution pipelines.</span></span>

* [<span data-ttu-id="45fe4-113">Dokumentáció</span><span class="sxs-lookup"><span data-stu-id="45fe4-113">Documentation</span></span>](https://azure.microsoft.com/documentation/services/batch/)
* <span data-ttu-id="45fe4-114">[.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), és [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API-referencia</span><span class="sxs-lookup"><span data-stu-id="45fe4-114">[.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), and [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API reference</span></span>
* <span data-ttu-id="45fe4-115">[Batch management .NET kódtár](https://msdn.microsoft.com/library/mt463120.aspx) referencia</span><span class="sxs-lookup"><span data-stu-id="45fe4-115">[Batch management .NET library](https://msdn.microsoft.com/library/mt463120.aspx) reference</span></span>
* <span data-ttu-id="45fe4-116">Oktatóanyag: Ismerkedés a [Azure Batch .NET-keretrendszerhez készült](batch-dotnet-get-started.md) és [kötegelt Python-ügyfél](batch-python-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="45fe4-116">Tutorials: Get started with [Azure Batch library for .NET](batch-dotnet-get-started.md) and [Batch Python client](batch-python-tutorial.md)</span></span>
* [<span data-ttu-id="45fe4-117">Batch fórum</span><span class="sxs-lookup"><span data-stu-id="45fe4-117">Batch forum</span></span>](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)
* [<span data-ttu-id="45fe4-118">Kötegelt videók</span><span class="sxs-lookup"><span data-stu-id="45fe4-118">Batch videos</span></span>](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a><span data-ttu-id="45fe4-119">HPC-fürt megoldások</span><span class="sxs-lookup"><span data-stu-id="45fe4-119">HPC cluster solutions</span></span>
<span data-ttu-id="45fe4-120">Központi telepítése, illetve kibővíthetők a meglévő Windows vagy Linux HPC-fürtöt az Azure-ba, a számítási számításigényes munkaterhelést futtatni.</span><span class="sxs-lookup"><span data-stu-id="45fe4-120">Deploy or extend your existing Windows or Linux HPC cluster to Azure to run your compute intensive workloads.</span></span>  

### <a name="microsoft-hpc-pack"></a><span data-ttu-id="45fe4-121">A Microsoft HPC csomag</span><span class="sxs-lookup"><span data-stu-id="45fe4-121">Microsoft HPC Pack</span></span>
<span data-ttu-id="45fe4-122">HPC Pack a Microsoft Windows és Linux HPC munkaterhelések futtatására képes, vagyis a Microsoft Azure és a Windows Server technológiáira szabad HPC megoldása.</span><span class="sxs-lookup"><span data-stu-id="45fe4-122">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies, capable of running Windows and Linux HPC workloads.</span></span>  

* [<span data-ttu-id="45fe4-123">HPC Pack 2016 letöltése</span><span class="sxs-lookup"><span data-stu-id="45fe4-123">Download HPC Pack 2016</span></span>](https://www.microsoft.com/download/details.aspx?id=54507)
* [<span data-ttu-id="45fe4-124">Töltse le a HPC Pack 2012 R2 Update 3</span><span class="sxs-lookup"><span data-stu-id="45fe4-124">Download HPC Pack 2012 R2 Update 3</span></span>](https://www.microsoft.com/download/details.aspx?id=49922)
* [<span data-ttu-id="45fe4-125">Dokumentáció</span><span class="sxs-lookup"><span data-stu-id="45fe4-125">Documentation</span></span>](https://technet.microsoft.com/library/jj899572.aspx)
* <span data-ttu-id="45fe4-126">HPC Pack fürt beállításai az Azure-ban: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="45fe4-126">HPC Pack cluster options in Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) and [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span> 
* [<span data-ttu-id="45fe4-127">-Kapacitásnövelés a HPC Pack Azure feldolgozói példány</span><span class="sxs-lookup"><span data-stu-id="45fe4-127">Burst to Azure worker instances with HPC Pack</span></span>](https://technet.microsoft.com/library/gg481749.aspx)
* [<span data-ttu-id="45fe4-128">-Kapacitásnövelés a HPC Pack az Azure Batch</span><span class="sxs-lookup"><span data-stu-id="45fe4-128">Burst to Azure  Batch with HPC Pack</span></span>](https://technet.microsoft.com/library/mt612877.aspx)
* [<span data-ttu-id="45fe4-129">A Windows HPC-fórumok</span><span class="sxs-lookup"><span data-stu-id="45fe4-129">Windows HPC forums</span></span>](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a><span data-ttu-id="45fe4-130">Linux- és OSS fürtmegoldások</span><span class="sxs-lookup"><span data-stu-id="45fe4-130">Linux and OSS cluster solutions</span></span>
<span data-ttu-id="45fe4-131">Használja ezeket a sablonokat az Azure Linux HPC-fürtök üzembe helyezésekor.</span><span class="sxs-lookup"><span data-stu-id="45fe4-131">Use these Azure templates to deploy Linux HPC clusters.</span></span>

* <span data-ttu-id="45fe4-132">[Léptetéses SLURM fürt](https://azure.microsoft.com/documentation/templates/slurm/) és [blogbejegyzés](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="45fe4-132">[Spin up a SLURM cluster](https://azure.microsoft.com/documentation/templates/slurm/) and [blog post](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span></span>
* [<span data-ttu-id="45fe4-133">Léptetéses nyomaték fürt</span><span class="sxs-lookup"><span data-stu-id="45fe4-133">Spin up a Torque cluster</span></span>](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [<span data-ttu-id="45fe4-134">Rács sablonok számítási PBS Professional</span><span class="sxs-lookup"><span data-stu-id="45fe4-134">Compute grid templates with PBS Professional</span></span>](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

## <a name="hpc-storage"></a><span data-ttu-id="45fe4-135">HPC-tároló</span><span class="sxs-lookup"><span data-stu-id="45fe4-135">HPC storage</span></span>
* [<span data-ttu-id="45fe4-136">Párhuzamos fájlrendszerek HPC tárolás az Azure-on</span><span class="sxs-lookup"><span data-stu-id="45fe4-136">Parallel file systems for HPC storage on Azure</span></span>](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)
* [<span data-ttu-id="45fe4-137">Intel felhő Edition fényesség szoftver - kiértékelés</span><span class="sxs-lookup"><span data-stu-id="45fe4-137">Intel Cloud Edition for Lustre Software - Eval</span></span>](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)
* [<span data-ttu-id="45fe4-138">CentOS 7.2 sablon BeeGFS</span><span class="sxs-lookup"><span data-stu-id="45fe4-138">BeeGFS on CentOS 7.2 template</span></span>](https://github.com/smith1511/hpc/tree/master/beegfs-shared-on-centos7.2)




## <a name="microsoft-mpi"></a><span data-ttu-id="45fe4-139">Microsoft MPI</span><span class="sxs-lookup"><span data-stu-id="45fe4-139">Microsoft MPI</span></span>
<span data-ttu-id="45fe4-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) az üzenet továbbításához szabvány fejlesztéséhez és párhuzamos alkalmazások a Windows-platformon futó Microsoft-implementációját.</span><span class="sxs-lookup"><span data-stu-id="45fe4-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) is a Microsoft implementation of the Message Passing Interface standard for developing and running parallel applications on the Windows platform.</span></span>

* [<span data-ttu-id="45fe4-141">MS-MPI letöltése</span><span class="sxs-lookup"><span data-stu-id="45fe4-141">Download MS-MPI</span></span>](http://go.microsoft.com/FWLink/p/?LinkID=389556)
* [<span data-ttu-id="45fe4-142">MS-MPI-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="45fe4-142">MS-MPI reference</span></span>](https://msdn.microsoft.com/library/dn473458.aspx)
* [<span data-ttu-id="45fe4-143">MPI-fórum</span><span class="sxs-lookup"><span data-stu-id="45fe4-143">MPI forum</span></span>](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a><span data-ttu-id="45fe4-144">Számításigényes példányok</span><span class="sxs-lookup"><span data-stu-id="45fe4-144">Compute-intensive instances</span></span>
<span data-ttu-id="45fe4-145">Azure ajánlatok egy [tartomány a Virtuálisgép-méretek](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), többek között a következőket [számítási igényű H-sorozat](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) példány képes kapcsolódni a háttér-RDMA hálózati, a Linux és a Windows HPC munkaterhelések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="45fe4-145">Azure offers a [range of VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), including [compute-intensive H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instances capable of connecting to a back-end RDMA network, to run your Linux and Windows HPC workloads.</span></span> 

* [<span data-ttu-id="45fe4-146">MPI-alkalmazások futtatására Linux RDMA fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="45fe4-146">Set up a Linux RDMA cluster to run MPI applications</span></span>](../virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="45fe4-147">MPI-alkalmazások futtatására és a Microsoft HPC Pack Windows RDMA fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="45fe4-147">Set up a Windows RDMA cluster with Microsoft HPC Pack to run MPI applications</span></span>](../virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

<span data-ttu-id="45fe4-148">GPU-igényes munkaterhelések, tekintse meg [NC és a portok HV](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span><span class="sxs-lookup"><span data-stu-id="45fe4-148">For GPU-intensive workloads, check out [NC and NV sizes](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span></span>

## <a name="samples-and-demos"></a><span data-ttu-id="45fe4-149">Kódminták és bemutatók</span><span class="sxs-lookup"><span data-stu-id="45fe4-149">Samples and demos</span></span>
* [<span data-ttu-id="45fe4-150">Azure Batch C# és Python-Kódminták</span><span class="sxs-lookup"><span data-stu-id="45fe4-150">Azure Batch C# and Python code samples</span></span>](https://github.com/Azure/azure-batch-samples)
* <span data-ttu-id="45fe4-151">[A Batch-hajógyárnak](https://azure.github.io/batch-shipyard/) a batch-stílusú Dockerized munkaterhelések Azure Batch egyszerű telepítés eszköztára</span><span class="sxs-lookup"><span data-stu-id="45fe4-151">[Batch Shipyard](https://azure.github.io/batch-shipyard/) toolkit for easy deployment of batch-style Dockerized workloads to Azure Batch</span></span>
* <span data-ttu-id="45fe4-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) R csomagot épülő Azure Batch</span><span class="sxs-lookup"><span data-stu-id="45fe4-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) R package, built on top of Azure Batch</span></span>
* [<span data-ttu-id="45fe4-153">SUSE Linux Enterprise Server HPC tesztelése</span><span class="sxs-lookup"><span data-stu-id="45fe4-153">Test drive SUSE Linux Enterprise Server for HPC</span></span>](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a><span data-ttu-id="45fe4-154">Kapcsolódó Azure-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="45fe4-154">Related Azure services</span></span>
* [<span data-ttu-id="45fe4-155">Data Factory</span><span class="sxs-lookup"><span data-stu-id="45fe4-155">Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)
* [<span data-ttu-id="45fe4-156">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="45fe4-156">Machine Learning</span></span>](https://azure.microsoft.com/documentation/services/machine-learning/)
* [<span data-ttu-id="45fe4-157">HDInsight</span><span class="sxs-lookup"><span data-stu-id="45fe4-157">HDInsight</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="45fe4-158">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="45fe4-158">Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="45fe4-159">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="45fe4-159">Virtual Machine Scale Sets</span></span>](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)
* [<span data-ttu-id="45fe4-160">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="45fe4-160">Cloud Services</span></span>](https://azure.microsoft.com/documentation/services/cloud-services/)
* [<span data-ttu-id="45fe4-161">APP SERVICE</span><span class="sxs-lookup"><span data-stu-id="45fe4-161">App Service</span></span>](https://azure.microsoft.com/documentation/services/app-service/)
* [<span data-ttu-id="45fe4-162">Médiaszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="45fe4-162">Media Services</span></span>](https://azure.microsoft.com/documentation/services/media-services/)
* [<span data-ttu-id="45fe4-163">Functions</span><span class="sxs-lookup"><span data-stu-id="45fe4-163">Functions</span></span>](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a><span data-ttu-id="45fe4-164">Az architektúrát szemléltető ábrák</span><span class="sxs-lookup"><span data-stu-id="45fe4-164">Architecture blueprints</span></span>
* <span data-ttu-id="45fe4-165">[Azure Batch és az Azure Data Factory használatával HPC és az orchestration](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) és [cikk](../data-factory/data-factory-data-processing-using-batch.md)</span><span class="sxs-lookup"><span data-stu-id="45fe4-165">[HPC and data orchestration using Azure Batch and Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) and [article](../data-factory/data-factory-data-processing-using-batch.md)</span></span>

## <a name="industry-solutions"></a><span data-ttu-id="45fe4-166">Iparági megoldások</span><span class="sxs-lookup"><span data-stu-id="45fe4-166">Industry solutions</span></span>
* [<span data-ttu-id="45fe4-167">Banki és nagy piacok</span><span class="sxs-lookup"><span data-stu-id="45fe4-167">Banking and capital markets</span></span>](https://finance.azure.com/)
* [<span data-ttu-id="45fe4-168">Mérnöki csapathoz szimulációja</span><span class="sxs-lookup"><span data-stu-id="45fe4-168">Engineering simulations</span></span>](https://simulation.azure.com/) 

## <a name="customer-stories"></a><span data-ttu-id="45fe4-169">Ügyfelek történetei</span><span class="sxs-lookup"><span data-stu-id="45fe4-169">Customer stories</span></span>
* [<span data-ttu-id="45fe4-170">ANEO</span><span class="sxs-lookup"><span data-stu-id="45fe4-170">ANEO</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 
* [<span data-ttu-id="45fe4-171">d3View</span><span class="sxs-lookup"><span data-stu-id="45fe4-171">d3View</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)
* [<span data-ttu-id="45fe4-172">Ludwig Intézet kapcsolatos kutatás</span><span class="sxs-lookup"><span data-stu-id="45fe4-172">Ludwig Institute of Cancer Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)
* [<span data-ttu-id="45fe4-173">A Microsoft Research</span><span class="sxs-lookup"><span data-stu-id="45fe4-173">Microsoft Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)
* [<span data-ttu-id="45fe4-174">Milliman</span><span class="sxs-lookup"><span data-stu-id="45fe4-174">Milliman</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)
* [<span data-ttu-id="45fe4-175">Mitsubishi UFJ biztosítékok nemzetközi</span><span class="sxs-lookup"><span data-stu-id="45fe4-175">Mitsubishi UFJ Securities International</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)
* [<span data-ttu-id="45fe4-176">Schlumberger</span><span class="sxs-lookup"><span data-stu-id="45fe4-176">Schlumberger</span></span>](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [<span data-ttu-id="45fe4-177">Torony Watson</span><span class="sxs-lookup"><span data-stu-id="45fe4-177">Towers Watson</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)
* [<span data-ttu-id="45fe4-178">UberCloud</span><span class="sxs-lookup"><span data-stu-id="45fe4-178">UberCloud</span></span>](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

## <a name="next-steps"></a><span data-ttu-id="45fe4-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45fe4-179">Next steps</span></span>
* <span data-ttu-id="45fe4-180">A legújabb bejelentésekért lásd: [A Microsoft HPC és Batch csapatának blogja](http://blogs.technet.com/b/windowshpc/) és [Azure-blog](https://azure.microsoft.com/blog/tag/hpc/).</span><span class="sxs-lookup"><span data-stu-id="45fe4-180">For the latest announcements, see the [Microsoft HPC and Batch team blog](http://blogs.technet.com/b/windowshpc/) and the [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span></span>
* <span data-ttu-id="45fe4-181">Lásd még: [what's new in kötegelt](https://azure.microsoft.com/updates/?service=batch) , illetve fizethetnek elő a [RSS-hírcsatorna](https://azure.microsoft.com/updates/feed/?service=batch).</span><span class="sxs-lookup"><span data-stu-id="45fe4-181">Also see [what's new in Batch](https://azure.microsoft.com/updates/?service=batch) or subscribe to the [RSS feed](https://azure.microsoft.com/updates/feed/?service=batch).</span></span>

