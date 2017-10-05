---
title: "Azure Tárolószolgáltatás útmutató – DC/OS kezelése |} Microsoft Docs"
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
ms.openlocfilehash: e93f782c26c32f97749e817ec59ee3c2ecb7e119
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a><span data-ttu-id="00771-104">Azure Tárolószolgáltatás útmutató – DC/OS kezelése</span><span class="sxs-lookup"><span data-stu-id="00771-104">Azure Container Service tutorial - Manage DC/OS</span></span>

<span data-ttu-id="00771-105">A DC/OS elosztott platformot kínál a futó modern és indexelése alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="00771-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="00771-106">Teljes Azure Tárolószolgáltatás egy éles készen áll a DC/OS-fürtben üzembe, akkor egyszerűen és gyorsan.</span><span class="sxs-lookup"><span data-stu-id="00771-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="00771-107">A gyors üzembe helyezési részletek szükséges alapvető lépések a DC/OS-fürtről és futtatási alapvető munkaterhelés központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="00771-107">This quick start details basic steps needed to deploy a DC/OS cluster and run basic workload.</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="00771-108">Az ACS a DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="00771-108">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="00771-109">Csatlakozás a fürthöz</span><span class="sxs-lookup"><span data-stu-id="00771-109">Connect to the cluster</span></span>
> * <span data-ttu-id="00771-110">A DC/OS parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="00771-110">Install the DC/OS CLI</span></span>
> * <span data-ttu-id="00771-111">A fürt alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="00771-111">Deploy an application to the cluster</span></span>
> * <span data-ttu-id="00771-112">Egy alkalmazás méretezni a fürtön</span><span class="sxs-lookup"><span data-stu-id="00771-112">Scale an application on the cluster</span></span>
> * <span data-ttu-id="00771-113">A DC/OS-fürt csomópontjai méretezése</span><span class="sxs-lookup"><span data-stu-id="00771-113">Scale the DC/OS cluster nodes</span></span>
> * <span data-ttu-id="00771-114">Alapszintű DC/OS-kezelés</span><span class="sxs-lookup"><span data-stu-id="00771-114">Basic DC/OS management</span></span>
> * <span data-ttu-id="00771-115">A DC/OS-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="00771-115">Delete the DC/OS cluster</span></span>

<span data-ttu-id="00771-116">Az oktatóanyaghoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="00771-116">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="00771-117">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="00771-117">Run `az --version` to find the version.</span></span> <span data-ttu-id="00771-118">Ha frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="00771-118">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-dcos-cluster"></a><span data-ttu-id="00771-119">A DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="00771-119">Create DC/OS cluster</span></span>

