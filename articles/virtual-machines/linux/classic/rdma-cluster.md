---
title: "MPI alkalmazások futtatásához hozzon létre egy Linux RDMA fürt |} Microsoft Docs"
description: "Méretű H16r, H16mr, A8 vagy A9 virtuális gépeket az Azure RDMA hálózati MPI alkalmazások futtatásához használandó Linux-fürt létrehozása"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 4b2ceb64b1737918458f6d5c692fc2bfbc0f12ed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a><span data-ttu-id="7e5aa-103">Linuxos RDMA-fürt beállítása MPI-alkalmazások futtatására</span><span class="sxs-lookup"><span data-stu-id="7e5aa-103">Set up a Linux RDMA cluster to run MPI applications</span></span>
<span data-ttu-id="7e5aa-104">Ismerje meg, hogyan állíthat be az Azure-ban Linux RDMA fürt [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) párhuzamos Message Passing Interface (MPI) alkalmazások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-104">Learn how to set up a Linux RDMA cluster in Azure with [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to run parallel Message Passing Interface (MPI) applications.</span></span> <span data-ttu-id="7e5aa-105">Ez a cikk lépéseit Intel MPI futhat egy fürt Linux HPC lemezkép előkészítése.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-105">This article provides steps to prepare a Linux HPC image to run Intel MPI on a cluster.</span></span> <span data-ttu-id="7e5aa-106">Előkészítő, miután a virtuális gépek használata a lemezkép és az RDMA-kompatibilis Azure Virtuálisgép-méretek, (jelenleg H16r, H16mr, A8 és A9) egy fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-106">After preparation, you deploy a cluster of VMs using this image and one of the RDMA-capable Azure VM sizes (currently H16r, H16mr, A8, or A9).</span></span> <span data-ttu-id="7e5aa-107">A fürt használatával, amely a távoli közvetlen memória-hozzáférés (RDMA) technológián alapulnak, alacsony késésű, nagy átviteli hálózati hatékonyan kommunikációhoz MPI-alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-107">Use the cluster to run MPI applications that communicate efficiently over a low-latency, high-throughput network based on remote direct memory access (RDMA) technology.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7e5aa-108">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-108">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="7e5aa-109">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-109">This article covers using the classic deployment model.</span></span> <span data-ttu-id="7e5aa-110">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

## <a name="cluster-deployment-options"></a><span data-ttu-id="7e5aa-111">Fürt üzembe helyezési lehetőségei</span><span class="sxs-lookup"><span data-stu-id="7e5aa-111">Cluster deployment options</span></span>
<span data-ttu-id="7e5aa-112">Az alábbiakban módszert hozhat létre Linux RDMA-fürtöt, vagy a Feladatütemező nélkül használhat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-112">Following are methods you can use to create a Linux RDMA cluster with or without a job scheduler.</span></span>

