---
title: "aaaUsing hello Azure Linux Docker Virtuálisgép-bővítménnyel"
description: "Docker és hello Azure virtuálisgép-bővítmények ismerteti és bemutatja, hogyan tooprogrammatically létre, amelyek a docker-gazdagépekből hello Azure parancssori felület használatával hello parancssorból Azure virtuális gépek."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: 1e192ad7c273aa9c997ea7bfa53b7de0b41a43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-from-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="2e9d4-103">Hello Docker Virtuálisgép-bővítmény a hello Azure parancssori felület (CLI) használatával</span><span class="sxs-lookup"><span data-stu-id="2e9d4-103">Using hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="2e9d4-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="2e9d4-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="2e9d4-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="2e9d4-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="2e9d4-107">Hello Resource Manager modellt hello Docker Virtuálisgép-bővítmény használatával kapcsolatos információkért lásd: [Itt](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2e9d4-107">For information about using hello Docker VM extension with hello Resource Manager model, see [here](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="2e9d4-108">Ez a témakör ismerteti, hogyan toocreate virtuális gép és a hello Docker Virtuálisgép-bővítmény hello szolgáltatás bármilyen platformon az Azure parancssori felület kezelési (asm) módban.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-108">This topic describes how toocreate a VM with hello Docker VM Extension from hello service management (asm) mode in Azure CLI on any platform.</span></span> <span data-ttu-id="2e9d4-109">[Docker](https://www.docker.com/) az hello egyik legnépszerűbb virtualizálási megközelítések használó [Linux tárolók](http://en.wikipedia.org/wiki/LXC) ahelyett, hogy a virtuális gépek adatforgalom elkülönítése, és a megosztott erőforrások számítástechnikai módja.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-109">[Docker](https://www.docker.com/) is one of hello most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="2e9d4-110">Hello Docker Virtuálisgép-bővítmény és hello [Azure Linux ügynök](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate egy tároló korlátlan számú tárolót tárolhat az Azure-alkalmazások Docker virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-110">You can use hello Docker VM extension and hello [Azure Linux Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate a Docker VM that hosts any number of containers for your applications on Azure.</span></span> <span data-ttu-id="2e9d4-111">toosee magas szintű információt a tárolók és azok előnyeit, lásd: hello [Docker magas szintű Faliújság](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="2e9d4-111">toosee a high-level discussion of containers and their advantages, see hello [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>

## <a name="how-toouse-hello-docker-vm-extension-with-azure"></a><span data-ttu-id="2e9d4-112">Hogyan toouse hello Docker Virtuálisgép-bővítmény az Azure-ral</span><span class="sxs-lookup"><span data-stu-id="2e9d4-112">How toouse hello Docker VM Extension with Azure</span></span>
<span data-ttu-id="2e9d4-113">toouse hello Docker Virtuálisgép-bővítmény az Azure-ral, telepítenie kell egy hello verziója [Azure parancssori felület](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) nagyobb, mint 0.8.6 (mivel az írás hello a jelenlegi verzió: 0.10.0-s).</span><span class="sxs-lookup"><span data-stu-id="2e9d4-113">toouse hello Docker VM extension with Azure, you must install a version of hello [Azure Command-Line Interface](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) higher than 0.8.6 (as of this writing hello current version is 0.10.0).</span></span> <span data-ttu-id="2e9d4-114">Telepítheti a hello Azure parancssori felület Mac, Linux és Windows rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-114">You can install hello Azure CLI on Mac, Linux, and Windows.</span></span>

<span data-ttu-id="2e9d4-115">hello teljes folyamat toouse Azure Docker felettébb egyszerű:</span><span class="sxs-lookup"><span data-stu-id="2e9d4-115">hello complete process toouse Docker on Azure is simple:</span></span>

* <span data-ttu-id="2e9d4-116">Hello Azure CLI és annak függőségeit, amelyből el kívánja toocontrol Azure hello számítógépre telepítse (Windows, ez lesz egy Linux-disztribúció virtuális gépként fut)</span><span class="sxs-lookup"><span data-stu-id="2e9d4-116">Install hello Azure CLI and its dependencies on hello computer from which you want toocontrol Azure (on Windows, this will be a Linux distribution running as a virtual machine)</span></span>
* <span data-ttu-id="2e9d4-117">Hello Azure CLI Docker parancsok toocreate egy virtuális gép Docker állomás használja az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="2e9d4-117">Use hello Azure CLI Docker commands toocreate a VM Docker host in Azure</span></span>
* <span data-ttu-id="2e9d4-118">Hello helyi Docker parancsok toomanage a Docker-tárolók használatára a Docker virtuális Gépet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-118">Use hello local Docker commands toomanage your Docker containers in your Docker VM in Azure.</span></span>

### <a name="install-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="2e9d4-119">Hello Azure parancssori felület (CLI) telepítése</span><span class="sxs-lookup"><span data-stu-id="2e9d4-119">Install hello Azure Command-Line Interface (Azure CLI)</span></span>
<span data-ttu-id="2e9d4-120">tooinstall és az Azure parancssori felület hello konfigurálása, lásd: [hogyan tooinstall hello Azure parancssori felület](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2e9d4-120">tooinstall and configure hello Azure CLI, see [How tooinstall hello Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span> <span data-ttu-id="2e9d4-121">tooconfirm hello telepítési típus `azure` hello parancssorban és rövid rövid időn belül kell megjelennie hello Azure CLI ASCII ábrákat, amely alapszintű hello sorolja fel a rendelkezésre álló tooyou parancsokat.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-121">tooconfirm hello installation, type `azure` at hello command prompt and after a short moment you should see hello Azure CLI ASCII art, which lists hello basic commands available tooyou.</span></span> <span data-ttu-id="2e9d4-122">Hello telepítési megfelelően működött, ha el tudja tootype `azure help vm` és felsorolt hello parancsok van-e "docker".</span><span class="sxs-lookup"><span data-stu-id="2e9d4-122">If hello installation worked correctly, you should be able tootype `azure help vm` and see that one of hello listed commands is "docker".</span></span>

> [!NOTE]
> <span data-ttu-id="2e9d4-123">Docker vannak a Windows-eszközök [Docker gép](https://docs.docker.com/installation/windows/), mely tooautomate hello létrehozása egy docker-ügyfél használható toowork Azure virtuális gépeken futó, docker-gazdagépekből is használható.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-123">Docker has tools for Windows, [Docker Machine](https://docs.docker.com/installation/windows/), which you can also use tooautomate hello creation of a docker client that you can use toowork with Azure VMs as docker hosts.</span></span>
> 
> 

### <a name="connect-hello-azure-cli-tootooyour-azure-account"></a><span data-ttu-id="2e9d4-124">Csatlakozás hello Azure CLI tootooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="2e9d4-124">Connect hello Azure CLI tootooyour Azure Account</span></span>
<span data-ttu-id="2e9d4-125">Hello Azure CLI használata előtt össze kell kapcsolnia az Azure fiókhitelesítő adatok hello Azure parancssori felület a platformon.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-125">Before you can use hello Azure CLI you must associate your Azure account credentials with hello Azure CLI on your platform.</span></span> <span data-ttu-id="2e9d4-126">szakasz hello [hogyan tooconnect tooyour Azure-előfizetés](../../../xplat-cli-connect.md) ismerteti, hogyan tooeither töltse le, és importálja a **.publishsettings** fájlt, vagy rendelje hozzá az Azure CLI szervezeti azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-126">hello section [How tooconnect tooyour Azure subscription](../../../xplat-cli-connect.md) explains how tooeither download and import your **.publishsettings** file or associate your Azure CLI with an organizational id.</span></span>

> [!NOTE]
> <span data-ttu-id="2e9d4-127">Néhány különbségek vannak viselkedés egy használatakor vagy hello más módszerekkel hitelesítési, így meg arról, hogy tooread hello dokumentum fenti toounderstand hello másképpen fog működni.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-127">There are some differences in behavior when using one or hello other methods of authentication, so do be sure tooread hello document above toounderstand hello different functionality.</span></span>
> 
> 

### <a name="install-docker-and-use-hello-docker-vm-extension-for-azure"></a><span data-ttu-id="2e9d4-128">Docker telepítéséhez és hello Docker Virtuálisgép-bővítmény használatához az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="2e9d4-128">Install Docker and use hello Docker VM Extension for Azure</span></span>
<span data-ttu-id="2e9d4-129">Hajtsa végre a hello [Docker telepítési utasításokat](https://docs.docker.com/installation/#installation) tooinstall Docker helyileg a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-129">Follow hello [Docker installation instructions](https://docs.docker.com/installation/#installation) tooinstall Docker locally on your computer.</span></span>

<span data-ttu-id="2e9d4-130">toouse Docker az Azure virtuális gépeket, a virtuális gép hello használt hello Linux kép rendelkeznie kell hello [Azure Linux Virtuálisgép-ügynök](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) telepítve.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-130">toouse Docker with an Azure Virtual Machine, hello Linux image used for hello VM must have hello [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) installed.</span></span> <span data-ttu-id="2e9d4-131">Jelenleg nincsenek lemezképekről Ez csak két típusa:</span><span class="sxs-lookup"><span data-stu-id="2e9d4-131">Currently, there are only two types of images that provide this:</span></span>

* <span data-ttu-id="2e9d4-132">Az Ubuntu lemezképének hello Azure kép gyűjtemény vagy</span><span class="sxs-lookup"><span data-stu-id="2e9d4-132">An Ubuntu image from hello Azure Image Gallery or</span></span>
* <span data-ttu-id="2e9d4-133">Egy egyéni Linux lemezképnek, amely a korábban létrehozott hello Azure Linux Virtuálisgép-ügynök telepítve és konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-133">A custom Linux image that you have created with hello Azure Linux VM Agent installed and configured.</span></span> <span data-ttu-id="2e9d4-134">Lásd: [Azure Linux Virtuálisgép-ügynök](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt toobuild egyéni Linux virtuális gép és hello Azure Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-134">See [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about how toobuild a custom Linux VM with hello Azure VM Agent.</span></span>

### <a name="using-hello-azure-image-gallery"></a><span data-ttu-id="2e9d4-135">Hello Azure lemezkép-katalógus használatával</span><span class="sxs-lookup"><span data-stu-id="2e9d4-135">Using hello Azure Image Gallery</span></span>
<span data-ttu-id="2e9d4-136">Bash vagy terminál-munkamenetet használja az Azure CLI parancs toolocate hello VM gyűjtemény toouse legutóbbi Ubuntu lemezképet hello írja be a következő hello</span><span class="sxs-lookup"><span data-stu-id="2e9d4-136">From a Bash or Terminal session, use hello following Azure CLI command toolocate hello most recent Ubuntu image in hello VM gallery toouse by typing</span></span>

`azure vm image list | grep Ubuntu-14_04`

<span data-ttu-id="2e9d4-137">Válassza ki valamelyik hello képek nevének, például a `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, és használjon hello következő parancsot a toocreate egy új virtuális Gépet, hogy a lemezkép használatával.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-137">and select one of hello image names, such as `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, and use hello following command toocreate a new VM using that image.</span></span>

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

<span data-ttu-id="2e9d4-138">Ahol:</span><span class="sxs-lookup"><span data-stu-id="2e9d4-138">where:</span></span>

* <span data-ttu-id="2e9d4-139">*&lt;VM-cloudservice neve&gt;*  hello hello lesz hello Docker tároló állomására Azure virtuális gép neve</span><span class="sxs-lookup"><span data-stu-id="2e9d4-139">*&lt;vm-cloudservice name&gt;* is hello name of hello VM that will become hello Docker container host computer in Azure</span></span>
* <span data-ttu-id="2e9d4-140">*&lt;felhasználónév&gt;*  hello alapértelmezett legfelső szintű felhasználó a virtuális gép hello hello felhasználónév</span><span class="sxs-lookup"><span data-stu-id="2e9d4-140">*&lt;username&gt;* is hello username of hello default root user of hello VM</span></span>
* <span data-ttu-id="2e9d4-141">*&lt;jelszó&gt;*  hello jelszó a hello *felhasználónév* szabványoknak hello összetettségi az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="2e9d4-141">*&lt;password&gt;* is hello password of hello *username* account that meets hello standards of complexity for Azure</span></span>

> [!NOTE]
> <span data-ttu-id="2e9d4-142">Jelenleg a jelszót kell legalább 8 karakterből álljon, egy kisbetű és egy nagybetű, szám és egy, a következő karaktereket hello egy speciális karaktert tartalmaz: `!@#$%^&+=`.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-142">Currently, a password must be at least 8 characters, contain one lower case and one upper case character, a number, and a special character such as one of hello following characters: `!@#$%^&+=`.</span></span> <span data-ttu-id="2e9d4-143">Nem, hello időszaka mondat megelőző hello hello végén nincs egy speciális karaktert.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-143">No, hello period at hello end of hello preceding sentence is NOT a special character.</span></span>
> 
> 

<span data-ttu-id="2e9d4-144">Ha hello parancs sikeres volt, akkor kell kinéznie: hello következő, attól függően, hogy hello pontos argumentumok és használt beállítások</span><span class="sxs-lookup"><span data-stu-id="2e9d4-144">If hello command was successful, you should see something like hello following, depending on hello precise arguments and options you used:</span></span>

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> <span data-ttu-id="2e9d4-145">A virtuális gépek létrehozására is igénybe vehet néhány percet, de után van kiépítve (hello állapot értéke `ReadyRole`) hello Docker démon (hello Docker szolgáltatás) kezdődik, és csatlakozhat a Docker-tároló toohello-gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-145">Creating a virtual machine can take a few minutes, but after it has been provisioned (hello state value is `ReadyRole`) hello Docker daemon (hello Docker service) starts and you can connect toohello Docker container host.</span></span>
> 
> 

<span data-ttu-id="2e9d4-146">tootest hello típusa, az Azure-ban létrehozott Docker VM</span><span class="sxs-lookup"><span data-stu-id="2e9d4-146">tootest hello Docker VM you have created in Azure, type</span></span>

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

<span data-ttu-id="2e9d4-147">Ha  *&lt;vm-neve--használt&gt;*  hello hello virtuális gép nevét, a hívás túl használt van`azure vm docker create`.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-147">where *&lt;vm-name-you-used&gt;* is hello name of hello virtual machine that you used in your call too`azure vm docker create`.</span></span> <span data-ttu-id="2e9d4-148">Valami hasonló toohello következő, az azt jelzi, hogy a Docker állomás virtuális gép mentése Azure-ban futó és a parancsok Várakozás kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-148">You should see something similar toohello following, which indicates that your Docker Host VM is up and running in Azure and waiting for your commands.</span></span> 

<span data-ttu-id="2e9d4-149">Most megpróbálhatja tooconnect a docker ügyfél tooobtain információk segítségével (az egyes Docker ügyfél beállítások, például a Mac, előfordulhat, hogy toouse `sudo`):</span><span class="sxs-lookup"><span data-stu-id="2e9d4-149">Now you can try tooconnect using your docker client tooobtain information (in some Docker client setups, such as that on Mac, you may have toouse `sudo`):</span></span>

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

<span data-ttu-id="2e9d4-150">Csak toobe, hogy szerepel-e az összes működő, a hello Docker-bővítményt a virtuális gép hello láthatja:</span><span class="sxs-lookup"><span data-stu-id="2e9d4-150">Just toobe certain that it's all working, you can examine hello VM for hello Docker extension:</span></span>

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a><span data-ttu-id="2e9d4-151">Docker gazdagép VM hitelesítés</span><span class="sxs-lookup"><span data-stu-id="2e9d4-151">Docker Host VM Authentication</span></span>
<span data-ttu-id="2e9d4-152">Ezenkívül toocreating hello Docker VM hello `azure vm docker create` parancs automatikusan is hoz létre hello szükséges tanúsítványok tooallow a Docker ügyfél számítógép tooconnect toohello Azure tároló-gazdagépen HTTPS-kapcsolaton keresztül, és hello tanúsítványokat is tárolja hello ügyfél és a gazdagép gépnek, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-152">In addition toocreating hello Docker VM, hello `azure vm docker create` command also automatically creates hello necessary certificates tooallow your Docker client computer tooconnect toohello Azure container host using HTTPS, and hello certificates are stored on both hello client and host machines, as appropriate.</span></span> <span data-ttu-id="2e9d4-153">Az ezt követő kísérletek hello meglévő újra felhasználható és tanúsítványokat hello új állomás megosztva.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-153">On subsequent attempts, hello existing certificates are reused and shared with hello new host.</span></span>

<span data-ttu-id="2e9d4-154">Alapértelmezés szerint tanúsítványok kerülnek `~/.docker`, és a Docker lesz portra beállított toorun **2376**.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-154">By default, certificates are placed in `~/.docker`, and Docker will be configured toorun on port **2376**.</span></span> <span data-ttu-id="2e9d4-155">Ha szeretné toouse egy másik portot vagy a könyvtárat, majd segítségével hello következő `azure vm docker create` parancssori beállítások tooconfigure a Docker tároló gazdagép VM toouse egy másik portot vagy különböző tanúsítványok csatlakozó ügyfeleken:</span><span class="sxs-lookup"><span data-stu-id="2e9d4-155">If you would like toouse a different port or directory, then you may use one of hello following `azure vm docker create` command line options tooconfigure your Docker container host VM toouse a different port or different certificates for connecting clients:</span></span>

```
-dp, --docker-port [port]              Port toouse for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

<span data-ttu-id="2e9d4-156">hello hello gazdagépen Docker démon a konfigurált toolisten és hello kapcsolatok megadott port hello tanúsítványok használatával hello által generált ügyfél hitelesítésére `azure vm docker create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-156">hello Docker daemon on hello host is configured toolisten for and authenticate client connections on hello specified port using hello certificates generated by hello `azure vm docker create` command.</span></span> <span data-ttu-id="2e9d4-157">hello ügyfélszámítógép ezen tanúsítványok toogain hozzáférés toohello Docker állomással kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-157">hello client machine must have these certificates toogain access toohello Docker host.</span></span>

> [!NOTE]
> <span data-ttu-id="2e9d4-158">Hálózati futtató gazdagépet, amelyen ezek a tanúsítványok nélkül lesz téve tooanyone tooconnect toohello számítógép által.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-158">A networked host running without these certificates will be vulnerable tooanyone that can tooconnect toohello machine.</span></span> <span data-ttu-id="2e9d4-159">Hello alapértelmezett konfiguráció módosítása előtt győződjön meg arról, hogy tudomásul veszi hello kockázatok tooyour számítógépeknek és alkalmazásoknak.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-159">Before you modify hello default configuration, ensure that you understand hello risks tooyour computers and applications.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2e9d4-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e9d4-160">Next steps</span></span>
* <span data-ttu-id="2e9d4-161">Készen áll a toogo toohello áll [Docker felhasználói útmutató] és a Docker virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-161">You are ready toogo toohello [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="2e9d4-162">egy Docker-kompatibilis virtuális Gépet az új portálon hello toocreate lásd [hogyan toouse hello-Docker Virtuálisgép-bővítmény hello Portal].</span><span class="sxs-lookup"><span data-stu-id="2e9d4-162">toocreate a Docker-enabled VM in hello new portal, see [How toouse hello Docker VM Extension with hello Portal].</span></span>
* <span data-ttu-id="2e9d4-163">hello Azure Docker Virtuálisgép-bővítmény is támogatja a Docker Compose, teljes bármely környezet egy deklaratív YAM-fájl tootake egy fejlesztői modellezve alkalmazást használó, és egy egységes központi telepítés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2e9d4-163">hello Azure Docker VM extension also supports Docker Compose, which uses a declarative YAML file tootake a developer-modeled application across any environment and generate a consistent deployment.</span></span> <span data-ttu-id="2e9d4-164">Lásd: [Ismerkedés a Docker és toodefine összeállítása, és futtassa a több tároló alkalmazást egy Azure virtuális gépen a].</span><span class="sxs-lookup"><span data-stu-id="2e9d4-164">See [Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine].</span></span>  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How toouse hello Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 tooanother azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 tooanother azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md
[hogyan toouse hello-Docker Virtuálisgép-bővítmény hello Portal]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker felhasználói útmutató]:https://docs.docker.com/userguide/

[Ismerkedés a Docker és toodefine összeállítása, és futtassa a több tároló alkalmazást egy Azure virtuális gépen a]:../docker-compose-quickstart.md
