---
title: "a Linux virtuális gépeken HPC Pack OpenFOAM aaaRun |} Microsoft Docs"
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
ms.openlocfilehash: 1a51ffe6804abb5156f01d580c9b7a4a9649377a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a><span data-ttu-id="7af64-103">Az OpenFoam futtatása a Microsoft HPC Packkal Azure-beli linuxos RDMA-fürtön</span><span class="sxs-lookup"><span data-stu-id="7af64-103">Run OpenFoam with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>
<span data-ttu-id="7af64-104">Ez a cikk bemutatja, egyirányú toorun OpenFoam az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="7af64-104">This article shows you one way toorun OpenFoam in Azure virtual machines.</span></span> <span data-ttu-id="7af64-105">Itt, a Microsoft HPC Pack-fürt Linux számítási csomópontok az Azure és a Futtatás telepít egy [OpenFoam](http://openfoam.com/) Intel MPI feladatot.</span><span class="sxs-lookup"><span data-stu-id="7af64-105">Here, you deploy a Microsoft HPC Pack cluster with Linux compute nodes on Azure and run an [OpenFoam](http://openfoam.com/) job with Intel MPI.</span></span> <span data-ttu-id="7af64-106">Használhatja az RDMA-kompatibilis Azure virtuális gépek hello számítási csomópontokat, így hello számítási csomópontok hello Azure RDMA hálózati protokollt használó kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="7af64-106">You can use RDMA-capable Azure VMs for hello compute nodes, so that hello compute nodes communicate over hello Azure RDMA network.</span></span> <span data-ttu-id="7af64-107">Egyéb beállítások toorun teljesen tartalmazza az Azure-ban OpenFoam konfigurált hello UberCloud meg például a piactér kereskedelmi lemezképeket [OpenFoam 2.3 a CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), és futtatja a [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span><span class="sxs-lookup"><span data-stu-id="7af64-107">Other options toorun OpenFoam in Azure include fully configured commercial images available in hello Marketplace, such as UberCloud's [OpenFoam 2.3 on CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), and by running on [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="7af64-108">(A nyissa meg a mező művelet és adatkezelési) OpenFOAM egy nyílt forráskódú számítási folyadékból dynamics (CFD) csomagot, amely széles körben használt mérnöki és tudományos, kereskedelmi és a academic szervezetekben.</span><span class="sxs-lookup"><span data-stu-id="7af64-108">OpenFOAM (for Open Field Operation and Manipulation) is an open-source computational fluid dynamics (CFD) software package that is used widely in engineering and science, in both commercial and academic organizations.</span></span> <span data-ttu-id="7af64-109">Ez magában foglalja az eszközök meshing, nevezetesen snappyHexMesh, egy parallelized mesher összetett CAD-geometriája, és előtti és utáni feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="7af64-109">It includes tools for meshing, notably snappyHexMesh, a parallelized mesher for complex CAD geometries, and for pre- and post-processing.</span></span> <span data-ttu-id="7af64-110">Szinte minden folyamatok engedélyezése a felhasználók tootake teljes mértékben a rendelkezésükre hardverek párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="7af64-110">Almost all processes run in parallel, enabling users tootake full advantage of computer hardware at their disposal.</span></span>  

<span data-ttu-id="7af64-111">A Microsoft HPC Pack szolgáltatások toorun biztosít a nagyméretű HPC és párhuzamos alkalmazások, beleértve a MPI alkalmazásokat, a Microsoft Azure virtuális gépek fürtjein.</span><span class="sxs-lookup"><span data-stu-id="7af64-111">Microsoft HPC Pack provides features toorun large-scale HPC and parallel applications, including MPI applications, on clusters of Microsoft Azure virtual machines.</span></span> <span data-ttu-id="7af64-112">HPC Pack támogatja a virtuális gépeken telepített HPC Pack-fürtben lévő Linux alkalmazások csomópontjain futó Linux HPC is.</span><span class="sxs-lookup"><span data-stu-id="7af64-112">HPC Pack also supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="7af64-113">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) egy bevezető toousing a Linux számítási csomópontok HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="7af64-113">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction toousing Linux compute nodes with HPC Pack.</span></span>

> [!NOTE]
> <span data-ttu-id="7af64-114">Ez a cikk bemutatja, hogyan toorun egy Linux MPI-feladatok HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="7af64-114">This article illustrates how toorun a Linux MPI workload with HPC Pack.</span></span> <span data-ttu-id="7af64-115">Azt feltételezi, hogy rendelkezik-e bizonyos fokú ismeretét, Linux rendszer felügyeleti és a Linux fürtökön MPI munkaterheket futtatnak.</span><span class="sxs-lookup"><span data-stu-id="7af64-115">It assumes you have some familiarity with Linux system administration and with running MPI workloads on Linux clusters.</span></span> <span data-ttu-id="7af64-116">MPI és OpenFOAM hello állók közül. Ez a cikk látható a különböző verzióit használhatja, ha lehetséges, hogy toomodify néhány telepítési és konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7af64-116">If you use versions of MPI and OpenFOAM different from hello ones shown in this article, you might have toomodify some installation and configuration steps.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="7af64-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7af64-117">Prerequisites</span></span>
* <span data-ttu-id="7af64-118">**HPC Pack fürt RDMA-kompatibilisek-e a Linux és a számítási csomópontok** - fürt üzembe helyezése HPC Pack méretű A8, A9, H16r, vagy H16rm Linux számítási csomópontok használatával egy [Azure Resource Manager sablon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) vagy egy [Azure PowerShell parancsfájl](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="7af64-118">**HPC Pack cluster with RDMA-capable Linux compute nodes** - Deploy an HPC Pack cluster with size A8, A9, H16r, or H16rm Linux compute nodes using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="7af64-119">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) hello Előfeltételek és az egyik beállítási mód lépéseit.</span><span class="sxs-lookup"><span data-stu-id="7af64-119">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for hello prerequisites and steps for either option.</span></span> <span data-ttu-id="7af64-120">Ha hello PowerShell parancsfájl központi telepítés lehetőséget választja, tekintse meg a hello minta konfigurációs fájlt a mintafájlok hello hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="7af64-120">If you choose hello PowerShell script deployment option, see hello sample configuration file in hello sample files at hello end of this article.</span></span> <span data-ttu-id="7af64-121">A méret A8 Windows Server 2012 R2 átjárócsomópont álló konfigurációs egy Azure-alapú toodeploy HPC Pack fürtöt használ, és 2 mérete A8 SUSE Linux Enterprise Server 12 számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="7af64-121">Use this configuration toodeploy an Azure-based HPC Pack cluster consisting of a size A8 Windows Server 2012 R2 head node and 2 size A8 SUSE Linux Enterprise Server 12 compute nodes.</span></span> <span data-ttu-id="7af64-122">Az előfizetés és a szolgáltatás nevének megfelelő értékeit helyettesítse.</span><span class="sxs-lookup"><span data-stu-id="7af64-122">Substitute appropriate values for your subscription and service names.</span></span> 
  
  <span data-ttu-id="7af64-123">**További dolgot tooknow**</span><span class="sxs-lookup"><span data-stu-id="7af64-123">**Additional things tooknow**</span></span>
  
  * <span data-ttu-id="7af64-124">Linux RDMA hálózati Előfeltételek az Azure-ban, lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7af64-124">For Linux RDMA networking prerequisites in Azure, see [High performance compute VM sizes](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  * <span data-ttu-id="7af64-125">Ha hello Powershell parancsprogram rendszerbe állítási beállításának használata esetén telepítse az összes hello Linux számítási csomópontokat belül egy felhőalapú szolgáltatás toouse hello RDMA hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="7af64-125">If you use hello Powershell script deployment option, deploy all hello Linux compute nodes within one cloud service toouse hello RDMA network connection.</span></span>
  * <span data-ttu-id="7af64-126">Miután üzembe hello Linux csomópontok, csatlakozzon az SSH tooperform minden további felügyeleti feladatot.</span><span class="sxs-lookup"><span data-stu-id="7af64-126">After deploying hello Linux nodes, connect by SSH tooperform any additional administrative tasks.</span></span> <span data-ttu-id="7af64-127">Hello SSH-kapcsolat adatai az egyes Linux virtuális gépek található hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7af64-127">Find hello SSH connection details for each Linux VM in hello Azure portal.</span></span>  
