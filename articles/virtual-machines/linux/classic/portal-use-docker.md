---
title: "Linux Virtuálisgép-bővítmény Docker aaaUsing |} Microsoft Docs"
description: "Docker és hello Azure virtuálisgép-bővítmények és hogyan toocreate Azure virtuális gépek, amelyek segítségével docker-gazdagépekből hello klasszikus üzembe helyezési modellel Azure CLI ismerteti."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
ms.openlocfilehash: 9d455b63c48b0c1b6f14862e072f899a73b46153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-with-hello-azure-classic-portal"></a><span data-ttu-id="53b49-103">Hello Docker Virtuálisgép-bővítmény hello klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="53b49-103">Using hello Docker VM Extension with hello Azure classic portal</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="53b49-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="53b49-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="53b49-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="53b49-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="53b49-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="53b49-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="53b49-107">[Docker](https://www.docker.com/) az hello egyik legnépszerűbb virtualizálási megközelítések használó [Linux tárolók](http://en.wikipedia.org/wiki/LXC) ahelyett, hogy a virtuális gépek adatforgalom elkülönítése, és a megosztott erőforrások számítástechnikai módja.</span><span class="sxs-lookup"><span data-stu-id="53b49-107">[Docker](https://www.docker.com/) is one of hello most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="53b49-108">Használhatja a hello Docker Virtuálisgép-bővítmény kezeli [Azure Linux ügynök] toocreate egy tároló korlátlan számú tárolót tárolhat az Azure-alkalmazások Docker virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="53b49-108">You can use hello Docker VM extension managed by [Azure Linux Agent] toocreate a Docker VM that hosts any number of containers for your applications on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="53b49-109">Ez a témakör ismerteti, hogyan toocreate a virtuális gép Docker hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="53b49-109">This topic describes how toocreate a Docker VM from hello Azure classic portal.</span></span> <span data-ttu-id="53b49-110">Hogyan toocreate egy Docker-VM hello parancssorból: toosee [hogyan toouse hello hello Azure parancssori felület (CLI) a Virtuálisgép-bővítmény Docker].</span><span class="sxs-lookup"><span data-stu-id="53b49-110">toosee how toocreate a Docker VM at hello command line, see [How toouse hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)].</span></span> <span data-ttu-id="53b49-111">toosee magas szintű információt a tárolók és azok előnyeit, lásd: hello [Docker magas szintű Faliújság](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="53b49-111">toosee a high-level discussion of containers and their advantages, see hello [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>
> 
> 

## <a name="create-a-new-vm-from-hello-image-gallery"></a><span data-ttu-id="53b49-112">Új virtuális gép létrehozása a hello kép gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="53b49-112">Create a new VM from hello Image Gallery</span></span>
<span data-ttu-id="53b49-113">hello első lépése az Azure virtuális gép, amely támogatja a Docker Virtuálisgép-bővítmény, az Ubuntu 14.04 LTS lemezképének kép gyűjtemény hello használata egy példa kiszolgálói lemezképet és az Ubuntu 14.04 asztali ügyfélként hello Linux lemezképéről igényel.</span><span class="sxs-lookup"><span data-stu-id="53b49-113">hello first step requires an Azure VM from a Linux image that supports hello Docker VM Extension, using an Ubuntu 14.04 LTS image from hello Image Gallery as an example server image and Ubuntu 14.04 Desktop as a client.</span></span> <span data-ttu-id="53b49-114">Hello portálon kattintson **+ új** hello az alsó bal sarok toocreate egy új Virtuálisgép-példány, és jelöljön ki egy Ubuntu 14.04 LTS képet hello választható vagy hello befejezéséhez kép gyűjteménye, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="53b49-114">In hello portal, click **+ New** in hello bottom left corner toocreate a new VM instance and select an Ubuntu 14.04 LTS image from hello selections available or from hello complete Image Gallery, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="53b49-115">Jelenleg csak Ubuntu 14.04 LTS lemezképek újabb a 2014. július támogatja a Virtuálisgép-bővítmény Docker hello.</span><span class="sxs-lookup"><span data-stu-id="53b49-115">Currently, only Ubuntu 14.04 LTS images more recent than July 2014 support hello Docker VM Extension.</span></span>
> 
> 

