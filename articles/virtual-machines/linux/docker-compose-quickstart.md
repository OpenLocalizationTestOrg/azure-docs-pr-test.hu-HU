---
title: "Használjon Docker Compose a Linux virtuális gép az Azure-ban |} Microsoft Docs"
description: "Docker és Compose használata az Azure parancssori Felülettel rendelkező Linux virtuális gépeken"
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
ms.openlocfilehash: 541722cb02dd991228726e62a2304b49cdd806f2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a><span data-ttu-id="3e0a8-103">Ismerkedés a Docker és a Compose határozza meg, és futtassa a több tároló alkalmazást az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3e0a8-103">Get started with Docker and Compose to define and run a multi-container application in Azure</span></span>
<span data-ttu-id="3e0a8-104">A [Compose](http://github.com/docker/compose), egy egyszerű szöveges fájl használatával definiálja az alkalmazást, amely több Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-104">With [Compose](http://github.com/docker/compose), you use a simple text file to define an application consisting of multiple Docker containers.</span></span> <span data-ttu-id="3e0a8-105">Majd lépett fel az alkalmazást egy parancs, amelyet az adott környezet telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-105">You then spin up your application in a single command that does everything to deploy your defined environment.</span></span> <span data-ttu-id="3e0a8-106">Tegyük fel ez a cikk bemutatja, hogyan gyorsan be lehessen állítani egy WordPress-bloghoz az Ubuntu virtuális gép MariaDB SQL database-fájlok.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-106">As an example, this article shows you how to quickly set up a WordPress blog with a backend MariaDB SQL database on an Ubuntu VM.</span></span> <span data-ttu-id="3e0a8-107">Használhatja a Compose összetettebb alkalmazásokat beállítása.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-107">You can also use Compose to set up more complex applications.</span></span>


## <a name="set-up-a-linux-vm-as-a-docker-host"></a><span data-ttu-id="3e0a8-108">Linux virtuális gépet egy Docker-gazdagépként</span><span class="sxs-lookup"><span data-stu-id="3e0a8-108">Set up a Linux VM as a Docker host</span></span>
<span data-ttu-id="3e0a8-109">Segítségével különböző Azure eljárások és a rendelkezésre álló képeket vagy a Resource Manager-sablonok az Azure piactéren hozzon létre egy Linux virtuális Gépet, és állítsa be a Docker-gazdagépként.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-109">You can use various Azure procedures and available images or Resource Manager templates in the Azure Marketplace to create a Linux VM and set it up as a Docker host.</span></span> <span data-ttu-id="3e0a8-110">Lásd például: [a Docker Virtuálisgép-bővítmény használatával a környezet](dockerextension.md) létrehozhat egy Ubuntu virtuális gép az Azure Docker Virtuálisgép-bővítmény használatával egy [gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-110">For example, see [Using the Docker VM Extension to deploy your environment](dockerextension.md) to quickly create an Ubuntu VM with the Azure Docker VM extension by using a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="3e0a8-111">A Docker Virtuálisgép-bővítmény használata esetén a virtuális gép automatikusan be van állítva egy Docker-gazdagépként, és új már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-111">When you use the Docker VM extension, your VM is automatically set up as a Docker host and Compose is already installed.</span></span>


### <a name="create-docker-host-with-azure-cli-20"></a><span data-ttu-id="3e0a8-112">Az Azure CLI 2.0 Docker állomás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e0a8-112">Create Docker host with Azure CLI 2.0</span></span>
<span data-ttu-id="3e0a8-113">Telepítse a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és való bejelentkezéshez az Azure fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-113">Install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="3e0a8-114">Először hozzon létre egy erőforráscsoportot a Docker környezetében [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-114">First, create a resource group for your Docker environment with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="3e0a8-115">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *westus* helye:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-115">The following example creates a resource group named *myResourceGroup* in the *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="3e0a8-116">Ezután telepítse a virtuális gép és [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) , amely tartalmazza az Azure Docker Virtuálisgép-bővítmény a [a Githubon az Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-116">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes the Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="3e0a8-117">Adja meg a saját értékeit *newStorageAccountName*, *adminUsername*, *adminPassword*, és *dnsNameForPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-117">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="3e0a8-118">Az üzembe helyezés befejeződik néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-118">It takes a few minutes for the deployment to finish.</span></span> <span data-ttu-id="3e0a8-119">A telepítés befejeződése után [következő lépésének](#verify-that-compose-is-installed) az SSH-kapcsolatot a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-119">Once the deployment is finished, [move to next step](#verify-that-compose-is-installed) to SSH to your VM.</span></span> 

<span data-ttu-id="3e0a8-120">Ha szükséges, ehelyett vezérlő vissza kell a figyelmeztetésre, és lehetővé teszik a központi telepítés a háttérben folytatódik, vegye fel a `--no-wait` jelzőjét, hogy a fenti paranccsal.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-120">Optionally, to instead return control to the prompt and let the deployment continue in the background, add the `--no-wait` flag to the preceding command.</span></span> <span data-ttu-id="3e0a8-121">Ez a folyamat teszi lehetővé a parancssori felület a más feladatok végrehajtására, amíg a telepítés továbbra is fennáll, néhány percig.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-121">This process allows you to perform other work in the CLI while the deployment continues for a few minutes.</span></span> <span data-ttu-id="3e0a8-122">Megtekintheti a Docker állomás állapotának részleteit [az vm megjelenítése](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-122">You can then view details about the Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="3e0a8-123">A következő példa a nevű virtuális gép állapotát ellenőrzi *myDockerVM* (az alapértelmezett név a sablonból - ne módosítsa a nevet) az erőforráscsoport neve *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-123">The following example checks the status of the VM named *myDockerVM* (the default name from the template - don't change this name) in the resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="3e0a8-124">Ha ez a parancs visszaadja *sikeres*, a telepítés véget ért, és a következő lépésben a virtuális gép SSH is.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-124">When this command returns *Succeeded*, the deployment has finished and you can SSH to the VM in the following step.</span></span>


## <a name="verify-that-compose-is-installed"></a><span data-ttu-id="3e0a8-125">Győződjön meg arról, hogy telepítve van-e a Compose</span><span class="sxs-lookup"><span data-stu-id="3e0a8-125">Verify that Compose is installed</span></span>
<span data-ttu-id="3e0a8-126">A telepítés befejeződése után az SSH-kapcsolatot az új Docker-állomás a DNS-nevét, a telepítés során megadott.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-126">Once the deployment is finished, SSH to your new Docker host using the DNS name you provided during deployment.</span></span> <span data-ttu-id="3e0a8-127">Használhat `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` a virtuális gép, beleértve a DNS-nevet a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-127">You can use  `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` to view details of your VM, including the DNS name.</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="3e0a8-128">Ellenőrizze, hogy az új telepítve van-e a virtuális Gépre, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-128">To check that Compose is installed on the VM, run the following command:</span></span>

```bash
docker-compose --version
```

<span data-ttu-id="3e0a8-129">Megjelenik a hasonló kimenetet *docker compose 1.6.2, a 4d 72027 build*.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-129">You see output similar to *docker-compose 1.6.2, build 4d72027*.</span></span>

> [!TIP]
> <span data-ttu-id="3e0a8-130">Ha egy másik módszer segítségével hozzon létre egy Docker-állomás, és telepítenie kell a Compose saját kezűleg, lásd: a [dokumentáció ír](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-130">If you used another method to create a Docker host and need to install Compose yourself, see the [Compose documentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span></span>


## <a name="create-a-docker-composeyml-configuration-file"></a><span data-ttu-id="3e0a8-131">Egy docker-compose.yml konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e0a8-131">Create a docker-compose.yml configuration file</span></span>
<span data-ttu-id="3e0a8-132">Ezután létrehozhat egy `docker-compose.yml` fájlt, amely csak egy konfigurációs szövegfájl, a Docker-tárolókban, a virtuális Gépen futtatandó meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-132">Next you create a `docker-compose.yml` file, which is just a text configuration file, to define the Docker containers to run on the VM.</span></span> <span data-ttu-id="3e0a8-133">A fájl határozza meg a lemezkép futhat az egyes tárolókban (vagy annak oka lehet egy Dockerfile a build), szükséges környezeti változókat és a függőségeket, a portok és a tárolók közötti kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-133">The file specifies the image to run on each container (or it could be a build from a Dockerfile), necessary environment variables and dependencies, ports, and the links between containers.</span></span> <span data-ttu-id="3e0a8-134">További részletek a yml fájl Szintaxis: [hivatkozást a Compose](https://docs.docker.com/compose/compose-file/).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-134">For details on yml file syntax, see [Compose file reference](https://docs.docker.com/compose/compose-file/).</span></span>

<span data-ttu-id="3e0a8-135">Hozzon létre a *docker-compose.yml* fájlt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-135">Create the *docker-compose.yml* file as follows:</span></span>

```bash
touch docker-compose.yml
```

<span data-ttu-id="3e0a8-136">Kedvenc szövegszerkesztőjével segítségével bizonyos adatok hozzáadása a fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-136">Use your favorite text editor to add some data to the file.</span></span> <span data-ttu-id="3e0a8-137">Az alábbi példában a *vi* szerkesztő:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-137">The following example uses the *vi* editor:</span></span>

```bash
vi docker-compose.yml
```

<span data-ttu-id="3e0a8-138">Az alábbi példa illessze be a szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-138">Paste the following example into your text file.</span></span> <span data-ttu-id="3e0a8-139">Ezt a konfigurációt használja a képek a [DockerHub beállításjegyzék](https://registry.hub.docker.com/_/wordpress/) WordPress (a nyílt forráskódú bloggolás és a tartalom rendszerrel) és a csatolt háttérkiszolgálón MariaDB SQL-adatbázis telepítése.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-139">This configuration uses images from the [DockerHub Registry](https://registry.hub.docker.com/_/wordpress/) to install WordPress (the open source blogging and content management system) and a linked backend MariaDB SQL database.</span></span> <span data-ttu-id="3e0a8-140">Adja meg a saját *MYSQL_ROOT_PASSWORD* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-140">Enter your own *MYSQL_ROOT_PASSWORD* as follows:</span></span>

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

## <a name="start-the-containers-with-compose"></a><span data-ttu-id="3e0a8-141">Indítsa el a tárolók Compose</span><span class="sxs-lookup"><span data-stu-id="3e0a8-141">Start the containers with Compose</span></span>
<span data-ttu-id="3e0a8-142">Ugyanabban a könyvtárban a *docker-compose.yml* fájl, a következő parancsot (a használt környezettől függően szükség lehet futtatásához `docker-compose` használatával `sudo`):</span><span class="sxs-lookup"><span data-stu-id="3e0a8-142">In the same directory as your *docker-compose.yml* file, run the following command (depending on your environment, you might need to run `docker-compose` using `sudo`):</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="3e0a8-143">A paranccsal elindítja a Docker-tároló megadott *docker-compose.yml*.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-143">This command starts the Docker containers specified in *docker-compose.yml*.</span></span> <span data-ttu-id="3e0a8-144">Egy percet, amíg ez a lépés végrehajtásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-144">It takes a minute or two for this step to complete.</span></span> <span data-ttu-id="3e0a8-145">Az alábbi példához hasonló kimenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-145">You see output similar to the following example:</span></span>

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> <span data-ttu-id="3e0a8-146">Ügyeljen arra, hogy használja a **-d** indítási lehetőséget, hogy a tárolók folyamatosan fut a háttérben.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-146">Be sure to use the **-d** option on start-up so that the containers run in the background continuously.</span></span>


<span data-ttu-id="3e0a8-147">Arról, hogy a tárolók, írja be a következőt `docker-compose ps`.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-147">To verify that the containers are up, type `docker-compose ps`.</span></span> <span data-ttu-id="3e0a8-148">Hasonlót kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="3e0a8-148">You should see something like:</span></span>

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

<span data-ttu-id="3e0a8-149">WordPress közvetlenül a 80-as porton virtuális Gépre is csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-149">You can now connect to WordPress directly on the VM on port 80.</span></span> <span data-ttu-id="3e0a8-150">Nyisson meg egy webböngészőt, és írja be a virtuális Gépet DNS-nevét (például `http://mypublicdns.westus.cloudapp.azure.com`).</span><span class="sxs-lookup"><span data-stu-id="3e0a8-150">Open a web browser and enter the DNS name of your VM (such as `http://mypublicdns.westus.cloudapp.azure.com`).</span></span> <span data-ttu-id="3e0a8-151">Meg kell jelennie a WordPress kezdőképernyőn, ahol fejezheti be a telepítést, és ismerkedjen meg az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-151">You should now see the WordPress start screen, where you can complete the installation and get started with the application.</span></span>

![WordPress kezdőképernyője][wordpress_start]

## <a name="next-steps"></a><span data-ttu-id="3e0a8-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e0a8-153">Next steps</span></span>
* <span data-ttu-id="3e0a8-154">Lépjen a [Docker VM bővítményének használati útmutatója](https://github.com/Azure/azure-docker-extension/blob/master/README.md) Docker és Compose konfigurálása a Docker VM további lehetőségekért.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-154">Go to the [Docker VM extension user guide](https://github.com/Azure/azure-docker-extension/blob/master/README.md) for more options to configure Docker and Compose in your Docker VM.</span></span> <span data-ttu-id="3e0a8-155">Például egy lehetőség áll a Compose yml fájlt (JSON formátumúvá konvertálni) közvetlenül a Docker Virtuálisgép-bővítmény konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-155">For example, one option is to put the Compose yml file (converted to JSON) directly in the configuration of the Docker VM extension.</span></span>
* <span data-ttu-id="3e0a8-156">Tekintse meg a [állítható össze a parancssori útmutatójának](http://docs.docker.com/compose/reference/) és [felhasználói útmutató](http://docs.docker.com/compose/) további példák a kialakításához, és több tároló alkalmazások központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-156">Check out the [Compose command-line reference](http://docs.docker.com/compose/reference/) and [user guide](http://docs.docker.com/compose/) for more examples of building and deploying multi-container apps.</span></span>
* <span data-ttu-id="3e0a8-157">Az Azure Resource Manager-sablon, vagy használja a saját vagy egy része volt a a [közösségi](https://azure.microsoft.com/documentation/templates/), hogy az Azure virtuális gép Docker és állítsa be a Compose kérelmet.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-157">Use an Azure Resource Manager template, either your own or one contributed from the [community](https://azure.microsoft.com/documentation/templates/), to deploy an Azure VM with Docker and an application set up with Compose.</span></span> <span data-ttu-id="3e0a8-158">Például a [központi telepítése egy WordPress-bloghoz az Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) sablon és használ a Docker Compose gyorsan üzembe helyezhet WordPress egy Ubuntu virtuális gép a MySQL-fájlok.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-158">For example, the [Deploy a WordPress blog with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) template uses Docker and Compose to quickly deploy WordPress with a MySQL backend on an Ubuntu VM.</span></span>
* <span data-ttu-id="3e0a8-159">Próbálja a Docker Compose integrálása a Docker Swarm-fürt.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-159">Try integrating Docker Compose with a Docker Swarm cluster.</span></span> <span data-ttu-id="3e0a8-160">Lásd: [használatával állítsa össze a Swarm](https://docs.docker.com/compose/swarm/) forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="3e0a8-160">See [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) for scenarios.</span></span>

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
