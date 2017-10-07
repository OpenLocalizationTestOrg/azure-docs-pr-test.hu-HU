---
title: "a Microsoft HPC Pack Linux virtuális gépeken aaaNAMD |} Microsoft Docs"
description: "Azure a Microsoft HPC Pack fürt központi telepítése, és futtassa a NAMD szimuláció charmrun több Linux számítási csomóponton"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: 90c722f66b494335a62c0613079366ae99002076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a><span data-ttu-id="f5593-103">Az NAMD futtatása a Microsoft HPC Packkal Azure-beli linuxos számítási csomópontokon</span><span class="sxs-lookup"><span data-stu-id="f5593-103">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>
<span data-ttu-id="f5593-104">A cikkből megtudhatja, egyirányú toorun egy Linux nagy teljesítményű számítástechnikai rendszerek (HPC) munkaterhelés Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f5593-104">This article shows you one way toorun a Linux high-performance computing (HPC) workload on Azure virtual machines.</span></span> <span data-ttu-id="f5593-105">Itt, állítsa be a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) -fürtöt az Azure-on Linux számítási csomópontokat, és futtassa a [NAMD](http://www.ks.uiuc.edu/Research/namd/) szimuláció toocalculate egy nagy biomolekuláris rendszer hello szerkezetét, és.</span><span class="sxs-lookup"><span data-stu-id="f5593-105">Here, you set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster on Azure with Linux compute nodes and run a [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulation toocalculate and visualize hello structure of a large biomolecular system.</span></span>  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* <span data-ttu-id="f5593-106">**NAMD** (Nanoscale molekuláris Dynamics program) van egy nagy teljesítményű szimulálása a nagy biomolekuláris rendszerekhez készült párhuzamos molekuláris dynamics csomagot tartalmazó az Atom toomillions fel.</span><span class="sxs-lookup"><span data-stu-id="f5593-106">**NAMD** (for Nanoscale Molecular Dynamics program) is a parallel molecular dynamics package designed for high-performance simulation of large biomolecular systems containing up toomillions of atoms.</span></span> <span data-ttu-id="f5593-107">Ezek a rendszerek közé vírusok, cella és a nagy fehérjék.</span><span class="sxs-lookup"><span data-stu-id="f5593-107">Examples of these systems include viruses, cell structures, and large proteins.</span></span> <span data-ttu-id="f5593-108">NAMD arányosan toohundreds tipikus szimulációja és toomore mint 500 000 magos hello legnagyobb szimulációja magszámra vonatkozó követelménynek.</span><span class="sxs-lookup"><span data-stu-id="f5593-108">NAMD scales toohundreds of cores for typical simulations and toomore than 500,000 cores for hello largest simulations.</span></span>
* <span data-ttu-id="f5593-109">**A Microsoft HPC Pack** toorun funkciókat biztosít a nagyméretű HPC és a helyszíni számítógépek vagy az Azure virtuális gépek fürtök párhuzamos alkalmazásait.</span><span class="sxs-lookup"><span data-stu-id="f5593-109">**Microsoft HPC Pack** provides features toorun large-scale HPC and parallel applications in clusters of on-premises computers or Azure virtual machines.</span></span> <span data-ttu-id="f5593-110">Eredetileg fejlesztett munkaterheléseknél Windows HPC HPC Pack megoldásként mostantól támogatja a futó Linux HPC alkalmazások Linux rendszeren telepített HPC Pack-fürtben lévő csomópont virtuális gépek számítási.</span><span class="sxs-lookup"><span data-stu-id="f5593-110">Originally developed as a solution for Windows HPC workloads, HPC Pack now supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="f5593-111">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) bevezető.</span><span class="sxs-lookup"><span data-stu-id="f5593-111">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction.</span></span>