![Hozzon létre egy új Ubuntu kép](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a><span data-ttu-id="53b49-117">Docker tanúsítványok létrehozása</span><span class="sxs-lookup"><span data-stu-id="53b49-117">Create Docker Certificates</span></span>
<span data-ttu-id="53b49-118">Virtuális gép létrehozása után hello, győződjön meg arról, hogy Docker telepítve van-e az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="53b49-118">After hello VM has been created, ensure that Docker is installed on your client computer.</span></span> <span data-ttu-id="53b49-119">(További információkért lásd: [Docker telepítési utasításokat](https://docs.docker.com/installation/#installation).)</span><span class="sxs-lookup"><span data-stu-id="53b49-119">(For details, see [Docker installation instructions](https://docs.docker.com/installation/#installation).)</span></span>

<span data-ttu-id="53b49-120">Hozzon létre hello szerint túl Docker kommunikációs tanúsítványt és kulcsot fájlok[Docker fut, a https] és helyezze el őket hello  **`~/.docker`**  könyvtárhoz, az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="53b49-120">Create hello certificate and key files for Docker communication according too[Running Docker with https] and place them in hello **`~/.docker`** directory on your client computer.</span></span>

> [!NOTE]
> <span data-ttu-id="53b49-121">hello Docker Virtuálisgép-bővítmény hello portálon jelenleg szükséges hitelesítő adatokat, amelyek base64-kódolású.</span><span class="sxs-lookup"><span data-stu-id="53b49-121">hello Docker VM Extension in hello portal currently requires credentials that are base64 encoded.</span></span>
> 
> 

<span data-ttu-id="53b49-122">Hello parancssorból használható  **`base64`**  vagy egy másik kedvenc eszköz toocreate base64-kódolású témakörök kódolást.</span><span class="sxs-lookup"><span data-stu-id="53b49-122">At hello command line, use **`base64`** or another favorite encoding tool toocreate base64-encoded topics.</span></span> <span data-ttu-id="53b49-123">Ezzel a tanúsítvány és kulcs fájlok egyszerű számú hasonló toothis nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="53b49-123">Doing this with a simple set of certificate and key files might look similar toothis:</span></span>

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-hello-docker-vm-extension"></a><span data-ttu-id="53b49-124">Hello Docker Virtuálisgép-bővítmény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="53b49-124">Add hello Docker VM Extension</span></span>
<span data-ttu-id="53b49-125">tooadd hello Docker Virtuálisgép-bővítmény, keresse meg a létrehozott hello Virtuálisgép-példány, és görgessen lefelé, túl**bővítmények** , és kattintson rá a Virtuálisgép-bővítmények mentése toobring alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="53b49-125">tooadd hello Docker VM Extension, locate hello VM instance you created and scroll down too**Extensions** and click it toobring up VM Extensions, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="53b49-126">Ez a funkció csak a hello betekintő portálon támogatott: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="53b49-126">This functionality is supported in hello preview portal only: https://portal.azure.com/</span></span>
> 
> 

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a><span data-ttu-id="53b49-127">Egy bővítmény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="53b49-127">Add an Extension</span></span>
<span data-ttu-id="53b49-128">Kattintson a hello **+ Hozzáadás** toodisplay hello lehetséges Virtuálisgép-bővítmények toothis VM is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="53b49-128">Click hello **+ Add** toodisplay hello possible VM Extensions you can add toothis VM.</span></span>

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-hello-docker-vm-extension"></a><span data-ttu-id="53b49-129">Válassza ki a hello Docker Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="53b49-129">Select hello Docker VM Extension</span></span>
<span data-ttu-id="53b49-130">Válassza ki a hello Docker Virtuálisgép-bővítmény, amely hello Docker leírás és fontos hivatkozásokat, és kattintson a **létrehozása** : hello alsó toobegin hello telepítési eljárást.</span><span class="sxs-lookup"><span data-stu-id="53b49-130">Choose hello Docker VM Extension, which brings up hello Docker description and important links, and then click **Create** at hello bottom toobegin hello installation procedure.</span></span>

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a><span data-ttu-id="53b49-131">A tanúsítvány és kulcs fájlok hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="53b49-131">Add your Certificate and Key Files:</span></span>
<span data-ttu-id="53b49-132">A hello mezőket adja meg hello base64-kódolású verziói a Hitelesítésszolgáltatói tanúsítvány, a kiszolgálói tanúsítvány és a kiszolgáló kulcs, ahogy az ábra a következő hello.</span><span class="sxs-lookup"><span data-stu-id="53b49-132">In hello form fields, enter hello base64-encoded versions of your CA Certificate, your Server Certificate, and your Server Key, as shown in hello following graphic.</span></span>

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> <span data-ttu-id="53b49-133">Vegye figyelembe, hogy (ahogy kép megelőző hello) hello 2376 ki van töltve alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="53b49-133">Note that (as in hello preceding image) hello 2376 is filled in by default.</span></span> <span data-ttu-id="53b49-134">Bármely végpont Itt adhat meg, de hello tovább tooopen megfelelő végpont hello fel.</span><span class="sxs-lookup"><span data-stu-id="53b49-134">You can enter any endpoint here, but hello next step will be tooopen up hello matching endpoint.</span></span> <span data-ttu-id="53b49-135">Hello alapértelmezett módosítása esetén győződjön meg arról, hogy tooopen megfelelő végpont a következő lépésben hello hello fel.</span><span class="sxs-lookup"><span data-stu-id="53b49-135">If you change hello default, make sure tooopen up hello matching endpoint in hello next step.</span></span>
> 
> 

## <a name="add-hello-docker-communication-endpoint"></a><span data-ttu-id="53b49-136">Hello Docker kommunikációs végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="53b49-136">Add hello Docker Communication Endpoint</span></span>
<span data-ttu-id="53b49-137">Korábban létrehozott erőforráscsoportot hello megtekintésekor válassza ki a virtuális Géphez társított hálózati biztonsági csoport hello, majd kattintson **bejövő biztonsági szabályok** tooview hello szabályok itt látható módon.</span><span class="sxs-lookup"><span data-stu-id="53b49-137">When viewing hello resource group you've created, select hello Network Security Group associated with your VM, and click **Inbound Security Rules** tooview hello rules as shown here.</span></span>

![](media/portal-use-docker/AddingEndpoint.png)

<span data-ttu-id="53b49-138">Kattintson a **+ Hozzáadás** tooadd egy másik szabály, és hello alapértelmezett esetben adja meg a hello végpont nevét (ebben a példában **Docker**), és 2376 "Célporttartomány".</span><span class="sxs-lookup"><span data-stu-id="53b49-138">Click **+ Add** tooadd another rule, and in hello default case, enter a name for hello endpoint (in this example, **Docker**), and 2376 'Destination Port Range'.</span></span> <span data-ttu-id="53b49-139">Állítsa be a hello protokoll értéket megjelenítő **TCP**, és kattintson a **OK** toocreate hello szabály.</span><span class="sxs-lookup"><span data-stu-id="53b49-139">Set hello protocol value showing **TCP**, and Click **OK** toocreate hello rule.</span></span>

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a><span data-ttu-id="53b49-140">A Docker-ügyfél és az Azure Docker állomás tesztelése</span><span class="sxs-lookup"><span data-stu-id="53b49-140">Test your Docker Client and Azure Docker Host</span></span>
<span data-ttu-id="53b49-141">Keresse meg és másolja a virtuális gép tartomány, valamint a parancssorból hello az ügyfélszámítógép típus hello neve `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (ahol *dockerextension* hello helyébe altartomány a virtuális gép).</span><span class="sxs-lookup"><span data-stu-id="53b49-141">Locate and copy hello name of your VM's domain, and at hello command line of your client computer, type `docker --tls -H tcp://`*dockerextension*`.cloudapp.net:2376 info` (where *dockerextension* is replaced by hello subdomain for your VM).</span></span>

<span data-ttu-id="53b49-142">hello eredmény hasonló toothis kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="53b49-142">hello result should appear similar toothis:</span></span>

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

<span data-ttu-id="53b49-143">Ha befejezte a fenti lépéseket hello, most már rendelkezik egy Azure virtuális gépen, konfigurált futtató teljesen működőképes Docker állomás toobe tooremotely csatlakoztatott egyéb ügyfelekről.</span><span class="sxs-lookup"><span data-stu-id="53b49-143">Once you complete hello above steps, you now have a fully functional Docker host running on an Azure VM, configured toobe connected tooremotely from other clients.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="53b49-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53b49-144">Next steps</span></span>
<span data-ttu-id="53b49-145">Készen áll a toogo toohello áll [Docker felhasználói útmutató] és a Docker virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="53b49-145">You are ready toogo toohello [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="53b49-146">Ha azt szeretné, hogy a Docker-gazdagépekből létrehozása az Azure virtuális gépeken futó parancssori felületen keresztül tooautomate, [hogyan toouse hello hello Azure parancssori felület (CLI) a Virtuálisgép-bővítmény Docker]</span><span class="sxs-lookup"><span data-stu-id="53b49-146">If you want tooautomate creating Docker hosts on Azure VMs through command line interface, see [How toouse hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)]</span></span>

<!--Anchors-->
[Create a new VM from hello Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add hello Docker VM Extension]:#adddockerextension
[Test Docker Client and Azure Docker Host]:#testclientandserver
[Next steps]:#next-steps

<!--Image references-->
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[6]:./media/markdown-template-for-new-articles/pretty49.png
[7]:./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[hogyan toouse hello hello Azure parancssori felület (CLI) a Virtuálisgép-bővítmény Docker]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux ügynök]:../agent-user-guide.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md

[Docker fut, a https]:http://docs.docker.com/articles/https/
[Docker felhasználói útmutató]:https://docs.docker.com/userguide/
