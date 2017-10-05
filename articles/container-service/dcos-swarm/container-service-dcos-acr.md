---
title: "ACR használata az Azure DC/OS-fürtről |} Microsoft Docs"
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
ms.openlocfilehash: 618d32ca919e50d41b85c800225fe72c3b94e537
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-acr-with-a-dcos-cluster-to-deploy-your-application"></a><span data-ttu-id="02c4c-104">A DC/OS-fürtről ACR használni az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="02c4c-104">Use ACR with a DC/OS cluster to deploy your application</span></span>

<span data-ttu-id="02c4c-105">Ebben a cikkben megismerkedhet a Microsoft Azure tároló beállításjegyzék használata a DC/OS-fürtről.</span><span class="sxs-lookup"><span data-stu-id="02c4c-105">In this article, we explore how to use Azure Container Registry with a DC/OS cluster.</span></span> <span data-ttu-id="02c4c-106">ACR használatával lehetővé teszi, hogy közvetlenül a Microsoftnak tárolásához és tároló-lemezképek kezelése.</span><span class="sxs-lookup"><span data-stu-id="02c4c-106">Using ACR allows you to privately store and manage container images.</span></span> <span data-ttu-id="02c4c-107">Ez az oktatóanyag ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="02c4c-107">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="02c4c-108">Telepítse az Azure-tároló beállításjegyzék, (ha szükséges)</span><span class="sxs-lookup"><span data-stu-id="02c4c-108">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="02c4c-109">A DC/OS-fürtről ACR hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="02c4c-109">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="02c4c-110">Lemezkép feltöltése az Azure-tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="02c4c-110">Uploaded an image to the Azure Container Registry</span></span>
> * <span data-ttu-id="02c4c-111">A tároló-lemezkép az Azure-tároló regisztrációs futtatása</span><span class="sxs-lookup"><span data-stu-id="02c4c-111">Run a container image from the Azure Container Registry</span></span>

<span data-ttu-id="02c4c-112">Az ACS DC/OS-fürt az oktatóanyag lépéseinek végrehajtásához van szüksége.</span><span class="sxs-lookup"><span data-stu-id="02c4c-112">You need an ACS DC/OS cluster to complete the steps in this tutorial.</span></span> <span data-ttu-id="02c4c-113">Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="02c4c-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="02c4c-114">Az oktatóanyaghoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="02c4c-114">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="02c4c-115">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="02c4c-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="02c4c-116">Ha frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="02c4c-116">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="02c4c-117">Telepítse az Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="02c4c-117">Deploy Azure Container Registry</span></span>

