---
title: "aaaAzure tároló szolgáltatás gyors üzembe helyezés – DC/OS-fürt központi telepítése |} Microsoft Docs"
description: "Azure-tárolót szolgáltatás gyors üzembe helyezés – DC/OS-fürt központi telepítése"
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
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b961f15bd73deeafda5a3fc25ab53c839195219b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-dcos-cluster"></a><span data-ttu-id="630d5-104">A DC/OS-fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="630d5-104">Deploy a DC/OS cluster</span></span>

<span data-ttu-id="630d5-105">A DC/OS elosztott platformot kínál a futó modern és indexelése alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="630d5-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="630d5-106">Teljes Azure Tárolószolgáltatás egy éles készen áll a DC/OS-fürtben üzembe, akkor egyszerűen és gyorsan.</span><span class="sxs-lookup"><span data-stu-id="630d5-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="630d5-107">A gyors üzembe helyezési részletek hello lépéseken toodeploy szükséges, a DC/OS-fürtről és a Futtatás alapvető munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="630d5-107">This quick start details hello basic steps needed toodeploy a DC/OS cluster and run basic workload.</span></span>

<span data-ttu-id="630d5-108">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="630d5-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="630d5-109">Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="630d5-109">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="630d5-110">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="630d5-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="630d5-111">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="630d5-111">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="log-in-tooazure"></a><span data-ttu-id="630d5-112">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="630d5-112">Log in tooAzure</span></span> 

<span data-ttu-id="630d5-113">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="630d5-113">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="630d5-114">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="630d5-114">Create a resource group</span></span>

<span data-ttu-id="630d5-115">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="630d5-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="630d5-116">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="630d5-116">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="630d5-117">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="630d5-117">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a><span data-ttu-id="630d5-118">A DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="630d5-118">Create DC/OS cluster</span></span>