<span data-ttu-id="f5593-112">Az egyéb beállítások toorun Linux HPC munkaterhelések az Azure, lásd: [kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások](../../../batch/batch-hpc-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="f5593-112">For other options toorun Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/batch-hpc-solutions.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5593-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f5593-113">Prerequisites</span></span>
* <span data-ttu-id="f5593-114">**A számítási csomópontok HPC Pack fürt Linux** -fürt üzembe helyezése HPC Pack Linux számítási csomópontok az Azure-ban vagy egy [Azure Resource Manager sablon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) vagy egy [Azure PowerShell-parancsfájl](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="f5593-114">**HPC Pack cluster with Linux compute nodes** - Deploy an HPC Pack cluster with Linux compute nodes on Azure using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="f5593-115">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) hello Előfeltételek és az egyik beállítási mód lépéseit.</span><span class="sxs-lookup"><span data-stu-id="f5593-115">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for hello prerequisites and steps for either option.</span></span> <span data-ttu-id="f5593-116">Ha hello PowerShell parancsfájl központi telepítés lehetőséget választja, tekintse meg a hello minta konfigurációs fájlt a mintafájlok hello hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="f5593-116">If you choose hello PowerShell script deployment option, see hello sample configuration file in hello sample files at hello end of this article.</span></span> <span data-ttu-id="f5593-117">Ez a fájl az Azure-alapú HPC Pack fürtöt egy Windows Server 2012 R2 átjárócsomópont és a számítási csomópontok négy mérete nagy CentOS 6.6 konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="f5593-117">This file configures an Azure-based HPC Pack cluster consisting of a Windows Server 2012 R2 head node and four size Large CentOS 6.6 compute nodes.</span></span> <span data-ttu-id="f5593-118">Testre szabhatja ezt a fájlt, a saját környezetéhez szükséges módon.</span><span class="sxs-lookup"><span data-stu-id="f5593-118">Customize this file as needed for your environment.</span></span>
* <span data-ttu-id="f5593-119">**Szoftver- és oktatóanyag fájlok NAMD** -letöltése NAMD szoftverek Linux hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) hely (regisztráció szükséges).</span><span class="sxs-lookup"><span data-stu-id="f5593-119">**NAMD software and tutorial files** - Download NAMD software for Linux from hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) site (registration required).</span></span> <span data-ttu-id="f5593-120">Ez a cikk NAMD verzió 2.10 alapul, és használja a hello [Linux-x86_64 (64 bites Intel vagy AMD Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archív.</span><span class="sxs-lookup"><span data-stu-id="f5593-120">This article is based on NAMD version 2.10, and uses hello [Linux-x86_64 (64-bit Intel/AMD with Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archive.</span></span> <span data-ttu-id="f5593-121">Töltse le a is hello [NAMD oktatóanyag fájlok](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span><span class="sxs-lookup"><span data-stu-id="f5593-121">Also download hello [NAMD tutorial files](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span></span> <span data-ttu-id="f5593-122">hello letöltések .tar fájlokat, és egy Windows eszköz tooextract hello fájlok hello átjárócsomóponthoz van szüksége.</span><span class="sxs-lookup"><span data-stu-id="f5593-122">hello downloads are .tar files, and you need a Windows tool tooextract hello files on hello cluster head node.</span></span> <span data-ttu-id="f5593-123">tooextract hello fájlok, kövesse a cikkben később hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="f5593-123">tooextract hello files, follow hello instructions later in this article.</span></span> 
* <span data-ttu-id="f5593-124">**VMD** (nem kötelező) – a NAMD feladat eredményeinek toosee hello töltse le és telepítse a hello molekuláris képi megjelenítés program [VMD](http://www.ks.uiuc.edu/Research/vmd/) a kiválasztott számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f5593-124">**VMD** (optional) - toosee hello results of your NAMD job, download and install hello molecular visualization program [VMD](http://www.ks.uiuc.edu/Research/vmd/) on a computer of your choice.</span></span> <span data-ttu-id="f5593-125">hello nem 1.9.2.</span><span class="sxs-lookup"><span data-stu-id="f5593-125">hello current version is 1.9.2.</span></span> <span data-ttu-id="f5593-126">Lásd: hello VMD letöltési hely tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="f5593-126">See hello VMD download site tooget started.</span></span>  

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="f5593-127">Kölcsönös, a számítási csomópontok közötti megbízhatósági kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="f5593-127">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="f5593-128">A kereszt-csomópont feladat több Linux csomópontokon futó szükséges hello csomópontok tootrust egymáshoz (által **rsh** vagy **ssh**).</span><span class="sxs-lookup"><span data-stu-id="f5593-128">Running a cross-node job on multiple Linux nodes requires hello nodes tootrust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="f5593-129">Hello HPC Pack fürt létrehozásakor a Microsoft HPC Pack IaaS telepítési parancsfájl hello hello parancsfájl automatikusan beállítja állandó kölcsönös megbízhatósági hello rendszergazdai fiókhoz megadott.</span><span class="sxs-lookup"><span data-stu-id="f5593-129">When you create hello HPC Pack cluster with hello Microsoft HPC Pack IaaS deployment script, hello script automatically sets up permanent mutual trust for hello administrator account you specify.</span></span> <span data-ttu-id="f5593-130">A nem rendszergazda felhasználók hello fürt a tartományban létrehozta meg kell tooset mentése hello csomópontok közötti ideiglenes kölcsönös megbízhatósági Ha egy feladat le van foglalva toothem.</span><span class="sxs-lookup"><span data-stu-id="f5593-130">For non-administrator users you create in hello cluster's domain, you have tooset up temporary mutual trust among hello nodes when a job is allocated toothem.</span></span> <span data-ttu-id="f5593-131">Majd semmisítse meg hello kapcsolat hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f5593-131">Then, destroy hello relationship after hello job is complete.</span></span> <span data-ttu-id="f5593-132">toodo minden felhasználónak, így egy RSA kulcspár toohello fürt melyik HPC Pack tooestablish hello megbízhatósági kapcsolatot használ.</span><span class="sxs-lookup"><span data-stu-id="f5593-132">toodo this for each user, provide an RSA key pair toohello cluster which HPC Pack uses tooestablish hello trust relationship.</span></span> <span data-ttu-id="f5593-133">Kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="f5593-133">Instructions follow.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="f5593-134">Egy RSA kulcspár létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5593-134">Generate an RSA key pair</span></span>
<span data-ttu-id="f5593-135">Könnyen toogenerate egy RSA kulcsból álló kulcspárt, amely tartalmazza a nyilvános kulcsot és titkos kulccsal, futtatásával hello Linux **ssh-keygen** parancsot.</span><span class="sxs-lookup"><span data-stu-id="f5593-135">It's easy toogenerate an RSA key pair, which contains a public key and a private key, by running hello Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="f5593-136">Jelentkezzen be tooa Linux-számítógép.</span><span class="sxs-lookup"><span data-stu-id="f5593-136">Log on tooa Linux computer.</span></span>
2. <span data-ttu-id="f5593-137">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f5593-137">Run hello following command:</span></span>
   
   ```bash
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="f5593-138">Nyomja le az **Enter** toouse hello alapértelmezett beállítások hello parancs befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="f5593-138">Press **Enter** toouse hello default settings until hello command is completed.</span></span> <span data-ttu-id="f5593-139">Ne adjon meg egy jelszót. Amikor a rendszer kéri a jelszót, csak Entert **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f5593-139">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Egy RSA kulcspár létrehozása][keygen]
3. <span data-ttu-id="f5593-141">Módosítsa a könyvtárat toohello ~/.ssh könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="f5593-141">Change directory toohello ~/.ssh directory.</span></span> <span data-ttu-id="f5593-142">hello titkos kulcs id_rsa.pub a id_rsa és hello nyilvános kulcs tárolja.</span><span class="sxs-lookup"><span data-stu-id="f5593-142">hello private key is stored in id_rsa and hello public key in id_rsa.pub.</span></span>
   
   ![Nyilvános és titkos kulcsai][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a><span data-ttu-id="f5593-144">Hello kulcspár toohello HPC Pack fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f5593-144">Add hello key pair toohello HPC Pack cluster</span></span>
1. <span data-ttu-id="f5593-145">[Csatlakozás távoli asztal](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello átjárócsomópont VM használatával hello (például hpc\clusteradmin) hello fürt telepítésekor megadott tartományi hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="f5593-145">[Connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello head node VM using hello domain credentials you provided when you deployed hello cluster (for example, hpc\clusteradmin).</span></span> <span data-ttu-id="f5593-146">Hello átjárócsomópont származó hello fürt kezelése.</span><span class="sxs-lookup"><span data-stu-id="f5593-146">You manage hello cluster from hello head node.</span></span>
2. <span data-ttu-id="f5593-147">Szabványos Windows Server eljárások toocreate tartományi felhasználói fiókot használja a hello fürt Active Directory-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="f5593-147">Use standard Windows Server procedures toocreate a domain user account in hello cluster's Active Directory domain.</span></span> <span data-ttu-id="f5593-148">Hello Active Directory – felhasználók és számítógépek eszközzel például hello átjárócsomópont használja.</span><span class="sxs-lookup"><span data-stu-id="f5593-148">For example, use hello Active Directory User and Computers tool on hello head node.</span></span> <span data-ttu-id="f5593-149">hello a cikkben szereplő példák azt feltételezik, hpcuser hello hpclab tartományban (hpclab\hpcuser) nevű tartományi felhasználót kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f5593-149">hello examples in this article assume you create a domain user named hpcuser in hello hpclab domain (hpclab\hpcuser).</span></span>
3. <span data-ttu-id="f5593-150">Adja hozzá a fürt felhasználói hello tartományi felhasználói toohello HPC Pack fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f5593-150">Add hello domain user toohello HPC Pack cluster as a cluster user.</span></span> <span data-ttu-id="f5593-151">Útmutatásért lásd: [hozzáadását vagy eltávolítását fürt felhasználók](https://technet.microsoft.com/library/ff919330.aspx).</span><span class="sxs-lookup"><span data-stu-id="f5593-151">For instructions, see [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).</span></span>
4. <span data-ttu-id="f5593-152">Hozzon létre egy fájlt C:\cred.xml, és másolja hello RSA kulcsadatokat bele.</span><span class="sxs-lookup"><span data-stu-id="f5593-152">Create a file named C:\cred.xml and copy hello RSA key data into it.</span></span> <span data-ttu-id="f5593-153">Található példa hello mintafájlok hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="f5593-153">You can find an example in hello sample files at hello end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. <span data-ttu-id="f5593-154">Nyisson meg egy parancssort, és írja be a következő parancs tooset hello adatok hello hpclab\hpcuser fiók hitelesítő adatai hello.</span><span class="sxs-lookup"><span data-stu-id="f5593-154">Open a Command Prompt and enter hello following command tooset hello credentials data for hello hpclab\hpcuser account.</span></span> <span data-ttu-id="f5593-155">Hello használata **extendeddata** paraméter toopass hello hello C:\cred.xml fájljának létrehozott hello kulcsadatokat nevét.</span><span class="sxs-lookup"><span data-stu-id="f5593-155">You use hello **extendeddata** parameter toopass hello name of hello C:\cred.xml file you created for hello key data.</span></span>
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="f5593-156">Ez a parancs sikeresen kimeneti nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f5593-156">This command completes successfully without output.</span></span> <span data-ttu-id="f5593-157">Miután beállította a hello toorun feladatok kell hello felhasználói fiókok hitelesítő adatait, hello cred.xml fájlt tárolja biztonságos helyen, vagy törölje azt.</span><span class="sxs-lookup"><span data-stu-id="f5593-157">After setting hello credentials for hello user accounts you need toorun jobs, store hello cred.xml file in a secure location, or delete it.</span></span>
6. <span data-ttu-id="f5593-158">Ha RSA kulcspár hello a Linux-csomópontok egyikén jön létre, azok használatának befejezése után ne feledje toodelete hello kulcsok.</span><span class="sxs-lookup"><span data-stu-id="f5593-158">If you generated hello RSA key pair on one of your Linux nodes, remember toodelete hello keys after you finish using them.</span></span> <span data-ttu-id="f5593-159">Ha úgy találja, hogy egy meglévő id_rsa vagy id_rsa.pub fájl nincs beállítva HPC Pack kölcsönös megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="f5593-159">HPC Pack does not set up mutual trust if it finds an existing id_rsa file or id_rsa.pub file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f5593-160">Nem ajánlott a Linux feladat futtatásával a fürt rendszergazdai megosztott fürtön, mivel hello Linux csomópontján hello legfelső szintű fiók alatt fut egy feladat, a rendszergazda által küldött.</span><span class="sxs-lookup"><span data-stu-id="f5593-160">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under hello root account on hello Linux nodes.</span></span> <span data-ttu-id="f5593-161">Egy feladat, a nem rendszergazda felhasználók által benyújtott Linux helyi felhasználói fiók alatt fut hello azonos nevet hello feladat felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="f5593-161">A job submitted by a non-administrator user runs under a local Linux user account with hello same name as hello job user.</span></span> <span data-ttu-id="f5593-162">Ebben az esetben HPC Pack állít be a Linux-felhasználó kölcsönös megbízhatósági toohello feladathoz kiosztott összes hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="f5593-162">In this case, HPC Pack sets up mutual trust for this Linux user across all hello nodes allocated toohello job.</span></span> <span data-ttu-id="f5593-163">Hello feladat futtatása előtt manuálisan csomópontján hello Linux hello Linux felhasználói állíthat be, vagy a HPC Pack hello felhasználó létrehozása automatikusan hello feladat elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="f5593-163">You can set up hello Linux user manually on hello Linux nodes before running hello job, or HPC Pack creates hello user automatically when hello job is submitted.</span></span> <span data-ttu-id="f5593-164">HPC Pack hello felhasználó hoz létre, ha HPC Pack törli azt hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f5593-164">If HPC Pack creates hello user, HPC Pack deletes it after hello job completes.</span></span> <span data-ttu-id="f5593-165">tooreduce biztonsági kockázatot jelentenek, hello kulcsok el lesznek távolítva, befejeződött hello feladatok hello csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f5593-165">tooreduce security threat, hello keys are removed after hello job completes on hello nodes.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="f5593-166">Linux-csomópontok fájlmegosztás beállítása</span><span class="sxs-lookup"><span data-stu-id="f5593-166">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="f5593-167">Most beállítása az SMB-fájlmegosztásra, és a csatlakoztatási hello megosztott mappában található összes Linux csomópontok tooallow hello Linux csomópontok tooaccess NAMD fájlt egy közös elérési utat.</span><span class="sxs-lookup"><span data-stu-id="f5593-167">Now set up an SMB file share, and mount hello shared folder on all Linux nodes tooallow hello Linux nodes tooaccess NAMD files with a common path.</span></span> <span data-ttu-id="f5593-168">Az alábbiakban lépéseket toomount hello átjárócsomópont lévő megosztott mappához.</span><span class="sxs-lookup"><span data-stu-id="f5593-168">Following are steps toomount a shared folder on hello head node.</span></span> <span data-ttu-id="f5593-169">A megosztás például a CentOS 6.6 terjesztéseket, amelyek jelenleg nem támogatja a hello Azure File service ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f5593-169">A share is recommended for distributions such as CentOS 6.6 that currently don’t support hello Azure File service.</span></span> <span data-ttu-id="f5593-170">Ha a Linux-csomópontok támogatja egy Azure-fájlmegosztás, látható [hogyan toouse Linux Azure File storage](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="f5593-170">If your Linux nodes support an Azure File share, see [How toouse Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="f5593-171">További fájl megosztási beállítások HPC Pack, lásd: [Ismerkedés az Azure-ban HPC Pack fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f5593-171">For additional file sharing options with HPC Pack, see [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="f5593-172">Hozzon létre egy mappát a hello átjárócsomópont, és ossza meg úgy, hogy olvasási/írási jogosultsággal tooEveryone.</span><span class="sxs-lookup"><span data-stu-id="f5593-172">Create a folder on hello head node, and share it tooEveryone by setting Read/Write privileges.</span></span> <span data-ttu-id="f5593-173">Ebben a példában \\ \\CentOS66HN\Namd hello mappát, ahol a CentOS66HN hello átjárócsomópont hello állomásnevét hello neve.</span><span class="sxs-lookup"><span data-stu-id="f5593-173">In this example, \\\\CentOS66HN\Namd is hello name of hello folder, where CentOS66HN is hello host name of hello head node.</span></span>
2. <span data-ttu-id="f5593-174">Hozzon létre egy almappát namd2 hello megosztott mappában.</span><span class="sxs-lookup"><span data-stu-id="f5593-174">Create a subfolder named namd2 in hello shared folder.</span></span> <span data-ttu-id="f5593-175">Namd2 hozzon létre egy másik almappát namdsample.</span><span class="sxs-lookup"><span data-stu-id="f5593-175">In namd2, create another subfolder named namdsample.</span></span>
3. <span data-ttu-id="f5593-176">Bontsa ki a hello NAMD fájlt hello mappában által a Windows verziójával **bont** vagy egy másik Windows-segédprogram, amely .tar kiterjesztett archívumokat.</span><span class="sxs-lookup"><span data-stu-id="f5593-176">Extract hello NAMD files in hello folder by using a Windows version of **tar** or another Windows utility that operates on .tar archives.</span></span> 
   
   * <span data-ttu-id="f5593-177">Hello NAMD bont archívum kibontása túl\\\\CentOS66HN\Namd\namd2.</span><span class="sxs-lookup"><span data-stu-id="f5593-177">Extract hello NAMD tar archive too\\\\CentOS66HN\Namd\namd2.</span></span>
   * <span data-ttu-id="f5593-178">Bontsa ki a hello oktatóanyag fájlt a \\ \\CentOS66HN\Namd\namd2\namdsample.</span><span class="sxs-lookup"><span data-stu-id="f5593-178">Extract hello tutorial files under \\\\CentOS66HN\Namd\namd2\namdsample.</span></span>
4. <span data-ttu-id="f5593-179">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok toomount hello hello Linux csomópontok megosztott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="f5593-179">Open a Windows PowerShell window and run hello following commands toomount hello shared folder on hello Linux nodes.</span></span>
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="f5593-180">hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /namd2 nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="f5593-180">hello first command creates a folder named /namd2 on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="f5593-181">hello második parancs csatlakoztatja hello megosztott mappa //CentOS66HN/Namd/namd2 alakzatot dir_mode és file_mode bit beállítása too777 hello mappában.</span><span class="sxs-lookup"><span data-stu-id="f5593-181">hello second command mounts hello shared folder //CentOS66HN/Namd/namd2 onto hello folder with dir_mode and file_mode bits set too777.</span></span> <span data-ttu-id="f5593-182">Hello *felhasználónév* és *jelszó* hello parancs hello hello átjárócsomópont a felhasználó hitelesítő adatait kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f5593-182">hello *username* and *password* in hello command should be hello credentials of a user on hello head node.</span></span>

> [!NOTE]
> <span data-ttu-id="f5593-183">hello "\\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f5593-183">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="f5593-184">"\\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.</span><span class="sxs-lookup"><span data-stu-id="f5593-184">“\\`,” means hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

## <a name="create-a-bash-script-toorun-a-namd-job"></a><span data-ttu-id="f5593-185">A Bash parancsfájlok toorun NAMD feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5593-185">Create a Bash script toorun a NAMD job</span></span>
<span data-ttu-id="f5593-186">A NAMD feladatot kell egy *csomópontlista* fájlt **charmrun** toodetermine hello száma csomópontok toouse NAMD folyamatok indításakor.</span><span class="sxs-lookup"><span data-stu-id="f5593-186">Your NAMD job needs a *nodelist* file for **charmrun** toodetermine hello number of nodes toouse when starting NAMD processes.</span></span> <span data-ttu-id="f5593-187">A Bash parancsfájlt, amely hello csomópontlista fájlt hoz létre, és futtatja **charmrun** a csomópontlista fájllal.</span><span class="sxs-lookup"><span data-stu-id="f5593-187">You use a Bash script that generates hello nodelist file and runs **charmrun** with this nodelist file.</span></span> <span data-ttu-id="f5593-188">Majd elküldheti a HPC-Fürtkezelőben ezt a parancsfájlt meghívó NAMD feladat.</span><span class="sxs-lookup"><span data-stu-id="f5593-188">You can then submit a NAMD job in HPC Cluster Manager that calls this script.</span></span>

<span data-ttu-id="f5593-189">Az Ön által választott szövegszerkesztő használatával hozzon létre egy Bash parancsfájlok hello NAMD programfájljait tartalmazó hello /namd2 mappában, és nevezze el hpccharmrun.sh. A koncepció igazolása kész, másolja át a cikk végén hello megadott hello példa hpccharmrun.sh parancsfájl, és nyissa meg túl[NAMD feladat elküldése](#submit-a-namd-job).</span><span class="sxs-lookup"><span data-stu-id="f5593-189">Using a text editor of your choice, create a Bash script in hello /namd2 folder containing hello NAMD program files and name it hpccharmrun.sh. For a quick proof of concept, copy hello example hpccharmrun.sh script provided at hello end of this article and go too[Submit a NAMD job](#submit-a-namd-job).</span></span>

> [!TIP]
> <span data-ttu-id="f5593-190">Mentse a parancsfájlt a Linux és szövegfájlként sorvégeket a (csak LF, nem a CR LF).</span><span class="sxs-lookup"><span data-stu-id="f5593-190">Save your script as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="f5593-191">Ez biztosítja, hogy fusson megfelelően hello Linux csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f5593-191">This ensures that it runs properly on hello Linux nodes.</span></span>
> 
> 

<span data-ttu-id="f5593-192">Az alábbiakban a bash parancsfájlok funkciója részleteit.</span><span class="sxs-lookup"><span data-stu-id="f5593-192">Following are details about what this bash script does.</span></span> 

1. <span data-ttu-id="f5593-193">Adja meg a bizonyos változókat.</span><span class="sxs-lookup"><span data-stu-id="f5593-193">Define some variables.</span></span>
   
   ```bash
   #!/bin/bash
   
   # hello path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. <span data-ttu-id="f5593-194">Csomópont-információk lekérése hello környezeti változók.</span><span class="sxs-lookup"><span data-stu-id="f5593-194">Get node information from hello environment variables.</span></span> <span data-ttu-id="f5593-195">$NODESCORES $CCP_NODES_CORES vegyes szavak listáját tárolja.</span><span class="sxs-lookup"><span data-stu-id="f5593-195">$NODESCORES stores a list of split words from $CCP_NODES_CORES.</span></span> <span data-ttu-id="f5593-196">$COUNT $NODESCORES hello méretét.</span><span class="sxs-lookup"><span data-stu-id="f5593-196">$COUNT is hello size of $NODESCORES.</span></span>
   
   ```
   # Get node information from hello environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   <span data-ttu-id="f5593-197">hello hello $CCP_NODES_CORES változó formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="f5593-197">hello format for hello $CCP_NODES_CORES variable is as follows:</span></span>
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   <span data-ttu-id="f5593-198">Ez a változó hello száma csomópontok csomópontnevek és minden egyes csomóponton a toohello feladat lefoglalt magok száma sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="f5593-198">This variable lists hello total number of nodes, node names, and number of cores on each node that are allocated toohello job.</span></span> <span data-ttu-id="f5593-199">Például ha a hello feladat 10 magok toorun, hello $CCP_NODES_CORES értéke hasonló:</span><span class="sxs-lookup"><span data-stu-id="f5593-199">For example, if hello job needs 10 cores toorun, hello value of $CCP_NODES_CORES is similar to:</span></span>
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. <span data-ttu-id="f5593-200">Ha hello $CCP_NODES_CORES változó nincs beállítva, indítsa el **charmrun** közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="f5593-200">If hello $CCP_NODES_CORES variable is not set, start **charmrun** directly.</span></span> <span data-ttu-id="f5593-201">(Ez csak történjen, ha a parancsfájl futtatását közvetlenül a Linux-csomóponton.)</span><span class="sxs-lookup"><span data-stu-id="f5593-201">(This should only occur when you run this script directly on your Linux nodes.)</span></span>
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. <span data-ttu-id="f5593-202">Vagy hozzon létre egy csomópontlista fájlt **charmrun**.</span><span class="sxs-lookup"><span data-stu-id="f5593-202">Or create a nodelist file for **charmrun**.</span></span>
   
   ```
   else
     # Create hello nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write hello head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into hello nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. <span data-ttu-id="f5593-203">Futtatás **charmrun** hello csomópontlista fájl, a visszatérési állapotának beolvasása és eltávolítása hello csomópontlista fájl hello végén.</span><span class="sxs-lookup"><span data-stu-id="f5593-203">Run **charmrun** with hello nodelist file, get its return status, and remove hello nodelist file at hello end.</span></span>
   
   <span data-ttu-id="f5593-204">{CCP_NUMCPUS} $ egy másik környezeti változó hello HPC Pack átjárócsomópont állítja be.</span><span class="sxs-lookup"><span data-stu-id="f5593-204">${CCP_NUMCPUS} is another environment variable set by hello HPC Pack head node.</span></span> <span data-ttu-id="f5593-205">Teljes toothis feladat lefoglalt magok száma hello tárol.</span><span class="sxs-lookup"><span data-stu-id="f5593-205">It stores hello number of total cores allocated toothis job.</span></span> <span data-ttu-id="f5593-206">A Microsoft használhatjuk a folyamatok száma toospecify hello charmrun.</span><span class="sxs-lookup"><span data-stu-id="f5593-206">We use it toospecify hello number of processes for charmrun.</span></span>
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. <span data-ttu-id="f5593-207">Kilépés a hello **charmrun** visszatérési állapota.</span><span class="sxs-lookup"><span data-stu-id="f5593-207">Exit with hello **charmrun** return status.</span></span>
   
   ```
   exit ${RTNSTS}
   ```

<span data-ttu-id="f5593-208">Az alábbiakban látható hello adatokat hello csomópontlista fájlban, mely hello parancsfájlt hoz létre:</span><span class="sxs-lookup"><span data-stu-id="f5593-208">Following is hello information in hello nodelist file, which hello script generates:</span></span>

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

<span data-ttu-id="f5593-209">Példa:</span><span class="sxs-lookup"><span data-stu-id="f5593-209">For example:</span></span>

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a><span data-ttu-id="f5593-210">NAMD feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="f5593-210">Submit a NAMD job</span></span>
<span data-ttu-id="f5593-211">Most már készen áll a toosubmit a NAMD feladatok HPC-Fürtkezelőben áll.</span><span class="sxs-lookup"><span data-stu-id="f5593-211">Now you are ready toosubmit a NAMD job in HPC Cluster Manager.</span></span>

1. <span data-ttu-id="f5593-212">Csatlakozás tooyour fürt átjárócsomópontjával, és indítsa el a HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="f5593-212">Connect tooyour cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="f5593-213">A **erőforrás-kezelés**, akkor győződjön meg arról, hogy hello Linux számítási csomópontok hello **Online** állapotát.</span><span class="sxs-lookup"><span data-stu-id="f5593-213">In **Resource Management**, ensure that hello Linux compute nodes are in hello **Online** state.</span></span> <span data-ttu-id="f5593-214">Ha nem, válassza ki azokat, és kattintson a **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="f5593-214">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="f5593-215">A **feladatkezelés**, kattintson a **új feladat**.</span><span class="sxs-lookup"><span data-stu-id="f5593-215">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="f5593-216">Írjon be egy feladat nevét, például *hpccharmrun*.</span><span class="sxs-lookup"><span data-stu-id="f5593-216">Enter a name for job such as *hpccharmrun*.</span></span>
   
   ![Új HPC-feladat][namd_job]
5. <span data-ttu-id="f5593-218">A hello **feladat részletei** lapján, a **feladat erőforrások**, válassza ki, mint erőforrás hello típusú **csomópont** és set hello **minimális** too3.</span><span class="sxs-lookup"><span data-stu-id="f5593-218">On hello **Job Details** page, under **Job Resources**, select hello type of resource as **Node** and set hello **Minimum** too3.</span></span> <span data-ttu-id="f5593-219">, a Microsoft hello feladat futtatása a három Linux csomópontok, és minden fürtcsomópont négy magok.</span><span class="sxs-lookup"><span data-stu-id="f5593-219">, we run hello job on three Linux nodes and each node has four cores.</span></span>
   
   ![Feladat erőforrások][job_resources]
6. <span data-ttu-id="f5593-221">Kattintson a **szerkesztése feladatok** a bal oldali navigációs hello, és kattintson a **Hozzáadás** tooadd egy feladat toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="f5593-221">Click **Edit Tasks** in hello left navigation, and then click **Add** tooadd a task toohello job.</span></span>    
7. <span data-ttu-id="f5593-222">A hello **feladat részleteinek és az i/o-átirányítás** lapon, a következő értékek set hello:</span><span class="sxs-lookup"><span data-stu-id="f5593-222">On hello **Task Details and I/O Redirection** page, set hello following values:</span></span>
   
   * <span data-ttu-id="f5593-223">**Parancssori**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span><span class="sxs-lookup"><span data-stu-id="f5593-223">**Command line** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span></span>
     
     > [!TIP]
     > <span data-ttu-id="f5593-224">hello előző parancssora sortörés nélkül egyetlen paranccsal.</span><span class="sxs-lookup"><span data-stu-id="f5593-224">hello preceding command line is a single command without line breaks.</span></span> <span data-ttu-id="f5593-225">Az tooappear becsomagolja alatt több sorban **parancssori**.</span><span class="sxs-lookup"><span data-stu-id="f5593-225">It wraps tooappear on several lines under **Command line**.</span></span>
     > 
     > 
   * <span data-ttu-id="f5593-226">**Munkakönyvtár** -/namd2</span><span class="sxs-lookup"><span data-stu-id="f5593-226">**Working directory** - /namd2</span></span>
   * <span data-ttu-id="f5593-227">**Minimális** – 3</span><span class="sxs-lookup"><span data-stu-id="f5593-227">**Minimum** - 3</span></span>
     
     ![Feladat részletei][task_details]
     
     > [!NOTE]
     > <span data-ttu-id="f5593-229">Itt beállította az hello munkakönyvtár mert **charmrun** toonavigate toohello megpróbál azonos munkakönyvtára, minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="f5593-229">You set hello working directory here because **charmrun** tries toonavigate toohello same working directory on each node.</span></span> <span data-ttu-id="f5593-230">Ha hello munkakönyvtár nem van beállítva, HPC Pack hello parancs indítja el hello Linux csomópontok egyikén létrehozni egy véletlenszerűen előállított nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="f5593-230">If hello working directory isn't set, HPC Pack starts hello command in a randomly named folder created on one of hello Linux nodes.</span></span> <span data-ttu-id="f5593-231">Ennek hatására hello a következő hiba hello más csomópontok: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid a probléma, adja meg a mappa elérési útját hello munkakönyvtára, minden csomópontja által elérhető.</span><span class="sxs-lookup"><span data-stu-id="f5593-231">This causes hello following error on hello other nodes: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid this problem, specify a folder path that can be accessed by all nodes as hello working directory.</span></span>
     > 
     > 
8. <span data-ttu-id="f5593-232">Kattintson a **OK** , majd **Submit** toorun a feladatot.</span><span class="sxs-lookup"><span data-stu-id="f5593-232">Click **OK** and then click **Submit** toorun this job.</span></span>
   
   <span data-ttu-id="f5593-233">Alapértelmezés szerint a HPC Pack hello feladat, a jelenlegi bejelentkezett felhasználói fiók küldi el.</span><span class="sxs-lookup"><span data-stu-id="f5593-233">By default, HPC Pack submits hello job as your current logged-on user account.</span></span> <span data-ttu-id="f5593-234">A párbeszédpanel kérhetik tooenter hello felhasználónevet és jelszót kattintás után **Submit**.</span><span class="sxs-lookup"><span data-stu-id="f5593-234">A dialog box might prompt you tooenter hello user name and password after you click **Submit**.</span></span>
   
   ![Feladat hitelesítő adatai][creds]
   
   <span data-ttu-id="f5593-236">Bizonyos körülmények között a HPC Pack emlékszik hello felhasználói adatok előtt adjon meg, és nem jeleníti meg ezen a párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="f5593-236">Under some conditions, HPC Pack remembers hello user information you input before and doesn't show this dialog box.</span></span> <span data-ttu-id="f5593-237">HPC Pack toomake újbóli, hello a következő parancsot a parancssorba írja be, és majd elküldeni a hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="f5593-237">toomake HPC Pack show it again, enter hello following command at a Command Prompt and then submit hello job.</span></span>
   
   ```command
   hpccred delcreds
   ```
9. <span data-ttu-id="f5593-238">hello feladat végrehajtásához szükséges néhány perc toofinish.</span><span class="sxs-lookup"><span data-stu-id="f5593-238">hello job takes several minutes toofinish.</span></span>
10. <span data-ttu-id="f5593-239">Hello feladat naplójának található \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log és hello kimeneti fájlok \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span><span class="sxs-lookup"><span data-stu-id="f5593-239">Find hello job log at \\<headnodeName>\Namd\namd2\namd2_hpccharmrun.log and hello output files in \\<headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span></span>
11. <span data-ttu-id="f5593-240">Szükség esetén indítsa el a VMD tooview a feladat eredményeinek.</span><span class="sxs-lookup"><span data-stu-id="f5593-240">Optionally, start VMD tooview your job results.</span></span> <span data-ttu-id="f5593-241">hello hello NAMD a kimenő fájlok (ebben az esetben a vízjel területén ubiquitin fehérje molekula) megjelenítésére lépésekre Ez a cikk hello terjed.</span><span class="sxs-lookup"><span data-stu-id="f5593-241">hello steps for visualizing hello NAMD output files (in this case, a ubiquitin protein molecule in a water sphere) are beyond hello scope of this article.</span></span> <span data-ttu-id="f5593-242">Lásd: [NAMD oktatóanyag](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f5593-242">See [NAMD Tutorial](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) for details.</span></span>
    
    ![Feladat eredményeinek][vmd_view]

## <a name="sample-files"></a><span data-ttu-id="f5593-244">Mintafájlok</span><span class="sxs-lookup"><span data-stu-id="f5593-244">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="f5593-245">A minta XML konfigurációs fájl fürttelepítés PowerShell-parancsprogram</span><span class="sxs-lookup"><span data-stu-id="f5593-245">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a><span data-ttu-id="f5593-246">Mintafájl cred.xml</span><span class="sxs-lookup"><span data-stu-id="f5593-246">Sample cred.xml file</span></span>
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

### <a name="sample-hpccharmrunsh-script"></a><span data-ttu-id="f5593-247">Mintaparancsfájl hpccharmrun.sh</span><span class="sxs-lookup"><span data-stu-id="f5593-247">Sample hpccharmrun.sh script</span></span>
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run hello charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create hello nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write hello head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into hello nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
