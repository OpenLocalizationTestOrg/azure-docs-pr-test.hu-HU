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
# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a>Az Azure-ban nagy számítási: a kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások
Ez a műszaki forrásanyagaihoz útmutatóját segítséget nyújt az Azure-ban a nagy méretű párhuzamos köteg és nagy teljesítményű számítástechnikai rendszerek (HPC) munkaterhelések futtatásához. A meglévő kötegelt vagy HPC-munkaterhelések az Azure felhőbe bővítheti, vagy új nagy számítási megoldások egy tartományt az Azure-szolgáltatásokkal.

## <a name="solutions-options"></a>Megoldás beállításai
További tudnivalók az Azure-ban nagy számítási beállítások, és válassza ki a megfelelő módszer az alkalmazások és szolgáltatások és az üzleti igényeknek.

* [Kötegelt vagy HPC-megoldások](batch-hpc-solutions.md)
* [Videó: Nagy számítási az Azure és a HPC a felhőben](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)

## <a name="azure-batch"></a>Azure Batch
[Kötegelt](https://azure.microsoft.com/services/batch/) platform szolgáltatás, amely megkönnyíti a felhő-engedélyezése a Linux és Windows-alkalmazások és a feladatok futtatása és a fürt és a feladat ütemezési irányítása nélkül. Az SDK-val ügyfél alkalmazások integrálása az Azure Batch keresztül különböző nyelveken szakasz adatokat az Azure-ba, és build feladat végrehajtási folyamatok.

* [Dokumentáció](https://azure.microsoft.com/documentation/services/batch/)
* [.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), és [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API-referencia
* [Batch management .NET kódtár](https://msdn.microsoft.com/library/mt463120.aspx) referencia
* Oktatóanyag: Ismerkedés a [Azure Batch .NET-keretrendszerhez készült](batch-dotnet-get-started.md) és [kötegelt Python-ügyfél](batch-python-tutorial.md)
* [Batch fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)
* [Kötegelt videók](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a>HPC-fürt megoldások
Központi telepítése, illetve kibővíthetők a meglévő Windows vagy Linux HPC-fürtöt az Azure-ba, a számítási számításigényes munkaterhelést futtatni.  

### <a name="microsoft-hpc-pack"></a>A Microsoft HPC csomag
HPC Pack a Microsoft Windows és Linux HPC munkaterhelések futtatására képes, vagyis a Microsoft Azure és a Windows Server technológiáira szabad HPC megoldása.  

* [HPC Pack 2016 letöltése](https://www.microsoft.com/download/details.aspx?id=54507)
* [Töltse le a HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49922)
* [Dokumentáció](https://technet.microsoft.com/library/jj899572.aspx)
* HPC Pack fürt beállításai az Azure-ban: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 
* [-Kapacitásnövelés a HPC Pack Azure feldolgozói példány](https://technet.microsoft.com/library/gg481749.aspx)
* [-Kapacitásnövelés a HPC Pack az Azure Batch](https://technet.microsoft.com/library/mt612877.aspx)
* [A Windows HPC-fórumok](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a>Linux- és OSS fürtmegoldások
Használja ezeket a sablonokat az Azure Linux HPC-fürtök üzembe helyezésekor.

* [Léptetéses SLURM fürt](https://azure.microsoft.com/documentation/templates/slurm/) és [blogbejegyzés](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)
* [Léptetéses nyomaték fürt](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [Rács sablonok számítási PBS Professional](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

## <a name="hpc-storage"></a>HPC-tároló
* [Párhuzamos fájlrendszerek HPC tárolás az Azure-on](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)
* [Intel felhő Edition fényesség szoftver - kiértékelés](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)
* [CentOS 7.2 sablon BeeGFS](https://github.com/smith1511/hpc/tree/master/beegfs-shared-on-centos7.2)




## <a name="microsoft-mpi"></a>Microsoft MPI
[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) az üzenet továbbításához szabvány fejlesztéséhez és párhuzamos alkalmazások a Windows-platformon futó Microsoft-implementációját.

* [MS-MPI letöltése](http://go.microsoft.com/FWLink/p/?LinkID=389556)
* [MS-MPI-hivatkozás](https://msdn.microsoft.com/library/dn473458.aspx)
* [MPI-fórum](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a>Számításigényes példányok
Azure ajánlatok egy [tartomány a Virtuálisgép-méretek](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), többek között a következőket [számítási igényű H-sorozat](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) példány képes kapcsolódni a háttér-RDMA hálózati, a Linux és a Windows HPC munkaterhelések futtatásához. 

* [MPI-alkalmazások futtatására Linux RDMA fürt beállítása](../virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MPI-alkalmazások futtatására és a Microsoft HPC Pack Windows RDMA fürt beállítása](../virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

GPU-igényes munkaterhelések, tekintse meg [NC és a portok HV](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).

## <a name="samples-and-demos"></a>Kódminták és bemutatók
* [Azure Batch C# és Python-Kódminták](https://github.com/Azure/azure-batch-samples)
* [A Batch-hajógyárnak](https://azure.github.io/batch-shipyard/) a batch-stílusú Dockerized munkaterhelések Azure Batch egyszerű telepítés eszköztára
* [doAzureParallel](http://www.github.com/Azure/doAzureParallel) R csomagot épülő Azure Batch
* [SUSE Linux Enterprise Server HPC tesztelése](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a>Kapcsolódó Azure-szolgáltatások
* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Virtual Machine Scale Sets](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)
* [Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)
* [APP SERVICE](https://azure.microsoft.com/documentation/services/app-service/)
* [Médiaszolgáltatások](https://azure.microsoft.com/documentation/services/media-services/)
* [Functions](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a>Az architektúrát szemléltető ábrák
* [Azure Batch és az Azure Data Factory használatával HPC és az orchestration](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) és [cikk](../data-factory/data-factory-data-processing-using-batch.md)

## <a name="industry-solutions"></a>Iparági megoldások
* [Banki és nagy piacok](https://finance.azure.com/)
* [Mérnöki csapathoz szimulációja](https://simulation.azure.com/) 

## <a name="customer-stories"></a>Ügyfelek történetei
* [ANEO](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 
* [d3View](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)
* [Ludwig Intézet kapcsolatos kutatás](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)
* [A Microsoft Research](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)
* [Milliman](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)
* [Mitsubishi UFJ biztosítékok nemzetközi](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Torony Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)
* [UberCloud](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

## <a name="next-steps"></a>Következő lépések
* A legújabb bejelentésekért lásd: [A Microsoft HPC és Batch csapatának blogja](http://blogs.technet.com/b/windowshpc/) és [Azure-blog](https://azure.microsoft.com/blog/tag/hpc/).
* Lásd még: [what's new in kötegelt](https://azure.microsoft.com/updates/?service=batch) , illetve fizethetnek elő a [RSS-hírcsatorna](https://azure.microsoft.com/updates/feed/?service=batch).

