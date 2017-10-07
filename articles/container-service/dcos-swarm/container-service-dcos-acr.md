---
title: "egy Azure DC/OS-fürt ACR aaaUsing |} Microsoft Docs"
description: "Egy Azure-tároló beállításjegyzék használata a DC/OS fürtben, az Azure Tárolószolgáltatásban"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: "Docker, tárolók, Micro-szolgáltatások, a Mesos, a Azure, a fájlmegosztás, a cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 9a2802da788b50147d8b4259436bdcdb0c929db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-acr-with-a-dcos-cluster-toodeploy-your-application"></a><span data-ttu-id="80c70-104">A DC/OS-fürt toodeploy ACR használni az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="80c70-104">Use ACR with a DC/OS cluster toodeploy your application</span></span>

<span data-ttu-id="80c70-105">Ez a cikk azt felfedezés hogyan toouse Azure tároló beállításjegyzék DC/OS-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="80c70-105">In this article, we explore how toouse Azure Container Registry with a DC/OS cluster.</span></span> <span data-ttu-id="80c70-106">ACR használatával tooprivately tároló lehetővé teszi, és tároló-lemezképek kezelése.</span><span class="sxs-lookup"><span data-stu-id="80c70-106">Using ACR allows you tooprivately store and manage container images.</span></span> <span data-ttu-id="80c70-107">Ez az oktatóanyag ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="80c70-107">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80c70-108">Telepítse az Azure-tároló beállításjegyzék, (ha szükséges)</span><span class="sxs-lookup"><span data-stu-id="80c70-108">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="80c70-109">A DC/OS-fürtről ACR hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="80c70-109">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="80c70-110">Egy kép toohello Azure tároló beállításjegyzék feltöltése</span><span class="sxs-lookup"><span data-stu-id="80c70-110">Uploaded an image toohello Azure Container Registry</span></span>
> * <span data-ttu-id="80c70-111">A tároló lemezkép hello Azure tároló beállításjegyzék-ről futtatva</span><span class="sxs-lookup"><span data-stu-id="80c70-111">Run a container image from hello Azure Container Registry</span></span>

<span data-ttu-id="80c70-112">Az ACS a DC/OS fürtben toocomplete hello lépések ebben az oktatóanyagban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="80c70-112">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="80c70-113">Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="80c70-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="80c70-114">Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="80c70-114">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="80c70-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="80c70-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="80c70-116">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="80c70-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="80c70-117">Telepítse az Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="80c70-117">Deploy Azure Container Registry</span></span>

