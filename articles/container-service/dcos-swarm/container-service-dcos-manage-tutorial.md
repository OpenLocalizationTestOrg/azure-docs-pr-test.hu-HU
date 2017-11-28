---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – DC/OS kezelése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató – DC/OS kezelése"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b91c433bfd7e48ec405cc62be1486d9d4662839d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a><span data-ttu-id="19a8a-104">Azure Tárolószolgáltatás útmutató – DC/OS kezelése</span><span class="sxs-lookup"><span data-stu-id="19a8a-104">Azure Container Service tutorial - Manage DC/OS</span></span>

<span data-ttu-id="19a8a-105">A DC/OS elosztott platformot kínál a futó modern és indexelése alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="19a8a-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="19a8a-106">Teljes Azure Tárolószolgáltatás egy éles készen áll a DC/OS-fürtben üzembe, akkor egyszerűen és gyorsan.</span><span class="sxs-lookup"><span data-stu-id="19a8a-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="19a8a-107">A gyors üzembe helyezési részletek lépéseken toodeploy szükséges, a DC/OS-fürtről és a Futtatás alapvető munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="19a8a-107">This quick start details basic steps needed toodeploy a DC/OS cluster and run basic workload.</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="19a8a-108">Az ACS a DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="19a8a-108">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="19a8a-109">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="19a8a-109">Connect toohello cluster</span></span>
> * <span data-ttu-id="19a8a-110">Hello DC/OS parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="19a8a-110">Install hello DC/OS CLI</span></span>
> * <span data-ttu-id="19a8a-111">Az alkalmazás toohello fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="19a8a-111">Deploy an application toohello cluster</span></span>
> * <span data-ttu-id="19a8a-112">Alkalmazás skálázása hello fürtön</span><span class="sxs-lookup"><span data-stu-id="19a8a-112">Scale an application on hello cluster</span></span>
> * <span data-ttu-id="19a8a-113">A DC/OS-fürt csomópontjai hello méretezése</span><span class="sxs-lookup"><span data-stu-id="19a8a-113">Scale hello DC/OS cluster nodes</span></span>
> * <span data-ttu-id="19a8a-114">Alapszintű DC/OS-kezelés</span><span class="sxs-lookup"><span data-stu-id="19a8a-114">Basic DC/OS management</span></span>
> * <span data-ttu-id="19a8a-115">Hello DC/OS-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="19a8a-115">Delete hello DC/OS cluster</span></span>

<span data-ttu-id="19a8a-116">Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="19a8a-116">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="19a8a-117">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="19a8a-117">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="19a8a-118">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="19a8a-118">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-dcos-cluster"></a><span data-ttu-id="19a8a-119">A DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="19a8a-119">Create DC/OS cluster</span></span>

