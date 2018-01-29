Szervezet már rendelkezik a nagyméretű számítógépes igényeinek. Az ilyen nagy számítási terhelések például a műszaki tervezés és elemzés, pénzügyi kockázat számítások, kép megjelenítési, összetett modellezési, Monte Carlo szimulációja és több. 

Az Azure felhőalapú segítségével hatékonyan alkalmazásokat és szolgáltatásokat futtathatnak számítási igényű Linux és a Windows, a hagyományos HPC szimulációja párhuzamos kötegelt feladatok. Futtassa a HPC, és az Azure infrastruktúra, és a választott kötegelt munkaterhelését számítási szolgáltatások, a rács kezelők, a piactér megoldások és a szállító által szolgáltatott (SaaS) alkalmazások. Azure munkahelyi terjesztése és méretezhető, több ezer virtuális gépek vagy mag, vagy ha kevesebb erőforrást kell majd csökkentheti rugalmas megoldást kínál. 



## <a name="solution-options"></a>Megoldás beállításai



* **Saját munka megoldások**
    * Állítsa be a saját fürt környezetet az Azure virtuális gépeken vagy [virtuálisgép-méretezési csoportok](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Növekedési és az eltolás mértékét megadó egy helyi fürtöt, vagy az Azure-ban további kapacitást az új fürt központi telepítése. 
    * Bevezető telepítése Azure Resource Manager-sablonok segítségével [munkaterhelés kezelők](#workload-managers), infrastruktúra, és [alkalmazások](#hpc-applications). 
    * Válasszon [HPC-és a GPU VM](#hpc-and-gpu-sizes) , amelyek tartalmazzák a speciális hardver- és hálózati kapcsolatok MPI vagy GPU munkaterhelésekhez. 
    * Adja hozzá [nagy teljesítményű tárolási](#hpc-storage) I/O-igényes munkaterhelések.
* **Hibrid megoldások**
    * A helyszíni megoldás kiszervezéséhez ("kapacitásnövelés") csúcs munkaterhelések Azure-infrastruktúra bővítése
    * A meglévő felhőalapú számítási igény használata [munkaterhelés manager](#workload-manager).
    * Előnyeit [HPC-és a GPU VM](#hpc-and-gpu-sizes) MPI vagy GPU munkaterhelésekhez.
* **Nagy számítási megoldások szolgáltatásként**
    * Egyéni nagy számítási megoldások és munkafolyamatok használatával [Azure Batch](#azure-batch) és kapcsolódó [Azure-szolgáltatások](#related-azure-services).
    * Futtassa az Azure-kompatibilis termékgondozó csoportja és a szimuláció megoldások beleértve szállítóktól származó [Altair](http://www.altair.com/), [átméretezése](https://www.rescale.com/azure/), és [ciklus számítástechnikai](https://cyclecomputing.com/) (most [csatlakoztatni a Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
* **Piactér-megoldások**
    * A skála használati [HPC-alkalmazásokhoz](#hpc-applications) és [megoldások](#marketplace-solutions) érhető el a [Azure piactér](https://azuremarketplace.microsoft.com/). 
    


A következő szakaszokban további információt a támogató technológiákat és útmutatást mutató hivatkozásokat tartalmaz.



## <a name="marketplace-solutions"></a>Piactér-megoldások

Látogasson el a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/) a Linux és a Windows virtuális gép képek és készült HPC megoldások. Példák erre vonatkozóan:

* [RogueWave CentOS-alapú HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
* [SUSE Linux Enterprise Server HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [TIBCO rács Server motor](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Az Azure Data tudományos Windows és Linux rendszerű virtuális gép](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [D3View](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)
* [Intel felhő Edition fényesség](https://azuremarketplace.microsoft.com/marketplace/apps/intel.lustre-cloud-edition-eval?tab=Overview)


 
## <a name="hpc-applications"></a>HPC-alkalmazásokhoz

Futtassa az egyéni vagy kereskedelmi HPC-alkalmazásokhoz az Azure-ban. Ebben a szakaszban néhány példa további virtuális gépek hatékony méretezést vagy magok számítási rendszer benchmarked. Látogasson el a [Azure piactér](https://marketplace.azure.com) megoldások kész a központi telepítése.

> [!NOTE]
> Ellenőrizze a licencelési vagy más korlátozásokat a felhőben futó összes kereskedelmi alkalmazás gyártójával. Nem minden szállító kínál használatalapú licencet. Lehet, hogy a megoldást a felhőben egy licenckiszolgálóra kell, vagy a helyszíni licenc kiszolgálóhoz kapcsolódni.

### <a name="engineering-applications"></a>Mérnöki csapathoz alkalmazások


* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB elosztott számítástechnikai kiszolgáló](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM +](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



### <a name="graphics-and-rendering"></a>Grafikus és megjelenítés

* [Autodesk Maya, 3ds Max és Arnold](../articles/batch/batch-rendering-service.md) Azure Batch (előzetes verzió)

### <a name="ai-and-deep-learning"></a>AI és részletes tanulás

* [A Batch-AI](../articles/batch-ai/overview.md) mély tanulási modellek betanítása
* [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Virtuális gép tanulási mély](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Részletes biztonságával hajógyárnak receptet köteg](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)






## <a name="hpc-and-gpu-vm-sizes"></a>HPC-és a GPU VM
Azure számos mérete [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuális gépeken, beleértve a számítási-igényes munkaterhelések számára tervezett méretét. Például H16r és H16mr virtuális gépek magas teljesítmény háttér-RDMA hálózati is elérheti. A felhő hálózati javíthatja alatt futó szorosan összekapcsolt párhuzamos alkalmazások teljesítményének [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) vagy Intel MPI. 

N sorozatú virtuális gépek szolgáltatás NVIDIA Feldolgozóegységekkel, beleértve a mesterséges intelligencia (AI) learning és a képi megjelenítés számítási igényű vagy grafikai igényű alkalmazások számára tervezett. 

További információ:

* Nagy teljesítményű számítási mérete [Linux](../articles/virtual-machines/linux/sizes-hpc.md) és [Windows](../articles/virtual-machines/windows/sizes-hpc.md) virtuális gépek 
* GPU-kompatibilis mérete [Linux](../articles/virtual-machines/linux/sizes-gpu.md) és [Windows](../articles/virtual-machines/windows/sizes-gpu.md) virtuális gépek 

Az alábbiak végrehajtásának módját ismerheti meg:

* [MPI-alkalmazások futtatására Linux RDMA fürt beállítása](../articles/virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MPI-alkalmazások futtatására és a Microsoft HPC Pack Windows RDMA fürt beállítása](../articles/virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Kötegelt készletek számítási igényű virtuális gépek használata](../articles/batch/batch-pool-compute-intensive-sizes.md)



## <a name="azure-batch"></a>Azure Batch
[Kötegelt](../articles/batch/batch-technical-overview.md) van egy platform szolgáltatás a felügyeleti teendők központjaként párhuzamosan futó és nagy teljesítményű számítástechnikai (HPC) alkalmazások hatékonyan a felhőben. Azure Batch ütemezések számítási igényű működjenek a felügyelt készletbe, a virtuális gépek futtatásához, és is automatikusan méretezési számítási erőforrásokat a feladatok igényeinek. 

SaaS-szolgáltatók és a fejlesztőknek a kötegelt SDK-k és eszközök segítségével HPC alkalmazások vagy munkaterhelések tároló integrálása az Azure-szakasz adatokat az Azure-ba, és feladat végrehajtási folyamatok felépítéséhez. 

Az alábbiak végrehajtásának módját ismerheti meg:

* [A kötegelt fejlesztés első](../articles/batch/batch-dotnet-get-started.md)
* [Használja az Azure Batch-Kódminták](https://github.com/Azure/azure-batch-samples)
* [Kis prioritású virtuális gépek használata a kötegelt](../articles/batch/batch-low-pri-vms.md)
* [HPC tárolóalapú munkafolyamatok hibaüzenettel kötegelt hajógyárnak](https://github.com/Azure/batch-shipyard)
* [A kötegelt az R nyelv használatával](https://github.com/Azure/doAzureParallel)

## <a name="workload-managers"></a>Munkaterhelés-kezelők

A következő példák fürt és a munkaterhelés-kezelők futtatható Azure-infrastruktúra. Hozzon létre különálló fürtök Azure virtuális gépeken vagy kapacitásnövelés Azure virtuális gépek helyszíni fürtök. 
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Világos kezelő](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM pontszámot Symphony és Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [A Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029(v=ws.11).aspx) -beállítások futtatásához [Windows](../articles/virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Linux](../articles/virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) virtuális gépek 



## <a name="hpc-storage"></a>HPC-tároló

A nagyméretű kötegelt és HPC munkaterhelések igényekkel rendelkezhetnek adattárolás és hozzáférés terén, mint a hagyományos felhő fájlrendszerek képességeit. Például az Azure-ban párhuzamos fájl rendszer megoldások megvalósítását [fényesség](http://lustre.org/) és [BeeGFS](http://www.beegfs.com/content/).

További információ:

* [Párhuzamos fájlrendszerek HPC tárolás az Azure-on](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)


## <a name="related-azure-services"></a>Kapcsolódó Azure-szolgáltatások

Az Azure virtuális gépek, a virtuálisgép-méretezési csoportok, a kötegelt és a kapcsolódó számítási szolgáltatások olyan Azure HPC-megoldások többsége alapját. A megoldás azonban számos kapcsolódó Azure-szolgáltatások előnyeinek életbe. Ez egy részleges lista:

### <a name="storage"></a>Storage

* [A BLOB, table és a queue storage](../articles/storage/storage-introduction.md)
* [A File storage](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Adatok és analitika
* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md) a Hadoop-fürtök az Azure-on
* [Data Factory](../articles/data-factory/introduction.md)
* [Data Lake Store](../articles/data-lake-store/data-lake-store-overview.md)
* [Machine Learning](../articles/machine-learning/machine-learning-what-is-machine-learning.md)
* [SQL Database](../articles/sql-database/sql-database-technical-overview.md)

### <a name="networking"></a>Hálózat
* [Virtuális hálózat](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Tárolók
* [Container Service](../articles/container-service/dcos-swarm/container-service-intro.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)



## <a name="customer-stories"></a>Ügyfelek történetei

Az alábbiakban példát kell megoldani az üzleti problémák Azure HPC-megoldás az ügyfelek:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [Globális P & C AXA](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://enterprise.microsoft.com/en-us/customer-story/industries/insurance/metlife/)
* [A Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ biztosítékok nemzetközi](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Torony Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)


## <a name="next-steps"></a>Következő lépések
* További információ a megoldások nagy számítási [szimuláció mérnöki](https://simulation.azure.com/), [megjelenítési](https://simulation.azure.com/), [banki és nagy piacok](https://finance.azure.com/), és [genomika](https://enterprise.microsoft.com/en-us/industries/health/genomics/) .
* A legújabb bejelentésekért lásd: [A Microsoft HPC és Batch csapatának blogja](http://blogs.technet.com/b/windowshpc/) és [Azure-blog](https://azure.microsoft.com/blog/tag/hpc/).

* Használja a felügyelt és méretezhető Azure [kötegelt](https://azure.microsoft.com/services/batch/) szolgáltatás futtatásához a számítási-igényes munkaterhelések, alapul szolgáló infrastruktúra kezelése nélkül [további](https://azure.microsoft.com/en-us/solutions/architecture/hpc-big-compute-saas/)



