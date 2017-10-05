---
title: "HPC Pack OpenFOAM futó Linux virtuális gépek |} Microsoft Docs"
description: "Az Azure a Microsoft HPC Pack fürt központi telepítése, és egy OpenFOAM feladat futtatása több Linux számítási csomóponton az RDMA-hálózaton."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: ef124a8983fa112d499252460bff9ed2fcccc02b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a><span data-ttu-id="56d94-103">Az OpenFoam futtatása a Microsoft HPC Packkal Azure-beli linuxos RDMA-fürtön</span><span class="sxs-lookup"><span data-stu-id="56d94-103">Run OpenFoam with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>
<span data-ttu-id="56d94-104">Ez a cikk az Azure virtuális gépeken OpenFoam futtatásához egy módszert mutat.</span><span class="sxs-lookup"><span data-stu-id="56d94-104">This article shows you one way to run OpenFoam in Azure virtual machines.</span></span> <span data-ttu-id="56d94-105">Itt, a Microsoft HPC Pack-fürt Linux számítási csomópontok az Azure és a Futtatás telepít egy [OpenFoam](http://openfoam.com/) Intel MPI feladatot.</span><span class="sxs-lookup"><span data-stu-id="56d94-105">Here, you deploy a Microsoft HPC Pack cluster with Linux compute nodes on Azure and run an [OpenFoam](http://openfoam.com/) job with Intel MPI.</span></span> <span data-ttu-id="56d94-106">Az RDMA-kompatibilis Azure virtuális gépeken használhatja a számítási csomópontok, hogy a számítási csomópontok Azure RDMA hálózati kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="56d94-106">You can use RDMA-capable Azure VMs for the compute nodes, so that the compute nodes communicate over the Azure RDMA network.</span></span> <span data-ttu-id="56d94-107">Más OpenFoam Azure rendszerben való futtatásra a választható lehetőségek teljesen konfigurált kereskedelmi képek elérhető a piactéren, például a UberCloud tartozó [OpenFoam 2.3 a CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), és futtatja a [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span><span class="sxs-lookup"><span data-stu-id="56d94-107">Other options to run OpenFoam in Azure include fully configured commercial images available in the Marketplace, such as UberCloud's [OpenFoam 2.3 on CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), and by running on [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="56d94-108">(A nyissa meg a mező művelet és adatkezelési) OpenFOAM egy nyílt forráskódú számítási folyadékból dynamics (CFD) csomagot, amely széles körben használt mérnöki és tudományos, kereskedelmi és a academic szervezetekben.</span><span class="sxs-lookup"><span data-stu-id="56d94-108">OpenFOAM (for Open Field Operation and Manipulation) is an open-source computational fluid dynamics (CFD) software package that is used widely in engineering and science, in both commercial and academic organizations.</span></span> <span data-ttu-id="56d94-109">Ez magában foglalja az eszközök meshing, nevezetesen snappyHexMesh, egy parallelized mesher összetett CAD-geometriája, és előtti és utáni feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="56d94-109">It includes tools for meshing, notably snappyHexMesh, a parallelized mesher for complex CAD geometries, and for pre- and post-processing.</span></span> <span data-ttu-id="56d94-110">Szinte minden folyamatok párhuzamosan, így a felhasználók teljes körű kihasználása érdekében a számítógép a rendelkezésükre futnak.</span><span class="sxs-lookup"><span data-stu-id="56d94-110">Almost all processes run in parallel, enabling users to take full advantage of computer hardware at their disposal.</span></span>  

<span data-ttu-id="56d94-111">A Microsoft HPC Pack felügyeleti teendők központjaként HPC és párhuzamos alkalmazások, beleértve a MPI alkalmazásokat, a fürtökön a Microsoft Azure virtuális gépek futtatásához szolgáltatásokat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="56d94-111">Microsoft HPC Pack provides features to run large-scale HPC and parallel applications, including MPI applications, on clusters of Microsoft Azure virtual machines.</span></span> <span data-ttu-id="56d94-112">HPC Pack támogatja a virtuális gépeken telepített HPC Pack-fürtben lévő Linux alkalmazások csomópontjain futó Linux HPC is.</span><span class="sxs-lookup"><span data-stu-id="56d94-112">HPC Pack also supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="56d94-113">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) használata Linux bemutatása a számítási csomópontok HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="56d94-113">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction to using Linux compute nodes with HPC Pack.</span></span>

