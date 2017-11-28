---
title: "Azure DC/OS-fürtről fájlmegosztásához |} Microsoft Docs"
description: "Hozzon létre, és azt csatlakoztatja a fájlmegosztást a DC/OS fürtben, az Azure Tárolószolgáltatásban"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, Micro-szolgáltatások, a Mesos, a Azure, a fájlmegosztás, a cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 549b52bfb0a0268f754da26c6a374b267861f6d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-mount-a-file-share-to-a-dcos-cluster"></a><span data-ttu-id="e811d-104">Hozzon létre, és azt csatlakoztatja a fájlmegosztást a DC/OS-fürtről</span><span class="sxs-lookup"><span data-stu-id="e811d-104">Create and mount a file share to a DC/OS cluster</span></span>
<span data-ttu-id="e811d-105">Ez az oktatóanyag fájlmegosztás létrehozása az Azure-ban, és csatlakoztassa azt minden ügynök és a DC/OS-fürt fő részletezi.</span><span class="sxs-lookup"><span data-stu-id="e811d-105">This tutorial details how to create a file share in Azure and mount it on each agent and master of the DC/OS cluster.</span></span> <span data-ttu-id="e811d-106">Fájlmegosztás beállítása megkönnyíti a fürt, például a konfigurációs, access, naplók és egyéb fájlok megosztása.</span><span class="sxs-lookup"><span data-stu-id="e811d-106">Setting up a file share makes it easier to share files across your cluster such as configuration, access, logs, and more.</span></span> <span data-ttu-id="e811d-107">Ebben az oktatóanyagban a következő műveleteket foglalja:</span><span class="sxs-lookup"><span data-stu-id="e811d-107">The following tasks are completed in this tutorial:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e811d-108">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e811d-108">Create an Azure storage account</span></span>
> * <span data-ttu-id="e811d-109">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e811d-109">Create a file share</span></span>
> * <span data-ttu-id="e811d-110">A DC/OS fürtben a megosztás csatlakoztatásához</span><span class="sxs-lookup"><span data-stu-id="e811d-110">Mount the share in the DC/OS cluster</span></span>

<span data-ttu-id="e811d-111">Az ACS DC/OS-fürt az oktatóanyag lépéseinek végrehajtásához van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e811d-111">You need an ACS DC/OS cluster to complete the steps in this tutorial.</span></span> <span data-ttu-id="e811d-112">Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="e811d-112">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="e811d-113">Az oktatóanyaghoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="e811d-113">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="e811d-114">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="e811d-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="e811d-115">Ha frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e811d-115">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a><span data-ttu-id="e811d-116">Fájlmegosztás létrehozása a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e811d-116">Create a file share on Microsoft Azure</span></span>

<span data-ttu-id="e811d-117">Az Azure fájlmegosztások használ egy ACS DC/OS-fürtről, mielőtt a storage-fiókot és -fájlmegosztást kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e811d-117">Before using an Azure file share with an ACS DC/OS cluster, the storage account and file share must be created.</span></span> <span data-ttu-id="e811d-118">Az alábbi parancsprogrammal hozzon létre a tároló és a fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="e811d-118">Run the following script to create the storage and file share.</span></span> <span data-ttu-id="e811d-119">Frissítse a paraméterek thoes a környezetből.</span><span class="sxs-lookup"><span data-stu-id="e811d-119">Update the parameters with thoes from your environment.</span></span>

