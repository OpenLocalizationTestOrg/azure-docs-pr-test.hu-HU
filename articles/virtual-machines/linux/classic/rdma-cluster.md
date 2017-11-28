---
title: "egy Linux RDMA fürt toorun MPI alkalmazások mentése aaaSet |} Microsoft Docs"
description: "Hello Azure RDMA hálózati toorun MPI alkalmazások mérete H16r, H16mr, A8 és A9 virtuális gépek toouse Linux fürt létrehozására"
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
ms.openlocfilehash: 3199317a37b095e80718d6724954687d30aea3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-linux-rdma-cluster-toorun-mpi-applications"></a><span data-ttu-id="e6ae4-103">A Linux RDMA fürt toorun MPI alkalmazások beállítása</span><span class="sxs-lookup"><span data-stu-id="e6ae4-103">Set up a Linux RDMA cluster toorun MPI applications</span></span>
<span data-ttu-id="e6ae4-104">Ismerje meg, hogyan tooset be egy Linux RDMA fürtön, az Azure-ban [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun párhuzamos Message Passing Interface (MPI) alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-104">Learn how tooset up a Linux RDMA cluster in Azure with [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun parallel Message Passing Interface (MPI) applications.</span></span> <span data-ttu-id="e6ae4-105">Ez a cikk a Linux HPC kép toorun Intel MPI biztosít lépéseket tooprepare fürtön.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-105">This article provides steps tooprepare a Linux HPC image toorun Intel MPI on a cluster.</span></span> <span data-ttu-id="e6ae4-106">Előkészítése, miután a virtuális gépek használata a lemezkép és hello RDMA-kompatibilis Azure Virtuálisgép-méretek (jelenleg H16r, H16mr, A8 és A9) egy fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-106">After preparation, you deploy a cluster of VMs using this image and one of hello RDMA-capable Azure VM sizes (currently H16r, H16mr, A8, or A9).</span></span> <span data-ttu-id="e6ae4-107">Hello fürt toorun MPI alkalmazásokat használnak a távoli közvetlen memória-hozzáférés (RDMA) technológián alapulnak, alacsony késésű, nagy átviteli hálózati hatékonyan kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-107">Use hello cluster toorun MPI applications that communicate efficiently over a low-latency, high-throughput network based on remote direct memory access (RDMA) technology.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6ae4-108">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-108">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="e6ae4-109">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="e6ae4-110">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

## <a name="cluster-deployment-options"></a><span data-ttu-id="e6ae4-111">Fürt üzembe helyezési lehetőségei</span><span class="sxs-lookup"><span data-stu-id="e6ae4-111">Cluster deployment options</span></span>
<span data-ttu-id="e6ae4-112">Az alábbiakban módszert használhat toocreate Linux RDMA fürt, vagy a Feladatütemező nélkül.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-112">Following are methods you can use toocreate a Linux RDMA cluster with or without a job scheduler.</span></span>

* <span data-ttu-id="e6ae4-113">**Az Azure CLI-parancsfájlok**: a cikk későbbi részében látható, használja a hello [Azure parancssori felület](../../../cli-install-nodejs.md) (CLI) tooscript hello telepítési RDMA-kompatibilisek-e virtuális gépek a fürt.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-113">**Azure CLI scripts**: As shown later in this article, use hello [Azure command-line interface](../../../cli-install-nodejs.md) (CLI) tooscript hello deployment of a cluster of RDMA-capable VMs.</span></span> <span data-ttu-id="e6ae4-114">hello CLI szolgáltatásfelügyelet módban hoz létre hello fürtcsomópontok Feladattervek hello klasszikus üzembe helyezési modellel, így sok számítási csomópont telepítése több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-114">hello CLI in Service Management mode creates hello cluster nodes serially in hello classic deployment model, so deploying many compute nodes might take several minutes.</span></span> <span data-ttu-id="e6ae4-115">tooenable hello RDMA hálózati kapcsolatot hello klasszikus üzembe helyezési modellel, használatakor hello virtuális gépek telepítése a hello ugyanaz a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-115">tooenable hello RDMA network connection when you use hello classic deployment model, deploy hello VMs in hello same cloud service.</span></span>
* <span data-ttu-id="e6ae4-116">**Az Azure Resource Manager-sablonok**: hello erőforrás-kezelő telepítési modell toodeploy, amely a toohello RDMA hálózati RDMA-kompatibilisek-e virtuális gépek fürtben is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-116">**Azure Resource Manager templates**: You can also use hello Resource Manager deployment model toodeploy a cluster of RDMA-capable VMs that connects toohello RDMA network.</span></span> <span data-ttu-id="e6ae4-117">Is [létrehozhat saját sablont](../../../resource-group-authoring-templates.md), vagy ellenőrizze a hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/) Microsoft vagy hello közösségi toodeploy hello megoldást által közzétett sablonokat.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-117">You can [create your own template](../../../resource-group-authoring-templates.md), or check hello [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/) for templates contributed by Microsoft or hello community toodeploy hello solution you want.</span></span> <span data-ttu-id="e6ae4-118">Resource Manager-sablonok biztosíthat egy gyors és megbízható módot toodeploy egy Linux-fürt.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-118">Resource Manager templates can provide a fast and reliable way toodeploy a Linux cluster.</span></span> <span data-ttu-id="e6ae4-119">tooenable hello RDMA hálózati kapcsolatot hello Resource Manager üzembe helyezési modelljével használatakor telepíteni hello hello virtuális gépek azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-119">tooenable hello RDMA network connection when you use hello Resource Manager deployment model, deploy hello VMs in hello same availability set.</span></span>
* <span data-ttu-id="e6ae4-120">**HPC Pack**: Microsoft HPC Pack fürt létrehozása az Azure-ban, és adja hozzá az RDMA-kompatibilis számítási csomópontokat, amelyek a támogatott Linux terjesztési tooaccess hello RDMA hálózati futnak.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-120">**HPC Pack**: Create a Microsoft HPC Pack cluster in Azure and add RDMA-capable compute nodes that run a supported Linux distribution tooaccess hello RDMA network.</span></span> <span data-ttu-id="e6ae4-121">További információkért lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="e6ae4-121">For more information, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="sample-deployment-steps-in-hello-classic-model"></a><span data-ttu-id="e6ae4-122">Üzembe helyezési minta hello klasszikus modellben lépések</span><span class="sxs-lookup"><span data-stu-id="e6ae4-122">Sample deployment steps in hello classic model</span></span>
<span data-ttu-id="e6ae4-123">hello következő lépések bemutatják, hogyan toouse hello Azure CLI toodeploy SUSE Linux Enterprise Server (SLES) 12 SP1 HPC virtuális gép hello Azure Piactérről származó testre szabhatja, és hozzon létre egy egyéni Virtuálisgép-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-123">hello following steps show how toouse hello Azure CLI toodeploy a SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM from hello Azure Marketplace, customize it, and create a custom VM image.</span></span> <span data-ttu-id="e6ae4-124">Majd hello lemezkép tooscript hello telepítéséhez a fürt RDMA-kompatibilisek-e virtuális gépek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-124">Then you can use hello image tooscript hello deployment of a cluster of RDMA-capable VMs.</span></span>

> [!TIP]
> <span data-ttu-id="e6ae4-125">RDMA-kompatibilisek-e virtuális gépek a fürt alapján hello Azure piactér CentOS alapú HPC képek hasonló lépéseket toodeploy használja.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-125">Use similar steps toodeploy a cluster of RDMA-capable VMs based on CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="e6ae4-126">Néhány lépést esetekben némileg eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-126">Some steps differ slightly, as noted.</span></span> 
>
>

### <a name="prerequisites"></a><span data-ttu-id="e6ae4-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e6ae4-127">Prerequisites</span></span>
* <span data-ttu-id="e6ae4-128">**Ügyfélszámítógép**: a Mac, Linux vagy a Windows ügyfél számítógép toocommunicate az Azure-ral van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-128">**Client computer**: You need a Mac, Linux, or Windows client computer toocommunicate with Azure.</span></span> <span data-ttu-id="e6ae4-129">Ezek a lépések feltételezik, hogy a Linux-ügyfelet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-129">These steps assume you are using a Linux client.</span></span>
* <span data-ttu-id="e6ae4-130">**Azure-előfizetés**: Ha nem rendelkezik előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-130">**Azure subscription**: If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span> <span data-ttu-id="e6ae4-131">Nagyobb fürtök esetén fontolja meg a használatalapú előfizetés vagy egyéb beszerzési lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-131">For larger clusters, consider a pay-as-you-go subscription or other purchase options.</span></span>
* <span data-ttu-id="e6ae4-132">**Virtuális gép mérete rendelkezésre állási**: a következő példányok hello RDMA-kompatibilis: H16r, H16mr, A8 és A9.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-132">**VM size availability**: hello following instance sizes are RDMA capable: H16r, H16mr, A8, and A9.</span></span> <span data-ttu-id="e6ae4-133">Ellenőrizze [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/) a Azure-régiók rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-133">Check [Products available by region](https://azure.microsoft.com/regions/services/) for availability in Azure regions.</span></span>
* <span data-ttu-id="e6ae4-134">**Magok kvóta**: szükség lehet a számítási igényű virtuális gépek a fürt magok toodeploy tooincrease hello beállított kvótát.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-134">**Cores quota**: You might need tooincrease hello quota of cores toodeploy a cluster of compute-intensive VMs.</span></span> <span data-ttu-id="e6ae4-135">Például legalább 128 magok kell, ha azt szeretné, hogy toodeploy 8 A9 VMs ebben a cikkben ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-135">For example, you need at least 128 cores if you want toodeploy 8 A9 VMs as shown in this article.</span></span> <span data-ttu-id="e6ae4-136">Az előfizetés is korlátozhatja az egyes virtuális gép mérete családok, többek között a hello H-sorozat telepítése magok hello száma.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-136">Your subscription might also limit hello number of cores you can deploy in certain VM size families, including hello H-series.</span></span> <span data-ttu-id="e6ae4-137">toorequest a kvóta növeléséhez [nyissa meg az online támogatás ügyfélkérés](../../../azure-supportability/how-to-create-azure-support-request.md) díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-137">toorequest a quota increase, [open an online customer support request](../../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
* <span data-ttu-id="e6ae4-138">**Az Azure CLI**: [telepítése](../../../cli-install-nodejs.md) hello Azure CLI és [csatlakozás Azure-előfizetés tooyour](../../../xplat-cli-connect.md) hello ügyfélszámítógépről.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-138">**Azure CLI**: [Install](../../../cli-install-nodejs.md) hello Azure CLI and [connect tooyour Azure subscription](../../../xplat-cli-connect.md) from hello client computer.</span></span>

### <a name="provision-an-sles-12-sp1-hpc-vm"></a><span data-ttu-id="e6ae4-139">Az SLES 12 SP1 HPC virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="e6ae4-139">Provision an SLES 12 SP1 HPC VM</span></span>
<span data-ttu-id="e6ae4-140">Az Azure CLI hello tooAzure történő bejelentkezés után futtassa `azure config list` , hogy a kimeneti hello tooconfirm jeleníti meg a szolgáltatásfelügyeleti módban.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-140">After signing in tooAzure with hello Azure CLI, run `azure config list` tooconfirm that hello output shows Service Management mode.</span></span> <span data-ttu-id="e6ae4-141">Ha nem, hello mód beállítása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-141">If it does not, set hello mode by running this command:</span></span>

    azure config mode asm


<span data-ttu-id="e6ae4-142">Írja be a következő toolist hello engedélyezett toouse áll előfizetéseket hello:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-142">Type hello following toolist all hello subscriptions you are authorized toouse:</span></span>

    azure account list

<span data-ttu-id="e6ae4-143">hello érvényes aktív előfizetéssel, amelynél az `Current` túl beállítása`true`.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-143">hello current active subscription is identified with `Current` set too`true`.</span></span> <span data-ttu-id="e6ae4-144">Ha ez az előfizetés nem hello egy akkor toouse toocreate hello fürt, aktív előfizetéssel hello beállítása hello megfelelő előfizetés-azonosító:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-144">If this subscription isn't hello one you want toouse toocreate hello cluster, set hello appropriate subscription ID as hello active subscription:</span></span>

    azure account set <subscription-Id>

<span data-ttu-id="e6ae4-145">toosee hello Azure-ban futtatni egy parancsot például hello a következő, feltéve, hogy a rendszerhéj-környezet támogatja a nyilvánosan elérhető SLES 12 SP1 HPC képek **grep**:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-145">toosee hello publicly available SLES 12 SP1 HPC images in Azure, run a command like hello following, assuming your shell environment supports **grep**:</span></span>

    azure vm image list | grep "suse.*hpc"

<span data-ttu-id="e6ae4-146">SLES 12 SP1 HPC képének az RDMA-kompatibilisek-e virtuális gép kiépítése hello következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-146">Provision an RDMA-capable VM with a SLES 12 SP1 HPC image by running a command like hello following:</span></span>

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

<span data-ttu-id="e6ae4-147">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-147">Where:</span></span>

* <span data-ttu-id="e6ae4-148">hello mérete (ebben a példában A9) egyike Virtuálisgép-méretek hello RDMA-kompatibilisek-e.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-148">hello size (A9 in this example) is one of hello RDMA-capable VM sizes.</span></span>
* <span data-ttu-id="e6ae4-149">hello külső SSH-portszám (ebben a példában, amely hello SSH alapértelmezett 22) bármilyen érvényes portszámot.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-149">hello external SSH port number (22 in this example, which is hello SSH default) is any valid port number.</span></span> <span data-ttu-id="e6ae4-150">hello belső SSH-portszám too22 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-150">hello internal SSH port number is set too22.</span></span>
* <span data-ttu-id="e6ae4-151">Új felhőalapú szolgáltatás létrejön hello hello hely által megadott Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-151">A new cloud service is created in hello Azure region specified by hello location.</span></span> <span data-ttu-id="e6ae4-152">Adjon meg egy helyet, mely hello virtuális gép méretét a választott érhető el.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-152">Specify a location in which hello VM size you choose is available.</span></span>
* <span data-ttu-id="e6ae4-153">SUSE rangsorolási támogatással (amely további költségeket terhel), a SLES 12 SP1 hello kép neve jelenleg lehet ezek két lehetőség közül:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-153">For SUSE priority support (which incurs additional charges), hello SLES 12 SP1 image name currently can be one of these two options:</span></span> 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-hello-vm"></a><span data-ttu-id="e6ae4-154">Hello virtuális gép testreszabása</span><span class="sxs-lookup"><span data-stu-id="e6ae4-154">Customize hello VM</span></span>
<span data-ttu-id="e6ae4-155">Kiépítés hello VM befejezése után a virtuális gép SSH-toohello hello a virtuális gép külső IP-cím (vagy a DNS-név), és hello külső portszám konfigurálva, és ezután testre szabhatja.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-155">After hello VM finishes provisioning, SSH toohello VM by using hello VM's external IP address (or DNS name) and hello external port number you configured, and then customize it.</span></span> <span data-ttu-id="e6ae4-156">Kapcsolat részletekért lásd: [hogyan toolog a Linux rendszerű virtuális gép tooa](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e6ae4-156">For connection details, see [How toolog on tooa virtual machine running Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="e6ae4-157">Parancsok végrehajtása a virtuális gép, hello konfigurált hello felhasználóként, kivéve, ha legfelső szintű hozzáférés szükséges toocomplete egy lépést.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-157">Perform commands as hello user you configured on hello VM, unless root access is required toocomplete a step.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6ae4-158">A Microsoft Azure nem legfelső szintű hozzáférést biztosítanak tooLinux virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-158">Microsoft Azure does not provide root access tooLinux VMs.</span></span> <span data-ttu-id="e6ae4-159">toogain rendszergazdai hozzáféréssel, a felhasználó toohello VM, futtassa a parancsokat a csatlakozáskor `sudo`.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-159">toogain administrative access when connected as a user toohello VM, run commands by using `sudo`.</span></span>
>
>

* <span data-ttu-id="e6ae4-160">**Frissítések**: frissítések telepítése zypper használatával.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-160">**Updates**: Install updates by using zypper.</span></span> <span data-ttu-id="e6ae4-161">Érdemes tooinstall NFS segédprogramok is.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-161">You might also want tooinstall NFS utilities.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e6ae4-162">SLES 12 SP1 HPC virtuális gépen azt javasoljuk, hogy a rendszermag-frissítéseket, amelyek problémákat okozhatnak hello Linux RDMA illesztőprogramok nem alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-162">In a SLES 12 SP1 HPC VM, we recommend that you don't apply kernel updates, which can cause issues with hello Linux RDMA drivers.</span></span>
  >
  >
* <span data-ttu-id="e6ae4-163">**Intel MPI**: hello a következő parancs futtatásával végezze el a SLES 12 SP1 HPC VM hello hello Intel MPI telepítését:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-163">**Intel MPI**: Complete hello installation of Intel MPI on hello SLES 12 SP1 HPC VM by running hello following command:</span></span>

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* <span data-ttu-id="e6ae4-164">**Memóriát zárolni**: MPI kódok toolock hello számára rendelkezésre álló memória RDMA, hozzáadása vagy módosítása a következő beállításokat a hello /etc/security/limits.conf fájl hello.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-164">**Lock memory**: For MPI codes toolock hello memory available for RDMA, add or change hello following settings in hello /etc/security/limits.conf file.</span></span> <span data-ttu-id="e6ae4-165">Legfelső szintű hozzáférés tooedit kell ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-165">You need root access tooedit this file.</span></span>

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > <span data-ttu-id="e6ae4-166">Tesztelési célokra memlock toounlimited is beállíthat.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-166">For testing purposes, you can also set memlock toounlimited.</span></span> <span data-ttu-id="e6ae4-167">Például: `<User or group name>    hard    memlock unlimited`.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-167">For example: `<User or group name>    hard    memlock unlimited`.</span></span> <span data-ttu-id="e6ae4-168">További információkért lásd: [beállítás ismert legjobb módszerei zárolt memória mérete](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span><span class="sxs-lookup"><span data-stu-id="e6ae4-168">For more information, see [Best known methods for setting locked memory size](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span></span>
  >
  >
* <span data-ttu-id="e6ae4-169">**SLES virtuális gépek SSH-kulcsok**: készítése SSH kulcsok tooestablish megbízhatósági hello között a felhasználói fiókjához számítási hello SLES fürt csomópontja, MPI-feladatok futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-169">**SSH keys for SLES VMs**: Generate SSH keys tooestablish trust for your user account among hello compute nodes in hello SLES cluster when running MPI jobs.</span></span> <span data-ttu-id="e6ae4-170">Ha telepítette a HPC CentOS-alapú virtuális gépek, ne ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-170">If you deployed a CentOS-based HPC VM, don't follow this step.</span></span> <span data-ttu-id="e6ae4-171">Tudnivalókat később Ez a cikk tooset passwordless SSH megbízhatósági hello fürtcsomópontok között felfelé hello lemezképének és hello fürt központi telepítése után.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-171">See instructions later in this article tooset up passwordless SSH trust among hello cluster nodes after you capture hello image and deploy hello cluster.</span></span>

    <span data-ttu-id="e6ae4-172">toocreate SSH-kulcsok, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-172">toocreate SSH keys, run hello following command.</span></span> <span data-ttu-id="e6ae4-173">Amikor a bemeneti kéri, válassza ki **Enter** toogenerate hello kulcsok hello alapértelmezett helyen jelszó beállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-173">When you are prompted for input, select **Enter** toogenerate hello keys in hello default location without setting a password.</span></span>

        ssh-keygen

    <span data-ttu-id="e6ae4-174">A mellékletfájl hello nyilvános kulcs toohello authorized_keys ismert nyilvános kulcsok.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-174">Append hello public key toohello authorized_keys file for known public keys.</span></span>

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    <span data-ttu-id="e6ae4-175">Hello ~/.ssh könyvtárban szerkesztése vagy hello konfigurációs fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-175">In hello ~/.ssh directory, edit or create hello config file.</span></span> <span data-ttu-id="e6ae4-176">Adja meg, hogy tervezi-e az Azure (ebben a példában 10.32.0.0/16) toouse hello hello magánhálózati IP-címtartománya:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-176">Provide hello IP address range of hello private network that you plan toouse in Azure (10.32.0.0/16 in this example):</span></span>

        host 10.32.0.*
        StrictHostKeyChecking no

    <span data-ttu-id="e6ae4-177">Azt is megteheti listában hello magánhálózati IP-cím az egyes virtuális gépek a fürt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-177">Alternatively, list hello private network IP address of each VM in your cluster as follows:</span></span>

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > <span data-ttu-id="e6ae4-178">Konfigurálás `StrictHostKeyChecking no` biztonsági kockázatot hozhat létre, ha egy adott IP-cím vagy a tartomány nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-178">Configuring `StrictHostKeyChecking no` can create a potential security risk when a specific IP address or range is not specified.</span></span>
  >
  >
* <span data-ttu-id="e6ae4-179">**Alkalmazások**: telepítse a szükséges, vagy más testreszabás is szerepelt végrehajtása előtt hello lemezképének alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-179">**Applications**: Install any applications you need or perform other customizations before you capture hello image.</span></span>

### <a name="capture-hello-image"></a><span data-ttu-id="e6ae4-180">Hello lemezképének rögzítése</span><span class="sxs-lookup"><span data-stu-id="e6ae4-180">Capture hello image</span></span>
<span data-ttu-id="e6ae4-181">toocapture hello kép, először futtassa a következő parancsot a Linux virtuális gép hello hello.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-181">toocapture hello image, first run hello following command on hello Linux VM.</span></span> <span data-ttu-id="e6ae4-182">Ez a parancs hello VM deprovisions, de megőrzi a felhasználói fiókok és a beállított SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-182">This command deprovisions hello VM but maintains user accounts and SSH keys that you set up.</span></span>

```
sudo waagent -deprovision
```

<span data-ttu-id="e6ae4-183">Az ügyfélszámítógépen futtassa az Azure parancssori felület parancsai toocapture hello kép a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-183">From your client computer, run hello following Azure CLI commands toocapture hello image.</span></span> <span data-ttu-id="e6ae4-184">További információkért lásd: [hogyan toocapture képként klasszikus Linuxos virtuális gép](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="e6ae4-184">For more information, see [How toocapture a classic Linux virtual machine as an image](capture-image.md).</span></span>  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

<span data-ttu-id="e6ae4-185">Ezek a parancsok futtatása után hello Virtuálisgép-lemezkép rögzítése a használatra, és hello virtuális gép törlődik.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-185">After you run these commands, hello VM image is captured for your use and hello VM is deleted.</span></span> <span data-ttu-id="e6ae4-186">Most, hogy az egyéni lemezkép készen toodeploy egy fürt.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-186">Now you have your custom image ready toodeploy a cluster.</span></span>

### <a name="deploy-a-cluster-with-hello-image"></a><span data-ttu-id="e6ae4-187">Hello lemezképpel fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="e6ae4-187">Deploy a cluster with hello image</span></span>
<span data-ttu-id="e6ae4-188">A következő Bash parancsfájlok a környezetének megfelelő értékekkel hello módosítása, és futtassa az ügyfélszámítógépről.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-188">Modify hello following Bash script with appropriate values for your environment and run it from your client computer.</span></span> <span data-ttu-id="e6ae4-189">Azure virtuális hello Feladattervek hello klasszikus üzembe helyezési modellel telepíti, mert toodeploy hello nyolc A9 virtuális gépek ezt a parancsfájlt a javasolt néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-189">Because Azure deploys hello VMs serially in hello classic deployment model, it takes a few minutes toodeploy hello eight A9 VMs suggested in this script.</span></span>

```
#!/bin/bash -x
# Script toocreate a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where hello VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All hello compute-intensive instances need toobe in hello same cloud service for Linux RDMA toowork across InfiniBand.
# Note: hello current maximum number of VMs in a cloud service is 50. If you need tooprovision more than 50 VMs in hello same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want tooturn off external ports and use only internal ports toocommunicate between compute nodes via port 22, don’t use this option. Since port numbers up too10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 toocluster18. Specify your captured image in <image-name>. Specify hello username and password you used when creating hello SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment tooprovision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a><span data-ttu-id="e6ae4-190">A CentOS HPC-fürt szempontjai</span><span class="sxs-lookup"><span data-stu-id="e6ae4-190">Considerations for a CentOS HPC cluster</span></span>
<span data-ttu-id="e6ae4-191">Ha azt szeretné, hogy a HPC alapján egy hello CentOS alapú HPC lemezképet hello Azure piactér SLES 12 helyett a fürt tooset, kövesse a szakasz megelőző hello hello általános lépéseit.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-191">If you want tooset up a cluster based on one of hello CentOS-based HPC images in hello Azure Marketplace instead of SLES 12 for HPC, follow hello general steps in hello preceding section.</span></span> <span data-ttu-id="e6ae4-192">Megjegyzés: hello kiépítése és virtuális gép hello konfigurálása során a következő különbségek:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-192">Note hello following differences when you provision and configure hello VM:</span></span>

- <span data-ttu-id="e6ae4-193">Intel MPI már telepítve van a virtuális gép kiépítése a CentOS-alapú HPC-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-193">Intel MPI is already installed on a VM provisioned from a CentOS-based HPC image.</span></span>
- <span data-ttu-id="e6ae4-194">Zárolási memóriabeállításait már kerülnek hello VM /etc/security/limits.conf fájlban.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-194">Lock memory settings are already added in hello VM's /etc/security/limits.conf file.</span></span>
- <span data-ttu-id="e6ae4-195">SSH-kulcsok a virtuális gép kiépítése hello rögzítési nem hoznak létre.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-195">Do not generate SSH keys on hello VM you provision for capture.</span></span> <span data-ttu-id="e6ae4-196">Ajánlja felhasználói hitelesítés beállítása hello fürt telepítése után.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-196">Instead, we recommend setting up user-based authentication after you deploy hello cluster.</span></span> <span data-ttu-id="e6ae4-197">További információkért tekintse meg a következő szakasz hello.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-197">For more information, see hello following section.</span></span>  

### <a name="set-up-passwordless-ssh-trust-on-hello-cluster"></a><span data-ttu-id="e6ae4-198">Hello fürt passwordless SSH megbízhatóságának beállítása</span><span class="sxs-lookup"><span data-stu-id="e6ae4-198">Set up passwordless SSH trust on hello cluster</span></span>
<span data-ttu-id="e6ae4-199">A CentOS-alapú HPC-fürtben lévő hello számítási csomópontok közötti megbízhatósági kapcsolat létrehozásához két módszer áll rendelkezésre: állomás alapú hitelesítés és a felhasználó-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-199">On a CentOS-based HPC cluster, there are two methods for establishing trust between hello compute nodes: host-based authentication and user-based authentication.</span></span> <span data-ttu-id="e6ae4-200">Gazdagép-alapú hitelesítés Ez a cikk hello hatókörén kívül esik, és általában kell elvégezni egy bővítmény parancsfájl központi telepítése során.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-200">Host-based authentication is outside of hello scope of this article and generally must be done through an extension script during deployment.</span></span> <span data-ttu-id="e6ae4-201">Felhasználó-alapú hitelesítés előnyei a megbízható kapcsolat kialakítása a telepítés után, és megköveteli a hello generációs és az SSH-kulcsok között hello megosztásának számítási hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-201">User-based authentication is convenient for establishing trust after deployment and requires hello generation and sharing of SSH keys among hello compute nodes in hello cluster.</span></span> <span data-ttu-id="e6ae4-202">Ezt a módszert gyakran nevezik passwordless SSH-bejelentkezéskor, és futó MPI-feladatok esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-202">This method is commonly known as passwordless SSH login and is required when running MPI jobs.</span></span>

<span data-ttu-id="e6ae4-203">Egy minta parancsfájlt hello Közösségtől hozzájárult érhető el a [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable könnyen felhasználóhitelesítés CentOS alapú HPC-fürt.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-203">A sample script contributed from hello community is available on [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable easy user authentication on a CentOS-based HPC cluster.</span></span> <span data-ttu-id="e6ae4-204">Töltse le és használja ezt a parancsfájlt a lépéseket követve hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-204">Download and use this script by using hello following steps.</span></span> <span data-ttu-id="e6ae4-205">Módosítsa ezt a parancsfájlt vagy bármely más módszer tooestablish passwordless SSH hitelesítés hello számítási fürtcsomópontok között is.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-205">You can also modify this script or use any other method tooestablish passwordless SSH authentication between hello cluster compute nodes.</span></span>

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

<span data-ttu-id="e6ae4-206">toorun hello parancsfájl, az alhálózati IP-címekhez szüksége tooknow hello előtag.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-206">toorun hello script, you need tooknow hello prefix for your subnet IP addresses.</span></span> <span data-ttu-id="e6ae4-207">Hello előtagja lekéréséhez futtassa a parancsot követő hello fürtcsomópontok egyik hello.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-207">Get hello prefix by running hello following command on one of hello cluster nodes.</span></span> <span data-ttu-id="e6ae4-208">A kimeneti hasonlóan kell kinéznie 10.1.3.5, és hello előtag hello 10.1.3 részét.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-208">Your output should look something like 10.1.3.5, and hello prefix is hello 10.1.3 portion.</span></span>

    ifconfig eth0 | grep -w inet | awk '{print $2}'

<span data-ttu-id="e6ae4-209">Most futtassa a hello parancsfájl segítségével történő három paramétert: hello közös felhasználónév hello a számítási csomópontok, hello közös jelszót hello felhasználóhoz tartozó számítási csomópontokat, és hello alhálózati előtag, amelyek adott vissza hello előző parancsot.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-209">Now run hello script using three parameters: hello common user name on hello compute nodes, hello common password for that user on hello compute nodes, and hello subnet prefix that was returned from hello previous command.</span></span>

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

<span data-ttu-id="e6ae4-210">Ez a parancsfájl hello a következő:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-210">This script does hello following:</span></span>

* <span data-ttu-id="e6ae4-211">Létrehoz egy könyvtárat nevű .ssh, ami azonban szükséges az passwordless bejelentkezési hello állomás csomóponton.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-211">Creates a directory on hello host node named .ssh, which is required for passwordless login.</span></span>
* <span data-ttu-id="e6ae4-212">Mappában hozza létre a konfigurációs fájl hello .ssh, amely arra utasítja a passwordless bejelentkezési tooallow bejelentkezési hello fürt bármely csomópontján.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-212">Creates a configuration file in hello .ssh directory that instructs passwordless login tooallow login from any node in hello cluster.</span></span>
* <span data-ttu-id="e6ae4-213">Hello csomópont nevét és IP-címek csomópont hello fürt összes csomópontján hello tartalmazó fájlokat hozza létre.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-213">Creates files containing hello node names and node IP addresses for all hello nodes in hello cluster.</span></span> <span data-ttu-id="e6ae4-214">Ezek a fájlok megmaradnak a későbbi felhasználás hello parancsfájl futtatása után.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-214">These files are left after hello script is run for later reference.</span></span>
* <span data-ttu-id="e6ae4-215">A privát és nyilvános kulcsból álló kulcspárt a fürt minden csomópontján (beleértve a hello gazdacsomópont) hoz létre, és létrehozza a bejegyzéseket hello authorized_keys fájlba.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-215">Creates a private and public key pair for each cluster node (including hello host node) and creates entries in hello authorized_keys file.</span></span>

> [!WARNING]
> <span data-ttu-id="e6ae4-216">A parancsfájl futtatása, biztonsági kockázatot is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-216">Running this script can create a potential security risk.</span></span> <span data-ttu-id="e6ae4-217">Győződjön meg arról, hogy a nyilvános kulcs információja hello ~/.ssh nem terjesztése.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-217">Ensure that hello public key information in ~/.ssh is not distributed.</span></span>
>
>

## <a name="configure-intel-mpi"></a><span data-ttu-id="e6ae4-218">Intel MPI konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6ae4-218">Configure Intel MPI</span></span>
<span data-ttu-id="e6ae4-219">tooconfigure szüksége toorun MPI alkalmazások Azure Linux RDMA, bizonyos környezeti változók adott tooIntel MPI.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-219">toorun MPI applications on Azure Linux RDMA, you need tooconfigure certain environment variables specific tooIntel MPI.</span></span> <span data-ttu-id="e6ae4-220">Íme egy minta Bash parancsfájlok tooconfigure hello szükséges változók toorun kérelmet.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-220">Here is a sample Bash script tooconfigure hello variables needed toorun an application.</span></span> <span data-ttu-id="e6ae4-221">Hello elérési toompivars.sh Intel MPI-példány szükség esetén módosítása</span><span class="sxs-lookup"><span data-stu-id="e6ae4-221">Change hello path toompivars.sh as needed for your installation of Intel MPI.</span></span>

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting hello variable tooshm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line toorun hello job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path toohello application exe> <arguments specific toohello application>

#end
```

<span data-ttu-id="e6ae4-222">hello hello állomás fájl formátuma a következő.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-222">hello format of hello host file is as follows.</span></span> <span data-ttu-id="e6ae4-223">Adja hozzá az egyes csomópontok egy sort a fürtön.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-223">Add one line for each node in your cluster.</span></span> <span data-ttu-id="e6ae4-224">Adja meg a korábban, nem a DNS-nevek meghatározott privát IP-címek hello virtuális hálózatról.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-224">Specify private IP addresses from hello virtual network defined earlier, not DNS names.</span></span> <span data-ttu-id="e6ae4-225">Két gazdagépek 10.32.0.1 és 10.32.0.2 IP-címekkel rendelkező, például hello fájl hello következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-225">For example, for two hosts with IP addresses 10.32.0.1 and 10.32.0.2, hello file contains hello following:</span></span>

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a><span data-ttu-id="e6ae4-226">MPI futtatnak egy alapszintű két csomópontot tartalmazó fürtben</span><span class="sxs-lookup"><span data-stu-id="e6ae4-226">Run MPI on a basic two-node cluster</span></span>
<span data-ttu-id="e6ae4-227">Ha még nem tette meg, először hello környezet beállítása az Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-227">If you haven't already done so, first set up hello environment for Intel MPI.</span></span>

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a><span data-ttu-id="e6ae4-228">Egy MPI parancs futtatása</span><span class="sxs-lookup"><span data-stu-id="e6ae4-228">Run an MPI command</span></span>
<span data-ttu-id="e6ae4-229">Parancsot egy MPI hello számítási csomópontok tooshow MPI megfelelően van telepítve, és képes kommunikálni valamelyik legalább két számítási csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-229">Run an MPI command on one of hello compute nodes tooshow that MPI is installed properly and can communicate between at least two compute nodes.</span></span> <span data-ttu-id="e6ae4-230">hello következő **mpirun** parancs futtatása hello **állomásnév** két csomópont parancs.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-230">hello following **mpirun** command runs hello **hostname** command on two nodes.</span></span>

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
<span data-ttu-id="e6ae4-231">A kimenetében hello csomópontjaihoz bemenetként továbbított hello nevei `-hosts`.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-231">Your output should list hello names of all hello nodes that you passed as input for `-hosts`.</span></span> <span data-ttu-id="e6ae4-232">Például egy **mpirun** két csomóponttal rendelkező parancs kimenetét hasonló hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="e6ae4-232">For example, an **mpirun** command with two nodes returns output like hello following:</span></span>

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a><span data-ttu-id="e6ae4-233">Egy MPI teljesítményteszt futtatása</span><span class="sxs-lookup"><span data-stu-id="e6ae4-233">Run an MPI benchmark</span></span>
<span data-ttu-id="e6ae4-234">a következő Intel MPI parancs hello egy pingpong referenciaalap tooverify hello fürt konfigurációs és a kapcsolat toohello RDMA hálózati fut.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-234">hello following Intel MPI command runs a pingpong benchmark tooverify hello cluster configuration and connection toohello RDMA network.</span></span>

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

<span data-ttu-id="e6ae4-235">Működő rendelkező fürtön két csomópont hello hasonló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-235">On a working cluster with two nodes, you should see output like hello following.</span></span> <span data-ttu-id="e6ae4-236">Hello Azure RDMA hálózati késés, vagy az az üzenet-méretek mentése too512 bájt 3 ezredmásodperc alatt várható.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-236">On hello Azure RDMA network, expect latency at or below 3 microseconds for message sizes up too512 bytes.</span></span>

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
# hello number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected toobe exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through hello flag => -time

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
# List of Benchmarks toorun:
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



## <a name="next-steps"></a><span data-ttu-id="e6ae4-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6ae4-237">Next steps</span></span>
* <span data-ttu-id="e6ae4-238">Regisztrálhat és futtathat a Linux MPI alkalmazások a Linux-fürt.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-238">Deploy and run your Linux MPI applications on your Linux cluster.</span></span>
* <span data-ttu-id="e6ae4-239">Lásd: hello [Intel MPI Library dokumentációjában](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Intel MPI útmutatót.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-239">See hello [Intel MPI Library documentation](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) for guidance on Intel MPI.</span></span>
* <span data-ttu-id="e6ae4-240">Próbálja meg egy [gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate egy Intel fényesség fürt HPC CentOS-alapú lemezkép használatával.</span><span class="sxs-lookup"><span data-stu-id="e6ae4-240">Try a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate an Intel Lustre cluster by using a CentOS-based HPC image.</span></span> <span data-ttu-id="e6ae4-241">További információkért lásd: [Intel felhő Edition telepítését a Microsoft Azure fényesség](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="e6ae4-241">For details, see [Deploying Intel Cloud Edition for Lustre on Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span></span>