* <span data-ttu-id="7af64-128">**Intel MPI** -toorun OpenFOAM SLES 12 HPC a számítási csomópontok az Azure-ban, a hello tooinstall hello Intel MPI könyvtár 5 futásidejű kell [Intel.com hely](https://software.intel.com/en-us/intel-mpi-library/).</span><span class="sxs-lookup"><span data-stu-id="7af64-128">**Intel MPI** - toorun OpenFOAM on SLES 12 HPC compute nodes in Azure, you need tooinstall hello Intel MPI Library 5 runtime from hello [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/).</span></span> <span data-ttu-id="7af64-129">(Intel MPI 5 előre telepítve van a CentOS-alapú HPC képek.)  Egy későbbi lépésben szükség esetén telepítse az Intel MPI a Linux számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="7af64-129">(Intel MPI 5 is preinstalled on CentOS-based HPC images.)  In a later step, if necessary, install Intel MPI on your Linux compute nodes.</span></span> <span data-ttu-id="7af64-130">Ebben a lépésben tooprepare Intel, hogy a regisztrálása után hivatkozásra hello hello megerősítő e-mail toohello kapcsolódó weblapon.</span><span class="sxs-lookup"><span data-stu-id="7af64-130">tooprepare for this step, after you register with Intel, follow hello link in hello confirmation email toohello related web page.</span></span> <span data-ttu-id="7af64-131">Ezt követően másolási hello letöltése hello .tgz fájl hello Intel MPI megfelelő verziójának hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="7af64-131">Then, copy hello download link for hello .tgz file for hello appropriate version of Intel MPI.</span></span> <span data-ttu-id="7af64-132">Ez a cikk Intel MPI verzió 5.0.3.048 alapul.</span><span class="sxs-lookup"><span data-stu-id="7af64-132">This article is based on Intel MPI version 5.0.3.048.</span></span>
* <span data-ttu-id="7af64-133">**OpenFOAM forrás Pack** -Linux hello OpenFOAM forrás Pack szoftverek letöltését hello [OpenFOAM Foundation webhely](http://openfoam.org/download/2-3-1-source/).</span><span class="sxs-lookup"><span data-stu-id="7af64-133">**OpenFOAM Source Pack** - Download hello OpenFOAM Source Pack software for Linux from hello [OpenFOAM Foundation site](http://openfoam.org/download/2-3-1-source/).</span></span> <span data-ttu-id="7af64-134">Ez a cikk a forrás csomag verziója letölthető 2.3.1 OpenFOAM-2.3.1.tgz van alapján.</span><span class="sxs-lookup"><span data-stu-id="7af64-134">This article is based on Source Pack version 2.3.1, available for download as OpenFOAM-2.3.1.tgz.</span></span> <span data-ttu-id="7af64-135">Később Ez a cikk toounpack hello utasításokat követve, és OpenFOAM fordítási hello Linux számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="7af64-135">Follow hello instructions later in this article toounpack and compile OpenFOAM on hello Linux compute nodes.</span></span>
* <span data-ttu-id="7af64-136">**EnSight** (nem kötelező) – toosee hello eredményeit a OpenFOAM szimuláció, töltse le és telepítse a hello [EnSight](https://www.ceisoftware.com/download/) képi megjelenítés és elemzések program.</span><span class="sxs-lookup"><span data-stu-id="7af64-136">**EnSight** (optional) - toosee hello results of your OpenFOAM simulation, download and install hello [EnSight](https://www.ceisoftware.com/download/) visualization and analysis program.</span></span> <span data-ttu-id="7af64-137">Licencelési és letöltési információkat hello EnSight helyen vannak.</span><span class="sxs-lookup"><span data-stu-id="7af64-137">Licensing and download information are at hello EnSight site.</span></span>

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="7af64-138">Kölcsönös, a számítási csomópontok közötti megbízhatósági kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="7af64-138">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="7af64-139">A kereszt-csomópont feladat több Linux csomópontokon futó szükséges hello csomópontok tootrust egymáshoz (által **rsh** vagy **ssh**).</span><span class="sxs-lookup"><span data-stu-id="7af64-139">Running a cross-node job on multiple Linux nodes requires hello nodes tootrust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="7af64-140">Hello HPC Pack fürt létrehozásakor a Microsoft HPC Pack IaaS telepítési parancsfájl hello hello parancsfájl automatikusan beállítja állandó kölcsönös megbízhatósági hello rendszergazdai fiókhoz megadott.</span><span class="sxs-lookup"><span data-stu-id="7af64-140">When you create hello HPC Pack cluster with hello Microsoft HPC Pack IaaS deployment script, hello script automatically sets up permanent mutual trust for hello administrator account you specify.</span></span> <span data-ttu-id="7af64-141">A nem rendszergazda felhasználók hello fürt tartományt hoz létre meg kell tooset hello csomópontok közötti ideiglenes kölcsönös megbízhatósági fel egy feladat le van foglalva toothem, és szüntesse meg a hello kapcsolat hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="7af64-141">For non-administrator users you create in hello cluster's domain, you have tooset up temporary mutual trust among hello nodes when a job is allocated toothem, and destroy hello relationship after hello job is complete.</span></span> <span data-ttu-id="7af64-142">minden felhasználó esetében tooestablish megbízhatósági adjon meg egy RSA kulcspár toohello fürt használó HPC Pack hello megbízhatósági kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="7af64-142">tooestablish trust for each user, provide an RSA key pair toohello cluster that HPC Pack uses for hello trust relationship.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="7af64-143">Egy RSA kulcspár létrehozása</span><span class="sxs-lookup"><span data-stu-id="7af64-143">Generate an RSA key pair</span></span>
<span data-ttu-id="7af64-144">Könnyen toogenerate egy RSA kulcsból álló kulcspárt, amely tartalmazza a nyilvános kulcsot és titkos kulccsal, futtatásával hello Linux **ssh-keygen** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7af64-144">It's easy toogenerate an RSA key pair, which contains a public key and a private key, by running hello Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="7af64-145">Jelentkezzen be tooa Linux-számítógép.</span><span class="sxs-lookup"><span data-stu-id="7af64-145">Log on tooa Linux computer.</span></span>
2. <span data-ttu-id="7af64-146">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="7af64-146">Run hello following command:</span></span>
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="7af64-147">Nyomja le az **Enter** toouse hello alapértelmezett beállítások hello parancs befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="7af64-147">Press **Enter** toouse hello default settings until hello command is completed.</span></span> <span data-ttu-id="7af64-148">Ne adjon meg egy jelszót. Amikor a rendszer kéri a jelszót, csak Entert **Enter**.</span><span class="sxs-lookup"><span data-stu-id="7af64-148">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Egy RSA kulcspár létrehozása][keygen]
3. <span data-ttu-id="7af64-150">Módosítsa a könyvtárat toohello ~/.ssh könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="7af64-150">Change directory toohello ~/.ssh directory.</span></span> <span data-ttu-id="7af64-151">hello titkos kulcs id_rsa.pub a id_rsa és hello nyilvános kulcs tárolja.</span><span class="sxs-lookup"><span data-stu-id="7af64-151">hello private key is stored in id_rsa and hello public key in id_rsa.pub.</span></span>
   
   ![Nyilvános és titkos kulcsai][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a><span data-ttu-id="7af64-153">Hello kulcspár toohello HPC Pack fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7af64-153">Add hello key pair toohello HPC Pack cluster</span></span>
1. <span data-ttu-id="7af64-154">Ellenőrizze a távoli asztali kapcsolat tooyour átjárócsomópont a HPC Pack rendszergazdai fiókkal (hello rendszergazdai fiók beállítása hello telepítési parancsfájl futtatásakor).</span><span class="sxs-lookup"><span data-stu-id="7af64-154">Make a Remote Desktop connection tooyour head node with your HPC Pack administrator account (hello administrator account you set up when you ran hello deployment script).</span></span>
2. <span data-ttu-id="7af64-155">Szabványos Windows Server eljárások toocreate tartományi felhasználói fiókot használja a hello fürt Active Directory-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="7af64-155">Use standard Windows Server procedures toocreate a domain user account in hello cluster's Active Directory domain.</span></span> <span data-ttu-id="7af64-156">Hello Active Directory – felhasználók és számítógépek eszközzel például hello átjárócsomópont használja.</span><span class="sxs-lookup"><span data-stu-id="7af64-156">For example, use hello Active Directory User and Computers tool on hello head node.</span></span> <span data-ttu-id="7af64-157">hello a cikkben szereplő példák azt feltételezik, hpclab\hpcuser nevű tartományi felhasználót kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7af64-157">hello examples in this article assume you create a domain user named hpclab\hpcuser.</span></span>
3. <span data-ttu-id="7af64-158">Hozzon létre egy fájlt C:\cred.xml, és másolja hello RSA kulcsadatokat bele.</span><span class="sxs-lookup"><span data-stu-id="7af64-158">Create a file named C:\cred.xml and copy hello RSA key data into it.</span></span> <span data-ttu-id="7af64-159">A mintafájl cred.xml van ez a cikk végén hello.</span><span class="sxs-lookup"><span data-stu-id="7af64-159">A sample cred.xml file is at hello end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. <span data-ttu-id="7af64-160">Nyisson meg egy parancssort, és írja be a következő parancs tooset hello adatok hello hpclab\hpcuser fiók hitelesítő adatai hello.</span><span class="sxs-lookup"><span data-stu-id="7af64-160">Open a Command Prompt and enter hello following command tooset hello credentials data for hello hpclab\hpcuser account.</span></span> <span data-ttu-id="7af64-161">Hello használata **extendeddata** paraméter toopass hello C:\cred.xml fájl létrehozott hello kulcs adatok nevét.</span><span class="sxs-lookup"><span data-stu-id="7af64-161">You use hello **extendeddata** parameter toopass hello name of C:\cred.xml file you created for hello key data.</span></span>
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="7af64-162">Ez a parancs sikeresen kimeneti nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7af64-162">This command completes successfully without output.</span></span> <span data-ttu-id="7af64-163">Miután beállította a hello toorun feladatok kell hello felhasználói fiókok hitelesítő adatait, hello cred.xml fájlt tárolja biztonságos helyen, vagy törölje azt.</span><span class="sxs-lookup"><span data-stu-id="7af64-163">After setting hello credentials for hello user accounts you need toorun jobs, store hello cred.xml file in a secure location, or delete it.</span></span>
5. <span data-ttu-id="7af64-164">Ha RSA kulcspár hello a Linux-csomópontok egyikén jön létre, azok használatának befejezése után ne feledje toodelete hello kulcsok.</span><span class="sxs-lookup"><span data-stu-id="7af64-164">If you generated hello RSA key pair on one of your Linux nodes, remember toodelete hello keys after you finish using them.</span></span> <span data-ttu-id="7af64-165">Ha HPC Pack talál egy meglévő id_rsa vagy id_rsa.pub fájl, akkor nem kölcsönös megbízhatóság kialakításához.</span><span class="sxs-lookup"><span data-stu-id="7af64-165">If HPC Pack finds an existing id_rsa file or id_rsa.pub file, it does not set up mutual trust.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7af64-166">Nem ajánlott a Linux feladat futtatásával a fürt rendszergazdai megosztott fürtön, mivel hello Linux csomópontján hello legfelső szintű fiók alatt fut egy feladat, a rendszergazda által küldött.</span><span class="sxs-lookup"><span data-stu-id="7af64-166">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under hello root account on hello Linux nodes.</span></span> <span data-ttu-id="7af64-167">A feladat a nem rendszergazda felhasználók által benyújtott azonban Linux helyi felhasználói fiók alatt fut, az azonos név hello feladat felhasználóként hello.</span><span class="sxs-lookup"><span data-stu-id="7af64-167">However, a job submitted by a non-administrator user runs under a local Linux user account with hello same name as hello job user.</span></span> <span data-ttu-id="7af64-168">Ebben az esetben HPC Pack állít be a Linux-felhasználó kölcsönös megbízhatósági toohello feladat lefoglalt hello csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="7af64-168">In this case, HPC Pack sets up mutual trust for this Linux user across hello nodes allocated toohello job.</span></span> <span data-ttu-id="7af64-169">Hello feladat futtatása előtt manuálisan csomópontján hello Linux hello Linux felhasználói állíthat be, vagy a HPC Pack hello felhasználó létrehozása automatikusan hello feladat elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="7af64-169">You can set up hello Linux user manually on hello Linux nodes before running hello job, or HPC Pack creates hello user automatically when hello job is submitted.</span></span> <span data-ttu-id="7af64-170">HPC Pack hello felhasználó hoz létre, ha HPC Pack törli azt hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="7af64-170">If HPC Pack creates hello user, HPC Pack deletes it after hello job completes.</span></span> <span data-ttu-id="7af64-171">tooreduce biztonsági fenyegetések HPC Pack hello kulcsok feladat befejeződése után eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="7af64-171">tooreduce security threats, HPC Pack removes hello keys after job completion.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="7af64-172">Linux-csomópontok fájlmegosztás beállítása</span><span class="sxs-lookup"><span data-stu-id="7af64-172">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="7af64-173">Most már készen egy szabványos SMB-megosztás hello átjárócsomópont az egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="7af64-173">Now set up a standard SMB share on a folder on hello head node.</span></span> <span data-ttu-id="7af64-174">tooallow hello Linux csomópontok tooaccess alkalmazásfájlok közös elérési úttal rendelkező, a csatlakoztatási hello megosztott mappa hello Linux csomópontján.</span><span class="sxs-lookup"><span data-stu-id="7af64-174">tooallow hello Linux nodes tooaccess application files with a common path, mount hello shared folder on hello Linux nodes.</span></span> <span data-ttu-id="7af64-175">Ha azt szeretné, használhatja a beállítást, például egy Azure fájlok fájlmegosztás - számos forgatókönyv - vagy az NFS-megosztások ajánlott egy másik fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="7af64-175">If you want, you can use another file sharing option, such as an Azure Files share - recommended for many scenarios - or an NFS share.</span></span> <span data-ttu-id="7af64-176">Hello fájlmegosztási információk és részletes lépéseit lásd: [Ismerkedés az Azure-ban HPC Pack fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7af64-176">See hello file sharing information and detailed steps in [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="7af64-177">Hozzon létre egy mappát a hello átjárócsomópont, és ossza meg úgy, hogy olvasási/írási jogosultsággal tooEveryone.</span><span class="sxs-lookup"><span data-stu-id="7af64-177">Create a folder on hello head node, and share it tooEveryone by setting Read/Write privileges.</span></span> <span data-ttu-id="7af64-178">Például megosztás C:\OpenFOAM hello átjárócsomópont, \\ \\SUSE12RDMA-HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="7af64-178">For example, share C:\OpenFOAM on hello head node as \\\\SUSE12RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="7af64-179">Itt *SUSE12RDMA-HN* hello átjárócsomópont hello állomás neve.</span><span class="sxs-lookup"><span data-stu-id="7af64-179">Here, *SUSE12RDMA-HN* is hello host name of hello head node.</span></span>
2. <span data-ttu-id="7af64-180">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="7af64-180">Open a Windows PowerShell window and run hello following commands:</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

<span data-ttu-id="7af64-181">hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /openfoam nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="7af64-181">hello first command creates a folder named /openfoam on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="7af64-182">hello második parancs a Linux-csomópont hello dir_mode és file_mode bit beállítása too777 hello megosztott mappa //SUSE12RDMA-HN/OpenFOAM csatlakoztatja.</span><span class="sxs-lookup"><span data-stu-id="7af64-182">hello second command mounts hello shared folder //SUSE12RDMA-HN/OpenFOAM on hello Linux nodes with dir_mode and file_mode bits set too777.</span></span> <span data-ttu-id="7af64-183">Hello *felhasználónév* és *jelszó* hello parancs hello hello átjárócsomópont a felhasználó hitelesítő adatait kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7af64-183">hello *username* and *password* in hello command should be hello credentials of a user on hello head node.</span></span>

> [!NOTE]
> <span data-ttu-id="7af64-184">hello "\\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7af64-184">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="7af64-185">"\\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.</span><span class="sxs-lookup"><span data-stu-id="7af64-185">“\\`,” means hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

## <a name="install-mpi-and-openfoam"></a><span data-ttu-id="7af64-186">MPI és OpenFOAM telepítése</span><span class="sxs-lookup"><span data-stu-id="7af64-186">Install MPI and OpenFOAM</span></span>
<span data-ttu-id="7af64-187">MPI feladatként hello RDMA hálózati OpenFOAM toorun, toocompile OpenFOAM hello Intel MPI könyvtárakkal kell.</span><span class="sxs-lookup"><span data-stu-id="7af64-187">toorun OpenFOAM as an MPI job on hello RDMA network, you need toocompile OpenFOAM with hello Intel MPI libraries.</span></span> 

<span data-ttu-id="7af64-188">Először futtassa több **clusrun** tooinstall Intel MPI függvénytárak (Ha még nincs telepítve) és OpenFOAM parancsok a Linux-csomóponton.</span><span class="sxs-lookup"><span data-stu-id="7af64-188">First, run several **clusrun** commands tooinstall Intel MPI libraries (if not already installed) and OpenFOAM on your Linux nodes.</span></span> <span data-ttu-id="7af64-189">Használjon hello átjárócsomópont megosztás korábban konfigurált tooshare hello telepítőfájlok hello Linux csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="7af64-189">Use hello head node share configured previously tooshare hello installation files among hello Linux nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7af64-190">Ezeket a telepítési és fordítása lépések példái.</span><span class="sxs-lookup"><span data-stu-id="7af64-190">These installation and compiling steps are examples.</span></span> <span data-ttu-id="7af64-191">Meg kell néhány a Linux rendszer felügyeleti tooensure, hogy a függő compilers – és a szalagtárak megfelelően vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="7af64-191">You need some knowledge of Linux system administration tooensure that dependent compilers and libraries are installed correctly.</span></span> <span data-ttu-id="7af64-192">Előfordulhat, hogy szüksége toomodify bizonyos környezeti változók és más beállítások az Intel MPI és OpenFOAM verzióit.</span><span class="sxs-lookup"><span data-stu-id="7af64-192">You might need toomodify certain environment variables or other settings for your versions of Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="7af64-193">További információkért lásd: [Intel MPI Library for Linux – telepítési útmutató](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) és [OpenFOAM forrás csomag telepítése](http://openfoam.org/download/2-3-1-source/) alkalmaz környezetében.</span><span class="sxs-lookup"><span data-stu-id="7af64-193">For details, see [Intel MPI Library for Linux Installation Guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) and [OpenFOAM Source Pack Installation](http://openfoam.org/download/2-3-1-source/) for your environment.</span></span>
> 
> 

### <a name="install-intel-mpi"></a><span data-ttu-id="7af64-194">Intel MPI telepítése</span><span class="sxs-lookup"><span data-stu-id="7af64-194">Install Intel MPI</span></span>
<span data-ttu-id="7af64-195">Hello letöltött telepítési csomag mentéséhez az Intel MPI (ebben a példában l_mpi_p_5.0.3.048.tgz) a C:\OpenFoam hello központi csomóponton, hogy hello Linux csomópontok /openfoam hozzáfér ehhez a fájl.</span><span class="sxs-lookup"><span data-stu-id="7af64-195">Save hello downloaded installation package for Intel MPI (l_mpi_p_5.0.3.048.tgz in this example) in C:\OpenFoam on hello head node so that hello Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="7af64-196">Ezután futtassa **clusrun** tooinstall Intel MPI könyvtár minden hello Linux csomópontján.</span><span class="sxs-lookup"><span data-stu-id="7af64-196">Then run **clusrun** tooinstall Intel MPI library on all hello Linux nodes.</span></span>

1. <span data-ttu-id="7af64-197">hello alábbi hello telepítési csomag másolása a parancsokat, és bontsa ki túl/opt/intel minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="7af64-197">hello following commands copy hello installation package and extract it too/opt/intel on each node.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. <span data-ttu-id="7af64-198">Intel MPI könyvtár tooinstall csendes módban, a silent.cfg fájl használható.</span><span class="sxs-lookup"><span data-stu-id="7af64-198">tooinstall Intel MPI Library silently, use a silent.cfg file.</span></span> <span data-ttu-id="7af64-199">Található példa hello mintafájlok hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="7af64-199">You can find an example in hello sample files at hello end of this article.</span></span> <span data-ttu-id="7af64-200">Hely a fájlt az hello megosztott mappa /openfoam.</span><span class="sxs-lookup"><span data-stu-id="7af64-200">Place this file in hello shared folder /openfoam.</span></span> <span data-ttu-id="7af64-201">Hello silent.cfg fájllal kapcsolatos részletekért lásd: [Intel MPI Library for Linux – telepítési útmutató - csendes telepítést](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span><span class="sxs-lookup"><span data-stu-id="7af64-201">For details about hello silent.cfg file, see [Intel MPI Library for Linux Installation Guide - Silent Installation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="7af64-202">Győződjön meg arról, hogy Ön a silent.cfg fájl szövegfájl, amelynek Linux sorvégeket (csak LF, nem a CR LF).</span><span class="sxs-lookup"><span data-stu-id="7af64-202">Make sure that you save your silent.cfg file as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="7af64-203">Ez a lépés biztosítja, hogy ez megfelelően hello Linux csomópontján.</span><span class="sxs-lookup"><span data-stu-id="7af64-203">This step ensures that it runs properly on hello Linux nodes.</span></span>
   > 
   > 
3. <span data-ttu-id="7af64-204">Intel MPI könyvtár telepítése csendes módban.</span><span class="sxs-lookup"><span data-stu-id="7af64-204">Install Intel MPI Library in silent mode.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a><span data-ttu-id="7af64-205">MPI konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7af64-205">Configure MPI</span></span>
<span data-ttu-id="7af64-206">Tesztelési, adja hozzá mindegyik hello Linux csomópontok sorok toohello /etc/security/limits.conf következő hello:</span><span class="sxs-lookup"><span data-stu-id="7af64-206">For testing, you should add hello following lines toohello /etc/security/limits.conf on each of hello Linux nodes:</span></span>

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


<span data-ttu-id="7af64-207">Indítsa újra a hello Linux csomópontok hello limits.conf fájl frissítése után.</span><span class="sxs-lookup"><span data-stu-id="7af64-207">Restart hello Linux nodes after you update hello limits.conf file.</span></span> <span data-ttu-id="7af64-208">Például használja az hello következő **clusrun** parancs:</span><span class="sxs-lookup"><span data-stu-id="7af64-208">For example, use hello following **clusrun** command:</span></span>

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

<span data-ttu-id="7af64-209">Az újraindítás után győződjön meg arról, mint /openfoam csatlakoztatva van a hello megosztott mappában.</span><span class="sxs-lookup"><span data-stu-id="7af64-209">After restarting, ensure that hello shared folder is mounted as /openfoam.</span></span>

### <a name="compile-and-install-openfoam"></a><span data-ttu-id="7af64-210">Fordítsa le és OpenFOAM telepítése</span><span class="sxs-lookup"><span data-stu-id="7af64-210">Compile and install OpenFOAM</span></span>
<span data-ttu-id="7af64-211">Letöltött telepítőcsomag hello hello OpenFOAM forrás Pack (OpenFOAM 2.3.1.tgz ebben a példában) tooC:\OpenFoam mentheti hello átjárócsomópont, hogy hello Linux csomópontok /openfoam hozzáfér ehhez a fájlt.</span><span class="sxs-lookup"><span data-stu-id="7af64-211">Save hello downloaded installation package for hello OpenFOAM Source Pack (OpenFOAM-2.3.1.tgz in this example) tooC:\OpenFoam on hello head node so that hello Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="7af64-212">Ezután futtassa **clusrun** toocompile OpenFOAM az összes Linux csomópontok hello parancsok.</span><span class="sxs-lookup"><span data-stu-id="7af64-212">Then run **clusrun** commands toocompile OpenFOAM on all hello Linux nodes.</span></span>

1. <span data-ttu-id="7af64-213">Hozzon létre egy mappát /opt/OpenFOAM minden egyes csomóponton Linux másolási hello csomag toothis forrásmappa, és bontsa ki van.</span><span class="sxs-lookup"><span data-stu-id="7af64-213">Create a folder /opt/OpenFOAM on each Linux node, copy hello source package toothis folder, and extract it there.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. <span data-ttu-id="7af64-214">toocompile OpenFOAM hello Intel MPI könyvtár, először állítsa be néhány környezetiblokk-változót Intel MPI és OpenFOAM együtt.</span><span class="sxs-lookup"><span data-stu-id="7af64-214">toocompile OpenFOAM with hello Intel MPI Library, first set up some environment variables for both Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="7af64-215">Nevű settings.sh tooset hello változók bash parancsfájl használatára.</span><span class="sxs-lookup"><span data-stu-id="7af64-215">Use a bash script called settings.sh tooset hello variables.</span></span> <span data-ttu-id="7af64-216">Található példa hello mintafájlok hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="7af64-216">You can find an example in hello sample files at hello end of this article.</span></span> <span data-ttu-id="7af64-217">Hely a hello (mentett rendelkező Linux sorvégződések) fájlhoz megosztott mappa /openfoam.</span><span class="sxs-lookup"><span data-stu-id="7af64-217">Place this file (saved with Linux line endings) in hello shared folder /openfoam.</span></span> <span data-ttu-id="7af64-218">Ez a fájl is, hogy használja-e újabb toorun egy OpenFOAM feladat hello MPI és OpenFOAM futtatókörnyezetek beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7af64-218">This file also contains settings for hello MPI and OpenFOAM runtimes that you use later toorun an OpenFOAM job.</span></span>
3. <span data-ttu-id="7af64-219">Függő csomagok telepítése toocompile OpenFOAM szükséges.</span><span class="sxs-lookup"><span data-stu-id="7af64-219">Install dependent packages needed toocompile OpenFOAM.</span></span> <span data-ttu-id="7af64-220">Attól függően, hogy a Linux-disztribúció először meg kell tooadd tára.</span><span class="sxs-lookup"><span data-stu-id="7af64-220">Depending on your Linux distribution, you might first need tooadd a repository.</span></span> <span data-ttu-id="7af64-221">Futtatás **clusrun** hasonló toohello következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="7af64-221">Run **clusrun** commands similar toohello following:</span></span>
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    <span data-ttu-id="7af64-222">Ha szükséges, az SSH tooeach Linux csomópont toorun hello megfelelően futásuk tooconfirm parancsokat.</span><span class="sxs-lookup"><span data-stu-id="7af64-222">If necessary, SSH tooeach Linux node toorun hello commands tooconfirm that they run properly.</span></span>
4. <span data-ttu-id="7af64-223">Futtassa a következő parancs toocompile OpenFOAM hello.</span><span class="sxs-lookup"><span data-stu-id="7af64-223">Run hello following command toocompile OpenFOAM.</span></span> <span data-ttu-id="7af64-224">hello összeállítási folyamat egyes idő toocomplete vesz igénybe, és nagy mennyiségű információ toostandard naplót hoz létre, ezért hello **/ időosztásos** toodisplay hello kimeneti időosztásos lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="7af64-224">hello compilation process takes some time toocomplete and generates a large amount of log information toostandard output, so use hello **/interleaved** option toodisplay hello output interleaved.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > <span data-ttu-id="7af64-225">hello "\\`" szimbólum hello parancsban értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7af64-225">hello “\\`” symbol in hello command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="7af64-226">"\\`&" azt jelenti, hogy a hello "&" hello parancs része.</span><span class="sxs-lookup"><span data-stu-id="7af64-226">“\\`&” means hello “&” is a part of hello command.</span></span>
   > 
   > 

## <a name="prepare-toorun-an-openfoam-job"></a><span data-ttu-id="7af64-227">Toorun OpenFOAM feladatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="7af64-227">Prepare toorun an OpenFOAM job</span></span>
<span data-ttu-id="7af64-228">Most get készen toorun egy MPI feladat sloshingTank3D, amely hello OpenFoam minta egyike, két Linux csomópont hívása.</span><span class="sxs-lookup"><span data-stu-id="7af64-228">Now get ready toorun an MPI job called sloshingTank3D, which is one of hello OpenFoam samples, on two Linux nodes.</span></span> 

### <a name="set-up-hello-runtime-environment"></a><span data-ttu-id="7af64-229">Hello futásidejű környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="7af64-229">Set up hello runtime environment</span></span>
<span data-ttu-id="7af64-230">tooset hello futásidejű környezetek MPI és OpenFOAM hello Linux csomóponton, futtassa a következő parancsot egy Windows PowerShell-ablakban hello központi csomóponton hello fel.</span><span class="sxs-lookup"><span data-stu-id="7af64-230">tooset up hello runtime environments for MPI and OpenFOAM on hello Linux nodes, run hello following command in a Windows PowerShell window on hello head node.</span></span> <span data-ttu-id="7af64-231">(Ez a parancs érvénytelen a SUSE Linux csak.)</span><span class="sxs-lookup"><span data-stu-id="7af64-231">(This command is valid for SUSE Linux only.)</span></span>

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a><span data-ttu-id="7af64-232">Mintaadatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="7af64-232">Prepare sample data</span></span>
<span data-ttu-id="7af64-233">Használjon hello átjárócsomópont megosztás korábban konfigurált tooshare fájlok közötti hello Linux csomópontok (mint /openfoam csatlakoztatott).</span><span class="sxs-lookup"><span data-stu-id="7af64-233">Use hello head node share you configured previously tooshare files among hello Linux nodes (mounted as /openfoam).</span></span>

1. <span data-ttu-id="7af64-234">A Linux SSH tooone számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="7af64-234">SSH tooone of your Linux compute nodes.</span></span>
2. <span data-ttu-id="7af64-235">A következő parancs tooset hello OpenFOAM futásidejű környezet hello üzemeltetéséhez, még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="7af64-235">Run hello following command tooset up hello OpenFOAM runtime environment, if you haven’t already done this.</span></span>
   
   ```
   $ source /openfoam/settings.sh
   ```
3. <span data-ttu-id="7af64-236">Másolja hello sloshingTank3D minta toohello megosztott mappát, és keresse meg a tooit.</span><span class="sxs-lookup"><span data-stu-id="7af64-236">Copy hello sloshingTank3D sample toohello shared folder and navigate tooit.</span></span>
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. <span data-ttu-id="7af64-237">Ez a minta alapértelmezett paraméterértékeivel hello használata esetén is igénybe vehet perc toorun, ahol, érdemes toomodify egyes paraméterek toomake gyorsabban futnak.</span><span class="sxs-lookup"><span data-stu-id="7af64-237">When you use hello default parameters of this sample, it can take tens of minutes toorun, so you might want toomodify some parameters toomake it run faster.</span></span> <span data-ttu-id="7af64-238">Egy egyszerű lehetőséget toomodify hello idő lépés változók deltaT és writeInterval hello rendszer/controlDict fájlban.</span><span class="sxs-lookup"><span data-stu-id="7af64-238">One simple choice is toomodify hello time step variables deltaT and writeInterval in hello system/controlDict file.</span></span> <span data-ttu-id="7af64-239">Ez a fájl tartalmazza az idő és adatok írásakor vagy olvasásakor megoldás toohello irányítását vonatkozó összes bemeneti adatot.</span><span class="sxs-lookup"><span data-stu-id="7af64-239">This file stores all input data relating toohello control of time and reading and writing solution data.</span></span> <span data-ttu-id="7af64-240">Például megváltoztathatja a 0,05 too0.5 deltaT hello értékének és a 0,05 too0.5 writeInterval hello értékét.</span><span class="sxs-lookup"><span data-stu-id="7af64-240">For example, you could change hello value of deltaT from 0.05 too0.5 and hello value of writeInterval from 0.05 too0.5.</span></span>
   
   ![Lépés változók módosítása][step_variables]
5. <span data-ttu-id="7af64-242">Adja meg a kívánt értékeket hello változók hello rendszer/decomposeParDict fájlban.</span><span class="sxs-lookup"><span data-stu-id="7af64-242">Specify desired values for hello variables in hello system/decomposeParDict file.</span></span> <span data-ttu-id="7af64-243">A példában két Linux-csomópont minden 8 magos, úgy állítsa be numberOfSubdomains too16 és n hierarchicalCoeffs too(1 1 16), ami azt jelenti, 16 folyamatokkal párhuzamosan futnak a OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="7af64-243">This example uses two Linux nodes each with 8 cores, so set numberOfSubdomains too16 and n of hierarchicalCoeffs too(1 1 16), which means run OpenFOAM in parallel with 16 processes.</span></span> <span data-ttu-id="7af64-244">További információkért lásd: [OpenFOAM felhasználói útmutatója: 3.4 futó alkalmazások párhuzamosan](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span><span class="sxs-lookup"><span data-stu-id="7af64-244">For more information, see [OpenFOAM User Guide: 3.4 Running applications in parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span></span>
   
   ![Folyamatok felbontani][decompose]
6. <span data-ttu-id="7af64-246">Futtassa a parancsokat követően – hello sloshingTank3D directory tooprepare hello mintaadatok hello.</span><span class="sxs-lookup"><span data-stu-id="7af64-246">Run hello following commands from hello sloshingTank3D directory tooprepare hello sample data.</span></span>
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. <span data-ttu-id="7af64-247">A hello átjárócsomópont meg kell jelennie hello mintaadatfájlok C:\OpenFoam\sloshingTank3D másol.</span><span class="sxs-lookup"><span data-stu-id="7af64-247">On hello head node, you should see hello sample data files are copied into C:\OpenFoam\sloshingTank3D.</span></span> <span data-ttu-id="7af64-248">(C:\OpenFoam hello hello átjárócsomópont lévő megosztott mappához.)</span><span class="sxs-lookup"><span data-stu-id="7af64-248">(C:\OpenFoam is hello shared folder on hello head node.)</span></span>
   
   ![Hello átjárócsomópont adatfájlokat][data_files]

### <a name="host-file-for-mpirun"></a><span data-ttu-id="7af64-250">Állomás fájlt mpirun</span><span class="sxs-lookup"><span data-stu-id="7af64-250">Host file for mpirun</span></span>
<span data-ttu-id="7af64-251">Ebben a lépésben létrehozott állomás-fájlokat (számítási csomópontok listáját) mely hello **mpirun** parancsot használja.</span><span class="sxs-lookup"><span data-stu-id="7af64-251">In this step, you create a host file (a list of compute nodes) which hello **mpirun** command uses.</span></span>

1. <span data-ttu-id="7af64-252">Hello Linux csomópontok egyik hozzon létre egy fájlt hostfile alatt /openfoam, ezért ezt a fájlt az összes Linux csomóponton /openfoam/hostfile címen érhető el.</span><span class="sxs-lookup"><span data-stu-id="7af64-252">On one of hello Linux nodes, create a file named hostfile under /openfoam, so this file can be reached at /openfoam/hostfile on all Linux nodes.</span></span>
2. <span data-ttu-id="7af64-253">A Linux csomópontnevek írni az ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="7af64-253">Write your Linux node names into this file.</span></span> <span data-ttu-id="7af64-254">Ebben a példában a hello fájl nevét a következő hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7af64-254">In this example, hello file contains hello following names:</span></span>
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > <span data-ttu-id="7af64-255">Ez a fájl C:\OpenFoam\hostfile: hello központi csomóponton is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7af64-255">You can also create this file at C:\OpenFoam\hostfile on hello head node.</span></span> <span data-ttu-id="7af64-256">Ha ezt a lehetőséget választja, a Linux és szövegfájlként menteni sorvégeket (csak LF, nem a CR LF).</span><span class="sxs-lookup"><span data-stu-id="7af64-256">If you choose this option, save it as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="7af64-257">Ez biztosítja, hogy fusson megfelelően hello Linux csomópontján.</span><span class="sxs-lookup"><span data-stu-id="7af64-257">This ensures that it runs properly on hello Linux nodes.</span></span>
   > 
   > 
   
   <span data-ttu-id="7af64-258">**Bash parancsfájlok burkoló**</span><span class="sxs-lookup"><span data-stu-id="7af64-258">**Bash script wrapper**</span></span>
   
   <span data-ttu-id="7af64-259">Ha sok Linux-csomópont van, és azt szeretné, hogy a feladat toorun csak az egyes, nincs egy jó ötlet toouse egy rögzített gazdagépet a fájlt, mert nem tudja, melyik csomópontok tooyour feladat fognak helyezkedni.</span><span class="sxs-lookup"><span data-stu-id="7af64-259">If you have many Linux nodes and you want your job toorun on only some of them, it’s not a good idea toouse a fixed host file, because you don’t know which nodes will be allocated tooyour job.</span></span> <span data-ttu-id="7af64-260">Ebben az esetben írni a bash parancsfájlok burkolót **mpirun** toocreate hello állomás fájl automatikusan.</span><span class="sxs-lookup"><span data-stu-id="7af64-260">In this case, write a bash script wrapper for **mpirun** toocreate hello host file automatically.</span></span> <span data-ttu-id="7af64-261">Egy példa bash parancsfájl burkoló nevű hpcimpirun.sh hello Ez a cikk végén található, és mentse /openfoam/hpcimpirun.sh. Ez a példa parancsfájl hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7af64-261">You can find an example bash script wrapper called hpcimpirun.sh at hello end of this article, and save it as /openfoam/hpcimpirun.sh. This example script does hello following:</span></span>
   
   1. <span data-ttu-id="7af64-262">Beállítja a hello környezeti változók **mpirun**, és néhány hozzáadása parancs paraméterei toorun hello MPI feladat hello RDMA a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="7af64-262">Sets up hello environment variables for **mpirun**, and some addition command parameters toorun hello MPI job through hello RDMA network.</span></span> <span data-ttu-id="7af64-263">Ebben az esetben azt állítja be a következő változók hello:</span><span class="sxs-lookup"><span data-stu-id="7af64-263">In this case, it sets hello following variables:</span></span>
      
      * <span data-ttu-id="7af64-264">I_MPI_FABRICS = shm:dapl</span><span class="sxs-lookup"><span data-stu-id="7af64-264">I_MPI_FABRICS=shm:dapl</span></span>
      * <span data-ttu-id="7af64-265">I_MPI_DAPL_PROVIDER bájtméretét-v2-ib0 =</span><span class="sxs-lookup"><span data-stu-id="7af64-265">I_MPI_DAPL_PROVIDER=ofa-v2-ib0</span></span>
      * <span data-ttu-id="7af64-266">I_MPI_DYNAMIC_CONNECTION = 0</span><span class="sxs-lookup"><span data-stu-id="7af64-266">I_MPI_DYNAMIC_CONNECTION=0</span></span>
   2. <span data-ttu-id="7af64-267">Létrehoz egy gazdagép szerint toohello környezeti változó $CCP_NODES_CORES hello HPC átjárócsomópont szerint beállított hello feladat aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="7af64-267">Creates a host file according toohello environment variable $CCP_NODES_CORES, which is set by hello HPC head node when hello job is activated.</span></span>
      
      <span data-ttu-id="7af64-268">$CCP_NODES_CORES hello formátuma ezt a mintát követi:</span><span class="sxs-lookup"><span data-stu-id="7af64-268">hello format of $CCP_NODES_CORES follows this pattern:</span></span>
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      <span data-ttu-id="7af64-269">Ha</span><span class="sxs-lookup"><span data-stu-id="7af64-269">where</span></span>
      
      * <span data-ttu-id="7af64-270">`<Number of nodes>`-csomópontok száma hello lefoglalt toothis feladat.</span><span class="sxs-lookup"><span data-stu-id="7af64-270">`<Number of nodes>` - hello number of nodes allocated toothis job.</span></span>  
      * <span data-ttu-id="7af64-271">`<Name of node_n_...>`-az egyes csomópontok hello neve lefoglalt toothis feladat.</span><span class="sxs-lookup"><span data-stu-id="7af64-271">`<Name of node_n_...>` - hello name of each node allocated toothis job.</span></span>
      * <span data-ttu-id="7af64-272">`<Cores of node_n_...>`-hello hello csomópont lefoglalt toothis feladaton magok száma.</span><span class="sxs-lookup"><span data-stu-id="7af64-272">`<Cores of node_n_...>` - hello number of cores on hello node allocated toothis job.</span></span>
      
      <span data-ttu-id="7af64-273">Például ha két csomópont toorun igényeinek hello feladat $CCP_NODES_CORES hasonlít</span><span class="sxs-lookup"><span data-stu-id="7af64-273">For example, if hello job needs two nodes toorun, $CCP_NODES_CORES is similar to</span></span>
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. <span data-ttu-id="7af64-274">Hívások hello **mpirun** parancsot, és hozzáfűzi a két paraméter toohello parancssor.</span><span class="sxs-lookup"><span data-stu-id="7af64-274">Calls hello **mpirun** command and appends two parameters toohello command line.</span></span>
      
      * <span data-ttu-id="7af64-275">`--hostfile <hostfilepath>: <hostfilepath>`-hello elérési útja hello állomás hello parancsfájlját hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7af64-275">`--hostfile <hostfilepath>: <hostfilepath>` - hello path of hello host file hello script creates</span></span>
      * <span data-ttu-id="7af64-276">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-olyan környezeti változó hello HPC Pack átjárócsomópont, amely tárolja a teljes magok száma hello által beállított lefoglalt toothis feladat.</span><span class="sxs-lookup"><span data-stu-id="7af64-276">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` - an environment variable set by hello HPC Pack head node, which stores hello number of total cores allocated toothis job.</span></span> <span data-ttu-id="7af64-277">Ebben az esetben adja meg a folyamatok száma hello **mpirun**.</span><span class="sxs-lookup"><span data-stu-id="7af64-277">In this case, it specifies hello number of processes for **mpirun**.</span></span>

## <a name="submit-an-openfoam-job"></a><span data-ttu-id="7af64-278">Egy OpenFOAM feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="7af64-278">Submit an OpenFOAM job</span></span>
<span data-ttu-id="7af64-279">Most elküldheti a HPC Cluster Manager feladat.</span><span class="sxs-lookup"><span data-stu-id="7af64-279">Now you can submit a job in HPC Cluster Manager.</span></span> <span data-ttu-id="7af64-280">Szükség van toopass hello parancsfájl hpcimpirun.sh a hello parancssorokat hello feladat feladatokat.</span><span class="sxs-lookup"><span data-stu-id="7af64-280">You need toopass hello script hpcimpirun.sh in hello command lines for some of hello job tasks.</span></span>

1. <span data-ttu-id="7af64-281">Csatlakozás tooyour fürt átjárócsomópontjával, és indítsa el a HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="7af64-281">Connect tooyour cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="7af64-282">**Az erőforrás-kezelés**, akkor győződjön meg arról, hogy hello Linux számítási csomópontok hello **Online** állapotát.</span><span class="sxs-lookup"><span data-stu-id="7af64-282">**In Resource Management**, ensure that hello Linux compute nodes are in hello **Online** state.</span></span> <span data-ttu-id="7af64-283">Ha nem, válassza ki azokat, és kattintson a **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="7af64-283">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="7af64-284">A **feladatkezelés**, kattintson a **új feladat**.</span><span class="sxs-lookup"><span data-stu-id="7af64-284">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="7af64-285">Írjon be egy feladat nevét, például *sloshingTank3D*.</span><span class="sxs-lookup"><span data-stu-id="7af64-285">Enter a name for job such as *sloshingTank3D*.</span></span>
   
   ![Feladat részletei][job_details]
5. <span data-ttu-id="7af64-287">A **feladat-erőforrások**, válassza ki az erőforrás "Csomópont" hello típusú, majd állítsa be a hello minimális too2.</span><span class="sxs-lookup"><span data-stu-id="7af64-287">In **Job resources**, choose hello type of resource as “Node” and set hello Minimum too2.</span></span> <span data-ttu-id="7af64-288">Ez a konfiguráció Linux két csomópont, amelyek mindegyikének nyolc processzormaggal ebben a példában hello feladatot futtat.</span><span class="sxs-lookup"><span data-stu-id="7af64-288">This configuration runs hello job on two Linux nodes, each of which has eight cores in this example.</span></span>
   
   ![Feladat erőforrások][job_resources]
6. <span data-ttu-id="7af64-290">Kattintson a **szerkesztése feladatok** a bal oldali navigációs hello, és kattintson a **Hozzáadás** tooadd egy feladat toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="7af64-290">Click **Edit Tasks** in hello left navigation, and then click **Add** tooadd a task toohello job.</span></span> <span data-ttu-id="7af64-291">Hello négy feladatok toohello feladat adja hozzá a következő parancssorok és a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7af64-291">Add four tasks toohello job with hello following command lines and settings.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7af64-292">Futó `source /openfoam/settings.sh` beállítja hello OpenFOAM és MPI futtatási környezet, így az egyes feladatok a következő hello meghívja az hello OpenFOAM parancs előtt.</span><span class="sxs-lookup"><span data-stu-id="7af64-292">Running `source /openfoam/settings.sh` sets up hello OpenFOAM and MPI runtime environments, so each of hello following tasks calls it before hello OpenFOAM command.</span></span>
   > 
   > 
   
   * <span data-ttu-id="7af64-293">**1. feladat**.</span><span class="sxs-lookup"><span data-stu-id="7af64-293">**Task 1**.</span></span> <span data-ttu-id="7af64-294">Futtatás **decomposePar** toogenerate adatfájlok futtatásához **interDyMFoam** párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="7af64-294">Run **decomposePar** toogenerate data files for running **interDyMFoam** in parallel.</span></span>
     
     * <span data-ttu-id="7af64-295">Rendelje hozzá egy csomópont toohello feladat</span><span class="sxs-lookup"><span data-stu-id="7af64-295">Assign one node toohello task</span></span>
     * <span data-ttu-id="7af64-296">**Parancssor** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="7af64-296">**Command line** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="7af64-297">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="7af64-297">**Working directory** - /openfoam/sloshingTank3D</span></span>
     
     <span data-ttu-id="7af64-298">Tekintse meg a következő ábra hello.</span><span class="sxs-lookup"><span data-stu-id="7af64-298">See hello following figure.</span></span> <span data-ttu-id="7af64-299">Hasonlóképpen hello hátralévő feladatok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7af64-299">You configure hello remaining tasks similarly.</span></span>
     
     ![1. feladat részletei][task_details1]
   * <span data-ttu-id="7af64-301">**2. feladat**.</span><span class="sxs-lookup"><span data-stu-id="7af64-301">**Task 2**.</span></span> <span data-ttu-id="7af64-302">Futtatás **interDyMFoam** párhuzamos toocompute hello mintában.</span><span class="sxs-lookup"><span data-stu-id="7af64-302">Run **interDyMFoam** in parallel toocompute hello sample.</span></span>
     
     * <span data-ttu-id="7af64-303">Két csomópont toohello feladatot</span><span class="sxs-lookup"><span data-stu-id="7af64-303">Assign two nodes toohello task</span></span>
     * <span data-ttu-id="7af64-304">**Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="7af64-304">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="7af64-305">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="7af64-305">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="7af64-306">**3. feladat**.</span><span class="sxs-lookup"><span data-stu-id="7af64-306">**Task 3**.</span></span> <span data-ttu-id="7af64-307">Futtatás **reconstructPar** toomerge hello beállítása idő könyvtárak minden processor_N_ könyvtárból egyetlen be.</span><span class="sxs-lookup"><span data-stu-id="7af64-307">Run **reconstructPar** toomerge hello sets of time directories from each processor_N_ directory into a single set.</span></span>
     
     * <span data-ttu-id="7af64-308">Rendelje hozzá egy csomópont toohello feladat</span><span class="sxs-lookup"><span data-stu-id="7af64-308">Assign one node toohello task</span></span>
     * <span data-ttu-id="7af64-309">**Parancssor** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="7af64-309">**Command line** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="7af64-310">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="7af64-310">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="7af64-311">**4. feladat**.</span><span class="sxs-lookup"><span data-stu-id="7af64-311">**Task 4**.</span></span> <span data-ttu-id="7af64-312">Futtatás **foamToEnsight** párhuzamos tooconvert hello OpenFOAM eredmény fájlok EnSight formázása és hello EnSight fájlokat helyezze Ensight nevű hello eset könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="7af64-312">Run **foamToEnsight** in parallel tooconvert hello OpenFOAM result files into EnSight format and place hello EnSight files in a directory named Ensight in hello case directory.</span></span>
     
     * <span data-ttu-id="7af64-313">Két csomópont toohello feladatot</span><span class="sxs-lookup"><span data-stu-id="7af64-313">Assign two nodes toohello task</span></span>
     * <span data-ttu-id="7af64-314">**Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="7af64-314">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="7af64-315">**Munkakönyvtár** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="7af64-315">**Working directory** - /openfoam/sloshingTank3D</span></span>
7. <span data-ttu-id="7af64-316">Adja hozzá a függőségek toothese feladatok növekvő sorrendben feladat.</span><span class="sxs-lookup"><span data-stu-id="7af64-316">Add dependencies toothese tasks in ascending task order.</span></span>
   
   ![Tevékenységfüggőségek][task_dependencies]
8. <span data-ttu-id="7af64-318">Kattintson a **Submit** toorun a feladatot.</span><span class="sxs-lookup"><span data-stu-id="7af64-318">Click **Submit** toorun this job.</span></span>
   
   <span data-ttu-id="7af64-319">Alapértelmezés szerint a HPC Pack hello feladat, a jelenlegi bejelentkezett felhasználói fiók küldi el.</span><span class="sxs-lookup"><span data-stu-id="7af64-319">By default, HPC Pack submits hello job as your current logged-on user account.</span></span> <span data-ttu-id="7af64-320">Miután rákattintott **Submit**, megjelenhet egy tooenter hello felhasználónevet és jelszót kérő párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7af64-320">After you click **Submit**, you might see a dialog box prompting you tooenter hello user name and password.</span></span>
   
   ![Feladat hitelesítő adatai][creds]
   
   <span data-ttu-id="7af64-322">Bizonyos körülmények között a HPC Pack emlékszik hello felhasználói adatok előtt adjon meg, és nem jeleníti meg ezen a párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="7af64-322">Under some conditions, HPC Pack remembers hello user information you input before and doesn’t show this dialog box.</span></span> <span data-ttu-id="7af64-323">HPC Pack toomake újbóli, hello a következő parancsot a parancssorba írja be, és majd elküldeni a hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="7af64-323">toomake HPC Pack show it again, enter hello following command at a Command prompt and then submit hello job.</span></span>
   
   ```
   hpccred delcreds
   ```
9. <span data-ttu-id="7af64-324">hello feladat telik el több tíz perc tooseveral órás toohello paraméterek hello minta beállítása alapján történik.</span><span class="sxs-lookup"><span data-stu-id="7af64-324">hello job takes from tens of minutes tooseveral hours according toohello parameters you have set for hello sample.</span></span> <span data-ttu-id="7af64-325">Hello hőtérkép hello Linux csomópontokon futó hello feladat látható.</span><span class="sxs-lookup"><span data-stu-id="7af64-325">In hello heat map, you see hello job running on hello Linux nodes.</span></span> 
   
   ![Hőtérkép][heat_map]
   
   <span data-ttu-id="7af64-327">Minden egyes csomóponton nyolc folyamatok indulnak el.</span><span class="sxs-lookup"><span data-stu-id="7af64-327">On each node, eight processes are started.</span></span>
   
   ![Linux-folyamat][linux_processes]
10. <span data-ttu-id="7af64-329">Hello feladat befejezése után található hello feladat eredményeinek C:\OpenFoam\sloshingTank3D és hello naplófájlokat a következő C:\OpenFoam alapján mappákban.</span><span class="sxs-lookup"><span data-stu-id="7af64-329">When hello job finishes, find hello job results in folders under C:\OpenFoam\sloshingTank3D, and hello log files at C:\OpenFoam.</span></span>

## <a name="view-results-in-ensight"></a><span data-ttu-id="7af64-330">EnSight az eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="7af64-330">View results in EnSight</span></span>
<span data-ttu-id="7af64-331">Ezenkívül szükség [EnSight](https://www.ceisoftware.com/) toovisualize és elemezze hello hello OpenFOAM feladat eredményét.</span><span class="sxs-lookup"><span data-stu-id="7af64-331">Optionally use [EnSight](https://www.ceisoftware.com/) toovisualize and analyze hello results of hello OpenFOAM job.</span></span> <span data-ttu-id="7af64-332">További információt a képi megjelenítés és EnSight animációt megjelenik ez [videó útmutató](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span><span class="sxs-lookup"><span data-stu-id="7af64-332">For more about visualization and animation in EnSight, see this [video guide](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span></span>

1. <span data-ttu-id="7af64-333">Miután EnSight hello átjárócsomópont telepítette, indítsa el.</span><span class="sxs-lookup"><span data-stu-id="7af64-333">After you install EnSight on hello head node, start it.</span></span>
2. <span data-ttu-id="7af64-334">Nyissa meg a C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span><span class="sxs-lookup"><span data-stu-id="7af64-334">Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span></span>
   
   <span data-ttu-id="7af64-335">Megjelenik egy tartály hello megjelenítőben.</span><span class="sxs-lookup"><span data-stu-id="7af64-335">You see a tank in hello viewer.</span></span>
   
   ![A EnSight tartály][tank]
3. <span data-ttu-id="7af64-337">Hozzon létre egy **Isosurface** a **internalMesh**, majd válassza a hello változó **alpha_water**.</span><span class="sxs-lookup"><span data-stu-id="7af64-337">Create an **Isosurface** from **internalMesh**, and then choose hello variable **alpha_water**.</span></span>
   
   ![Hozzon létre egy isosurface][isosurface]
4. <span data-ttu-id="7af64-339">Állítsa be a hello színét **Isosurface_part** hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="7af64-339">Set hello color for **Isosurface_part** created in hello previous step.</span></span> <span data-ttu-id="7af64-340">Például állítsa be toowater kék.</span><span class="sxs-lookup"><span data-stu-id="7af64-340">For example, set it toowater blue.</span></span>
   
   ![Isosurface szín módosítása][isosurface_color]
5. <span data-ttu-id="7af64-342">Hozzon létre egy **Iso-kötet** a **falvastagságát** kiválasztásával **falvastagságát** a hello **részek** panelen, majd kattintson a hello **Isosurfaces**  hello eszköztár gombjára.</span><span class="sxs-lookup"><span data-stu-id="7af64-342">Create an **Iso-volume** from **walls** by selecting **walls** in hello **Parts** panel and click hello **Isosurfaces** button in hello toolbar.</span></span>
6. <span data-ttu-id="7af64-343">Hello párbeszédpanelen válassza ki a **típus** , **Isovolume** és állítsa be a Min hello **Isovolume tartomány** too0.5.</span><span class="sxs-lookup"><span data-stu-id="7af64-343">In hello dialog box, select **Type** as **Isovolume** and set hello Min of **Isovolume range** too0.5.</span></span> <span data-ttu-id="7af64-344">toocreate hello isovolume, kattintson a **létrehozása a kijelölt alkotórészek**.</span><span class="sxs-lookup"><span data-stu-id="7af64-344">toocreate hello isovolume, click **Create with selected parts**.</span></span>
7. <span data-ttu-id="7af64-345">Állítsa be a hello színét **Iso_volume_part** hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="7af64-345">Set hello color for **Iso_volume_part** created in hello previous step.</span></span> <span data-ttu-id="7af64-346">Például állítsa be toodeep vízjel kék.</span><span class="sxs-lookup"><span data-stu-id="7af64-346">For example, set it toodeep water blue.</span></span>
8. <span data-ttu-id="7af64-347">Állítsa be a hello színét **falvastagságát**.</span><span class="sxs-lookup"><span data-stu-id="7af64-347">Set hello color for **walls**.</span></span> <span data-ttu-id="7af64-348">Például állítsa be azt tootransparent fehér.</span><span class="sxs-lookup"><span data-stu-id="7af64-348">For example, set it tootransparent white.</span></span>
9. <span data-ttu-id="7af64-349">Most kattintson **lejátszása** hello szimuláció toosee hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="7af64-349">Now click **Play** toosee hello results of hello simulation.</span></span>
   
    ![Tartály eredménye][tank_result]

## <a name="sample-files"></a><span data-ttu-id="7af64-351">Mintafájlok</span><span class="sxs-lookup"><span data-stu-id="7af64-351">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="7af64-352">A minta XML konfigurációs fájl fürttelepítés PowerShell-parancsprogram</span><span class="sxs-lookup"><span data-stu-id="7af64-352">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
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

### <a name="sample-credxml-file"></a><span data-ttu-id="7af64-353">Mintafájl cred.xml</span><span class="sxs-lookup"><span data-stu-id="7af64-353">Sample cred.xml file</span></span>
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
### <a name="sample-silentcfg-file-tooinstall-mpi"></a><span data-ttu-id="7af64-354">A minta silent.cfg fájl tooinstall MPI</span><span class="sxs-lookup"><span data-stu-id="7af64-354">Sample silent.cfg file tooinstall MPI</span></span>
```
# Patterns used toocheck silent configuration file
#
# anythingpat - any string
# filepat     - hello file location pattern (/file/location/to/license.lic)
# lspat       - hello license server address pattern (0123@hostname)
# snpat       - hello serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components tooinstall, valid values are: {ALL, DEFAULTS, anythingpat}
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

# Path toohello cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes tooenable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes tooupdate ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a><span data-ttu-id="7af64-355">Mintaparancsfájl settings.sh</span><span class="sxs-lookup"><span data-stu-id="7af64-355">Sample settings.sh script</span></span>
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


### <a name="sample-hpcimpirunsh-script"></a><span data-ttu-id="7af64-356">Mintaparancsfájl hpcimpirun.sh</span><span class="sxs-lookup"><span data-stu-id="7af64-356">Sample hpcimpirun.sh script</span></span>
```
#!/bin/bash

# hello path of this script
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
    # CCP_NODES_CORES is not found or is empty, just run hello mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into hello hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello mpirun with hostfile arg
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
