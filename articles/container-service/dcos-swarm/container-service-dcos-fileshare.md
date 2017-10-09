---
title: "Azure DC/OS-fürtről aaaFile megosztást |} Microsoft Docs"
description: "Hozzon létre, és csatlakoztassa egy fájl megosztási tooa DC/OS fürtben az Azure Tárolószolgáltatásban"
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
ms.openlocfilehash: d18090d414a3e00202ccde442ac9b865d74f1e34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-mount-a-file-share-tooa-dcos-cluster"></a><span data-ttu-id="f3973-104">Hozzon létre, és csatlakoztassa egy fájl megosztási tooa DC/OS-fürt</span><span class="sxs-lookup"><span data-stu-id="f3973-104">Create and mount a file share tooa DC/OS cluster</span></span>
<span data-ttu-id="f3973-105">Ez az oktatóanyag részletezi, hogyan toocreate fájl megosztása az Azure-ban, és csatlakoztassa azt minden ügynök és a fő hello DC/OS-fürtről.</span><span class="sxs-lookup"><span data-stu-id="f3973-105">This tutorial details how toocreate a file share in Azure and mount it on each agent and master of hello DC/OS cluster.</span></span> <span data-ttu-id="f3973-106">Fájlmegosztás beállítása teszi egyszerűbbé tooshare fájlok például konfigurációs hozzáférés, naplók vagy többet a fürtön.</span><span class="sxs-lookup"><span data-stu-id="f3973-106">Setting up a file share makes it easier tooshare files across your cluster such as configuration, access, logs, and more.</span></span> <span data-ttu-id="f3973-107">a következő feladatok hello ebben az oktatóanyagban töltik:</span><span class="sxs-lookup"><span data-stu-id="f3973-107">hello following tasks are completed in this tutorial:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f3973-108">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3973-108">Create an Azure storage account</span></span>
> * <span data-ttu-id="f3973-109">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3973-109">Create a file share</span></span>
> * <span data-ttu-id="f3973-110">Hello megosztás hello DC/OS-fürt csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="f3973-110">Mount hello share in hello DC/OS cluster</span></span>

<span data-ttu-id="f3973-111">Az ACS a DC/OS fürtben toocomplete hello lépések ebben az oktatóanyagban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="f3973-111">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="f3973-112">Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="f3973-112">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="f3973-113">Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f3973-113">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="f3973-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="f3973-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f3973-115">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f3973-115">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a><span data-ttu-id="f3973-116">Fájlmegosztás létrehozása a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f3973-116">Create a file share on Microsoft Azure</span></span>

<span data-ttu-id="f3973-117">Az Azure fájlmegosztások használ egy ACS DC/OS-fürtről, mielőtt hello tárolási fiókot és -fájlmegosztást kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f3973-117">Before using an Azure file share with an ACS DC/OS cluster, hello storage account and file share must be created.</span></span> <span data-ttu-id="f3973-118">Futtassa a következő parancsfájl toocreate hello tárolási és -fájlmegosztást hello.</span><span class="sxs-lookup"><span data-stu-id="f3973-118">Run hello following script toocreate hello storage and file share.</span></span> <span data-ttu-id="f3973-119">Frissítse a hello paraméterek thoes a környezetből.</span><span class="sxs-lookup"><span data-stu-id="f3973-119">Update hello parameters with thoes from your environment.</span></span>