```azurecli-interactive
# Change these four parameters
DCOS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
DCOS_PERS_RESOURCE_GROUP=myResourceGroup
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=dcosshare

# Create the storage account with the parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-the-share-in-your-cluster"></a><span data-ttu-id="e811d-120">A fürt a megosztás csatlakoztatásához</span><span class="sxs-lookup"><span data-stu-id="e811d-120">Mount the share in your cluster</span></span>

<span data-ttu-id="e811d-121">Ezután a fájlmegosztást kell csatlakoztatni kell a fürtben található összes virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="e811d-121">Next, the file share needs to be mounted on every virtual machine inside your cluster.</span></span> <span data-ttu-id="e811d-122">Ez a feladat befejeződött, a cifs eszköz/protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="e811d-122">This task is completed using the cifs tool/protocol.</span></span> <span data-ttu-id="e811d-123">A szalagcsatlakoztatási művelet manuálisan minden csomóponton, a fürt, vagy a fürt minden csomópontja elleni parancsfájl futtatásával is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="e811d-123">The mount operation can be completed manually on each node of the cluster, or by running a script against each node in the cluster.</span></span>

<span data-ttu-id="e811d-124">Ebben a példában két parancsfájlok, egy csatlakoztatása az Azure-fájlmegosztáshoz, és egy második, a parancsfájl futtatásához a DC/OS-fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="e811d-124">In this example, two scripts are run, one to mount the Azure file share, and a second to run this script on each node of the DC/OS cluster.</span></span>

<span data-ttu-id="e811d-125">Először a az Azure storage-fiók neve és elérési kulcs van szükség.</span><span class="sxs-lookup"><span data-stu-id="e811d-125">First, the Azure storage account name, and access key are needed.</span></span> <span data-ttu-id="e811d-126">Ezek az információk beolvasása a következő parancsok futtatásával.</span><span class="sxs-lookup"><span data-stu-id="e811d-126">Run the following commands to get this information.</span></span> <span data-ttu-id="e811d-127">Jegyezze fel az egyes, ezeket az értékeket használni egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="e811d-127">Take note of each, these values are used in a later step.</span></span>

<span data-ttu-id="e811d-128">Tárfiók nevét:</span><span class="sxs-lookup"><span data-stu-id="e811d-128">Storage account name:</span></span>

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

<span data-ttu-id="e811d-129">Tárfiók hozzáférési kulcsának:</span><span class="sxs-lookup"><span data-stu-id="e811d-129">Storage account access key:</span></span>

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

<span data-ttu-id="e811d-130">A következő beolvasása a DC/OS fő teljes Tartománynevét, és tárolható egy változóban.</span><span class="sxs-lookup"><span data-stu-id="e811d-130">Next, get the FQDN of the DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="e811d-131">Másolja a titkos kulcsot a fő csomópont.</span><span class="sxs-lookup"><span data-stu-id="e811d-131">Copy your private key to the master node.</span></span> <span data-ttu-id="e811d-132">Ez a kulcs létrehozásához szükséges egy ssh-kapcsolatot a fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="e811d-132">This key is needed to create an ssh connection with all nodes in the cluster.</span></span> <span data-ttu-id="e811d-133">Frissítse a felhasználó nevét, ha egy nem alapértelmezett érték lett megadva, a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="e811d-133">Update the user name if a non-default value was used when creating the cluster.</span></span> 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

<span data-ttu-id="e811d-134">Az SSH-kapcsolat létrehozása a főkiszolgáló (vagy az első főkiszolgálójának) a DC/OS-alapú fürt.</span><span class="sxs-lookup"><span data-stu-id="e811d-134">Create an SSH connection with the master (or the first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="e811d-135">Frissítse a felhasználó nevét, ha egy nem alapértelmezett érték lett megadva, a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="e811d-135">Update the user name if a non-default value was used when creating the cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="e811d-136">Hozzon létre egy fájlt **cifsMount.sh**, és a következő tartalom másolása.</span><span class="sxs-lookup"><span data-stu-id="e811d-136">Create a file named **cifsMount.sh**, and copy the following contents into it.</span></span> 

<span data-ttu-id="e811d-137">Ez a parancsfájl Azure fájlmegosztás csatlakoztatásához használatos.</span><span class="sxs-lookup"><span data-stu-id="e811d-137">This script is used to mount the Azure file share.</span></span> <span data-ttu-id="e811d-138">Frissítés a `STORAGE_ACCT_NAME` és `ACCESS_KEY` korábban összegyűjtött változóit.</span><span class="sxs-lookup"><span data-stu-id="e811d-138">Update the `STORAGE_ACCT_NAME` and `ACCESS_KEY` variables with the information collected earlier.</span></span>

```azurecli-interactive
#!/bin/bash

# Azure storage account name and access key
STORAGE_ACCT_NAME=mystorageaccount
ACCESS_KEY=mystorageaccountKey

# Install the cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create the local folder that will contain our share
if [ ! -d "/mnt/share/dcosshare" ]; then sudo mkdir -p "/mnt/share/dcosshare" ; fi

# Mount the share under the previous local folder created
sudo mount -t cifs //$STORAGE_ACCT_NAME.file.core.windows.net/dcosshare /mnt/share/dcosshare -o vers=3.0,username=$STORAGE_ACCT_NAME,password=$ACCESS_KEY,dir_mode=0777,file_mode=0777
```
<span data-ttu-id="e811d-139">Hozzon létre egy második fájlt **getNodesRunScript.sh** és másolja az alábbiakat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="e811d-139">Create a second file named **getNodesRunScript.sh** and copy the following contents into the file.</span></span> 

<span data-ttu-id="e811d-140">Ez a parancsfájl deríti fel a fürt összes csomópontján, és majd futtatja a **cifsMount.sh** parancsfájl minden egyes a fájlmegosztás csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="e811d-140">This script discovers all cluster nodes, and then runs the **cifsMount.sh** script to mount the file share on each.</span></span>

```azurecli-interactive
#!/bin/bash

# Install jq used for the next command
sudo apt-get install jq -y

# Get the IP address of each node using the mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes

# From the previous file created, run our script to mount our share on each node
cat nodes | while read line
do
  ssh `whoami`@$line -o StrictHostKeyChecking=no < ./cifsMount.sh
  done
```

<span data-ttu-id="e811d-141">Futtassa a parancsfájlt a fürt összes csomópontján Azure fájlmegosztás csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="e811d-141">Run the script to mount the Azure file share on all nodes of the cluster.</span></span>

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

<span data-ttu-id="e811d-142">A fájlmegosztás mostantól elérhető a `/mnt/share/dcosshare` a fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="e811d-142">The file share is now accessible at `/mnt/share/dcosshare` on each node of the cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e811d-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e811d-143">Next steps</span></span>

<span data-ttu-id="e811d-144">Ebben az oktatóanyagban az Azure fájlmegosztás lett elérhetővé tenni az a DC/OS fürtben, a lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="e811d-144">In this tutorial an Azure file share was made available to a DC/OS cluster using the steps:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e811d-145">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e811d-145">Create an Azure storage account</span></span>
> * <span data-ttu-id="e811d-146">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e811d-146">Create a file share</span></span>
> * <span data-ttu-id="e811d-147">A DC/OS fürtben a megosztás csatlakoztatásához</span><span class="sxs-lookup"><span data-stu-id="e811d-147">Mount the share in the DC/OS cluster</span></span>

<span data-ttu-id="e811d-148">A következő oktatóanyag további információt az Azure-tároló beállításjegyzék integrálása az Azure-ban a DC/OS továbblépés.</span><span class="sxs-lookup"><span data-stu-id="e811d-148">Advance to the next tutorial to learn about integrating an Azure Container Registry with DC/OS in Azure.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="e811d-149">Terheléselosztási alkalmazások</span><span class="sxs-lookup"><span data-stu-id="e811d-149">Load balance applications</span></span>](container-service-dcos-acr.md)