* <span data-ttu-id="7e5aa-113">**Az Azure CLI-parancsfájlok**: a cikk későbbi részében látható, használja a [Azure parancssori felület](../../../cli-install-nodejs.md) (CLI) egy fürt RDMA-kompatibilisek-e virtuális gépek telepítését parancsfájllal történő.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-113">**Azure CLI scripts**: As shown later in this article, use the [Azure command-line interface](../../../cli-install-nodejs.md) (CLI) to script the deployment of a cluster of RDMA-capable VMs.</span></span> <span data-ttu-id="7e5aa-114">A parancssori felület szolgáltatásfelügyelet módban a fürtcsomópontok Feladattervek hoz létre a klasszikus üzembe helyezési modellel, így sok számítási csomópont telepítése több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-114">The CLI in Service Management mode creates the cluster nodes serially in the classic deployment model, so deploying many compute nodes might take several minutes.</span></span> <span data-ttu-id="7e5aa-115">A klasszikus üzembe helyezési modellt használja az RDMA hálózati kapcsolat engedélyezéséhez telepítenie kell a virtuális gépek ugyanazt a felhőszolgáltatásban található.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-115">To enable the RDMA network connection when you use the classic deployment model, deploy the VMs in the same cloud service.</span></span>
* <span data-ttu-id="7e5aa-116">**Az Azure Resource Manager-sablonok**: egy fürt, amely összeköti az RDMA hálózati RDMA-kompatibilisek-e virtuális gépek telepítése a Resource Manager üzembe helyezési modellel is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-116">**Azure Resource Manager templates**: You can also use the Resource Manager deployment model to deploy a cluster of RDMA-capable VMs that connects to the RDMA network.</span></span> <span data-ttu-id="7e5aa-117">Is [létrehozhat saját sablont](../../../resource-group-authoring-templates.md), vagy ellenőrizze a [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/) Microsoft vagy a közösségi kívánt megoldás üzembe helyezéséhez által közzétett sablonokat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-117">You can [create your own template](../../../resource-group-authoring-templates.md), or check the [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/) for templates contributed by Microsoft or the community to deploy the solution you want.</span></span> <span data-ttu-id="7e5aa-118">Resource Manager-sablonok is biztosít a Linux-fürt üzembe gyors és megbízható módot.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-118">Resource Manager templates can provide a fast and reliable way to deploy a Linux cluster.</span></span> <span data-ttu-id="7e5aa-119">Szeretné engedélyezni az RDMA hálózati kapcsolatot a Resource Manager üzembe helyezési modellel használatakor, központi telepítését az azonos rendelkezésre állási csoport a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-119">To enable the RDMA network connection when you use the Resource Manager deployment model, deploy the VMs in the same availability set.</span></span>
* <span data-ttu-id="7e5aa-120">**HPC Pack**: Microsoft HPC Pack fürt létrehozása az Azure-ban, és adja hozzá az RDMA-kompatibilisek-e, a támogatott Linux-disztribúció az RDMA hálózati eléréséhez futtató számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-120">**HPC Pack**: Create a Microsoft HPC Pack cluster in Azure and add RDMA-capable compute nodes that run a supported Linux distribution to access the RDMA network.</span></span> <span data-ttu-id="7e5aa-121">További információkért lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7e5aa-121">For more information, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="sample-deployment-steps-in-the-classic-model"></a><span data-ttu-id="7e5aa-122">A klasszikus modellben minta az üzembe helyezés lépései</span><span class="sxs-lookup"><span data-stu-id="7e5aa-122">Sample deployment steps in the classic model</span></span>
<span data-ttu-id="7e5aa-123">A következő lépések bemutatják, hogyan telepítse a SUSE Linux Enterprise Server (SLES) 12 SP1 HPC virtuális Gépet az Azure piactérről, testre szabhatja, és hozzon létre egy egyéni Virtuálisgép-lemezkép az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-123">The following steps show how to use the Azure CLI to deploy a SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM from the Azure Marketplace, customize it, and create a custom VM image.</span></span> <span data-ttu-id="7e5aa-124">A lemezkép használatával majd RDMA-kompatibilisek-e virtuális gépek a fürt a telepítési parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-124">Then you can use the image to script the deployment of a cluster of RDMA-capable VMs.</span></span>

> [!TIP]
> <span data-ttu-id="7e5aa-125">Hasonló lépések segítségével az Azure piactéren CentOS alapú HPC képek alapján RDMA-kompatibilisek-e virtuális gépek fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-125">Use similar steps to deploy a cluster of RDMA-capable VMs based on CentOS-based HPC images in the Azure Marketplace.</span></span> <span data-ttu-id="7e5aa-126">Néhány lépést esetekben némileg eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-126">Some steps differ slightly, as noted.</span></span> 
>
>