<span data-ttu-id="02c4c-118">Szükség esetén hozzon létre egy Azure-tárolóba beállításjegyzéket a [az acr létrehozása](/cli/azure/acr#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="02c4c-118">If needed, create an Azure Container registry with the [az acr create](/cli/azure/acr#create) command.</span></span> 

<span data-ttu-id="02c4c-119">Az alábbi példakód létrehozza a beállításjegyzéket a véletlenszerű előállításához a neve.</span><span class="sxs-lookup"><span data-stu-id="02c4c-119">The following example creates a registry with a randomly generate name.</span></span> <span data-ttu-id="02c4c-120">A beállításjegyzék egy rendszergazdai fiók használatával is konfigurálva van a `--admin-enabled` argumentum.</span><span class="sxs-lookup"><span data-stu-id="02c4c-120">The registry is also configured with an admin account using the `--admin-enabled` argument.</span></span>

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

<span data-ttu-id="02c4c-121">A beállításkulcs létrehozása után az Azure parancssori felület kimenete az alábbihoz hasonló adatok.</span><span class="sxs-lookup"><span data-stu-id="02c4c-121">Once the registry has been created, the Azure CLI outputs data similar to the following.</span></span> <span data-ttu-id="02c4c-122">Vegye figyelembe a `name` és `loginServer`, ezek a későbbi lépésekben használhatók.</span><span class="sxs-lookup"><span data-stu-id="02c4c-122">Take note of the `name` and `loginServer`, these are used in later steps.</span></span>

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

<span data-ttu-id="02c4c-123">A tároló beállításjegyzék hitelesítő adatokat lekérni a [az acr hitelesítő adatok megjelenítése](/cli/azure/acr/credential) parancsot.</span><span class="sxs-lookup"><span data-stu-id="02c4c-123">Get the container registry credentials using the [az acr credential show](/cli/azure/acr/credential) command.</span></span> <span data-ttu-id="02c4c-124">Helyettesítő a `--name` az utolsó lépésben feljegyzett találhatóval.</span><span class="sxs-lookup"><span data-stu-id="02c4c-124">Substitute the `--name` with the one noted in the last step.</span></span> <span data-ttu-id="02c4c-125">Jegyezze fel a egy jelszó, szükség esetén egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="02c4c-125">Take note of one password, it is needed in a later step.</span></span>

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

<span data-ttu-id="02c4c-126">Azure tároló beállításjegyzékkel kapcsolatos további információkért lásd: [Docker-tároló TITKOS nyilvántartó bemutatása](../../container-registry/container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="02c4c-126">For more information on Azure Container Registry, see [Introduction to private Docker container registries](../../container-registry/container-registry-intro.md).</span></span> 

## <a name="manage-acr-authentication"></a><span data-ttu-id="02c4c-127">ACR hitelesítés kezelésére szolgál</span><span class="sxs-lookup"><span data-stu-id="02c4c-127">Manage ACR authentication</span></span>

<span data-ttu-id="02c4c-128">A hagyományos leküldéses és lekéréses kép titkos beállításjegyzékből módja először a beállításjegyzékben a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="02c4c-128">The conventional way to push and pull image from a private registry is to first authenticate with the registry.</span></span> <span data-ttu-id="02c4c-129">Ehhez használja a `docker login` parancs bármely ügyfélnek, amely a saját beállításjegyzék hozzáférésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="02c4c-129">To do so, you would use the `docker login` command on any client that needs to access the private registry.</span></span> <span data-ttu-id="02c4c-130">Mivel a DC/OS-fürtről tartalmazhat sok csomópontok, amelyek kell hitelesíteni a ACR adatokkal, akkor célszerű automatizálható a folyamat egyes csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="02c4c-130">Because a DC/OS cluster can contain many nodes, all of which need to be authenticated with the ACR, it is helpful to automate this process across each node.</span></span> 

### <a name="create-shared-storage"></a><span data-ttu-id="02c4c-131">Megosztott tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="02c4c-131">Create shared storage</span></span>

<span data-ttu-id="02c4c-132">Ez a folyamat használja az Azure fájlmegosztások csatlakoztatva lett a fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="02c4c-132">This process uses an Azure file share that has been mounted on each node in the cluster.</span></span> <span data-ttu-id="02c4c-133">Ha már nem beállított megosztott tárolót, lásd: [a DC/OS fürtben található fájlmegosztás beállítása](container-service-dcos-fileshare.md).</span><span class="sxs-lookup"><span data-stu-id="02c4c-133">If you have not already set up shared storage, see [Setup a file share inside a DC/OS cluster](container-service-dcos-fileshare.md).</span></span>

### <a name="configure-acr-authentication"></a><span data-ttu-id="02c4c-134">ACR hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="02c4c-134">Configure ACR authentication</span></span>

<span data-ttu-id="02c4c-135">Először beolvasása a DC/OS fő teljes Tartománynevét, és tárolható egy változóban.</span><span class="sxs-lookup"><span data-stu-id="02c4c-135">First, get the FQDN of the DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="02c4c-136">Az SSH-kapcsolat létrehozása a főkiszolgáló (vagy az első főkiszolgálójának) a DC/OS-alapú fürt.</span><span class="sxs-lookup"><span data-stu-id="02c4c-136">Create an SSH connection with the master (or the first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="02c4c-137">Frissítse a felhasználó nevét, ha egy nem alapértelmezett érték lett megadva, a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="02c4c-137">Update the user name if a non-default value was used when creating the cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="02c4c-138">A következő parancsot az Azure-tároló beállításjegyzék számára.</span><span class="sxs-lookup"><span data-stu-id="02c4c-138">Run the following command to login to the Azure Container Registry.</span></span> <span data-ttu-id="02c4c-139">Cserélje le a `--username` nevű, a tároló beállításjegyzék és a `--password` a megadott jelszavak egyikével.</span><span class="sxs-lookup"><span data-stu-id="02c4c-139">Replace the `--username` with the name of the container registry, and the `--password` with one of the provided passwords.</span></span> <span data-ttu-id="02c4c-140">Cserélje le az utolsó argumentumnak *mycontainerregistry.azurecr.io* loginServer nevű példájában a tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="02c4c-140">Replace the last argument *mycontainerregistry.azurecr.io* in the example with the loginServer name of the container registry.</span></span> 

<span data-ttu-id="02c4c-141">Ez a parancs hitelesítési értékeit a helyileg tárolja a `~/.docker` elérési útja.</span><span class="sxs-lookup"><span data-stu-id="02c4c-141">This command stores the authentication values locally under the `~/.docker` path.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

<span data-ttu-id="02c4c-142">Hozzon létre egy tömörített fájl, amely tartalmazza a tároló hitelesítési beállításazonosítókat.</span><span class="sxs-lookup"><span data-stu-id="02c4c-142">Create a compressed file that contains the container registry authentication values.</span></span>

```azurecli-interactive
tar czf docker.tar.gz .docker
```

<span data-ttu-id="02c4c-143">Ez a fájl átmásolása a fürt megosztott tároló.</span><span class="sxs-lookup"><span data-stu-id="02c4c-143">Copy this file to the cluster shared storage.</span></span> <span data-ttu-id="02c4c-144">Ebben a lépésben elérhetővé teszi a fájl a DC/OS-fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="02c4c-144">This step makes the file available on all nodes of the DC/OS cluster.</span></span>

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-to-acr"></a><span data-ttu-id="02c4c-145">ACR Rendszerkép feltöltése</span><span class="sxs-lookup"><span data-stu-id="02c4c-145">Upload image to ACR</span></span>

<span data-ttu-id="02c4c-146">Most már a fejlesztési számítógépén, vagy bármely más rendszer Docker telepítve, a lemezkép létrehozása, és töltse fel az Azure-tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="02c4c-146">Now from a development machine, or any other system with Docker installed, create an image and upload it to the Azure Container Registry.</span></span>

<span data-ttu-id="02c4c-147">Hozzon létre egy tárolót a Ubuntu lemezképből.</span><span class="sxs-lookup"><span data-stu-id="02c4c-147">Create a container from the Ubuntu image.</span></span>

```azurecli-interactive
docker run ubunut --name base-image
```

<span data-ttu-id="02c4c-148">Most rögzítése a tárolót az új lemezképet.</span><span class="sxs-lookup"><span data-stu-id="02c4c-148">Now capture the container into a new image.</span></span> <span data-ttu-id="02c4c-149">A lemezkép neve is kell tartalmaznia a `loginServer` neve a tároló registrywith formátuma a `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="02c4c-149">The image name needs to include the `loginServer` name of the container registrywith a format of `loginServer/imageName`.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

<span data-ttu-id="02c4c-150">Bejelentkezés az Azure-tárolót beállításjegyzékbe.</span><span class="sxs-lookup"><span data-stu-id="02c4c-150">Login into the Azure Container Registry.</span></span> <span data-ttu-id="02c4c-151">A név helyére loginServer nevét, a--felhasználónév nevű, a tároló beállításjegyzék és a – a megadott jelszavak egyikével jelszót.</span><span class="sxs-lookup"><span data-stu-id="02c4c-151">Replace the name with the loginServer name, the --username with the name of the container registry, and the --password with one of the provided passwords.</span></span>

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

<span data-ttu-id="02c4c-152">Végezetül feltölti a lemezképet a ACR beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="02c4c-152">Finally, upload the image to the ACR registry.</span></span> <span data-ttu-id="02c4c-153">Ez a példa feltölt egy képet vezénylőtípusú-bemutató nevű.</span><span class="sxs-lookup"><span data-stu-id="02c4c-153">This example uploads an image named dcos-demo.</span></span>

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a><span data-ttu-id="02c4c-154">Futtassa a képfájl ACR</span><span class="sxs-lookup"><span data-stu-id="02c4c-154">Run an image from ACR</span></span>

<span data-ttu-id="02c4c-155">A képfájl ACR beállításjegyzékből való használatához hozzon létre egy fájlt nevek *acrDemo.json* és a következő szöveg másolása.</span><span class="sxs-lookup"><span data-stu-id="02c4c-155">To use an image from the ACR registry, create a file names *acrDemo.json* and copy the following text into it.</span></span> <span data-ttu-id="02c4c-156">Cserélje le a lemezkép neve a beállításjegyzék loginServer Tárolónév és a lemezkép neve, például `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="02c4c-156">Replace the image name with the container registry loginServer name and image name, for example `loginServer/imageName`.</span></span> <span data-ttu-id="02c4c-157">Vegye figyelembe a `uris` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="02c4c-157">Take note of the `uris` property.</span></span> <span data-ttu-id="02c4c-158">Ez a tulajdonság tárolja a tároló beállításjegyzék hitelesítési fájl, amely ebben az esetben a DC/OS-fürt mindegyik csomópontján csatlakoztatott Azure fájlmegosztás helyét.</span><span class="sxs-lookup"><span data-stu-id="02c4c-158">This property holds the location of the container registry authentication file, which in this case is the Azure file share that is mounted on each node in the DC/OS cluster.</span></span>

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

<span data-ttu-id="02c4c-159">A DC/c CLI az alkalmazás központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="02c4c-159">Deploy the application with the DC/OC CLI.</span></span>

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a><span data-ttu-id="02c4c-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02c4c-160">Next steps</span></span>

<span data-ttu-id="02c4c-161">Ebben az oktatóanyagban konfigurálnia kell a DC/OS használata Azure tároló beállításjegyzék, beleértve a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="02c4c-161">In this tutorial you have configure DC/OS to use Azure Container Registry including the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="02c4c-162">Telepítse az Azure-tároló beállításjegyzék, (ha szükséges)</span><span class="sxs-lookup"><span data-stu-id="02c4c-162">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="02c4c-163">A DC/OS-fürtről ACR hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="02c4c-163">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="02c4c-164">Lemezkép feltöltése az Azure-tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="02c4c-164">Uploaded an image to the Azure Container Registry</span></span>
> * <span data-ttu-id="02c4c-165">A tároló-lemezkép az Azure-tároló regisztrációs futtatása</span><span class="sxs-lookup"><span data-stu-id="02c4c-165">Run a container image from the Azure Container Registry</span></span>