<span data-ttu-id="19a8a-120">Először hozzon létre egy erőforráscsoportot a hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-120">First, create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="19a8a-121">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="19a8a-121">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="19a8a-122">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westeurope* helyét.</span><span class="sxs-lookup"><span data-stu-id="19a8a-122">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="19a8a-123">Következő lépésként hozzon létre egy DC/OS-fürtről hello [az acs létre](/cli/azure/acs#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-123">Next, create a DC/OS cluster with hello [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="19a8a-124">hello alábbi példakód létrehozza a DC/OS-fürt nevű *myDCOSCluster* és SSH-kulcsok létrehozása, ha még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="19a8a-124">hello following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="19a8a-125">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="19a8a-125">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="19a8a-126">Pár perc múlva hello parancs befejeződik, és hello telepítési információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="19a8a-126">After several minutes, hello command completes, and returns information about hello deployment.</span></span>

## <a name="connect-toodcos-cluster"></a><span data-ttu-id="19a8a-127">Csatlakozás tooDC/OS-fürtről</span><span class="sxs-lookup"><span data-stu-id="19a8a-127">Connect tooDC/OS cluster</span></span>

<span data-ttu-id="19a8a-128">A DC/OS-fürt létrehozása után célszerű egy SSH-alagúton keresztül fér hozzá.</span><span class="sxs-lookup"><span data-stu-id="19a8a-128">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="19a8a-129">Futtassa a következő parancs tooreturn hello nyilvános IP-cím hello DC/OS főkiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="19a8a-129">Run hello following command tooreturn hello public IP address of hello DC/OS master.</span></span> <span data-ttu-id="19a8a-130">Az IP-cím egy változó tárolja, és hello következő lépésben szükség.</span><span class="sxs-lookup"><span data-stu-id="19a8a-130">This IP address is stored in a variable and used in hello next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="19a8a-131">toocreate hello SSH-alagút, futtassa a következő parancs hello és hello képernyőn megjelenő útmutatást.</span><span class="sxs-lookup"><span data-stu-id="19a8a-131">toocreate hello SSH tunnel, run hello following command and follow hello on-screen instructions.</span></span> <span data-ttu-id="19a8a-132">Ha a 80-as port már használatban van, hello parancs sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="19a8a-132">If port 80 is already in use, hello command fails.</span></span> <span data-ttu-id="19a8a-133">Frissítés hello bújtatott port tooone nincsenek használatban, például a `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="19a8a-133">Update hello tunneled port tooone not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a><span data-ttu-id="19a8a-134">DC/OS parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="19a8a-134">Install DC/OS CLI</span></span>

<span data-ttu-id="19a8a-135">Hello DC/OS parancssori felület használatával hello telepítése [az acs vezénylőtípusú install-cli](/azure/acs/dcos#install-cli) parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-135">Install hello DC/OS cli using hello [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="19a8a-136">Ha Azure CloudShell használ, a DC/OS parancssori felület hello már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="19a8a-136">If you are using Azure CloudShell, hello DC/OS CLI is already installed.</span></span> <span data-ttu-id="19a8a-137">Ha macOS vagy Linux hello Azure CLI-t futtat, szükség lehet a sudo toorun hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-137">If you are running hello Azure CLI on macOS or Linux, you might need toorun hello command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="19a8a-138">Hello CLI hello fürttel használható, mielőtt konfigurált toouse hello SSH-alagút kell lennie.</span><span class="sxs-lookup"><span data-stu-id="19a8a-138">Before hello CLI can be used with hello cluster, it must be configured toouse hello SSH tunnel.</span></span> <span data-ttu-id="19a8a-139">toodo úgy, futtassa a következő parancsot, hello port be, szükség esetén hello.</span><span class="sxs-lookup"><span data-stu-id="19a8a-139">toodo so, run hello following command, adjusting hello port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="19a8a-140">Alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="19a8a-140">Run an application</span></span>

<span data-ttu-id="19a8a-141">az ACS a DC/OS-fürt mechanizmus ütemezés hello alapértelmezett érték a Marathon.</span><span class="sxs-lookup"><span data-stu-id="19a8a-141">hello default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="19a8a-142">A Marathon használt toostart alkalmazás, és kezelhető hello állapot hello alkalmazás hello DC/OS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="19a8a-142">Marathon is used toostart an application and manage hello state of hello application on hello DC/OS cluster.</span></span> <span data-ttu-id="19a8a-143">tooschedule marathon, alkalmazás, hozzon létre egy fájlt **marathon-app.json**, és a következő tartalom másolása hello bele.</span><span class="sxs-lookup"><span data-stu-id="19a8a-143">tooschedule an application through Marathon, create a file named **marathon-app.json**, and copy hello following contents into it.</span></span> 

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

<span data-ttu-id="19a8a-144">Futtassa a parancsot tooschedule hello alkalmazás toorun hello DC/OS-fürtön a következő hello.</span><span class="sxs-lookup"><span data-stu-id="19a8a-144">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="19a8a-145">toosee hello központi telepítési állapotának hello alkalmazást, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="19a8a-145">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="19a8a-146">Ha hello **feladatok** oszlopérték vált *0 vagy 1* túl*1/1*, alkalmazás központi telepítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="19a8a-146">When hello **TASKS** column value switches from *0/1* too*1/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a><span data-ttu-id="19a8a-147">A Marathon alkalmazás skálázása</span><span class="sxs-lookup"><span data-stu-id="19a8a-147">Scale Marathon application</span></span>

<span data-ttu-id="19a8a-148">Hello előző példában egy egypéldányos alkalmazás lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="19a8a-148">In hello previous example, a single instance application was created.</span></span> <span data-ttu-id="19a8a-149">a központi telepítés, hogy hello alkalmazás három példányát érhetők el, megnyithatja hello tooupdate **marathon-app.json** fájlt, és hello példány tulajdonság too3 frissítése.</span><span class="sxs-lookup"><span data-stu-id="19a8a-149">tooupdate this deployment so that three instances of hello application are available, open up hello **marathon-app.json** file, and update hello instance property too3.</span></span>

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

<span data-ttu-id="19a8a-150">Hello segítségével hello alkalmazás frissítése `dcos marathon app update` parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-150">Update hello application using hello `dcos marathon app update` command.</span></span>

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

<span data-ttu-id="19a8a-151">toosee hello központi telepítési állapotának hello alkalmazást, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="19a8a-151">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="19a8a-152">Ha hello **feladatok** oszlopérték vált *1/3* túl*3/1*, alkalmazás központi telepítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="19a8a-152">When hello **TASKS** column value switches from *1/3* too*3/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a><span data-ttu-id="19a8a-153">Elérhető az internet-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="19a8a-153">Run internet accessible app</span></span>

<span data-ttu-id="19a8a-154">hello ACS DC/OS-fürtről áll két csomópont-készlet, amely elérhető egy nyilvános hello internet, és egy saját, amely nem érhető el az internet hello.</span><span class="sxs-lookup"><span data-stu-id="19a8a-154">hello ACS DC/OS cluster consists of two node sets, one public which is accessible on hello internet, and one private which is not accessible on hello internet.</span></span> <span data-ttu-id="19a8a-155">Alapértelmezés szerint hello hello titkos csomópontok, hello utolsó példában használt.</span><span class="sxs-lookup"><span data-stu-id="19a8a-155">hello default set is hello private nodes, which was used in hello last example.</span></span>

<span data-ttu-id="19a8a-156">toomake egy alkalmazás elérhető az hello internet, azok toohello nyilvános csomópont-készletet.</span><span class="sxs-lookup"><span data-stu-id="19a8a-156">toomake an application accessible on hello internet, deploy them toohello public node set.</span></span> <span data-ttu-id="19a8a-157">toodo úgy, hogy hello `acceptedResourceRoles` érték objektum `slave_public`.</span><span class="sxs-lookup"><span data-stu-id="19a8a-157">toodo so, give hello `acceptedResourceRoles` object a value of `slave_public`.</span></span>

<span data-ttu-id="19a8a-158">Hozzon létre egy fájlt **nginx-public.json** és a következő tartalom másolása hello bele.</span><span class="sxs-lookup"><span data-stu-id="19a8a-158">Create a file named **nginx-public.json** and copy hello following contents into it.</span></span>

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

<span data-ttu-id="19a8a-159">Futtassa a parancsot tooschedule hello alkalmazás toorun hello DC/OS-fürtön a következő hello.</span><span class="sxs-lookup"><span data-stu-id="19a8a-159">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli 
dcos marathon app add nginx-public.json
```

<span data-ttu-id="19a8a-160">Nyilvános IP-cím hello hello DC/OS-fürt nyilvános ügynökök beolvasni.</span><span class="sxs-lookup"><span data-stu-id="19a8a-160">Get hello public IP address of hello DC/OS public cluster agents.</span></span>

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="19a8a-161">Hello alapértelmezett NGINX webhely böngészési toothis címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="19a8a-161">Browsing toothis address returns hello default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a><span data-ttu-id="19a8a-163">Skála DC/OS-fürtről</span><span class="sxs-lookup"><span data-stu-id="19a8a-163">Scale DC/OS cluster</span></span>

<span data-ttu-id="19a8a-164">Az előző példákban hello alkalmazás méretezett toomultiple példány volt.</span><span class="sxs-lookup"><span data-stu-id="19a8a-164">In hello previous examples, an application was scaled toomultiple instance.</span></span> <span data-ttu-id="19a8a-165">hello DC/OS infrastruktúra méretezett tooprovide hosszabb vagy rövidebb számítási kapacitás is lehet.</span><span class="sxs-lookup"><span data-stu-id="19a8a-165">hello DC/OS infrastructure can also be scaled tooprovide more or less compute capacity.</span></span> <span data-ttu-id="19a8a-166">Ez a lépés hello [az acs méretezése]() parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-166">This is done with hello [az acs scale]() command.</span></span> 

<span data-ttu-id="19a8a-167">toosee hello aktuális száma a DC/OS-ügynökök, használja a hello [az acs megjelenítése](/cli/azure/acs#show) parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-167">toosee hello current count of DC/OS agents, use hello [az acs show](/cli/azure/acs#show) command.</span></span>

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

<span data-ttu-id="19a8a-168">tooincrease hello száma too5, használja a hello [az acs méretezése](/cli/azure/acs#scale) parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-168">tooincrease hello count too5, use hello [az acs scale](/cli/azure/acs#scale) command.</span></span> 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a><span data-ttu-id="19a8a-169">A DC/OS-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="19a8a-169">Delete DC/OS cluster</span></span>

<span data-ttu-id="19a8a-170">Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a DC/OS-fürtről és az összes kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="19a8a-170">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="19a8a-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19a8a-171">Next steps</span></span>

<span data-ttu-id="19a8a-172">Ebben az oktatóprogramban megtanulhatta, kapcsolatos alapvető DC/OS fájlkezelési feladat hello következőket beleértve.</span><span class="sxs-lookup"><span data-stu-id="19a8a-172">In this tutorial, you have learned about basic DC/OS management task including hello following.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="19a8a-173">Az ACS a DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="19a8a-173">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="19a8a-174">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="19a8a-174">Connect toohello cluster</span></span>
> * <span data-ttu-id="19a8a-175">Hello DC/OS parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="19a8a-175">Install hello DC/OS CLI</span></span>
> * <span data-ttu-id="19a8a-176">Az alkalmazás toohello fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="19a8a-176">Deploy an application toohello cluster</span></span>
> * <span data-ttu-id="19a8a-177">Alkalmazás skálázása hello fürtön</span><span class="sxs-lookup"><span data-stu-id="19a8a-177">Scale an application on hello cluster</span></span>
> * <span data-ttu-id="19a8a-178">A DC/OS-fürt csomópontjai hello méretezése</span><span class="sxs-lookup"><span data-stu-id="19a8a-178">Scale hello DC/OS cluster nodes</span></span>
> * <span data-ttu-id="19a8a-179">Hello DC/OS-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="19a8a-179">Delete hello DC/OS cluster</span></span>

<span data-ttu-id="19a8a-180">Előzetes toohello következő útmutató toolearn kapcsolatos betölteni a DC/OS Azure terheléselosztási alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="19a8a-180">Advance toohello next tutorial toolearn about load balancing application in DC/OS on Azure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="19a8a-181">Terheléselosztási alkalmazások</span><span class="sxs-lookup"><span data-stu-id="19a8a-181">Load balance applications</span></span>](container-service-load-balancing.md)