<span data-ttu-id="00771-120">Először hozzon létre egy erőforráscsoport a [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-120">First, create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="00771-121">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="00771-121">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="00771-122">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot a *westeurope* helyen.</span><span class="sxs-lookup"><span data-stu-id="00771-122">The following example creates a resource group named *myResourceGroup* in the *westeurope* location.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="00771-123">Ezután hozzon létre a DC/OS fürtben a [az acs létre](/cli/azure/acs#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-123">Next, create a DC/OS cluster with the [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="00771-124">Az alábbi példakód létrehozza a DC/OS-fürt nevű *myDCOSCluster* és SSH-kulcsok létrehozása, ha még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="00771-124">The following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="00771-125">Ha konkrét kulcsokat szeretné használni, használja az `--ssh-key-value` beállítást.</span><span class="sxs-lookup"><span data-stu-id="00771-125">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="00771-126">Pár perc múlva a parancs befejeződik, és a központi telepítés információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="00771-126">After several minutes, the command completes, and returns information about the deployment.</span></span>

## <a name="connect-to-dcos-cluster"></a><span data-ttu-id="00771-127">Csatlakozzon a DC/OS-fürt</span><span class="sxs-lookup"><span data-stu-id="00771-127">Connect to DC/OS cluster</span></span>

<span data-ttu-id="00771-128">A DC/OS-fürt létrehozása után célszerű egy SSH-alagúton keresztül fér hozzá.</span><span class="sxs-lookup"><span data-stu-id="00771-128">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="00771-129">A következő parancsot a DC/OS-főkiszolgáló nyilvános IP-címét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="00771-129">Run the following command to return the public IP address of the DC/OS master.</span></span> <span data-ttu-id="00771-130">Az IP-cím egy változó tárolja és használja a következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="00771-130">This IP address is stored in a variable and used in the next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="00771-131">Az SSH-alagút létrehozásához a következő parancsot, és kövesse a képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="00771-131">To create the SSH tunnel, run the following command and follow the on-screen instructions.</span></span> <span data-ttu-id="00771-132">Ha a 80-as port már használatban van, a parancs sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="00771-132">If port 80 is already in use, the command fails.</span></span> <span data-ttu-id="00771-133">Frissítse a bújtatott port nincs a használatát, például a `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="00771-133">Update the tunneled port to one not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a><span data-ttu-id="00771-134">DC/OS parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="00771-134">Install DC/OS CLI</span></span>

<span data-ttu-id="00771-135">A DC/OS parancssori felület használatával telepítse a [az acs vezénylőtípusú install-cli](/azure/acs/dcos#install-cli) parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-135">Install the DC/OS cli using the [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="00771-136">Ha Azure CloudShell használ, a DC/OS parancssori felület már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="00771-136">If you are using Azure CloudShell, the DC/OS CLI is already installed.</span></span> <span data-ttu-id="00771-137">Ha az Azure parancssori felület macOS vagy Linux rendszer használata esetén szükség lehet a sudo futtassa a parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-137">If you are running the Azure CLI on macOS or Linux, you might need to run the command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="00771-138">Megelőzően a parancssori felület használható a fürt, az SSH-alagút használatára kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="00771-138">Before the CLI can be used with the cluster, it must be configured to use the SSH tunnel.</span></span> <span data-ttu-id="00771-139">Ehhez futtassa a következő parancsot, és beállítja a port, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="00771-139">To do so, run the following command, adjusting the port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="00771-140">Alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="00771-140">Run an application</span></span>

<span data-ttu-id="00771-141">Az alapértelmezett ütemezés mechanizmus egy ACS DC/OS-fürt Marathon.</span><span class="sxs-lookup"><span data-stu-id="00771-141">The default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="00771-142">Marathon olyan alkalmazást, és a DC/OS-fürtön alkalmazás állapota kezelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="00771-142">Marathon is used to start an application and manage the state of the application on the DC/OS cluster.</span></span> <span data-ttu-id="00771-143">Ütemezés marathon egy alkalmazás, hozzon létre egy fájlt **marathon-app.json**, és a következő tartalom másolása.</span><span class="sxs-lookup"><span data-stu-id="00771-143">To schedule an application through Marathon, create a file named **marathon-app.json**, and copy the following contents into it.</span></span> 

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

<span data-ttu-id="00771-144">A következő parancsot az alkalmazás futtatásához a DC/OS-fürtön ütemezni.</span><span class="sxs-lookup"><span data-stu-id="00771-144">Run the following command to schedule the application to run on the DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="00771-145">Az alkalmazás központi telepítési állapotának megtekintéséhez futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-145">To see the deployment status for the app, run the following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="00771-146">Ha a **feladatok** oszlopérték vált *0 vagy 1* való *1/1*, alkalmazás központi telepítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="00771-146">When the **TASKS** column value switches from *0/1* to *1/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a><span data-ttu-id="00771-147">A Marathon alkalmazás skálázása</span><span class="sxs-lookup"><span data-stu-id="00771-147">Scale Marathon application</span></span>

<span data-ttu-id="00771-148">Az előző példában egy egypéldányos alkalmazás lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="00771-148">In the previous example, a single instance application was created.</span></span> <span data-ttu-id="00771-149">A központi telepítés, hogy az alkalmazás három példányainak frissítéséhez nyissa meg a **marathon-app.json** fájlt, és frissítse a példányt tulajdonságát a 3.</span><span class="sxs-lookup"><span data-stu-id="00771-149">To update this deployment so that three instances of the application are available, open up the **marathon-app.json** file, and update the instance property to 3.</span></span>

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

<span data-ttu-id="00771-150">Az alkalmazás használatával frissítse a `dcos marathon app update` parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-150">Update the application using the `dcos marathon app update` command.</span></span>

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

<span data-ttu-id="00771-151">Az alkalmazás központi telepítési állapotának megtekintéséhez futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-151">To see the deployment status for the app, run the following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="00771-152">Ha a **feladatok** oszlopérték vált *1/3* való *3/1*, alkalmazás központi telepítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="00771-152">When the **TASKS** column value switches from *1/3* to *3/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a><span data-ttu-id="00771-153">Elérhető az internet-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="00771-153">Run internet accessible app</span></span>

<span data-ttu-id="00771-154">Az ACS DC/OS fürtben két csomópont-készlet, az interneten elérhető egy nyilvános és egy saját, amely nem érhető el az interneten áll.</span><span class="sxs-lookup"><span data-stu-id="00771-154">The ACS DC/OS cluster consists of two node sets, one public which is accessible on the internet, and one private which is not accessible on the internet.</span></span> <span data-ttu-id="00771-155">Alapértelmezés szerint az utolsó példában használt a titkos csomópontok.</span><span class="sxs-lookup"><span data-stu-id="00771-155">The default set is the private nodes, which was used in the last example.</span></span>

<span data-ttu-id="00771-156">Ahhoz, hogy az alkalmazás elérhető az interneten, azok a nyilvános csomópont-készlethez.</span><span class="sxs-lookup"><span data-stu-id="00771-156">To make an application accessible on the internet, deploy them to the public node set.</span></span> <span data-ttu-id="00771-157">Ehhez az szükséges, hogy a `acceptedResourceRoles` érték objektum `slave_public`.</span><span class="sxs-lookup"><span data-stu-id="00771-157">To do so, give the `acceptedResourceRoles` object a value of `slave_public`.</span></span>

<span data-ttu-id="00771-158">Hozzon létre egy fájlt **nginx-public.json** és a következő tartalom másolása.</span><span class="sxs-lookup"><span data-stu-id="00771-158">Create a file named **nginx-public.json** and copy the following contents into it.</span></span>

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

<span data-ttu-id="00771-159">A következő parancsot az alkalmazás futtatásához a DC/OS-fürtön ütemezni.</span><span class="sxs-lookup"><span data-stu-id="00771-159">Run the following command to schedule the application to run on the DC/OS cluster.</span></span>

```azurecli 
dcos marathon app add nginx-public.json
```

<span data-ttu-id="00771-160">A nyilvános IP-cím a DC/OS nyilvános fürt ügynökök beolvasni.</span><span class="sxs-lookup"><span data-stu-id="00771-160">Get the public IP address of the DC/OS public cluster agents.</span></span>

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="00771-161">Az alapértelmezett NGINX hely keresse meg ezt a címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="00771-161">Browsing to this address returns the default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a><span data-ttu-id="00771-163">Skála DC/OS-fürtről</span><span class="sxs-lookup"><span data-stu-id="00771-163">Scale DC/OS cluster</span></span>

<span data-ttu-id="00771-164">A fenti példákban egy alkalmazás több példánya lett méretezhető.</span><span class="sxs-lookup"><span data-stu-id="00771-164">In the previous examples, an application was scaled to multiple instance.</span></span> <span data-ttu-id="00771-165">A DC/OS-infrastruktúra is méretezhetők, amelyek több vagy kevesebb számítási kapacitást biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="00771-165">The DC/OS infrastructure can also be scaled to provide more or less compute capacity.</span></span> <span data-ttu-id="00771-166">Ez a lépés a [az acs méretezése]() parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-166">This is done with the [az acs scale]() command.</span></span> 

<span data-ttu-id="00771-167">A DC/OS-ügynökök jelenlegi száma megjelenítéséhez használja a [az acs megjelenítése](/cli/azure/acs#show) parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-167">To see the current count of DC/OS agents, use the [az acs show](/cli/azure/acs#show) command.</span></span>

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

<span data-ttu-id="00771-168">5 számának növeléséhez használja a [az acs méretezése](/cli/azure/acs#scale) parancsot.</span><span class="sxs-lookup"><span data-stu-id="00771-168">To increase the count to 5, use the [az acs scale](/cli/azure/acs#scale) command.</span></span> 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a><span data-ttu-id="00771-169">A DC/OS-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="00771-169">Delete DC/OS cluster</span></span>

<span data-ttu-id="00771-170">Ha már nincs szüksége, használhatja a [az csoport törlése](/cli/azure/group#delete) parancs eltávolítja az erőforráscsoportot, a DC/OS-fürtről, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="00771-170">When no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="00771-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00771-171">Next steps</span></span>

<span data-ttu-id="00771-172">Ebben az oktatóprogramban megtanulhatta, kapcsolatos alapvető DC/OS fájlkezelési feladat, többek között a következőket.</span><span class="sxs-lookup"><span data-stu-id="00771-172">In this tutorial, you have learned about basic DC/OS management task including the following.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="00771-173">Az ACS a DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="00771-173">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="00771-174">Csatlakozás a fürthöz</span><span class="sxs-lookup"><span data-stu-id="00771-174">Connect to the cluster</span></span>
> * <span data-ttu-id="00771-175">A DC/OS parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="00771-175">Install the DC/OS CLI</span></span>
> * <span data-ttu-id="00771-176">A fürt alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="00771-176">Deploy an application to the cluster</span></span>
> * <span data-ttu-id="00771-177">Egy alkalmazás méretezni a fürtön</span><span class="sxs-lookup"><span data-stu-id="00771-177">Scale an application on the cluster</span></span>
> * <span data-ttu-id="00771-178">A DC/OS-fürt csomópontjai méretezése</span><span class="sxs-lookup"><span data-stu-id="00771-178">Scale the DC/OS cluster nodes</span></span>
> * <span data-ttu-id="00771-179">A DC/OS-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="00771-179">Delete the DC/OS cluster</span></span>

<span data-ttu-id="00771-180">Továbblépés a következő oktatóanyag terheléselosztási a DC/OS Azure alkalmazás megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="00771-180">Advance to the next tutorial to learn about load balancing application in DC/OS on Azure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="00771-181">Terheléselosztási alkalmazások</span><span class="sxs-lookup"><span data-stu-id="00771-181">Load balance applications</span></span>](container-service-load-balancing.md)