<span data-ttu-id="80c70-118">Szükség esetén hozzon létre egy Azure-tárolóba beállításjegyzék hello [az acr létrehozása](/cli/azure/acr#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="80c70-118">If needed, create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> 

<span data-ttu-id="80c70-119">hello alábbi példa létrehoz egy beállításjegyzéket egy véletlenszerű előállításához a neve.</span><span class="sxs-lookup"><span data-stu-id="80c70-119">hello following example creates a registry with a randomly generate name.</span></span> <span data-ttu-id="80c70-120">hello beállításjegyzék is konfigurálva van egy rendszergazdai fiókkal hello segítségével `--admin-enabled` argumentum.</span><span class="sxs-lookup"><span data-stu-id="80c70-120">hello registry is also configured with an admin account using hello `--admin-enabled` argument.</span></span>

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

<span data-ttu-id="80c70-121">Hello beállításkulcs létrehozása után hello Azure CLI kimeneti adatok hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="80c70-121">Once hello registry has been created, hello Azure CLI outputs data similar toohello following.</span></span> <span data-ttu-id="80c70-122">Jegyezze fel a hello `name` és `loginServer`, ezek a későbbi lépésekben használhatók.</span><span class="sxs-lookup"><span data-stu-id="80c70-122">Take note of hello `name` and `loginServer`, these are used in later steps.</span></span>

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

<span data-ttu-id="80c70-123">Hello tároló beállításjegyzék hitelesítő adatainak használatával hello lekérése [az acr hitelesítő adatok megjelenítése](/cli/azure/acr/credential) parancsot.</span><span class="sxs-lookup"><span data-stu-id="80c70-123">Get hello container registry credentials using hello [az acr credential show](/cli/azure/acr/credential) command.</span></span> <span data-ttu-id="80c70-124">Helyettesítő hello `--name` az egyik hello utolsó lépésben feljegyzett hello.</span><span class="sxs-lookup"><span data-stu-id="80c70-124">Substitute hello `--name` with hello one noted in hello last step.</span></span> <span data-ttu-id="80c70-125">Jegyezze fel a egy jelszó, szükség esetén egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="80c70-125">Take note of one password, it is needed in a later step.</span></span>

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

<span data-ttu-id="80c70-126">Azure tároló beállításjegyzékkel kapcsolatos további információkért lásd: [bemutatása tooprivate Docker-tároló nyilvántartó](../../container-registry/container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="80c70-126">For more information on Azure Container Registry, see [Introduction tooprivate Docker container registries](../../container-registry/container-registry-intro.md).</span></span> 

## <a name="manage-acr-authentication"></a><span data-ttu-id="80c70-127">ACR hitelesítés kezelésére szolgál</span><span class="sxs-lookup"><span data-stu-id="80c70-127">Manage ACR authentication</span></span>

<span data-ttu-id="80c70-128">hello hagyományos módon személyes beállításjegyzékből toopush és lekéréses kép toofirst hello beállításjegyzék hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="80c70-128">hello conventional way toopush and pull image from a private registry is toofirst authenticate with hello registry.</span></span> <span data-ttu-id="80c70-129">toodo hello használja, ezért `docker login` minden ügyfélen, amelyet a tooaccess hello titkos beállításjegyzék parancsot.</span><span class="sxs-lookup"><span data-stu-id="80c70-129">toodo so, you would use hello `docker login` command on any client that needs tooaccess hello private registry.</span></span> <span data-ttu-id="80c70-130">A DC/OS-fürtről sok csomópontok, ezek mindegyike kell, hogy a felhasználók hitelesítése a hello ACR toobe, szerepelhetnek benne, ezért hasznos tooautomate között a folyamat minden egyes csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="80c70-130">Because a DC/OS cluster can contain many nodes, all of which need toobe authenticated with hello ACR, it is helpful tooautomate this process across each node.</span></span> 

### <a name="create-shared-storage"></a><span data-ttu-id="80c70-131">Megosztott tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="80c70-131">Create shared storage</span></span>

<span data-ttu-id="80c70-132">Ez a folyamat használja az Azure fájlmegosztások csatlakoztatva hello fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="80c70-132">This process uses an Azure file share that has been mounted on each node in hello cluster.</span></span> <span data-ttu-id="80c70-133">Ha már nem beállított megosztott tárolót, lásd: [a DC/OS fürtben található fájlmegosztás beállítása](container-service-dcos-fileshare.md).</span><span class="sxs-lookup"><span data-stu-id="80c70-133">If you have not already set up shared storage, see [Setup a file share inside a DC/OS cluster](container-service-dcos-fileshare.md).</span></span>

### <a name="configure-acr-authentication"></a><span data-ttu-id="80c70-134">ACR hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="80c70-134">Configure ACR authentication</span></span>

<span data-ttu-id="80c70-135">Szereznie hello hello DC/OS fő és tárolható egy változóban teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="80c70-135">First, get hello FQDN of hello DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="80c70-136">Hozzon létre egy SSH-kapcsolat hello főkiszolgáló (vagy hello első főkiszolgálójának) a DC/OS-alapú fürt.</span><span class="sxs-lookup"><span data-stu-id="80c70-136">Create an SSH connection with hello master (or hello first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="80c70-137">Frissítse a hello felhasználói nevét, ha egy nem alapértelmezett érték lett megadva, amikor hello fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="80c70-137">Update hello user name if a non-default value was used when creating hello cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="80c70-138">Futtassa a következő parancs toologin toohello Azure tároló beállításjegyzék hello.</span><span class="sxs-lookup"><span data-stu-id="80c70-138">Run hello following command toologin toohello Azure Container Registry.</span></span> <span data-ttu-id="80c70-139">Cserélje le a hello `--username` hello tároló beállításjegyzék és hello hello nevű `--password` hello foglalt jelszavak egyikével.</span><span class="sxs-lookup"><span data-stu-id="80c70-139">Replace hello `--username` with hello name of hello container registry, and hello `--password` with one of hello provided passwords.</span></span> <span data-ttu-id="80c70-140">Cserélje le az utolsó argumentumnak hello *mycontainerregistry.azurecr.io* hello példájában hello loginServer nevű hello tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="80c70-140">Replace hello last argument *mycontainerregistry.azurecr.io* in hello example with hello loginServer name of hello container registry.</span></span> 

<span data-ttu-id="80c70-141">Ezzel a paranccsal tárolhatók hello-értékek hitelesítési helyileg hello `~/.docker` elérési útja.</span><span class="sxs-lookup"><span data-stu-id="80c70-141">This command stores hello authentication values locally under hello `~/.docker` path.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

<span data-ttu-id="80c70-142">Hozzon létre egy hello tároló beállításazonosítókat hitelesítési tartalmazó tömörített fájl.</span><span class="sxs-lookup"><span data-stu-id="80c70-142">Create a compressed file that contains hello container registry authentication values.</span></span>

```azurecli-interactive
tar czf docker.tar.gz .docker
```

<span data-ttu-id="80c70-143">A fájl toohello megosztott fürttároló másolja.</span><span class="sxs-lookup"><span data-stu-id="80c70-143">Copy this file toohello cluster shared storage.</span></span> <span data-ttu-id="80c70-144">Ebben a lépésben elérhetővé teszi hello fájl hello DC/OS-fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="80c70-144">This step makes hello file available on all nodes of hello DC/OS cluster.</span></span>

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-tooacr"></a><span data-ttu-id="80c70-145">Kép tooACR feltöltése</span><span class="sxs-lookup"><span data-stu-id="80c70-145">Upload image tooACR</span></span>

<span data-ttu-id="80c70-146">Most a fejlesztési számítógépén, vagy bármely más rendszer Docker telepítve, a lemezkép létrehozása, és töltse fel az Azure-tároló beállításjegyzék toohello.</span><span class="sxs-lookup"><span data-stu-id="80c70-146">Now from a development machine, or any other system with Docker installed, create an image and upload it toohello Azure Container Registry.</span></span>

<span data-ttu-id="80c70-147">Hozzon létre egy tároló hello Ubuntu lemezképéről.</span><span class="sxs-lookup"><span data-stu-id="80c70-147">Create a container from hello Ubuntu image.</span></span>

```azurecli-interactive
docker run ubunut --name base-image
```

<span data-ttu-id="80c70-148">Most rögzítési hello tárolót az új lemezképet.</span><span class="sxs-lookup"><span data-stu-id="80c70-148">Now capture hello container into a new image.</span></span> <span data-ttu-id="80c70-149">hello lemezkép nevének kell tooinclude hello `loginServer` hello tároló registrywith formátuma a neve `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="80c70-149">hello image name needs tooinclude hello `loginServer` name of hello container registrywith a format of `loginServer/imageName`.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

<span data-ttu-id="80c70-150">Bejelentkezés a hello Azure tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="80c70-150">Login into hello Azure Container Registry.</span></span> <span data-ttu-id="80c70-151">Hello neve hello loginServer nevű lecseréléséhez hello felhasználónév – hello nevű hello tároló beállításjegyzék és hello – hello foglalt jelszavak egyikével jelszót.</span><span class="sxs-lookup"><span data-stu-id="80c70-151">Replace hello name with hello loginServer name, hello --username with hello name of hello container registry, and hello --password with one of hello provided passwords.</span></span>

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

<span data-ttu-id="80c70-152">Végezetül feltöltése hello kép toohello ACR beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="80c70-152">Finally, upload hello image toohello ACR registry.</span></span> <span data-ttu-id="80c70-153">Ez a példa feltölt egy képet vezénylőtípusú-bemutató nevű.</span><span class="sxs-lookup"><span data-stu-id="80c70-153">This example uploads an image named dcos-demo.</span></span>

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a><span data-ttu-id="80c70-154">Futtassa a képfájl ACR</span><span class="sxs-lookup"><span data-stu-id="80c70-154">Run an image from ACR</span></span>

<span data-ttu-id="80c70-155">egy kép hello ACR beállításjegyzékből toouse hozzon létre egy fájlt nevek *acrDemo.json* és a következő szöveg másolása hello bele.</span><span class="sxs-lookup"><span data-stu-id="80c70-155">toouse an image from hello ACR registry, create a file names *acrDemo.json* and copy hello following text into it.</span></span> <span data-ttu-id="80c70-156">Cserélje le hello lemezképnév hello tároló beállításjegyzék loginServer nevének és a lemezkép neve, például `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="80c70-156">Replace hello image name with hello container registry loginServer name and image name, for example `loginServer/imageName`.</span></span> <span data-ttu-id="80c70-157">Jegyezze fel a hello `uris` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="80c70-157">Take note of hello `uris` property.</span></span> <span data-ttu-id="80c70-158">Ez a tulajdonság tárolja hello hello tároló beállításjegyzék hitelesítési-fájl helyét, amely ebben az esetben hello DC/OS-fürt minden csomópontja csatlakoztattak hello Azure fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="80c70-158">This property holds hello location of hello container registry authentication file, which in this case is hello Azure file share that is mounted on each node in hello DC/OS cluster.</span></span>

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

<span data-ttu-id="80c70-159">DC/c CLI hello hello alkalmazás központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="80c70-159">Deploy hello application with hello DC/OC CLI.</span></span>

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a><span data-ttu-id="80c70-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80c70-160">Next steps</span></span>

<span data-ttu-id="80c70-161">Ebben az oktatóanyagban a DC/OS toouse többek között a következő hello Azure tároló beállításjegyzék feladatok rendelkezik konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="80c70-161">In this tutorial you have configure DC/OS toouse Azure Container Registry including hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80c70-162">Telepítse az Azure-tároló beállításjegyzék, (ha szükséges)</span><span class="sxs-lookup"><span data-stu-id="80c70-162">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="80c70-163">A DC/OS-fürtről ACR hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="80c70-163">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="80c70-164">Egy kép toohello Azure tároló beállításjegyzék feltöltése</span><span class="sxs-lookup"><span data-stu-id="80c70-164">Uploaded an image toohello Azure Container Registry</span></span>
> * <span data-ttu-id="80c70-165">A tároló lemezkép hello Azure tároló beállításjegyzék-ről futtatva</span><span class="sxs-lookup"><span data-stu-id="80c70-165">Run a container image from hello Azure Container Registry</span></span>