```azurecli-interactive
# Change these four parameters
DCOS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
DCOS_PERS_RESOURCE_GROUP=myResourceGroup
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=dcosshare

# Create hello storage account with hello parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export hello connection string as an environment variable, this is used when creating hello Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create hello share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-hello-share-in-your-cluster"></a><span data-ttu-id="f3973-120">A fürt hello megosztás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="f3973-120">Mount hello share in your cluster</span></span>

<span data-ttu-id="f3973-121">A következő hello fájlmegosztás igények toobe csatlakoztatva a fürtben található összes virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="f3973-121">Next, hello file share needs toobe mounted on every virtual machine inside your cluster.</span></span> <span data-ttu-id="f3973-122">A feladat befejezése hello cifs eszköz/protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="f3973-122">This task is completed using hello cifs tool/protocol.</span></span> <span data-ttu-id="f3973-123">hello csatlakoztatási művelet befejezéséhez manuálisan minden egyes csomóponton hello fürt, vagy egy parancsfájl futtatásával hello fürt minden csomópontja ellen.</span><span class="sxs-lookup"><span data-stu-id="f3973-123">hello mount operation can be completed manually on each node of hello cluster, or by running a script against each node in hello cluster.</span></span>

<span data-ttu-id="f3973-124">Ebben a példában két parancsfájlok, egy toomount hello Azure fájl megosztást, és egy második toorun ezt a parancsfájlt hello DC/OS-fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f3973-124">In this example, two scripts are run, one toomount hello Azure file share, and a second toorun this script on each node of hello DC/OS cluster.</span></span>

<span data-ttu-id="f3973-125">Először hello az Azure storage-fiók neve és elérési kulcs van szükség.</span><span class="sxs-lookup"><span data-stu-id="f3973-125">First, hello Azure storage account name, and access key are needed.</span></span> <span data-ttu-id="f3973-126">Futtassa a következő parancsok tooget hello ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="f3973-126">Run hello following commands tooget this information.</span></span> <span data-ttu-id="f3973-127">Jegyezze fel az egyes, ezeket az értékeket használni egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="f3973-127">Take note of each, these values are used in a later step.</span></span>

<span data-ttu-id="f3973-128">Tárfiók nevét:</span><span class="sxs-lookup"><span data-stu-id="f3973-128">Storage account name:</span></span>

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

<span data-ttu-id="f3973-129">Tárfiók hozzáférési kulcsának:</span><span class="sxs-lookup"><span data-stu-id="f3973-129">Storage account access key:</span></span>

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

<span data-ttu-id="f3973-130">A következő get hello hello DC/OS fő és tárolható egy változóban teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="f3973-130">Next, get hello FQDN of hello DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="f3973-131">A titkos kulcs toohello főcsomópont másolja.</span><span class="sxs-lookup"><span data-stu-id="f3973-131">Copy your private key toohello master node.</span></span> <span data-ttu-id="f3973-132">A kulcs mindenképpen szükséges toocreate egy ssh-kapcsolatot hello fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f3973-132">This key is needed toocreate an ssh connection with all nodes in hello cluster.</span></span> <span data-ttu-id="f3973-133">Frissítse a hello felhasználói nevét, ha egy nem alapértelmezett érték lett megadva, amikor hello fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f3973-133">Update hello user name if a non-default value was used when creating hello cluster.</span></span> 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

<span data-ttu-id="f3973-134">Hozzon létre egy SSH-kapcsolat hello főkiszolgáló (vagy hello első főkiszolgálójának) a DC/OS-alapú fürt.</span><span class="sxs-lookup"><span data-stu-id="f3973-134">Create an SSH connection with hello master (or hello first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="f3973-135">Frissítse a hello felhasználói nevét, ha egy nem alapértelmezett érték lett megadva, amikor hello fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f3973-135">Update hello user name if a non-default value was used when creating hello cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="f3973-136">Hozzon létre egy fájlt **cifsMount.sh**, és a következő tartalom másolása hello bele.</span><span class="sxs-lookup"><span data-stu-id="f3973-136">Create a file named **cifsMount.sh**, and copy hello following contents into it.</span></span> 

<span data-ttu-id="f3973-137">Ez a parancsfájl használt toomount hello Azure fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="f3973-137">This script is used toomount hello Azure file share.</span></span> <span data-ttu-id="f3973-138">Frissítés hello `STORAGE_ACCT_NAME` és `ACCESS_KEY` korábban összegyűjtött változók hello adatokkal.</span><span class="sxs-lookup"><span data-stu-id="f3973-138">Update hello `STORAGE_ACCT_NAME` and `ACCESS_KEY` variables with hello information collected earlier.</span></span>

```azurecli-interactive
#!/bin/bash

# Azure storage account name and access key
STORAGE_ACCT_NAME=mystorageaccount
ACCESS_KEY=mystorageaccountKey

# Install hello cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create hello local folder that will contain our share
if [ ! -d "/mnt/share/dcosshare" ]; then sudo mkdir -p "/mnt/share/dcosshare" ; fi

# Mount hello share under hello previous local folder created
sudo mount -t cifs //$STORAGE_ACCT_NAME.file.core.windows.net/dcosshare /mnt/share/dcosshare -o vers=3.0,username=$STORAGE_ACCT_NAME,password=$ACCESS_KEY,dir_mode=0777,file_mode=0777
```
<span data-ttu-id="f3973-139">Hozzon létre egy második fájlt **getNodesRunScript.sh** és a következő tartalom másolása hello hello fájlba.</span><span class="sxs-lookup"><span data-stu-id="f3973-139">Create a second file named **getNodesRunScript.sh** and copy hello following contents into hello file.</span></span> 

<span data-ttu-id="f3973-140">Ez a parancsfájl deríti fel a fürt összes csomópontján, és majd futtatja a hello **cifsMount.sh** parancsfájl toomount hello fájlmegosztást mindegyik.</span><span class="sxs-lookup"><span data-stu-id="f3973-140">This script discovers all cluster nodes, and then runs hello **cifsMount.sh** script toomount hello file share on each.</span></span>

```azurecli-interactive
#!/bin/bash

# Install jq used for hello next command
sudo apt-get install jq -y

# Get hello IP address of each node using hello mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes

# From hello previous file created, run our script toomount our share on each node
cat nodes | while read line
do
  ssh `whoami`@$line -o StrictHostKeyChecking=no < ./cifsMount.sh
  done
```

<span data-ttu-id="f3973-141">Futtassa a hello parancsfájl toomount hello Azure fájlmegosztás hello fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f3973-141">Run hello script toomount hello Azure file share on all nodes of hello cluster.</span></span>

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

<span data-ttu-id="f3973-142">hello fájlmegosztás érhető el, `/mnt/share/dcosshare` hello fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f3973-142">hello file share is now accessible at `/mnt/share/dcosshare` on each node of hello cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3973-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3973-143">Next steps</span></span>

<span data-ttu-id="f3973-144">Ebben az oktatóanyagban az Azure fájlmegosztás lett készült elérhető tooa DC/OS fürtben hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="f3973-144">In this tutorial an Azure file share was made available tooa DC/OS cluster using hello steps:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f3973-145">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3973-145">Create an Azure storage account</span></span>
> * <span data-ttu-id="f3973-146">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3973-146">Create a file share</span></span>
> * <span data-ttu-id="f3973-147">Hello megosztás hello DC/OS-fürt csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="f3973-147">Mount hello share in hello DC/OS cluster</span></span>

<span data-ttu-id="f3973-148">Előzetes toohello oktatóanyag következő toolearn kapcsolatos egy Azure-tároló beállításjegyzék integrálása a DC/OS az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f3973-148">Advance toohello next tutorial toolearn about integrating an Azure Container Registry with DC/OS in Azure.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="f3973-149">Terheléselosztási alkalmazások</span><span class="sxs-lookup"><span data-stu-id="f3973-149">Load balance applications</span></span>](container-service-dcos-acr.md)