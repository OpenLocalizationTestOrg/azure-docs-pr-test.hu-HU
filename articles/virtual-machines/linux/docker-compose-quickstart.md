---
title: "a Linux virtuális gépek Azure-ban a Docker Compose aaaUse |} Microsoft Docs"
description: "Hogyan toouse Docker és a Linux virtuális gépek a Compose hello Azure parancssori felület"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: cf7254ad4813ccdc641fcacbb06ed1514a93cee5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-docker-and-compose-toodefine-and-run-a-multi-container-application-in-azure"></a><span data-ttu-id="5f485-103">Beolvasása használatába a Docker és Compose toodefine és futtatni egy több tároló alkalmazást az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="5f485-103">Get started with Docker and Compose toodefine and run a multi-container application in Azure</span></span>
<span data-ttu-id="5f485-104">A [Compose](http://github.com/docker/compose), egy egyszerű szöveges fájl toodefine álló több Docker-tároló egy alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="5f485-104">With [Compose](http://github.com/docker/compose), you use a simple text file toodefine an application consisting of multiple Docker containers.</span></span> <span data-ttu-id="5f485-105">Majd lépett fel az alkalmazást, amelyet minden egy parancs toodeploy meghatározott környezetét.</span><span class="sxs-lookup"><span data-stu-id="5f485-105">You then spin up your application in a single command that does everything toodeploy your defined environment.</span></span> <span data-ttu-id="5f485-106">Tegyük fel ez a cikk bemutatja, hogyan tooquickly beállítani egy WordPress-bloghoz egy háttérrendszerrel MariaDB SQL-adatbázis egy Ubuntu virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5f485-106">As an example, this article shows you how tooquickly set up a WordPress blog with a backend MariaDB SQL database on an Ubuntu VM.</span></span> <span data-ttu-id="5f485-107">Használhatja a Compose tooset mentése összetettebb alkalmazásokat is.</span><span class="sxs-lookup"><span data-stu-id="5f485-107">You can also use Compose tooset up more complex applications.</span></span>


## <a name="set-up-a-linux-vm-as-a-docker-host"></a><span data-ttu-id="5f485-108">Linux virtuális gépet egy Docker-gazdagépként</span><span class="sxs-lookup"><span data-stu-id="5f485-108">Set up a Linux VM as a Docker host</span></span>
<span data-ttu-id="5f485-109">Különböző Azure eljárások és a rendelkezésre álló képeket vagy a Resource Manager-sablonok hello Azure piactér toocreate Linux virtuális gép használja, és állítsa be a Docker-gazdagépként.</span><span class="sxs-lookup"><span data-stu-id="5f485-109">You can use various Azure procedures and available images or Resource Manager templates in hello Azure Marketplace toocreate a Linux VM and set it up as a Docker host.</span></span> <span data-ttu-id="5f485-110">Lásd például: [hello Docker Virtuálisgép-bővítmény toodeploy a környezet használatával](dockerextension.md) tooquickly hozzon létre egy Ubuntu virtuális gép hello Azure Docker Virtuálisgép-bővítmény használatával egy [gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="5f485-110">For example, see [Using hello Docker VM Extension toodeploy your environment](dockerextension.md) tooquickly create an Ubuntu VM with hello Azure Docker VM extension by using a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="5f485-111">Hello Docker Virtuálisgép-bővítmény használata esetén a virtuális gép automatikusan be van állítva egy Docker-gazdagépként, és új már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="5f485-111">When you use hello Docker VM extension, your VM is automatically set up as a Docker host and Compose is already installed.</span></span>


### <a name="create-docker-host-with-azure-cli-20"></a><span data-ttu-id="5f485-112">Az Azure CLI 2.0 Docker állomás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f485-112">Create Docker host with Azure CLI 2.0</span></span>
<span data-ttu-id="5f485-113">Legutóbbi telepítés hello [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5f485-113">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="5f485-114">Először hozzon létre egy erőforráscsoportot a Docker környezetében [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="5f485-114">First, create a resource group for your Docker environment with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="5f485-115">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westus* helye:</span><span class="sxs-lookup"><span data-stu-id="5f485-115">hello following example creates a resource group named *myResourceGroup* in hello *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="5f485-116">Ezután telepítse a virtuális gép és [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) , amely tartalmazza az Azure Docker Virtuálisgép-bővítmény hello [a Githubon az Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="5f485-116">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes hello Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="5f485-117">Adja meg a saját értékeit *newStorageAccountName*, *adminUsername*, *adminPassword*, és *dnsNameForPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="5f485-117">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="5f485-118">Hello telepítési toofinish néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5f485-118">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="5f485-119">Hello telepítés befejeződése után [áthelyezése toonext lépéssel](#verify-that-compose-is-installed) tooSSH tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5f485-119">Once hello deployment is finished, [move toonext step](#verify-that-compose-is-installed) tooSSH tooyour VM.</span></span> 

<span data-ttu-id="5f485-120">Opcionálisan tooinstead visszatérési felügyeletének toohello kérése és a telepítés lehetővé hello továbbra hello háttérben, adja hozzá a hello `--no-wait` parancs megelőző toohello jelzőt.</span><span class="sxs-lookup"><span data-stu-id="5f485-120">Optionally, tooinstead return control toohello prompt and let hello deployment continue in hello background, add hello `--no-wait` flag toohello preceding command.</span></span> <span data-ttu-id="5f485-121">Ez a folyamat lehetővé teszi a tooperform más tevékenységet hello CLI közben hello telepítési továbbra is fennáll, néhány percig.</span><span class="sxs-lookup"><span data-stu-id="5f485-121">This process allows you tooperform other work in hello CLI while hello deployment continues for a few minutes.</span></span> <span data-ttu-id="5f485-122">Majd megtekintheti az hello Docker-gazdagép állapotával kapcsolatos adatokat [az vm megjelenítése](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="5f485-122">You can then view details about hello Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="5f485-123">hello alábbi példa ellenőrzi hello nevű virtuális gép állapotát hello *myDockerVM* (alapértelmezett neve hello sablonból hello – ne módosítsa a nevet) nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="5f485-123">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="5f485-124">Ha ez a parancs visszaadja *sikeres*, hello telepítés véget ért, és a következő lépés hello a virtuális gép SSH toohello is.</span><span class="sxs-lookup"><span data-stu-id="5f485-124">When this command returns *Succeeded*, hello deployment has finished and you can SSH toohello VM in hello following step.</span></span>


## <a name="verify-that-compose-is-installed"></a><span data-ttu-id="5f485-125">Győződjön meg arról, hogy telepítve van-e a Compose</span><span class="sxs-lookup"><span data-stu-id="5f485-125">Verify that Compose is installed</span></span>
<span data-ttu-id="5f485-126">Hello telepítés befejeződése után SSH tooyour új Docker állomás hello DNS-név használatával megadott központi telepítése során.</span><span class="sxs-lookup"><span data-stu-id="5f485-126">Once hello deployment is finished, SSH tooyour new Docker host using hello DNS name you provided during deployment.</span></span> <span data-ttu-id="5f485-127">Használhat `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview részleteit a virtuális Gépet, beleértve a hello DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="5f485-127">You can use  `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview details of your VM, including hello DNS name.</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="5f485-128">alkotó toocheck hello VM, futtassa a következő parancs hello telepítve van:</span><span class="sxs-lookup"><span data-stu-id="5f485-128">toocheck that Compose is installed on hello VM, run hello following command:</span></span>

```bash
docker-compose --version
```

<span data-ttu-id="5f485-129">Kimenet jelenik meg hasonló túl*docker compose 1.6.2, a 4d 72027 build*.</span><span class="sxs-lookup"><span data-stu-id="5f485-129">You see output similar too*docker-compose 1.6.2, build 4d72027*.</span></span>

> [!TIP]
> <span data-ttu-id="5f485-130">Ha egy másik módszer toocreate egy Docker-állomás és kell tooinstall állítható össze saját magának, lásd: hello [dokumentáció ír](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="5f485-130">If you used another method toocreate a Docker host and need tooinstall Compose yourself, see hello [Compose documentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span></span>


## <a name="create-a-docker-composeyml-configuration-file"></a><span data-ttu-id="5f485-131">Egy docker-compose.yml konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f485-131">Create a docker-compose.yml configuration file</span></span>
<span data-ttu-id="5f485-132">Ezután létrehozhat egy `docker-compose.yml` fájl, amely csak a szöveg konfigurációs fájl, a toodefine hello Docker tárolók toorun hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5f485-132">Next you create a `docker-compose.yml` file, which is just a text configuration file, toodefine hello Docker containers toorun on hello VM.</span></span> <span data-ttu-id="5f485-133">hello fájl minden egyes tárolóban hello kép toorun határozza meg (vagy annak oka lehet egy Dockerfile a build), szükséges környezeti változókat és a függőségeket, a portok és a tárolók közötti hello hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="5f485-133">hello file specifies hello image toorun on each container (or it could be a build from a Dockerfile), necessary environment variables and dependencies, ports, and hello links between containers.</span></span> <span data-ttu-id="5f485-134">További részletek a yml fájl Szintaxis: [hivatkozást a Compose](https://docs.docker.com/compose/compose-file/).</span><span class="sxs-lookup"><span data-stu-id="5f485-134">For details on yml file syntax, see [Compose file reference](https://docs.docker.com/compose/compose-file/).</span></span>

<span data-ttu-id="5f485-135">Hozzon létre hello *docker-compose.yml* fájlt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5f485-135">Create hello *docker-compose.yml* file as follows:</span></span>

```bash
touch docker-compose.yml
```

<span data-ttu-id="5f485-136">A kedvenc text editor tooadd bizonyos adatok toohello fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="5f485-136">Use your favorite text editor tooadd some data toohello file.</span></span> <span data-ttu-id="5f485-137">hello alábbi példában hello *vi* szerkesztő:</span><span class="sxs-lookup"><span data-stu-id="5f485-137">hello following example uses hello *vi* editor:</span></span>

```bash
vi docker-compose.yml
```

<span data-ttu-id="5f485-138">Illessze be a következő példa a szövegfájlba hello.</span><span class="sxs-lookup"><span data-stu-id="5f485-138">Paste hello following example into your text file.</span></span> <span data-ttu-id="5f485-139">Ezt a konfigurációt használja a képek a hello [DockerHub beállításjegyzék](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello nyílt forráskódú bloggolás és a tartalom rendszerrel) és a csatolt háttér MariaDB SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5f485-139">This configuration uses images from hello [DockerHub Registry](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello open source blogging and content management system) and a linked backend MariaDB SQL database.</span></span> <span data-ttu-id="5f485-140">Adja meg a saját *MYSQL_ROOT_PASSWORD* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5f485-140">Enter your own *MYSQL_ROOT_PASSWORD* as follows:</span></span>

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-hello-containers-with-compose"></a><span data-ttu-id="5f485-141">Kezdő hello tárolók Compose</span><span class="sxs-lookup"><span data-stu-id="5f485-141">Start hello containers with Compose</span></span>
<span data-ttu-id="5f485-142">Az azonos hello könyvtárhoz, mint a *docker-compose.yml* fájl, futtassa a következő parancs hello (a használt környezettől függően szükség lehet a toorun `docker-compose` használatával `sudo`):</span><span class="sxs-lookup"><span data-stu-id="5f485-142">In hello same directory as your *docker-compose.yml* file, run hello following command (depending on your environment, you might need toorun `docker-compose` using `sudo`):</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="5f485-143">A paranccsal elindítja a megadott hello Docker-tároló *docker-compose.yml*.</span><span class="sxs-lookup"><span data-stu-id="5f485-143">This command starts hello Docker containers specified in *docker-compose.yml*.</span></span> <span data-ttu-id="5f485-144">Egy percet, amíg ez a lépés toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5f485-144">It takes a minute or two for this step toocomplete.</span></span> <span data-ttu-id="5f485-145">A következő példa kimenet hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="5f485-145">You see output similar toohello following example:</span></span>

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> <span data-ttu-id="5f485-146">Lehet, hogy toouse hello **-d** , hogy hello tárolók folyamatos Futtatás hello háttérben indítási lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5f485-146">Be sure toouse hello **-d** option on start-up so that hello containers run in hello background continuously.</span></span>


<span data-ttu-id="5f485-147">hello tárolók, amelyek tooverify típus `docker-compose ps`.</span><span class="sxs-lookup"><span data-stu-id="5f485-147">tooverify that hello containers are up, type `docker-compose ps`.</span></span> <span data-ttu-id="5f485-148">Hasonlót kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5f485-148">You should see something like:</span></span>

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

<span data-ttu-id="5f485-149">Most már közvetlenül a 80-as porton VM hello tooWordPress is elérheti.</span><span class="sxs-lookup"><span data-stu-id="5f485-149">You can now connect tooWordPress directly on hello VM on port 80.</span></span> <span data-ttu-id="5f485-150">Nyisson meg egy webböngészőt, és írja be a virtuális gép hello DNS-nevét (például `http://mypublicdns.westus.cloudapp.azure.com`).</span><span class="sxs-lookup"><span data-stu-id="5f485-150">Open a web browser and enter hello DNS name of your VM (such as `http://mypublicdns.westus.cloudapp.azure.com`).</span></span> <span data-ttu-id="5f485-151">Meg kell jelennie hello WordPress kezdőképernyőjén, ahol fejezheti be hello telepítését, valamint Ismerkedés a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5f485-151">You should now see hello WordPress start screen, where you can complete hello installation and get started with hello application.</span></span>

![WordPress kezdőképernyője][wordpress_start]

## <a name="next-steps"></a><span data-ttu-id="5f485-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f485-153">Next steps</span></span>
* <span data-ttu-id="5f485-154">Nyissa meg toohello [Docker VM bővítményének használati útmutatója](https://github.com/Azure/azure-docker-extension/blob/master/README.md) további beállítások tooconfigure Docker és a Docker virtuális gép új.</span><span class="sxs-lookup"><span data-stu-id="5f485-154">Go toohello [Docker VM extension user guide](https://github.com/Azure/azure-docker-extension/blob/master/README.md) for more options tooconfigure Docker and Compose in your Docker VM.</span></span> <span data-ttu-id="5f485-155">Például egy elem tooput hello Compose yml fájl (átalakított tooJSON) közvetlenül a Docker Virtuálisgép-bővítmény hello hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5f485-155">For example, one option is tooput hello Compose yml file (converted tooJSON) directly in hello configuration of hello Docker VM extension.</span></span>
* <span data-ttu-id="5f485-156">Tekintse meg a hello [állítható össze a parancssori útmutatójának](http://docs.docker.com/compose/reference/) és [felhasználói útmutató](http://docs.docker.com/compose/) további példák a kialakításához, és több tároló alkalmazások központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5f485-156">Check out hello [Compose command-line reference](http://docs.docker.com/compose/reference/) and [user guide](http://docs.docker.com/compose/) for more examples of building and deploying multi-container apps.</span></span>
* <span data-ttu-id="5f485-157">Az Azure Resource Manager-sablon, vagy használja a saját vagy annak egy része volt a hello [közösségi](https://azure.microsoft.com/documentation/templates/), egy Azure virtuális gép Docker és egy alkalmazás toodeploy állítsa be a Compose.</span><span class="sxs-lookup"><span data-stu-id="5f485-157">Use an Azure Resource Manager template, either your own or one contributed from hello [community](https://azure.microsoft.com/documentation/templates/), toodeploy an Azure VM with Docker and an application set up with Compose.</span></span> <span data-ttu-id="5f485-158">Például hello [központi telepítése egy WordPress-bloghoz az Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) sablon Docker használ, és új tooquickly WordPress telepítése egy Ubuntu virtuális gép a MySQL-fájlok.</span><span class="sxs-lookup"><span data-stu-id="5f485-158">For example, hello [Deploy a WordPress blog with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) template uses Docker and Compose tooquickly deploy WordPress with a MySQL backend on an Ubuntu VM.</span></span>
* <span data-ttu-id="5f485-159">Próbálja a Docker Compose integrálása a Docker Swarm-fürt.</span><span class="sxs-lookup"><span data-stu-id="5f485-159">Try integrating Docker Compose with a Docker Swarm cluster.</span></span> <span data-ttu-id="5f485-160">Lásd: [használatával állítsa össze a Swarm](https://docs.docker.com/compose/swarm/) forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="5f485-160">See [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) for scenarios.</span></span>

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