<span data-ttu-id="630d5-119">Hozzon létre egy DC/OS-fürtről hello [az acs létre](/cli/azure/acs#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="630d5-119">Create a DC/OS cluster with hello [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="630d5-120">hello alábbi példakód létrehozza a DC/OS-fürt nevű *myDCOSCluster* és SSH-kulcsok létrehozása, ha még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="630d5-120">hello following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="630d5-121">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="630d5-121">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="630d5-122">Pár perc múlva hello parancs befejeződik, és hello telepítési információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="630d5-122">After several minutes, hello command completes, and returns information about hello deployment.</span></span>

## <a name="connect-toodcos-cluster"></a><span data-ttu-id="630d5-123">Csatlakozás tooDC/OS-fürtről</span><span class="sxs-lookup"><span data-stu-id="630d5-123">Connect tooDC/OS cluster</span></span>

<span data-ttu-id="630d5-124">A DC/OS-fürt létrehozása után célszerű egy SSH-alagúton keresztül fér hozzá.</span><span class="sxs-lookup"><span data-stu-id="630d5-124">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="630d5-125">Futtassa a következő parancs tooreturn hello nyilvános IP-cím hello DC/OS főkiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="630d5-125">Run hello following command tooreturn hello public IP address of hello DC/OS master.</span></span> <span data-ttu-id="630d5-126">Az IP-cím egy változó tárolja, és hello következő lépésben szükség.</span><span class="sxs-lookup"><span data-stu-id="630d5-126">This IP address is stored in a variable and used in hello next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="630d5-127">toocreate hello SSH-alagút, futtassa a következő parancs hello és hello képernyőn megjelenő útmutatást.</span><span class="sxs-lookup"><span data-stu-id="630d5-127">toocreate hello SSH tunnel, run hello following command and follow hello on-screen instructions.</span></span> <span data-ttu-id="630d5-128">Ha a 80-as port már használatban van, hello parancs sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="630d5-128">If port 80 is already in use, hello command fails.</span></span> <span data-ttu-id="630d5-129">Frissítés hello bújtatott port tooone nincsenek használatban, például a `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="630d5-129">Update hello tunneled port tooone not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

<span data-ttu-id="630d5-130">SSH-alagút hello tesztelhető tallózással túl`http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="630d5-130">hello SSH tunnel can be tested by browsing too`http://localhost`.</span></span> <span data-ttu-id="630d5-131">Ha egy portot más, a 80-as használták, állítsa be a hello hely toomatch.</span><span class="sxs-lookup"><span data-stu-id="630d5-131">If a port other that 80 has been used, adjust hello location toomatch.</span></span> 

<span data-ttu-id="630d5-132">Ha hello SSH-alagút létrehozása sikeresen megtörtént, hello DC/OS portal ad vissza.</span><span class="sxs-lookup"><span data-stu-id="630d5-132">If hello SSH tunnel was successfully created, hello DC/OS portal is returned.</span></span>

![VEZÉNYLŐTÍPUSÚ FELHASZNÁLÓI FELÜLET](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a><span data-ttu-id="630d5-134">DC/OS parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="630d5-134">Install DC/OS CLI</span></span>

<span data-ttu-id="630d5-135">a DC/OS parancssori felület hello használt toomanage a DC/OS parancssori hello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="630d5-135">hello DC/OS command line interface is used toomanage a DC/OS cluster from hello command-line.</span></span> <span data-ttu-id="630d5-136">Hello DC/OS parancssori felület használatával hello telepítése [az acs vezénylőtípusú install-cli](/azure/acs/dcos#install-cli) parancsot.</span><span class="sxs-lookup"><span data-stu-id="630d5-136">Install hello DC/OS cli using hello [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="630d5-137">Ha Azure CloudShell használ, a DC/OS parancssori felület hello már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="630d5-137">If you are using Azure CloudShell, hello DC/OS CLI is already installed.</span></span> 

<span data-ttu-id="630d5-138">Ha macOS vagy Linux hello Azure CLI-t futtat, szükség lehet a sudo toorun hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="630d5-138">If you are running hello Azure CLI on macOS or Linux, you might need toorun hello command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="630d5-139">Hello CLI hello fürttel használható, mielőtt konfigurált toouse hello SSH-alagút kell lennie.</span><span class="sxs-lookup"><span data-stu-id="630d5-139">Before hello CLI can be used with hello cluster, it must be configured toouse hello SSH tunnel.</span></span> <span data-ttu-id="630d5-140">toodo úgy, futtassa a következő parancsot, hello port be, szükség esetén hello.</span><span class="sxs-lookup"><span data-stu-id="630d5-140">toodo so, run hello following command, adjusting hello port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="630d5-141">Alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="630d5-141">Run an application</span></span>

<span data-ttu-id="630d5-142">az ACS a DC/OS-fürt mechanizmus ütemezés hello alapértelmezett érték a Marathon.</span><span class="sxs-lookup"><span data-stu-id="630d5-142">hello default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="630d5-143">A Marathon használt toostart alkalmazás, és kezelhető hello állapot hello alkalmazás hello DC/OS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="630d5-143">Marathon is used toostart an application and manage hello state of hello application on hello DC/OS cluster.</span></span> <span data-ttu-id="630d5-144">tooschedule marathon, alkalmazás, hozzon létre egy fájlt *marathon-app.json*, és a következő tartalom másolása hello bele.</span><span class="sxs-lookup"><span data-stu-id="630d5-144">tooschedule an application through Marathon, create a file named *marathon-app.json*, and copy hello following contents into it.</span></span> 

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

<span data-ttu-id="630d5-145">Futtassa a parancsot tooschedule hello alkalmazás toorun hello DC/OS-fürtön a következő hello.</span><span class="sxs-lookup"><span data-stu-id="630d5-145">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="630d5-146">toosee hello központi telepítési állapotának hello alkalmazást, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="630d5-146">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="630d5-147">Ha hello **Várakozás** oszlopérték vált *igaz* túl*hamis*, alkalmazás központi telepítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="630d5-147">When hello **WAITING** column value switches from *True* too*False*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

<span data-ttu-id="630d5-148">Nyilvános IP-címe hello hello DC/OS-fürt ügynökök beolvasni.</span><span class="sxs-lookup"><span data-stu-id="630d5-148">Get hello public IP address of hello DC/OS cluster agents.</span></span>

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="630d5-149">Hello alapértelmezett NGINX webhely böngészési toothis címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="630d5-149">Browsing toothis address returns hello default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a><span data-ttu-id="630d5-151">A DC/OS-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="630d5-151">Delete DC/OS cluster</span></span>

<span data-ttu-id="630d5-152">Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a DC/OS-fürtről és az összes kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="630d5-152">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="630d5-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="630d5-153">Next steps</span></span>

<span data-ttu-id="630d5-154">A gyors üzembe helyezési a DC/OS-fürt telepítése után, és egy egyszerű Docker-tároló hello fürtön futtattak.</span><span class="sxs-lookup"><span data-stu-id="630d5-154">In this quick start, you’ve deployed a DC/OS cluster and have run a simple Docker container on hello cluster.</span></span> <span data-ttu-id="630d5-155">További információ az Azure Tárolószolgáltatás toolearn toohello ACS oktatóanyagok továbbra is.</span><span class="sxs-lookup"><span data-stu-id="630d5-155">toolearn more about Azure Container Service, continue toohello ACS tutorials.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="630d5-156">Az ACS a DC/OS-fürt kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="630d5-156">Manage an ACS DC/OS Cluster</span></span>](container-service-dcos-manage-tutorial.md)