### <a name="prerequisites"></a><span data-ttu-id="7e5aa-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7e5aa-127">Prerequisites</span></span>
* <span data-ttu-id="7e5aa-128">**Ügyfélszámítógép**: Azure kommunikálni Mac, Linux vagy a Windows ügyfél számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-128">**Client computer**: You need a Mac, Linux, or Windows client computer to communicate with Azure.</span></span> <span data-ttu-id="7e5aa-129">Ezek a lépések feltételezik, hogy a Linux-ügyfelet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-129">These steps assume you are using a Linux client.</span></span>
* <span data-ttu-id="7e5aa-130">**Azure-előfizetés**: Ha nem rendelkezik előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-130">**Azure subscription**: If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span> <span data-ttu-id="7e5aa-131">Nagyobb fürtök esetén fontolja meg a használatalapú előfizetés vagy egyéb beszerzési lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-131">For larger clusters, consider a pay-as-you-go subscription or other purchase options.</span></span>
* <span data-ttu-id="7e5aa-132">**Virtuális gép mérete rendelkezésre állási**: A következő példány értékek RDMA-kompatibilis: H16r, H16mr, A8 és A9.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-132">**VM size availability**: The following instance sizes are RDMA capable: H16r, H16mr, A8, and A9.</span></span> <span data-ttu-id="7e5aa-133">Ellenőrizze [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/) a Azure-régiók rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-133">Check [Products available by region](https://azure.microsoft.com/regions/services/) for availability in Azure regions.</span></span>
* <span data-ttu-id="7e5aa-134">**Magok kvóta**: szükség lehet a számítási igényű virtuális gépek fürt központi telepítése magszámra vonatkozó kvóta növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-134">**Cores quota**: You might need to increase the quota of cores to deploy a cluster of compute-intensive VMs.</span></span> <span data-ttu-id="7e5aa-135">Például legalább 128 magok kell, ha azt szeretné, 8 A9 virtuális gépek telepítése ebben a cikkben ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-135">For example, you need at least 128 cores if you want to deploy 8 A9 VMs as shown in this article.</span></span> <span data-ttu-id="7e5aa-136">Az előfizetés is lehet, hogy korlátozza az egyes virtuális gép mérete családok, többek között a H-sorozat telepítheti magok száma.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-136">Your subscription might also limit the number of cores you can deploy in certain VM size families, including the H-series.</span></span> <span data-ttu-id="7e5aa-137">A kvóta növelését [nyissa meg az online támogatás ügyfélkérés](../../../azure-supportability/how-to-create-azure-support-request.md) díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-137">To request a quota increase, [open an online customer support request](../../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
* <span data-ttu-id="7e5aa-138">**Az Azure CLI**: [telepítése](../../../cli-install-nodejs.md) az Azure CLI és [csatlakozni az Azure-előfizetéshez](../../../xplat-cli-connect.md) az ügyfélszámítógépről.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-138">**Azure CLI**: [Install](../../../cli-install-nodejs.md) the Azure CLI and [connect to your Azure subscription](../../../xplat-cli-connect.md) from the client computer.</span></span>

### <a name="provision-an-sles-12-sp1-hpc-vm"></a><span data-ttu-id="7e5aa-139">Az SLES 12 SP1 HPC virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="7e5aa-139">Provision an SLES 12 SP1 HPC VM</span></span>
<span data-ttu-id="7e5aa-140">Az Azure-bA az Azure parancssori felülettel történő bejelentkezés után futtassa `azure config list` annak ellenőrzéséhez, hogy a kimenet látható-e a szolgáltatásfelügyeleti módban.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-140">After signing in to Azure with the Azure CLI, run `azure config list` to confirm that the output shows Service Management mode.</span></span> <span data-ttu-id="7e5aa-141">Ha nem, a mód beállítása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-141">If it does not, set the mode by running this command:</span></span>

    azure config mode asm


<span data-ttu-id="7e5aa-142">Írja be az alábbi listában az összes olyan előfizetést, amelyet Ön jogosult-e használni:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-142">Type the following to list all the subscriptions you are authorized to use:</span></span>

    azure account list

<span data-ttu-id="7e5aa-143">Az aktuális aktív előfizetéssel, amelynél az `Current` beállítása `true`.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-143">The current active subscription is identified with `Current` set to `true`.</span></span> <span data-ttu-id="7e5aa-144">Ha ez az előfizetés nem az a fürt létrehozásához használni kívánt, az aktív előfizetéssel állítsa be a megfelelő előfizetés-azonosító:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-144">If this subscription isn't the one you want to use to create the cluster, set the appropriate subscription ID as the active subscription:</span></span>

    azure account set <subscription-Id>

<span data-ttu-id="7e5aa-145">Tekintse meg az Azure-ban a nyilvánosan elérhető SLES 12 SP1 HPC-lemezképek, a következőhöz hasonló parancs futtatása, feltéve, hogy a rendszerhéj-környezet támogatja **grep**:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-145">To see the publicly available SLES 12 SP1 HPC images in Azure, run a command like the following, assuming your shell environment supports **grep**:</span></span>

    azure vm image list | grep "suse.*hpc"

<span data-ttu-id="7e5aa-146">SLES 12 SP1 HPC képének az RDMA-kompatibilisek-e virtuális gép kiépítése a következőhöz hasonló parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-146">Provision an RDMA-capable VM with a SLES 12 SP1 HPC image by running a command like the following:</span></span>

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

<span data-ttu-id="7e5aa-147">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-147">Where:</span></span>

* <span data-ttu-id="7e5aa-148">A méret (ebben a példában A9) egyike az RDMA-kompatibilisek-e Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-148">The size (A9 in this example) is one of the RDMA-capable VM sizes.</span></span>
* <span data-ttu-id="7e5aa-149">A külső SSH-port száma (a példában az SSH alapértelmezés 22) bármilyen érvényes portszámot.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-149">The external SSH port number (22 in this example, which is the SSH default) is any valid port number.</span></span> <span data-ttu-id="7e5aa-150">A belső SSH-portszám 22 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-150">The internal SSH port number is set to 22.</span></span>
* <span data-ttu-id="7e5aa-151">Új felhőalapú szolgáltatás az Azure-régió, a hely által meghatározott jön létre.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-151">A new cloud service is created in the Azure region specified by the location.</span></span> <span data-ttu-id="7e5aa-152">Adjon meg egy helyet, ahol a Virtuálisgép-méretet választott érhető el.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-152">Specify a location in which the VM size you choose is available.</span></span>
* <span data-ttu-id="7e5aa-153">A SUSE rangsorolási támogatással (amely további költségeket terhel) a SLES 12 SP1 kép neve jelenleg lehet ezek két lehetőség közül:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-153">For SUSE priority support (which incurs additional charges), the SLES 12 SP1 image name currently can be one of these two options:</span></span> 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-the-vm"></a><span data-ttu-id="7e5aa-154">A virtuális gép testreszabása</span><span class="sxs-lookup"><span data-stu-id="7e5aa-154">Customize the VM</span></span>
<span data-ttu-id="7e5aa-155">A virtuális gép befejeződése kiépítés, a virtuális gép külső IP-cím (vagy DNS-név) a virtuális gép SSH, és a külső portra a számát, és majd testreszabása után.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-155">After the VM finishes provisioning, SSH to the VM by using the VM's external IP address (or DNS name) and the external port number you configured, and then customize it.</span></span> <span data-ttu-id="7e5aa-156">Kapcsolat részletekért lásd: [Linuxot futtató virtuális gép bejelentkezés](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7e5aa-156">For connection details, see [How to log on to a virtual machine running Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="7e5aa-157">Parancsok végrehajtása a felhasználó nevében, konfigurálta a virtuális Gépen, kivéve, ha a rendszergazdai hozzáférés szükséges egy lépés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-157">Perform commands as the user you configured on the VM, unless root access is required to complete a step.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7e5aa-158">A Microsoft Azure nem legfelső szintű hozzáférés biztosítása a Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-158">Microsoft Azure does not provide root access to Linux VMs.</span></span> <span data-ttu-id="7e5aa-159">Ahhoz, hogy a rendszergazdai hozzáférés esetén a virtuális gép felhasználóként, futtassa a parancsokat `sudo`.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-159">To gain administrative access when connected as a user to the VM, run commands by using `sudo`.</span></span>
>
>

* <span data-ttu-id="7e5aa-160">**Frissítések**: frissítések telepítése zypper használatával.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-160">**Updates**: Install updates by using zypper.</span></span> <span data-ttu-id="7e5aa-161">Akkor is telepíteni szeretne NFS segédprogramok.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-161">You might also want to install NFS utilities.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="7e5aa-162">SLES 12 SP1 HPC virtuális gépen azt javasoljuk, hogy a rendszermag-frissítéseket, amelyek problémákat okozhatnak a Linux RDMA illesztőprogramok nem alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-162">In a SLES 12 SP1 HPC VM, we recommend that you don't apply kernel updates, which can cause issues with the Linux RDMA drivers.</span></span>
  >
  >
* <span data-ttu-id="7e5aa-163">**Intel MPI**: Intel MPI a SLES 12 SP1 HPC virtuális Gépre telepítése után a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-163">**Intel MPI**: Complete the installation of Intel MPI on the SLES 12 SP1 HPC VM by running the following command:</span></span>

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* <span data-ttu-id="7e5aa-164">**Memóriát zárolni**: A MPI kódok zárolni a rendelkezésre álló memória az RDMA, adja hozzá, vagy módosítsa a /etc/security/limits.conf fájlban a következő beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-164">**Lock memory**: For MPI codes to lock the memory available for RDMA, add or change the following settings in the /etc/security/limits.conf file.</span></span> <span data-ttu-id="7e5aa-165">Ez a fájl szerkesztése legfelső szintű hozzáférés szükséges.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-165">You need root access to edit this file.</span></span>

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > <span data-ttu-id="7e5aa-166">Tesztelési célokra is állíthatja memlock korlátlan.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-166">For testing purposes, you can also set memlock to unlimited.</span></span> <span data-ttu-id="7e5aa-167">Például: `<User or group name>    hard    memlock unlimited`.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-167">For example: `<User or group name>    hard    memlock unlimited`.</span></span> <span data-ttu-id="7e5aa-168">További információkért lásd: [beállítás ismert legjobb módszerei zárolt memória mérete](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span><span class="sxs-lookup"><span data-stu-id="7e5aa-168">For more information, see [Best known methods for setting locked memory size](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span></span>
  >
  >
* <span data-ttu-id="7e5aa-169">**SLES virtuális gépek SSH-kulcsok**: készítése SSH-kulcsok a felhasználói fiókhoz, a számítási csomópontok közötti megbízhatósági kapcsolat létrehozása az SLES a fürt MPI-feladatok futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-169">**SSH keys for SLES VMs**: Generate SSH keys to establish trust for your user account among the compute nodes in the SLES cluster when running MPI jobs.</span></span> <span data-ttu-id="7e5aa-170">Ha telepítette a HPC CentOS-alapú virtuális gépek, ne ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-170">If you deployed a CentOS-based HPC VM, don't follow this step.</span></span> <span data-ttu-id="7e5aa-171">Tekintse meg a passwordless SSH megbízhatóság kialakításához fürt csomópontjai között a lemezképet, és a fürt központi telepítése után a cikk útmutatást.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-171">See instructions later in this article to set up passwordless SSH trust among the cluster nodes after you capture the image and deploy the cluster.</span></span>

    <span data-ttu-id="7e5aa-172">SSH-kulcsok létrehozásához futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-172">To create SSH keys, run the following command.</span></span> <span data-ttu-id="7e5aa-173">Amikor a bemeneti kéri, válassza ki **Enter** a kulcs létrehozásához az alapértelmezett helyen jelszó beállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-173">When you are prompted for input, select **Enter** to generate the keys in the default location without setting a password.</span></span>

        ssh-keygen

    <span data-ttu-id="7e5aa-174">A nyilvános kulcs hozzáfűzése ismert nyilvános kulcsok authorized_keys fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-174">Append the public key to the authorized_keys file for known public keys.</span></span>

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    <span data-ttu-id="7e5aa-175">A ~/.ssh könyvtárban szerkesztheti, és a konfigurációs fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-175">In the ~/.ssh directory, edit or create the config file.</span></span> <span data-ttu-id="7e5aa-176">Adja meg az IP-címtartományt, amely (ebben a példában 10.32.0.0/16) Azure-ban használni kívánt magánhálózat:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-176">Provide the IP address range of the private network that you plan to use in Azure (10.32.0.0/16 in this example):</span></span>

        host 10.32.0.*
        StrictHostKeyChecking no

    <span data-ttu-id="7e5aa-177">Alternatív megoldásként listában az alábbiak szerint a fürt egyes virtuális gépek magánhálózati IP-címe:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-177">Alternatively, list the private network IP address of each VM in your cluster as follows:</span></span>

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > <span data-ttu-id="7e5aa-178">Konfigurálás `StrictHostKeyChecking no` biztonsági kockázatot hozhat létre, ha egy adott IP-cím vagy a tartomány nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-178">Configuring `StrictHostKeyChecking no` can create a potential security risk when a specific IP address or range is not specified.</span></span>
  >
  >
* <span data-ttu-id="7e5aa-179">**Alkalmazások**: telepítse a szükséges, vagy hajtsa végre a más testreszabás is szerepelt, mielőtt a lemezképet rögzítené alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-179">**Applications**: Install any applications you need or perform other customizations before you capture the image.</span></span>

### <a name="capture-the-image"></a><span data-ttu-id="7e5aa-180">A lemezkép rögzítése</span><span class="sxs-lookup"><span data-stu-id="7e5aa-180">Capture the image</span></span>
<span data-ttu-id="7e5aa-181">A lemezkép rögzítése, először futtassa a következő parancsot a Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-181">To capture the image, first run the following command on the Linux VM.</span></span> <span data-ttu-id="7e5aa-182">Ezt a parancsot a virtuális gép deprovisions, de megőrzi a felhasználói fiókok és a beállított SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-182">This command deprovisions the VM but maintains user accounts and SSH keys that you set up.</span></span>

```
sudo waagent -deprovision
```

<span data-ttu-id="7e5aa-183">Az ügyfélszámítógépről a következő parancsokat az Azure parancssori felület a lemezkép rögzítése.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-183">From your client computer, run the following Azure CLI commands to capture the image.</span></span> <span data-ttu-id="7e5aa-184">További információkért lásd: [rögzítése képként klasszikus Linuxos virtuális gép](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="7e5aa-184">For more information, see [How to capture a classic Linux virtual machine as an image](capture-image.md).</span></span>  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

<span data-ttu-id="7e5aa-185">Ezek a parancsok futtatása után a virtuális gép lemezképének rögzítése a használatra, és a virtuális gép törlődik.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-185">After you run these commands, the VM image is captured for your use and the VM is deleted.</span></span> <span data-ttu-id="7e5aa-186">Most már rendelkezik egy fürt üzembe helyezésére az egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-186">Now you have your custom image ready to deploy a cluster.</span></span>

### <a name="deploy-a-cluster-with-the-image"></a><span data-ttu-id="7e5aa-187">A lemezképpel fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="7e5aa-187">Deploy a cluster with the image</span></span>
<span data-ttu-id="7e5aa-188">Módosítsa a következő Bash parancsfájlt a környezetének megfelelő értékekkel, és futtassa az ügyfélszámítógépről.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-188">Modify the following Bash script with appropriate values for your environment and run it from your client computer.</span></span> <span data-ttu-id="7e5aa-189">Azure telepíti a virtuális gépek Feladattervek a klasszikus üzembe helyezési modellel, mert a nyolc A9 virtuális gépeket, javasolt ezt a parancsfájlt a telepítendő néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-189">Because Azure deploys the VMs serially in the classic deployment model, it takes a few minutes to deploy the eight A9 VMs suggested in this script.</span></span>

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a><span data-ttu-id="7e5aa-190">A CentOS HPC-fürt szempontjai</span><span class="sxs-lookup"><span data-stu-id="7e5aa-190">Considerations for a CentOS HPC cluster</span></span>
<span data-ttu-id="7e5aa-191">Ha be szeretné állítani a HPC egy SLES 12 helyett az Azure piactéren CentOS alapú HPC lemezképet alapján fürt, kövesse az előző szakaszban leírt általános lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-191">If you want to set up a cluster based on one of the CentOS-based HPC images in the Azure Marketplace instead of SLES 12 for HPC, follow the general steps in the preceding section.</span></span> <span data-ttu-id="7e5aa-192">Vegye figyelembe a következő eltérésekkel, telepítéséhez és a virtuális gép konfigurálása során:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-192">Note the following differences when you provision and configure the VM:</span></span>

- <span data-ttu-id="7e5aa-193">Intel MPI már telepítve van a virtuális gép kiépítése a CentOS-alapú HPC-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-193">Intel MPI is already installed on a VM provisioned from a CentOS-based HPC image.</span></span>
- <span data-ttu-id="7e5aa-194">Zárolási memóriabeállításait már kerülnek a virtuális gép /etc/security/limits.conf fájlban.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-194">Lock memory settings are already added in the VM's /etc/security/limits.conf file.</span></span>
- <span data-ttu-id="7e5aa-195">SSH-kulcsok a virtuális gép kiépítése a rögzítési nem hoznak létre.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-195">Do not generate SSH keys on the VM you provision for capture.</span></span> <span data-ttu-id="7e5aa-196">Ajánlja felhasználói hitelesítés beállítása a fürt telepítése után.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-196">Instead, we recommend setting up user-based authentication after you deploy the cluster.</span></span> <span data-ttu-id="7e5aa-197">További információkért lásd a következő.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-197">For more information, see the following section.</span></span>  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a><span data-ttu-id="7e5aa-198">A fürt passwordless SSH megbízhatóságának beállítása</span><span class="sxs-lookup"><span data-stu-id="7e5aa-198">Set up passwordless SSH trust on the cluster</span></span>
<span data-ttu-id="7e5aa-199">Egy CentOS-alapú HPC-fürtre, a számítási csomópontok közötti megbízhatósági kapcsolat létrehozásához két módszer áll rendelkezésre: állomás alapú hitelesítés és a felhasználó-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-199">On a CentOS-based HPC cluster, there are two methods for establishing trust between the compute nodes: host-based authentication and user-based authentication.</span></span> <span data-ttu-id="7e5aa-200">Gazdagép-alapú hitelesítés Ez a cikk hatókörén kívül esik, és általában kell elvégezni egy bővítmény parancsfájl központi telepítése során.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-200">Host-based authentication is outside of the scope of this article and generally must be done through an extension script during deployment.</span></span> <span data-ttu-id="7e5aa-201">Felhasználó-alapú hitelesítés kényelmes, a telepítés után a megbízható kapcsolat kialakítása és a generáció és a fürt az SSH-kulcsok a számítási csomópontok közötti megosztásának igényel.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-201">User-based authentication is convenient for establishing trust after deployment and requires the generation and sharing of SSH keys among the compute nodes in the cluster.</span></span> <span data-ttu-id="7e5aa-202">Ezt a módszert gyakran nevezik passwordless SSH-bejelentkezéskor, és futó MPI-feladatok esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-202">This method is commonly known as passwordless SSH login and is required when running MPI jobs.</span></span>

<span data-ttu-id="7e5aa-203">Egy minta parancsfájlt a Közösségtől hozzájárult érhető el a [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) CentOS-alapú HPC-fürt könnyen felhasználói hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-203">A sample script contributed from the community is available on [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) to enable easy user authentication on a CentOS-based HPC cluster.</span></span> <span data-ttu-id="7e5aa-204">Töltse le és használja ezt a parancsfájlt a következő lépések segítségével.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-204">Download and use this script by using the following steps.</span></span> <span data-ttu-id="7e5aa-205">Módosítsa ezt a parancsfájlt vagy más módszerrel használatával szeretne létrehozni a számítási fürtcsomópontok közötti passwordless SSH hitelesítés is.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-205">You can also modify this script or use any other method to establish passwordless SSH authentication between the cluster compute nodes.</span></span>

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

<span data-ttu-id="7e5aa-206">A parancsfájl futtatásához kell tudni, hogy az előtag, az alhálózati IP-címek számára.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-206">To run the script, you need to know the prefix for your subnet IP addresses.</span></span> <span data-ttu-id="7e5aa-207">Az előtag lekérése a következő parancs futtatásával a fürtcsomópontok egyike.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-207">Get the prefix by running the following command on one of the cluster nodes.</span></span> <span data-ttu-id="7e5aa-208">A kimeneti hasonlóan kell kinéznie 10.1.3.5, és az előtag a 10.1.3 részét.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-208">Your output should look something like 10.1.3.5, and the prefix is the 10.1.3 portion.</span></span>

    ifconfig eth0 | grep -w inet | awk '{print $2}'

<span data-ttu-id="7e5aa-209">Most futtassa a parancsfájl segítségével történő három paramétert: a közös felhasználónevet, a számítási csomópontokat, a számítási csomópontokat, és az alhálózati előtag, az előző parancs által visszaküldött közös jelszót.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-209">Now run the script using three parameters: the common user name on the compute nodes, the common password for that user on the compute nodes, and the subnet prefix that was returned from the previous command.</span></span>

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

<span data-ttu-id="7e5aa-210">A parancsfájl a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-210">This script does the following:</span></span>

* <span data-ttu-id="7e5aa-211">A gazdacsomópont nevű .ssh, ami azonban szükséges az passwordless bejelentkezési hoz létre egy könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-211">Creates a directory on the host node named .ssh, which is required for passwordless login.</span></span>
* <span data-ttu-id="7e5aa-212">Létrehoz egy konfigurációs fájlt a .ssh könyvtárban, amely arra utasítja a fürt minden csomópontján bejelentkezési engedélyezése passwordless bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-212">Creates a configuration file in the .ssh directory that instructs passwordless login to allow login from any node in the cluster.</span></span>
* <span data-ttu-id="7e5aa-213">A csomópont nevét és a fürt összes csomópontjának csomópont IP-címet tartalmazó fájlokat hozza létre.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-213">Creates files containing the node names and node IP addresses for all the nodes in the cluster.</span></span> <span data-ttu-id="7e5aa-214">Ezek a fájlok megmaradnak a későbbi felhasználás a parancsfájl futtatása után.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-214">These files are left after the script is run for later reference.</span></span>
* <span data-ttu-id="7e5aa-215">A privát és nyilvános kulcsból álló kulcspárt a fürt minden csomópontján (beleértve a gazdacsomópont) hoz létre, és létrehozza a bejegyzéseket a authorized_keys fájlba.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-215">Creates a private and public key pair for each cluster node (including the host node) and creates entries in the authorized_keys file.</span></span>

> [!WARNING]
> <span data-ttu-id="7e5aa-216">A parancsfájl futtatása, biztonsági kockázatot is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-216">Running this script can create a potential security risk.</span></span> <span data-ttu-id="7e5aa-217">Győződjön meg arról, hogy a nyilvános kulcs információja ~/.ssh nem terjesztése.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-217">Ensure that the public key information in ~/.ssh is not distributed.</span></span>
>
>

## <a name="configure-intel-mpi"></a><span data-ttu-id="7e5aa-218">Intel MPI konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e5aa-218">Configure Intel MPI</span></span>
<span data-ttu-id="7e5aa-219">MPI alkalmazások futtatásához Azure Linux RDMA szüksége konfigurálása bizonyos Intel MPI vonatkozó környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-219">To run MPI applications on Azure Linux RDMA, you need to configure certain environment variables specific to Intel MPI.</span></span> <span data-ttu-id="7e5aa-220">Íme egy minta Bash parancsfájl konfigurálása a változókat, az alkalmazás futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-220">Here is a sample Bash script to configure the variables needed to run an application.</span></span> <span data-ttu-id="7e5aa-221">Intel MPI-példány igény szerint módosítsa az elérési utat mpivars.sh.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-221">Change the path to mpivars.sh as needed for your installation of Intel MPI.</span></span>

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

<span data-ttu-id="7e5aa-222">A Hosts fájl formátuma a következő.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-222">The format of the host file is as follows.</span></span> <span data-ttu-id="7e5aa-223">Adja hozzá az egyes csomópontok egy sort a fürtön.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-223">Add one line for each node in your cluster.</span></span> <span data-ttu-id="7e5aa-224">Adja meg a korábban, nem a DNS-nevek meghatározott privát IP-címek a virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-224">Specify private IP addresses from the virtual network defined earlier, not DNS names.</span></span> <span data-ttu-id="7e5aa-225">Például a két gazdagépek 10.32.0.1 és 10.32.0.2 IP-címekkel rendelkező, a fájl tartalmazza a következő:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-225">For example, for two hosts with IP addresses 10.32.0.1 and 10.32.0.2, the file contains the following:</span></span>

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a><span data-ttu-id="7e5aa-226">MPI futtatnak egy alapszintű két csomópontot tartalmazó fürtben</span><span class="sxs-lookup"><span data-stu-id="7e5aa-226">Run MPI on a basic two-node cluster</span></span>
<span data-ttu-id="7e5aa-227">Ha még nem tette meg, először állítsa be a környezetet az Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-227">If you haven't already done so, first set up the environment for Intel MPI.</span></span>

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a><span data-ttu-id="7e5aa-228">Egy MPI parancs futtatása</span><span class="sxs-lookup"><span data-stu-id="7e5aa-228">Run an MPI command</span></span>
<span data-ttu-id="7e5aa-229">Egy MPI paranccsal jelenítse meg, hogy MPI megfelelően van telepítve, és képes kommunikálni a közötti legalább két számítási csomópontjain a számítási csomópontok egyikén.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-229">Run an MPI command on one of the compute nodes to show that MPI is installed properly and can communicate between at least two compute nodes.</span></span> <span data-ttu-id="7e5aa-230">A következő **mpirun** parancs elindul a **állomásnév** két csomópont parancs.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-230">The following **mpirun** command runs the **hostname** command on two nodes.</span></span>

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
<span data-ttu-id="7e5aa-231">A kimenetében csomópontjaihoz bemenetként továbbított nevei `-hosts`.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-231">Your output should list the names of all the nodes that you passed as input for `-hosts`.</span></span> <span data-ttu-id="7e5aa-232">Például egy **mpirun** két csomóponttal rendelkező parancs kimenetét a következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="7e5aa-232">For example, an **mpirun** command with two nodes returns output like the following:</span></span>

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a><span data-ttu-id="7e5aa-233">Egy MPI teljesítményteszt futtatása</span><span class="sxs-lookup"><span data-stu-id="7e5aa-233">Run an MPI benchmark</span></span>
<span data-ttu-id="7e5aa-234">A következő Intel MPI parancs fut egy pingpong javasolt fürtkonfiguráció és az RDMA hálózati kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-234">The following Intel MPI command runs a pingpong benchmark to verify the cluster configuration and connection to the RDMA network.</span></span>

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

<span data-ttu-id="7e5aa-235">Működő rendelkező fürtön két csomópont a következőhöz hasonló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-235">On a working cluster with two nodes, you should see output like the following.</span></span> <span data-ttu-id="7e5aa-236">A Azure RDMA hálózati késés, vagy az üzenet 3 ezredmásodperc alatt legfeljebb 512 bájt méretű várt.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-236">On the Azure RDMA network, expect latency at or below 3 microseconds for message sizes up to 512 bytes.</span></span>

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a><span data-ttu-id="7e5aa-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e5aa-237">Next steps</span></span>
* <span data-ttu-id="7e5aa-238">Regisztrálhat és futtathat a Linux MPI alkalmazások a Linux-fürt.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-238">Deploy and run your Linux MPI applications on your Linux cluster.</span></span>
* <span data-ttu-id="7e5aa-239">Tekintse meg a [Intel MPI Library dokumentációjában](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Intel MPI útmutatót.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-239">See the [Intel MPI Library documentation](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) for guidance on Intel MPI.</span></span>
* <span data-ttu-id="7e5aa-240">Próbálja meg egy [gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) HPC CentOS-alapú lemezkép használatával az Intel fényesség fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7e5aa-240">Try a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) to create an Intel Lustre cluster by using a CentOS-based HPC image.</span></span> <span data-ttu-id="7e5aa-241">További információkért lásd: [Intel felhő Edition telepítését a Microsoft Azure fényesség](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="7e5aa-241">For details, see [Deploying Intel Cloud Edition for Lustre on Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span></span>