> [!NOTE]
> <span data-ttu-id="56d94-114">Ez a cikk bemutatja, hogyan HPC Pack a Linux MPI munkaterhelések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="56d94-114">This article illustrates how to run a Linux MPI workload with HPC Pack.</span></span> <span data-ttu-id="56d94-115">Azt feltételezi, hogy rendelkezik-e bizonyos fokú ismeretét, Linux rendszer felügyeleti és a Linux fürtökön MPI munkaterheket futtatnak.</span><span class="sxs-lookup"><span data-stu-id="56d94-115">It assumes you have some familiarity with Linux system administration and with running MPI workloads on Linux clusters.</span></span> <span data-ttu-id="56d94-116">Ha MPI és különbözik az ebben a cikkben szereplő OpenFOAM-verziót használ, lehetséges, hogy módosítani az egyes telepítési és konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="56d94-116">If you use versions of MPI and OpenFOAM different from the ones shown in this article, you might have to modify some installation and configuration steps.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="56d94-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="56d94-117">Prerequisites</span></span>
* <span data-ttu-id="56d94-118">**HPC Pack fürt RDMA-kompatibilisek-e a Linux és a számítási csomópontok** - fürt üzembe helyezése HPC Pack méretű A8, A9, H16r, vagy H16rm Linux számítási csomópontok használatával egy [Azure Resource Manager sablon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) vagy egy [Azure PowerShell parancsfájl](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="56d94-118">**HPC Pack cluster with RDMA-capable Linux compute nodes** - Deploy an HPC Pack cluster with size A8, A9, H16r, or H16rm Linux compute nodes using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="56d94-119">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) az Előfeltételek és az egyik beállítási mód lépéseit.</span><span class="sxs-lookup"><span data-stu-id="56d94-119">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for the prerequisites and steps for either option.</span></span> <span data-ttu-id="56d94-120">Ha a PowerShell parancsfájl központi telepítés lehetőséget választja, lásd: a minta konfigurációs fájlt a mintafájlok, ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="56d94-120">If you choose the PowerShell script deployment option, see the sample configuration file in the sample files at the end of this article.</span></span> <span data-ttu-id="56d94-121">Használja ezt a konfigurációt a fürt üzembe helyezése Azure-alapú HPC Pack mérete A8 Windows Server 2012 R2 átjárócsomópont és a számítási csomópontok 2 mérete A8 SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="56d94-121">Use this configuration to deploy an Azure-based HPC Pack cluster consisting of a size A8 Windows Server 2012 R2 head node and 2 size A8 SUSE Linux Enterprise Server 12 compute nodes.</span></span> <span data-ttu-id="56d94-122">Az előfizetés és a szolgáltatás nevének megfelelő értékeit helyettesítse.</span><span class="sxs-lookup"><span data-stu-id="56d94-122">Substitute appropriate values for your subscription and service names.</span></span> 
  
  <span data-ttu-id="56d94-123">**További tudnivaló**</span><span class="sxs-lookup"><span data-stu-id="56d94-123">**Additional things to know**</span></span>
  
  * <span data-ttu-id="56d94-124">Linux RDMA hálózati Előfeltételek az Azure-ban, lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="56d94-124">For Linux RDMA networking prerequisites in Azure, see [High performance compute VM sizes](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  * <span data-ttu-id="56d94-125">A Powershell parancsfájl központi telepítési beállítás használatakor telepítse az RDMA hálózati kapcsolat használata egy felhőalapú szolgáltatás belül az összes Linux számítási csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="56d94-125">If you use the Powershell script deployment option, deploy all the Linux compute nodes within one cloud service to use the RDMA network connection.</span></span>
  * <span data-ttu-id="56d94-126">Miután a Linux-csomópontok üzembe, csatlakozzon az SSH további felügyeleti feladatok végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="56d94-126">After deploying the Linux nodes, connect by SSH to perform any additional administrative tasks.</span></span> <span data-ttu-id="56d94-127">Az SSH-kapcsolat adatai található az egyes Linux virtuális gépek Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="56d94-127">Find the SSH connection details for each Linux VM in the Azure portal.</span></span>  
* <span data-ttu-id="56d94-128">**Intel MPI** - OpenFOAM SLES 12 HPC számítási csomópontok az Azure, a futtatásához telepítenie kell az Intel MPI könyvtár 5 futásidejű a [Intel.com hely](https://software.intel.com/en-us/intel-mpi-library/).</span><span class="sxs-lookup"><span data-stu-id="56d94-128">**Intel MPI** - To run OpenFOAM on SLES 12 HPC compute nodes in Azure, you need to install the Intel MPI Library 5 runtime from the [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/).</span></span> <span data-ttu-id="56d94-129">(Intel MPI 5 előre telepítve van a CentOS-alapú HPC képek.)  Egy későbbi lépésben szükség esetén telepítse az Intel MPI a Linux számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="56d94-129">(Intel MPI 5 is preinstalled on CentOS-based HPC images.)  In a later step, if necessary, install Intel MPI on your Linux compute nodes.</span></span> <span data-ttu-id="56d94-130">Ebben a lépésben előkészítése Intel regisztrálása után, hivatkozásra a megerősítési e-mailben a kapcsolódó weblapra.</span><span class="sxs-lookup"><span data-stu-id="56d94-130">To prepare for this step, after you register with Intel, follow the link in the confirmation email to the related web page.</span></span> <span data-ttu-id="56d94-131">Ezután másolja az Intel MPI megfelelő verzióját a .tgz fájl letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="56d94-131">Then, copy the download link for the .tgz file for the appropriate version of Intel MPI.</span></span> <span data-ttu-id="56d94-132">Ez a cikk Intel MPI verzió 5.0.3.048 alapul.</span><span class="sxs-lookup"><span data-stu-id="56d94-132">This article is based on Intel MPI version 5.0.3.048.</span></span>
* <span data-ttu-id="56d94-133">**OpenFOAM forrás csomag** -a Linux, a OpenFOAM forrás Pack szoftver töltse le a [OpenFOAM Foundation webhely](http://openfoam.org/download/2-3-1-source/).</span><span class="sxs-lookup"><span data-stu-id="56d94-133">**OpenFOAM Source Pack** - Download the OpenFOAM Source Pack software for Linux from the [OpenFOAM Foundation site](http://openfoam.org/download/2-3-1-source/).</span></span> <span data-ttu-id="56d94-134">Ez a cikk a forrás csomag verziója letölthető 2.3.1 OpenFOAM-2.3.1.tgz van alapján.</span><span class="sxs-lookup"><span data-stu-id="56d94-134">This article is based on Source Pack version 2.3.1, available for download as OpenFOAM-2.3.1.tgz.</span></span> <span data-ttu-id="56d94-135">Kövesse az utasításokat, ezzel csomagolja ki és OpenFOAM lefordítani a Linux számítási csomóponton a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="56d94-135">Follow the instructions later in this article to unpack and compile OpenFOAM on the Linux compute nodes.</span></span>
* <span data-ttu-id="56d94-136">**EnSight** (nem kötelező) – a OpenFOAM szimuláció, töltse le és telepítse az eredmények megtekintéséhez a [EnSight](https://www.ceisoftware.com/download/) képi megjelenítés és elemzések program.</span><span class="sxs-lookup"><span data-stu-id="56d94-136">**EnSight** (optional) - To see the results of your OpenFOAM simulation, download and install the [EnSight](https://www.ceisoftware.com/download/) visualization and analysis program.</span></span> <span data-ttu-id="56d94-137">Licencelési és letöltési információkat a EnSight helyen vannak.</span><span class="sxs-lookup"><span data-stu-id="56d94-137">Licensing and download information are at the EnSight site.</span></span>

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="56d94-138">Kölcsönös, a számítási csomópontok közötti megbízhatósági kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="56d94-138">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="56d94-139">Megbízhatósági kapcsolat áll fenn a csomópontok egy kereszt-csomópont feladat futó Linux több csomópont szükséges (által **rsh** vagy **ssh**).</span><span class="sxs-lookup"><span data-stu-id="56d94-139">Running a cross-node job on multiple Linux nodes requires the nodes to trust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="56d94-140">Ha a Microsoft HPC Pack IaaS telepítési parancsfájl a HPC Pack fürt létrehozásához, a parancsfájl automatikusan beállítja a rendszergazdai fiókhoz megadott állandó kölcsönös megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="56d94-140">When you create the HPC Pack cluster with the Microsoft HPC Pack IaaS deployment script, the script automatically sets up permanent mutual trust for the administrator account you specify.</span></span> <span data-ttu-id="56d94-141">A nem rendszergazda felhasználók számára hoz létre a fürt a tartományban a csomópontok közötti ideiglenes kölcsönös megbízhatósági beállítani, ha egy feladat őket rendelt kell, és szüntesse meg a kapcsolat, a feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="56d94-141">For non-administrator users you create in the cluster's domain, you have to set up temporary mutual trust among the nodes when a job is allocated to them, and destroy the relationship after the job is complete.</span></span> <span data-ttu-id="56d94-142">Megbízhatósági kapcsolat létrehozása az egyes felhasználók, adja meg a fürthöz, a megbízhatósági kapcsolathoz használó HPC Pack egy RSA kulcspár.</span><span class="sxs-lookup"><span data-stu-id="56d94-142">To establish trust for each user, provide an RSA key pair to the cluster that HPC Pack uses for the trust relationship.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="56d94-143">Egy RSA kulcspár létrehozása</span><span class="sxs-lookup"><span data-stu-id="56d94-143">Generate an RSA key pair</span></span>
<span data-ttu-id="56d94-144">Könnyen kulcspárlétrehozás egy RSA, egy nyilvános kulcs és a titkos kulcsot tartalmazó által a Linux operációs rendszert futtató **ssh-keygen** parancsot.</span><span class="sxs-lookup"><span data-stu-id="56d94-144">It's easy to generate an RSA key pair, which contains a public key and a private key, by running the Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="56d94-145">Jelentkezzen be a Linux-számítógép.</span><span class="sxs-lookup"><span data-stu-id="56d94-145">Log on to a Linux computer.</span></span>
2. <span data-ttu-id="56d94-146">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="56d94-146">Run the following command:</span></span>
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="56d94-147">Nyomja le az **Enter** az alapértelmezett beállításokkal, a parancs befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="56d94-147">Press **Enter** to use the default settings until the command is completed.</span></span> <span data-ttu-id="56d94-148">Ne adjon meg egy jelszót. Amikor a rendszer kéri a jelszót, csak Entert **Enter**.</span><span class="sxs-lookup"><span data-stu-id="56d94-148">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Egy RSA kulcspár létrehozása][keygen]
3. <span data-ttu-id="56d94-150">Módosítsa a könyvtárat a ~/.ssh könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="56d94-150">Change directory to the ~/.ssh directory.</span></span> <span data-ttu-id="56d94-151">A titkos kulcs id_rsa és a nyilvános kulcsot a id_rsa.pub tárolódik.</span><span class="sxs-lookup"><span data-stu-id="56d94-151">The private key is stored in id_rsa and the public key in id_rsa.pub.</span></span>
   
   ![Nyilvános és titkos kulcsai][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a><span data-ttu-id="56d94-153">Adja hozzá a kulcspárt a HPC Pack fürthöz</span><span class="sxs-lookup"><span data-stu-id="56d94-153">Add the key pair to the HPC Pack cluster</span></span>
1. <span data-ttu-id="56d94-154">A távoli asztali kapcsolat az átjárócsomópont a HPC Pack rendszergazdai fiókkal (a rendszergazdai fiók beállítása az üzembe helyezési parancsfájl futtatása).</span><span class="sxs-lookup"><span data-stu-id="56d94-154">Make a Remote Desktop connection to your head node with your HPC Pack administrator account (the administrator account you set up when you ran the deployment script).</span></span>
2. <span data-ttu-id="56d94-155">Szabványos Windows Server-eljárások segítségével hozzon létre egy tartományfelhasználói fiókot a fürt Active Directory-tartományban.</span><span class="sxs-lookup"><span data-stu-id="56d94-155">Use standard Windows Server procedures to create a domain user account in the cluster's Active Directory domain.</span></span> <span data-ttu-id="56d94-156">Az Active Directory – felhasználók és számítógépek eszközt például használatát az átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="56d94-156">For example, use the Active Directory User and Computers tool on the head node.</span></span> <span data-ttu-id="56d94-157">Ebben a cikkben szereplő példák azt feltételezik, hogy hpclab\hpcuser nevű tartományi felhasználót kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="56d94-157">The examples in this article assume you create a domain user named hpclab\hpcuser.</span></span>
3. <span data-ttu-id="56d94-158">Hozzon létre egy C:\cred.xml nevű fájlt, és az RSA-kulcs adatait átmásolja azt.</span><span class="sxs-lookup"><span data-stu-id="56d94-158">Create a file named C:\cred.xml and copy the RSA key data into it.</span></span> <span data-ttu-id="56d94-159">A mintafájl cred.xml van ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="56d94-159">A sample cred.xml file is at the end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. <span data-ttu-id="56d94-160">Nyisson meg egy parancssort, és írja be a következő parancsot a hpclab\hpcuser fiók a hitelesítő adatok beállításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-160">Open a Command Prompt and enter the following command to set the credentials data for the hpclab\hpcuser account.</span></span> <span data-ttu-id="56d94-161">Használja a **extendeddata** paraméter felelt meg a C:\cred.xml fájl hozott létre a fő adatok nevét.</span><span class="sxs-lookup"><span data-stu-id="56d94-161">You use the **extendeddata** parameter to pass the name of C:\cred.xml file you created for the key data.</span></span>
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="56d94-162">Ez a parancs sikeresen kimeneti nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="56d94-162">This command completes successfully without output.</span></span> <span data-ttu-id="56d94-163">Miután beállította a hitelesítő adatok a feladatok futtatásához szükséges felhasználói fiókok, a cred.xml fájlt tárolja biztonságos helyen, vagy törölje azt.</span><span class="sxs-lookup"><span data-stu-id="56d94-163">After setting the credentials for the user accounts you need to run jobs, store the cred.xml file in a secure location, or delete it.</span></span>
5. <span data-ttu-id="56d94-164">Ha az RSA kulcspár a Linux-csomópontok egyikén jön létre, ne felejtse el azok használatának befejezése után törölje.</span><span class="sxs-lookup"><span data-stu-id="56d94-164">If you generated the RSA key pair on one of your Linux nodes, remember to delete the keys after you finish using them.</span></span> <span data-ttu-id="56d94-165">Ha HPC Pack talál egy meglévő id_rsa vagy id_rsa.pub fájl, akkor nem kölcsönös megbízhatóság kialakításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-165">If HPC Pack finds an existing id_rsa file or id_rsa.pub file, it does not set up mutual trust.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56d94-166">Nem ajánlott a Linux feladat futtatásával a fürt rendszergazdai megosztott fürtön, mivel a Linux-csomópontokon a legfelső szintű fiók alatt fut egy feladatot, a rendszergazda által küldött.</span><span class="sxs-lookup"><span data-stu-id="56d94-166">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under the root account on the Linux nodes.</span></span> <span data-ttu-id="56d94-167">A feladat a nem rendszergazda felhasználók által benyújtott azonban Linux helyi felhasználói fiók alatt fut, a neve megegyezik a feladat felhasználó.</span><span class="sxs-lookup"><span data-stu-id="56d94-167">However, a job submitted by a non-administrator user runs under a local Linux user account with the same name as the job user.</span></span> <span data-ttu-id="56d94-168">Ebben az esetben HPC Pack állít be a Linux-felhasználó kölcsönös megbízhatósági a a feladathoz kiosztott csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="56d94-168">In this case, HPC Pack sets up mutual trust for this Linux user across the nodes allocated to the job.</span></span> <span data-ttu-id="56d94-169">A feladat futtatása előtt manuálisan a Linux-csomópontokon a Linux felhasználói állíthat be, vagy a HPC Pack a felhasználó automatikusan létrehozza a feladat elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="56d94-169">You can set up the Linux user manually on the Linux nodes before running the job, or HPC Pack creates the user automatically when the job is submitted.</span></span> <span data-ttu-id="56d94-170">HPC Pack a felhasználó hoz létre, ha HPC Pack törli őket a feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="56d94-170">If HPC Pack creates the user, HPC Pack deletes it after the job completes.</span></span> <span data-ttu-id="56d94-171">A biztonsági kockázatok csökkentése a HPC Pack törli a kulcsokat a feladat befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="56d94-171">To reduce security threats, HPC Pack removes the keys after job completion.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="56d94-172">Linux-csomópontok fájlmegosztás beállítása</span><span class="sxs-lookup"><span data-stu-id="56d94-172">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="56d94-173">Most már készen egy szabványos SMB-megosztáson az átjárócsomópont az egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="56d94-173">Now set up a standard SMB share on a folder on the head node.</span></span> <span data-ttu-id="56d94-174">Ahhoz, hogy a Linux-csomópontok közös elérési úttal rendelkező alkalmazás fájlok eléréséhez, csatlakoztassa a Linux-csomópontokon a megosztott mappához.</span><span class="sxs-lookup"><span data-stu-id="56d94-174">To allow the Linux nodes to access application files with a common path, mount the shared folder on the Linux nodes.</span></span> <span data-ttu-id="56d94-175">Ha azt szeretné, használhatja a beállítást, például egy Azure fájlok fájlmegosztás - számos forgatókönyv - vagy az NFS-megosztások ajánlott egy másik fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="56d94-175">If you want, you can use another file sharing option, such as an Azure Files share - recommended for many scenarios - or an NFS share.</span></span> <span data-ttu-id="56d94-176">A fájlmegosztási információk és részletes lépéseit lásd: [Ismerkedés az Azure-ban HPC Pack fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="56d94-176">See the file sharing information and detailed steps in [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="56d94-177">Hozzon létre egy mappát az átjárócsomóponthoz, és úgy, hogy olvasási/írási engedélyekkel mindenki számára ossza meg.</span><span class="sxs-lookup"><span data-stu-id="56d94-177">Create a folder on the head node, and share it to Everyone by setting Read/Write privileges.</span></span> <span data-ttu-id="56d94-178">Például, megosztás, az átjárócsomópont C:\OpenFOAM \\ \\SUSE12RDMA-HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="56d94-178">For example, share C:\OpenFOAM on the head node as \\\\SUSE12RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="56d94-179">Itt *SUSE12RDMA-HN* az átjárócsomópont állomás neve.</span><span class="sxs-lookup"><span data-stu-id="56d94-179">Here, *SUSE12RDMA-HN* is the host name of the head node.</span></span>
2. <span data-ttu-id="56d94-180">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="56d94-180">Open a Windows PowerShell window and run the following commands:</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

<span data-ttu-id="56d94-181">Az első parancs létrehoz egy /openfoam a LinuxNodes csoport minden csomópontján nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="56d94-181">The first command creates a folder named /openfoam on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="56d94-182">A második parancs a megosztott mappa //SUSE12RDMA-HN/OpenFOAM csatlakoztatja a dir_mode a Linux-csomópont, és file_mode bitet 777.</span><span class="sxs-lookup"><span data-stu-id="56d94-182">The second command mounts the shared folder //SUSE12RDMA-HN/OpenFOAM on the Linux nodes with dir_mode and file_mode bits set to 777.</span></span> <span data-ttu-id="56d94-183">A *felhasználónév* és *jelszó* parancsban kell lennie az átjárócsomópont a felhasználó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="56d94-183">The *username* and *password* in the command should be the credentials of a user on the head node.</span></span>

> [!NOTE]
> <span data-ttu-id="56d94-184">A "\\`" szimbólumra a második parancs a értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="56d94-184">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="56d94-185">"\\`," a "," (vessző karakter) azt jelenti, hogy a parancs része.</span><span class="sxs-lookup"><span data-stu-id="56d94-185">“\\`,” means the “,” (comma character) is a part of the command.</span></span>
> 
> 

## <a name="install-mpi-and-openfoam"></a><span data-ttu-id="56d94-186">MPI és OpenFOAM telepítése</span><span class="sxs-lookup"><span data-stu-id="56d94-186">Install MPI and OpenFOAM</span></span>
<span data-ttu-id="56d94-187">OpenFOAM MPI feladatként futtatja az RDMA hálózati, szüksége az Intel MPI könyvtárakkal OpenFOAM összeállításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-187">To run OpenFOAM as an MPI job on the RDMA network, you need to compile OpenFOAM with the Intel MPI libraries.</span></span> 

<span data-ttu-id="56d94-188">Először futtassa több **clusrun** parancsok futtatásával telepítse Intel MPI függvénytárak (Ha még nincs telepítve) és a Linux csomópontján OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="56d94-188">First, run several **clusrun** commands to install Intel MPI libraries (if not already installed) and OpenFOAM on your Linux nodes.</span></span> <span data-ttu-id="56d94-189">A telepítési fájlokat, a Linux-csomópontok közötti megosztásához korábban konfigurált átjárócsomópont megosztás használatára.</span><span class="sxs-lookup"><span data-stu-id="56d94-189">Use the head node share configured previously to share the installation files among the Linux nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56d94-190">Ezeket a telepítési és fordítása lépések példái.</span><span class="sxs-lookup"><span data-stu-id="56d94-190">These installation and compiling steps are examples.</span></span> <span data-ttu-id="56d94-191">Meg kell néhány a Linux rendszer felügyeleti annak érdekében, hogy a függő compilers – és szalagtárak megfelelően van telepítve.</span><span class="sxs-lookup"><span data-stu-id="56d94-191">You need some knowledge of Linux system administration to ensure that dependent compilers and libraries are installed correctly.</span></span> <span data-ttu-id="56d94-192">Előfordulhat, hogy módosítania bizonyos környezeti változókat és más beállításait az Intel MPI és OpenFOAM verzióit.</span><span class="sxs-lookup"><span data-stu-id="56d94-192">You might need to modify certain environment variables or other settings for your versions of Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="56d94-193">További információkért lásd: [Intel MPI Library for Linux – telepítési útmutató](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) és [OpenFOAM forrás csomag telepítése](http://openfoam.org/download/2-3-1-source/) alkalmaz környezetében.</span><span class="sxs-lookup"><span data-stu-id="56d94-193">For details, see [Intel MPI Library for Linux Installation Guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) and [OpenFOAM Source Pack Installation](http://openfoam.org/download/2-3-1-source/) for your environment.</span></span>
> 
> 

### <a name="install-intel-mpi"></a><span data-ttu-id="56d94-194">Intel MPI telepítése</span><span class="sxs-lookup"><span data-stu-id="56d94-194">Install Intel MPI</span></span>
<span data-ttu-id="56d94-195">Mentse a letöltött telepítési csomag az Intel MPI (ebben a példában l_mpi_p_5.0.3.048.tgz) C:\OpenFoam a head csomóponton, hogy a Linux-csomópontok /openfoam hozzáfér ehhez a fájlt.</span><span class="sxs-lookup"><span data-stu-id="56d94-195">Save the downloaded installation package for Intel MPI (l_mpi_p_5.0.3.048.tgz in this example) in C:\OpenFoam on the head node so that the Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="56d94-196">Ezután futtassa **clusrun** Intel MPI könyvtár telepítése Linux-csomópontjaihoz.</span><span class="sxs-lookup"><span data-stu-id="56d94-196">Then run **clusrun** to install Intel MPI library on all the Linux nodes.</span></span>

1. <span data-ttu-id="56d94-197">A következő parancsok másolja a csomagot, és bontsa ki a /opt/intel minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="56d94-197">The following commands copy the installation package and extract it to /opt/intel on each node.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. <span data-ttu-id="56d94-198">Intel MPI könyvtár csendes telepítéséhez használja a silent.cfg fájl.</span><span class="sxs-lookup"><span data-stu-id="56d94-198">To install Intel MPI Library silently, use a silent.cfg file.</span></span> <span data-ttu-id="56d94-199">A mintafájlok, ez a cikk végén található példa.</span><span class="sxs-lookup"><span data-stu-id="56d94-199">You can find an example in the sample files at the end of this article.</span></span> <span data-ttu-id="56d94-200">A megosztott mappa /openfoam helyezze a fájlt.</span><span class="sxs-lookup"><span data-stu-id="56d94-200">Place this file in the shared folder /openfoam.</span></span> <span data-ttu-id="56d94-201">A silent.cfg fájllal kapcsolatos részletekért lásd: [Intel MPI Library for Linux – telepítési útmutató - csendes telepítést](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span><span class="sxs-lookup"><span data-stu-id="56d94-201">For details about the silent.cfg file, see [Intel MPI Library for Linux Installation Guide - Silent Installation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="56d94-202">Győződjön meg arról, hogy Ön a silent.cfg fájl szövegfájl, amelynek Linux sorvégeket (csak LF, nem a CR LF).</span><span class="sxs-lookup"><span data-stu-id="56d94-202">Make sure that you save your silent.cfg file as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="56d94-203">Ez a lépés biztosítja, hogy ez megfelelően a Linux-csomóponton.</span><span class="sxs-lookup"><span data-stu-id="56d94-203">This step ensures that it runs properly on the Linux nodes.</span></span>
   > 
   > 
3. <span data-ttu-id="56d94-204">Intel MPI könyvtár telepítése csendes módban.</span><span class="sxs-lookup"><span data-stu-id="56d94-204">Install Intel MPI Library in silent mode.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a><span data-ttu-id="56d94-205">MPI konfigurálása</span><span class="sxs-lookup"><span data-stu-id="56d94-205">Configure MPI</span></span>
<span data-ttu-id="56d94-206">A vizsgálat, a következő sorokat kell hozzá minden egyes Linux csomópont /etc/security/limits.conf:</span><span class="sxs-lookup"><span data-stu-id="56d94-206">For testing, you should add the following lines to the /etc/security/limits.conf on each of the Linux nodes:</span></span>

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


<span data-ttu-id="56d94-207">Indítsa újra a Linux-csomópontot, a limits.conf fájl frissítése után.</span><span class="sxs-lookup"><span data-stu-id="56d94-207">Restart the Linux nodes after you update the limits.conf file.</span></span> <span data-ttu-id="56d94-208">Például használja a következő **clusrun** parancs:</span><span class="sxs-lookup"><span data-stu-id="56d94-208">For example, use the following **clusrun** command:</span></span>

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

<span data-ttu-id="56d94-209">Az újraindítás után győződjön meg arról, hogy a megosztott mappa, /openfoam csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="56d94-209">After restarting, ensure that the shared folder is mounted as /openfoam.</span></span>

### <a name="compile-and-install-openfoam"></a><span data-ttu-id="56d94-210">Fordítsa le és OpenFOAM telepítése</span><span class="sxs-lookup"><span data-stu-id="56d94-210">Compile and install OpenFOAM</span></span>
<span data-ttu-id="56d94-211">Mentse a letöltött telepítési csomagot a OpenFOAM forrás csomagnak (OpenFOAM 2.3.1.tgz ebben a példában) C:\OpenFoam a az átjárócsomóponthoz, hogy a Linux-csomópontok /openfoam hozzáfér ehhez a fájlt.</span><span class="sxs-lookup"><span data-stu-id="56d94-211">Save the downloaded installation package for the OpenFOAM Source Pack (OpenFOAM-2.3.1.tgz in this example) to C:\OpenFoam on the head node so that the Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="56d94-212">Ezután futtassa **clusrun** parancsokat minden csomóponton a Linux OpenFOAM összeállításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-212">Then run **clusrun** commands to compile OpenFOAM on all the Linux nodes.</span></span>

1. <span data-ttu-id="56d94-213">Hozzon létre egy mappát /opt/OpenFOAM minden egyes Linux csomóponton, forrás másolja ezt a mappát, és bontsa ki, hogy.</span><span class="sxs-lookup"><span data-stu-id="56d94-213">Create a folder /opt/OpenFOAM on each Linux node, copy the source package to this folder, and extract it there.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. <span data-ttu-id="56d94-214">Az Intel MPI könyvtárhoz, először állítsa be néhány környezetiblokk-változót Intel MPI és OpenFOAM OpenFOAM összeállításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-214">To compile OpenFOAM with the Intel MPI Library, first set up some environment variables for both Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="56d94-215">Nevű settings.sh bash parancsfájl segítségével a változókat.</span><span class="sxs-lookup"><span data-stu-id="56d94-215">Use a bash script called settings.sh to set the variables.</span></span> <span data-ttu-id="56d94-216">A mintafájlok, ez a cikk végén található példa.</span><span class="sxs-lookup"><span data-stu-id="56d94-216">You can find an example in the sample files at the end of this article.</span></span> <span data-ttu-id="56d94-217">Helyezze a fájlt (a Linux sorvégződések mentett) a megosztott mappa /openfoam.</span><span class="sxs-lookup"><span data-stu-id="56d94-217">Place this file (saved with Linux line endings) in the shared folder /openfoam.</span></span> <span data-ttu-id="56d94-218">Ez a fájl is a MPI és OpenFOAM futtatókörnyezetek később egy OpenFOAM feladat futtatásához használt beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="56d94-218">This file also contains settings for the MPI and OpenFOAM runtimes that you use later to run an OpenFOAM job.</span></span>
3. <span data-ttu-id="56d94-219">Telepítse a függő csomagok OpenFOAM összeállításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-219">Install dependent packages needed to compile OpenFOAM.</span></span> <span data-ttu-id="56d94-220">Attól függően, hogy a Linux-disztribúció előfordulhat, hogy először vegyen fel egy tárházat.</span><span class="sxs-lookup"><span data-stu-id="56d94-220">Depending on your Linux distribution, you might first need to add a repository.</span></span> <span data-ttu-id="56d94-221">Futtatás **clusrun** a következőhöz hasonló parancsot:</span><span class="sxs-lookup"><span data-stu-id="56d94-221">Run **clusrun** commands similar to the following:</span></span>
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    <span data-ttu-id="56d94-222">Ha szükséges, minden egyes Linux csomópontra SSH annak ellenőrzéséhez, hogy futnak megfelelően parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="56d94-222">If necessary, SSH to each Linux node to run the commands to confirm that they run properly.</span></span>
4. <span data-ttu-id="56d94-223">A következő parancsot OpenFOAM összeállításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-223">Run the following command to compile OpenFOAM.</span></span> <span data-ttu-id="56d94-224">Az összeállítási folyamat végrehajtásához némi időt vesz igénybe, és naplózási adatok normál a kimenetbe nagy mennyiségű állít elő, ezért a **/ időosztásos** lehetőséget a kimeneti időosztásos megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="56d94-224">The compilation process takes some time to complete and generates a large amount of log information to standard output, so use the **/interleaved** option to display the output interleaved.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > <span data-ttu-id="56d94-225">A "\\`" szimbólum a parancsban értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="56d94-225">The “\\`” symbol in the command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="56d94-226">"\\`&" azt jelenti, hogy a "&" parancs része.</span><span class="sxs-lookup"><span data-stu-id="56d94-226">“\\`&” means the “&” is a part of the command.</span></span>
   > 
   > 

## <a name="prepare-to-run-an-openfoam-job"></a><span data-ttu-id="56d94-227">Egy OpenFOAM feladat futtatásához előkészítése</span><span class="sxs-lookup"><span data-stu-id="56d94-227">Prepare to run an OpenFOAM job</span></span>
<span data-ttu-id="56d94-228">Most Felkészülés egy hívása sloshingTank3D, amely a OpenFoam minta egyike, két Linux csomópont MPI feladat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="56d94-228">Now get ready to run an MPI job called sloshingTank3D, which is one of the OpenFoam samples, on two Linux nodes.</span></span> 

### <a name="set-up-the-runtime-environment"></a><span data-ttu-id="56d94-229">A futásidejű környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="56d94-229">Set up the runtime environment</span></span>
<span data-ttu-id="56d94-230">Beállítása a futásidejű környezetek MPI és OpenFOAM a Linux-csomóponton, futtassa a következő parancsot egy Windows PowerShell-ablakban a head csomóponton.</span><span class="sxs-lookup"><span data-stu-id="56d94-230">To set up the runtime environments for MPI and OpenFOAM on the Linux nodes, run the following command in a Windows PowerShell window on the head node.</span></span> <span data-ttu-id="56d94-231">(Ez a parancs érvénytelen a SUSE Linux csak.)</span><span class="sxs-lookup"><span data-stu-id="56d94-231">(This command is valid for SUSE Linux only.)</span></span>

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a><span data-ttu-id="56d94-232">Mintaadatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="56d94-232">Prepare sample data</span></span>
<span data-ttu-id="56d94-233">Fájlmegosztás között a Linux-csomópontok (mint /openfoam csatlakoztatott) korábban konfigurált átjárócsomópont megosztás használatára.</span><span class="sxs-lookup"><span data-stu-id="56d94-233">Use the head node share you configured previously to share files among the Linux nodes (mounted as /openfoam).</span></span>

1. <span data-ttu-id="56d94-234">A számítási csomópontok SSH a Linux egyikére.</span><span class="sxs-lookup"><span data-stu-id="56d94-234">SSH to one of your Linux compute nodes.</span></span>
2. <span data-ttu-id="56d94-235">A következő parancsot a OpenFOAM futtatási környezetet, ha még nem tette meg ezt.</span><span class="sxs-lookup"><span data-stu-id="56d94-235">Run the following command to set up the OpenFOAM runtime environment, if you haven’t already done this.</span></span>
   
   ```
   $ source /openfoam/settings.sh
   ```
3. <span data-ttu-id="56d94-236">Másolja a sloshingTank3D minta a megosztott mappa, és keresse meg a fájlt.</span><span class="sxs-lookup"><span data-stu-id="56d94-236">Copy the sloshingTank3D sample to the shared folder and navigate to it.</span></span>
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. <span data-ttu-id="56d94-237">Ez a minta alapértelmezett paraméterértékeivel használatakor is igénybe vehet több tíz perc alatt szeretné futtatni, ezért érdemes néhány abba, hogy gyorsabbá paraméterek módosításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-237">When you use the default parameters of this sample, it can take tens of minutes to run, so you might want to modify some parameters to make it run faster.</span></span> <span data-ttu-id="56d94-238">Egy egyszerű rendszer idő lépés változók deltaT és a rendszer/controlDict fájlban writeInterval módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="56d94-238">One simple choice is to modify the time step variables deltaT and writeInterval in the system/controlDict file.</span></span> <span data-ttu-id="56d94-239">Ez a fájl tartalmazza az idő és adatok írásakor vagy olvasásakor megoldás ellenőrzésére vonatkozó összes bemeneti adatot.</span><span class="sxs-lookup"><span data-stu-id="56d94-239">This file stores all input data relating to the control of time and reading and writing solution data.</span></span> <span data-ttu-id="56d94-240">Például megváltoztathatja 0,5 a 0,05 deltaT értékének és a 0,05 writeInterval 0,5 értékét.</span><span class="sxs-lookup"><span data-stu-id="56d94-240">For example, you could change the value of deltaT from 0.05 to 0.5 and the value of writeInterval from 0.05 to 0.5.</span></span>
   
   ![Lépés változók módosítása][step_variables]
5. <span data-ttu-id="56d94-242">A rendszer/decomposeParDict fájlban adja meg a változót a kívánt értékeket.</span><span class="sxs-lookup"><span data-stu-id="56d94-242">Specify desired values for the variables in the system/decomposeParDict file.</span></span> <span data-ttu-id="56d94-243">Ebben a példában két Linux csomópont minden használ 8 magos, úgy állítsa be a numberOfSubdomains 16 és a hierarchicalCoeffs n (1 1 16), ami azt jelenti, 16 folyamatokkal párhuzamosan futnak a OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="56d94-243">This example uses two Linux nodes each with 8 cores, so set numberOfSubdomains to 16 and n of hierarchicalCoeffs to (1 1 16), which means run OpenFOAM in parallel with 16 processes.</span></span> <span data-ttu-id="56d94-244">További információkért lásd: [OpenFOAM felhasználói útmutatója: 3.4 futó alkalmazások párhuzamosan](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span><span class="sxs-lookup"><span data-stu-id="56d94-244">For more information, see [OpenFOAM User Guide: 3.4 Running applications in parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span></span>
   
   ![Folyamatok felbontani][decompose]
6. <span data-ttu-id="56d94-246">A következő parancsokat a mintaadatok létrehozása a sloshingTank3D könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="56d94-246">Run the following commands from the sloshingTank3D directory to prepare the sample data.</span></span>
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. <span data-ttu-id="56d94-247">A head csomóponton meg kell jelennie a mintaadatfájlok C:\OpenFoam\sloshingTank3D másolódnak.</span><span class="sxs-lookup"><span data-stu-id="56d94-247">On the head node, you should see the sample data files are copied into C:\OpenFoam\sloshingTank3D.</span></span> <span data-ttu-id="56d94-248">(A megosztott mappát a átjárócsomópontjához C:\OpenFoam érték).)</span><span class="sxs-lookup"><span data-stu-id="56d94-248">(C:\OpenFoam is the shared folder on the head node.)</span></span>
   
   ![Az átjárócsomópont adatfájlokat][data_files]

### <a name="host-file-for-mpirun"></a><span data-ttu-id="56d94-250">Állomás fájlt mpirun</span><span class="sxs-lookup"><span data-stu-id="56d94-250">Host file for mpirun</span></span>
<span data-ttu-id="56d94-251">Ebben a lépésben létrehozott állomás-fájlokat (számítási csomópontok listáját) amely a **mpirun** parancsot használja.</span><span class="sxs-lookup"><span data-stu-id="56d94-251">In this step, you create a host file (a list of compute nodes) which the **mpirun** command uses.</span></span>

1. <span data-ttu-id="56d94-252">A Linux-csomópontok egyikét hozzon létre egy fájlt hostfile alatt /openfoam, ezért ezt a fájlt az összes Linux csomóponton /openfoam/hostfile címen érhető el.</span><span class="sxs-lookup"><span data-stu-id="56d94-252">On one of the Linux nodes, create a file named hostfile under /openfoam, so this file can be reached at /openfoam/hostfile on all Linux nodes.</span></span>
2. <span data-ttu-id="56d94-253">A Linux csomópontnevek írni az ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="56d94-253">Write your Linux node names into this file.</span></span> <span data-ttu-id="56d94-254">Ebben a példában a fájl tartalmazza a következő szerepel:</span><span class="sxs-lookup"><span data-stu-id="56d94-254">In this example, the file contains the following names:</span></span>
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > <span data-ttu-id="56d94-255">Ez a fájl C:\OpenFoam\hostfile, a központi csomóponton is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="56d94-255">You can also create this file at C:\OpenFoam\hostfile on the head node.</span></span> <span data-ttu-id="56d94-256">Ha ezt a lehetőséget választja, a Linux és szövegfájlként menteni sorvégeket (csak LF, nem a CR LF).</span><span class="sxs-lookup"><span data-stu-id="56d94-256">If you choose this option, save it as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="56d94-257">Ez biztosítja, hogy fusson megfelelően a Linux-csomóponton.</span><span class="sxs-lookup"><span data-stu-id="56d94-257">This ensures that it runs properly on the Linux nodes.</span></span>
   > 
   > 
   
   <span data-ttu-id="56d94-258">**Bash parancsfájlok burkoló**</span><span class="sxs-lookup"><span data-stu-id="56d94-258">**Bash script wrapper**</span></span>
   
   <span data-ttu-id="56d94-259">Sok Linux-csomópont van, és azt szeretné, hogy a feladat futásának csak az egyes, esetén nem érdemes egy rögzített gazdagépet fájl, mert nem tudja, melyik csomópontok rendszer rendeli hozzá a feladatot.</span><span class="sxs-lookup"><span data-stu-id="56d94-259">If you have many Linux nodes and you want your job to run on only some of them, it’s not a good idea to use a fixed host file, because you don’t know which nodes will be allocated to your job.</span></span> <span data-ttu-id="56d94-260">Ebben az esetben írni a bash parancsfájlok burkolót **mpirun** automatikusan a gazdagép-fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="56d94-260">In this case, write a bash script wrapper for **mpirun** to create the host file automatically.</span></span> <span data-ttu-id="56d94-261">Egy példa bash parancsfájl burkoló nevű hpcimpirun.sh Ez a cikk végén található, és mentse /openfoam/hpcimpirun.sh.</span><span class="sxs-lookup"><span data-stu-id="56d94-261">You can find an example bash script wrapper called hpcimpirun.sh at the end of this article, and save it as /openfoam/hpcimpirun.sh.</span></span> <span data-ttu-id="56d94-262">Ebben a példaként megadott parancsfájlt a következőket teszi:</span><span class="sxs-lookup"><span data-stu-id="56d94-262">This example script does the following:</span></span>
   
   1. <span data-ttu-id="56d94-263">Beállítja a környezeti változók a **mpirun**, és néhány hozzáadása parancs paraméterei a MPI feladat futtatásához az RDMA a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="56d94-263">Sets up the environment variables for **mpirun**, and some addition command parameters to run the MPI job through the RDMA network.</span></span> <span data-ttu-id="56d94-264">Ebben az esetben azt állítja be a következő változókat:</span><span class="sxs-lookup"><span data-stu-id="56d94-264">In this case, it sets the following variables:</span></span>
      
      * <span data-ttu-id="56d94-265">I_MPI_FABRICS = shm:dapl</span><span class="sxs-lookup"><span data-stu-id="56d94-265">I_MPI_FABRICS=shm:dapl</span></span>
      * <span data-ttu-id="56d94-266">I_MPI_DAPL_PROVIDER bájtméretét-v2-ib0 =</span><span class="sxs-lookup"><span data-stu-id="56d94-266">I_MPI_DAPL_PROVIDER=ofa-v2-ib0</span></span>
      * <span data-ttu-id="56d94-267">I_MPI_DYNAMIC_CONNECTION = 0</span><span class="sxs-lookup"><span data-stu-id="56d94-267">I_MPI_DYNAMIC_CONNECTION=0</span></span>
   2. <span data-ttu-id="56d94-268">Létrehoz egy gazdagép megfelelően a környezeti változó $CCP_NODES_CORES, amely szerint a HPC átjárócsomópont van beállítva, a feladat aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="56d94-268">Creates a host file according to the environment variable $CCP_NODES_CORES, which is set by the HPC head node when the job is activated.</span></span>
      
      <span data-ttu-id="56d94-269">$CCP_NODES_CORES formátuma ezt a mintát követi:</span><span class="sxs-lookup"><span data-stu-id="56d94-269">The format of $CCP_NODES_CORES follows this pattern:</span></span>
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      <span data-ttu-id="56d94-270">Ha</span><span class="sxs-lookup"><span data-stu-id="56d94-270">where</span></span>
      
      * <span data-ttu-id="56d94-271">`<Number of nodes>`– a feladat számára lefoglalt csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="56d94-271">`<Number of nodes>` - the number of nodes allocated to this job.</span></span>  
      * <span data-ttu-id="56d94-272">`<Name of node_n_...>`– a feladat rendelt minden csomópont neve.</span><span class="sxs-lookup"><span data-stu-id="56d94-272">`<Name of node_n_...>` - the name of each node allocated to this job.</span></span>
      * <span data-ttu-id="56d94-273">`<Cores of node_n_...>`-a csomóponton a feladat számára lefoglalt magok száma.</span><span class="sxs-lookup"><span data-stu-id="56d94-273">`<Cores of node_n_...>` - the number of cores on the node allocated to this job.</span></span>
      
      <span data-ttu-id="56d94-274">Például ha a feladat futtatásához két csomópont van szüksége, $CCP_NODES_CORES hasonlít</span><span class="sxs-lookup"><span data-stu-id="56d94-274">For example, if the job needs two nodes to run, $CCP_NODES_CORES is similar to</span></span>
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. <span data-ttu-id="56d94-275">Hívások a **mpirun** parancsot, és hozzáfűzi a két paraméter a parancssorban.</span><span class="sxs-lookup"><span data-stu-id="56d94-275">Calls the **mpirun** command and appends two parameters to the command line.</span></span>
      
      * <span data-ttu-id="56d94-276">`--hostfile <hostfilepath>: <hostfilepath>`-a parancsfájl létrehozza a gazdagép-fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="56d94-276">`--hostfile <hostfilepath>: <hostfilepath>` - the path of the host file the script creates</span></span>
      * <span data-ttu-id="56d94-277">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-olyan környezeti változó beállítása a HPC Pack központi csomópont, amely tárolja az összes, a feladat számára lefoglalt magok száma.</span><span class="sxs-lookup"><span data-stu-id="56d94-277">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` - an environment variable set by the HPC Pack head node, which stores the number of total cores allocated to this job.</span></span> <span data-ttu-id="56d94-278">Ebben az esetben adja meg a folyamatok száma **mpirun**.</span><span class="sxs-lookup"><span data-stu-id="56d94-278">In this case, it specifies the number of processes for **mpirun**.</span></span>

## <a name="submit-an-openfoam-job"></a><span data-ttu-id="56d94-279">Egy OpenFOAM feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="56d94-279">Submit an OpenFOAM job</span></span>
<span data-ttu-id="56d94-280">Most elküldheti a HPC Cluster Manager feladat.</span><span class="sxs-lookup"><span data-stu-id="56d94-280">Now you can submit a job in HPC Cluster Manager.</span></span> <span data-ttu-id="56d94-281">A parancsfájl hpcimpirun.sh át a parancsokat a projekt feladatokat kell.</span><span class="sxs-lookup"><span data-stu-id="56d94-281">You need to pass the script hpcimpirun.sh in the command lines for some of the job tasks.</span></span>

1. <span data-ttu-id="56d94-282">A fürt átjárócsomópontjához csatlakozik, és indítsa el a HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="56d94-282">Connect to your cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="56d94-283">**Az erőforrás-kezelés**, akkor győződjön meg arról, hogy a Linux számítási csomópontok a **Online** állapotát.</span><span class="sxs-lookup"><span data-stu-id="56d94-283">**In Resource Management**, ensure that the Linux compute nodes are in the **Online** state.</span></span> <span data-ttu-id="56d94-284">Ha nem, válassza ki azokat, és kattintson a **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="56d94-284">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="56d94-285">A **feladatkezelés**, kattintson a **új feladat**.</span><span class="sxs-lookup"><span data-stu-id="56d94-285">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="56d94-286">Írjon be egy feladat nevét, például *sloshingTank3D*.</span><span class="sxs-lookup"><span data-stu-id="56d94-286">Enter a name for job such as *sloshingTank3D*.</span></span>
   
   ![Feladat részletei][job_details]
5. <span data-ttu-id="56d94-288">A **feladat-erőforrások**, válassza ki az erőforrás "Csomópont" típusú, és legalább a 2 értékűre állított.</span><span class="sxs-lookup"><span data-stu-id="56d94-288">In **Job resources**, choose the type of resource as “Node” and set the Minimum to 2.</span></span> <span data-ttu-id="56d94-289">Ez a konfiguráció futó Linux két csomópont, amelyek mindegyikének nyolc processzormaggal ebben a példában a feladat.</span><span class="sxs-lookup"><span data-stu-id="56d94-289">This configuration runs the job on two Linux nodes, each of which has eight cores in this example.</span></span>
   
   ![Feladat erőforrások][job_resources]
6. <span data-ttu-id="56d94-291">Kattintson a **feladatok szerkesztése** a bal oldali navigációs, majd **Hozzáadás** feladat hozzáadása a feladatot.</span><span class="sxs-lookup"><span data-stu-id="56d94-291">Click **Edit Tasks** in the left navigation, and then click **Add** to add a task to the job.</span></span> <span data-ttu-id="56d94-292">Négy feladatok hozzá a feladatot a következő parancssorok és beállítások.</span><span class="sxs-lookup"><span data-stu-id="56d94-292">Add four tasks to the job with the following command lines and settings.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="56d94-293">Futó `source /openfoam/settings.sh` beállítja a OpenFOAM és MPI futtatási környezet, ezért a következő feladatokat mindegyikének meghívja a OpenFOAM parancs előtt.</span><span class="sxs-lookup"><span data-stu-id="56d94-293">Running `source /openfoam/settings.sh` sets up the OpenFOAM and MPI runtime environments, so each of the following tasks calls it before the OpenFOAM command.</span></span>
   > 
   > 
   
   * <span data-ttu-id="56d94-294">**1. feladat**.</span><span class="sxs-lookup"><span data-stu-id="56d94-294">**Task 1**.</span></span> <span data-ttu-id="56d94-295">Futtatás **decomposePar** futtatásához adatfájlokat létrehozni **interDyMFoam** párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="56d94-295">Run **decomposePar** to generate data files for running **interDyMFoam** in parallel.</span></span>
     
     * <span data-ttu-id="56d94-296">A feladat egy csomópont hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="56d94-296">Assign one node to the task</span></span>
     * <span data-ttu-id="56d94-297">**Parancssor** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="56d94-297">**Command line** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="56d94-298">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="56d94-298">**Working directory** - /openfoam/sloshingTank3D</span></span>
     
     <span data-ttu-id="56d94-299">Lásd az alábbi ábrát.</span><span class="sxs-lookup"><span data-stu-id="56d94-299">See the following figure.</span></span> <span data-ttu-id="56d94-300">Hasonló módon konfigurálhatja a hátralévő műveletekkel.</span><span class="sxs-lookup"><span data-stu-id="56d94-300">You configure the remaining tasks similarly.</span></span>
     
     ![1. feladat részletei][task_details1]
   * <span data-ttu-id="56d94-302">**2. feladat**.</span><span class="sxs-lookup"><span data-stu-id="56d94-302">**Task 2**.</span></span> <span data-ttu-id="56d94-303">Futtatás **interDyMFoam** párhuzamosan a minta kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="56d94-303">Run **interDyMFoam** in parallel to compute the sample.</span></span>
     
     * <span data-ttu-id="56d94-304">A feladat két csomópont hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="56d94-304">Assign two nodes to the task</span></span>
     * <span data-ttu-id="56d94-305">**Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="56d94-305">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="56d94-306">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="56d94-306">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="56d94-307">**3. feladat**.</span><span class="sxs-lookup"><span data-stu-id="56d94-307">**Task 3**.</span></span> <span data-ttu-id="56d94-308">Futtatás **reconstructPar** idő könyvtárak minden processor_N_ könyvtárból készleteinek egyesíthet egyetlen.</span><span class="sxs-lookup"><span data-stu-id="56d94-308">Run **reconstructPar** to merge the sets of time directories from each processor_N_ directory into a single set.</span></span>
     
     * <span data-ttu-id="56d94-309">A feladat egy csomópont hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="56d94-309">Assign one node to the task</span></span>
     * <span data-ttu-id="56d94-310">**Parancssor** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="56d94-310">**Command line** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="56d94-311">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="56d94-311">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="56d94-312">**4. feladat**.</span><span class="sxs-lookup"><span data-stu-id="56d94-312">**Task 4**.</span></span> <span data-ttu-id="56d94-313">Futtatás **foamToEnsight** OpenFOAM eredménye konvertálható párhuzamosan EnSight fájlok formázása, és helyezze el a EnSight fájlok egy kis könyvtárban Ensight nevű könyvtár.</span><span class="sxs-lookup"><span data-stu-id="56d94-313">Run **foamToEnsight** in parallel to convert the OpenFOAM result files into EnSight format and place the EnSight files in a directory named Ensight in the case directory.</span></span>
     
     * <span data-ttu-id="56d94-314">A feladat két csomópont hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="56d94-314">Assign two nodes to the task</span></span>
     * <span data-ttu-id="56d94-315">**Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="56d94-315">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="56d94-316">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="56d94-316">**Working directory** - /openfoam/sloshingTank3D</span></span>
7. <span data-ttu-id="56d94-317">Adja hozzá ezeket a feladatokat, növekvő sorrendben feladat függőségek.</span><span class="sxs-lookup"><span data-stu-id="56d94-317">Add dependencies to these tasks in ascending task order.</span></span>
   
   ![Tevékenységfüggőségek][task_dependencies]
8. <span data-ttu-id="56d94-319">Kattintson a **Submit** a feladat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="56d94-319">Click **Submit** to run this job.</span></span>
   
   <span data-ttu-id="56d94-320">Alapértelmezés szerint a HPC Pack elküldi a feladatot, a jelenlegi bejelentkezett felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="56d94-320">By default, HPC Pack submits the job as your current logged-on user account.</span></span> <span data-ttu-id="56d94-321">Miután rákattintott **Submit**, megjelenhet egy párbeszédpanel, ahol megadhatja a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="56d94-321">After you click **Submit**, you might see a dialog box prompting you to enter the user name and password.</span></span>
   
   ![Feladat hitelesítő adatai][creds]
   
   <span data-ttu-id="56d94-323">Bizonyos körülmények között a HPC Pack emlékszik a felhasználói adatok előtt adjon meg, és nem jeleníti meg ezen a párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="56d94-323">Under some conditions, HPC Pack remembers the user information you input before and doesn’t show this dialog box.</span></span> <span data-ttu-id="56d94-324">HPC Pack jelenjen meg többé ez a következő parancsot a parancssorba írja be, és ezután a feladat elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="56d94-324">To make HPC Pack show it again, enter the following command at a Command prompt and then submit the job.</span></span>
   
   ```
   hpccred delcreds
   ```
9. <span data-ttu-id="56d94-325">A feladat több tíz perc alatt az a paraméterek, a minta beállítása alapján több órát vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="56d94-325">The job takes from tens of minutes to several hours according to the parameters you have set for the sample.</span></span> <span data-ttu-id="56d94-326">Hőtérkép a Linux csomópontokon futó feladat látható.</span><span class="sxs-lookup"><span data-stu-id="56d94-326">In the heat map, you see the job running on the Linux nodes.</span></span> 
   
   ![Hőtérkép][heat_map]
   
   <span data-ttu-id="56d94-328">Minden egyes csomóponton nyolc folyamatok indulnak el.</span><span class="sxs-lookup"><span data-stu-id="56d94-328">On each node, eight processes are started.</span></span>
   
   ![Linux-folyamat][linux_processes]
10. <span data-ttu-id="56d94-330">A feladat befejezése után található a feladat eredményeinek C:\OpenFoam\sloshingTank3D, és a naplófájlok a C:\OpenFoam mappát.</span><span class="sxs-lookup"><span data-stu-id="56d94-330">When the job finishes, find the job results in folders under C:\OpenFoam\sloshingTank3D, and the log files at C:\OpenFoam.</span></span>

## <a name="view-results-in-ensight"></a><span data-ttu-id="56d94-331">EnSight az eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="56d94-331">View results in EnSight</span></span>
<span data-ttu-id="56d94-332">Ezenkívül szükség [EnSight](https://www.ceisoftware.com/) megtekintheti és elemezheti a OpenFOAM feladat eredményeinek számára.</span><span class="sxs-lookup"><span data-stu-id="56d94-332">Optionally use [EnSight](https://www.ceisoftware.com/) to visualize and analyze the results of the OpenFOAM job.</span></span> <span data-ttu-id="56d94-333">További információt a képi megjelenítés és EnSight animációt megjelenik ez [videó útmutató](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span><span class="sxs-lookup"><span data-stu-id="56d94-333">For more about visualization and animation in EnSight, see this [video guide](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span></span>

1. <span data-ttu-id="56d94-334">Miután telepítette az átjárócsomópont EnSight, indítsa el.</span><span class="sxs-lookup"><span data-stu-id="56d94-334">After you install EnSight on the head node, start it.</span></span>
2. <span data-ttu-id="56d94-335">Nyissa meg a C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span><span class="sxs-lookup"><span data-stu-id="56d94-335">Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span></span>
   
   <span data-ttu-id="56d94-336">Megjelenik egy tartály a megjelenítőben.</span><span class="sxs-lookup"><span data-stu-id="56d94-336">You see a tank in the viewer.</span></span>
   
   ![A EnSight tartály][tank]
3. <span data-ttu-id="56d94-338">Hozzon létre egy **Isosurface** a **internalMesh**, majd válassza a változó **alpha_water**.</span><span class="sxs-lookup"><span data-stu-id="56d94-338">Create an **Isosurface** from **internalMesh**, and then choose the variable **alpha_water**.</span></span>
   
   ![Hozzon létre egy isosurface][isosurface]
4. <span data-ttu-id="56d94-340">Színét **Isosurface_part** az előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="56d94-340">Set the color for **Isosurface_part** created in the previous step.</span></span> <span data-ttu-id="56d94-341">Például állítsa vízjel kék.</span><span class="sxs-lookup"><span data-stu-id="56d94-341">For example, set it to water blue.</span></span>
   
   ![Isosurface szín módosítása][isosurface_color]
5. <span data-ttu-id="56d94-343">Hozzon létre egy **Iso-kötet** a **falvastagságát** kiválasztásával **falvastagságát** a a **részek** panelen, majd kattintson a **Isosurfaces** gomb az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="56d94-343">Create an **Iso-volume** from **walls** by selecting **walls** in the **Parts** panel and click the **Isosurfaces** button in the toolbar.</span></span>
6. <span data-ttu-id="56d94-344">A párbeszédpanelen válassza ki a **típus** , **Isovolume** és állítsa be a minimális **Isovolume tartomány** 0,5.</span><span class="sxs-lookup"><span data-stu-id="56d94-344">In the dialog box, select **Type** as **Isovolume** and set the Min of **Isovolume range** to 0.5.</span></span> <span data-ttu-id="56d94-345">A isovolume létrehozásához kattintson a **létrehozása a kijelölt alkotórészek**.</span><span class="sxs-lookup"><span data-stu-id="56d94-345">To create the isovolume, click **Create with selected parts**.</span></span>
7. <span data-ttu-id="56d94-346">Színét **Iso_volume_part** az előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="56d94-346">Set the color for **Iso_volume_part** created in the previous step.</span></span> <span data-ttu-id="56d94-347">Például állítsa a részletes vízjel kék.</span><span class="sxs-lookup"><span data-stu-id="56d94-347">For example, set it to deep water blue.</span></span>
8. <span data-ttu-id="56d94-348">Színét **falvastagságát**.</span><span class="sxs-lookup"><span data-stu-id="56d94-348">Set the color for **walls**.</span></span> <span data-ttu-id="56d94-349">Például állítsa átlátszó üres.</span><span class="sxs-lookup"><span data-stu-id="56d94-349">For example, set it to transparent white.</span></span>
9. <span data-ttu-id="56d94-350">Most kattintson **lejátszása** a szimuláció az eredmények megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="56d94-350">Now click **Play** to see the results of the simulation.</span></span>
   
    ![Tartály eredménye][tank_result]

## <a name="sample-files"></a><span data-ttu-id="56d94-352">Mintafájlok</span><span class="sxs-lookup"><span data-stu-id="56d94-352">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="56d94-353">A minta XML konfigurációs fájl fürttelepítés PowerShell-parancsprogram</span><span class="sxs-lookup"><span data-stu-id="56d94-353">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a><span data-ttu-id="56d94-354">Mintafájl cred.xml</span><span class="sxs-lookup"><span data-stu-id="56d94-354">Sample cred.xml file</span></span>
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a><span data-ttu-id="56d94-355">Mintafájl silent.cfg MPI telepítése</span><span class="sxs-lookup"><span data-stu-id="56d94-355">Sample silent.cfg file to install MPI</span></span>
```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a><span data-ttu-id="56d94-356">Mintaparancsfájl settings.sh</span><span class="sxs-lookup"><span data-stu-id="56d94-356">Sample settings.sh script</span></span>
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a><span data-ttu-id="56d94-357">Mintaparancsfájl hpcimpirun.sh</span><span class="sxs-lookup"><span data-stu-id="56d94-357">Sample hpcimpirun.sh script</span></span>
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
