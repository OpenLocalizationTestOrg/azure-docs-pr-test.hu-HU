---
title: "az Azure-ban üzemeltető aaaUse Docker gép toocreate Linux |} Microsoft Docs"
description: "Ismerteti, hogyan toouse Docker gép toocreate Docker futtatja az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: iainfou
ms.openlocfilehash: 905c645add51c78305aac85a7013441b3a73972f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-docker-machine-toocreate-hosts-in-azure"></a><span data-ttu-id="6b1f1-103">Hogyan toouse Docker gép toocreate futtatja az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6b1f1-103">How toouse Docker Machine toocreate hosts in Azure</span></span>
<span data-ttu-id="6b1f1-104">Ez a következő cikket: részletek hogyan toouse [Docker gép](https://docs.docker.com/machine/) toocreate gazdagépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-104">This article details how toouse [Docker Machine](https://docs.docker.com/machine/) toocreate hosts in Azure.</span></span> <span data-ttu-id="6b1f1-105">Hello `docker-machine` parancs létrehoz egy Linux virtuális gép (VM) az Azure-ban, majd telepíti a Docker.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-105">hello `docker-machine` command creates a Linux virtual machine (VM) in Azure then installs Docker.</span></span> <span data-ttu-id="6b1f1-106">A Docker-gazdagépekből Azure használatával kezelhetők hello helyi eszközök és munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-106">You can then manage your Docker hosts in Azure using hello same local tools and workflows.</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="6b1f1-107">Hozzon létre virtuális gépek Docker gép</span><span class="sxs-lookup"><span data-stu-id="6b1f1-107">Create VMs with Docker Machine</span></span>
<span data-ttu-id="6b1f1-108">Először szerezze be az Azure-előfizetése Azonosítóját [az fiók megjelenítése](/cli/azure/account#show) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-108">First, obtain your Azure subscription ID with [az account show](/cli/azure/account#show) as follows:</span></span>

```azurecli
sub=$(az account show --query "id" -o tsv)
```

<span data-ttu-id="6b1f1-109">Létrehozott egy Docker állomás virtuális gépet az Azure-ban `docker-machine create` megadásával *azure* hello illesztőprogramként.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-109">You create Docker host VMs in Azure with `docker-machine create` by specifying *azure* as hello driver.</span></span> <span data-ttu-id="6b1f1-110">További információkért lásd: hello [Docker Azure illesztőprogram dokumentáció](https://docs.docker.com/machine/drivers/azure/)</span><span class="sxs-lookup"><span data-stu-id="6b1f1-110">For more information, see hello [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/)</span></span>

<span data-ttu-id="6b1f1-111">hello alábbi példa létrehoz egy nevű virtuális gép *myVM*, létrehoz egy felhasználói fiókot nevű *azureuser*, és a portot nyit meg *80* hello gazdagép-virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-111">hello following example creates a VM named *myVM*, creates a user account named *azureuser*, and opens port *80* on hello host VM.</span></span> <span data-ttu-id="6b1f1-112">Bármely kér toolog az Azure-fiók tooyour kövesse és Docker gép engedélyek toocreate megadni, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-112">Follow any prompts toolog in tooyour Azure account and grant Docker Machine permissions toocreate and manage resources.</span></span>

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    myvm
```

<span data-ttu-id="6b1f1-113">hello kimenete a következő példa hasonló toohello néz ki:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-113">hello output looks similar toohello following example:</span></span>

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvmdocker) Completed machine pre-create checks.
Creating machine...
(myvmdocker) Querying existing resource group.  name="docker-machine"
(myvmdocker) Creating resource group.  name="docker-machine" location="westus"
(myvmdocker) Configuring availability set.  name="docker-machine"
(myvmdocker) Configuring network security group.  name="myvmdocker-firewall" location="westus"
(myvmdocker) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvmdocker) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvmdocker) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvmdocker) Creating public IP address.  name="myvmdocker-ip" static=false
(myvmdocker) Creating network interface.  name="myvmdocker-nic"
(myvmdocker) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvmdocker) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvmdocker"
Waiting for machine toobe running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH toobe available...
Detecting hello provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs toohello local machine directory...
Copying certs toohello remote machine...
Setting Docker configuration on hello remote daemon...
Checking connection tooDocker...
Docker is up and running!
toosee how tooconnect your Docker Client toohello Docker Engine running on this virtual machine, run: docker-machine env myvmdocker
```

## <a name="configure-your-docker-shell"></a><span data-ttu-id="6b1f1-114">A Docker rendszerhéj konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6b1f1-114">Configure your Docker shell</span></span>
<span data-ttu-id="6b1f1-115">tooconnect tooyour Docker állomás Azure hello megfelelő kapcsolat beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-115">tooconnect tooyour Docker host in Azure, define hello appropriate connection settings.</span></span> <span data-ttu-id="6b1f1-116">Amint hello kimeneti hello végén, tekintse meg hello kapcsolat a Docker-állomás az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-116">As noted at hello end of hello output, view hello connection information for your Docker host as follows:</span></span> 

```bash
docker-machine env myvmdocker
```

<span data-ttu-id="6b1f1-117">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-117">hello output is similar toohello following example:</span></span>

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command tooconfigure your shell:
# eval $(docker-machine env myvmdocker)
```

<span data-ttu-id="6b1f1-118">toodefine hello kapcsolatbeállítások vagy futtatási hello akkor javasolt konfiguráció parancs (`eval $(docker-machine env myvmdocker)`), vagy manuálisan állíthatja be a hello környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-118">toodefine hello connection settings you can either run hello suggested configuration command (`eval $(docker-machine env myvmdocker)`), or you can set hello environment variables manually.</span></span> 

## <a name="run-a-container"></a><span data-ttu-id="6b1f1-119">Egy tároló futtatása</span><span class="sxs-lookup"><span data-stu-id="6b1f1-119">Run a container</span></span>
<span data-ttu-id="6b1f1-120">toosee egy tárolót a művelet lehetővé teszi, hogy a Futtatás alapvető NGINX-webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-120">toosee a container in action, lets run a basic NGINX webserver.</span></span> <span data-ttu-id="6b1f1-121">Hozzon létre egy tárolót a `docker run` és tegye elérhetővé a webes forgalom 80-as porton az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-121">Create a container with `docker run` and expose port 80 for web traffic as follows:</span></span>

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="6b1f1-122">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-122">hello output is similar toohello following example:</span></span>

```bash
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

<span data-ttu-id="6b1f1-123">Tárolók futtató nézet `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-123">View running containers with `docker ps`.</span></span> <span data-ttu-id="6b1f1-124">hello következő egy példa a kimenetre látható hello NGINX tároló kitett 80-as port fut:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-124">hello following example output shows hello NGINX container running with port 80 exposed:</span></span>

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-hello-container"></a><span data-ttu-id="6b1f1-125">Hello tároló</span><span class="sxs-lookup"><span data-stu-id="6b1f1-125">Test hello container</span></span>
<span data-ttu-id="6b1f1-126">Szerezzen be Docker állomás hello nyilvános IP-címét az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-126">Obtain hello public IP address of Docker host as follows:</span></span>


```bash
docker-machine ip myvmdocker
```

<span data-ttu-id="6b1f1-127">toosee hello tároló műveletben, nyisson meg egy webböngészőt, és írja be a hello hello megelőző parancs kimenetét hello feljegyzett nyilvános IP-cím:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-127">toosee hello container in action, open a web browser and enter hello public IP address noted in hello output of hello preceding command:</span></span>

![Futó ngnix tároló](./media/docker-machine/nginx.png)

## <a name="next-steps"></a><span data-ttu-id="6b1f1-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b1f1-129">Next steps</span></span>
<span data-ttu-id="6b1f1-130">Gazdagépek is létrehozhat a hello [Docker Virtuálisgép-bővítmény](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="6b1f1-130">You can also create hosts with hello [Docker VM Extension](dockerextension.md).</span></span> <span data-ttu-id="6b1f1-131">A Docker Compose használ, tekintse meg a [Ismerkedés a Docker és az Azure-ban Compose](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="6b1f1-131">For examples on using Docker Compose, see [Get started with Docker and Compose in Azure](docker-compose-quickstart.md).</span></span>
