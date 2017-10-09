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
# <a name="create-and-mount-a-file-share-tooa-dcos-cluster"></a>Hozzon létre, és csatlakoztassa egy fájl megosztási tooa DC/OS-fürt
Ez az oktatóanyag részletezi, hogyan toocreate fájl megosztása az Azure-ban, és csatlakoztassa azt minden ügynök és a fő hello DC/OS-fürtről. Fájlmegosztás beállítása teszi egyszerűbbé tooshare fájlok például konfigurációs hozzáférés, naplók vagy többet a fürtön. a következő feladatok hello ebben az oktatóanyagban töltik:

> [!div class="checklist"]
> * Azure-tárfiók létrehozása
> * Fájlmegosztás létrehozása
> * Hello megosztás hello DC/OS-fürt csatlakoztatása

Az ACS a DC/OS fürtben toocomplete hello lépések ebben az oktatóanyagban van szüksége. Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.

Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a>Fájlmegosztás létrehozása a Microsoft Azure

Az Azure fájlmegosztások használ egy ACS DC/OS-fürtről, mielőtt hello tárolási fiókot és -fájlmegosztást kell létrehozni. Futtassa a következő parancsfájl toocreate hello tárolási és -fájlmegosztást hello. Frissítse a hello paraméterek thoes a környezetből.

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

## <a name="mount-hello-share-in-your-cluster"></a>A fürt hello megosztás csatlakoztatása

A következő hello fájlmegosztás igények toobe csatlakoztatva a fürtben található összes virtuális gépen. A feladat befejezése hello cifs eszköz/protokoll használatával. hello csatlakoztatási művelet befejezéséhez manuálisan minden egyes csomóponton hello fürt, vagy egy parancsfájl futtatásával hello fürt minden csomópontja ellen.

Ebben a példában két parancsfájlok, egy toomount hello Azure fájl megosztást, és egy második toorun ezt a parancsfájlt hello DC/OS-fürt mindegyik csomópontján.

Először hello az Azure storage-fiók neve és elérési kulcs van szükség. Futtassa a következő parancsok tooget hello ezt az információt. Jegyezze fel az egyes, ezeket az értékeket használni egy későbbi lépésben.

Tárfiók nevét:

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

Tárfiók hozzáférési kulcsának:

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

A következő get hello hello DC/OS fő és tárolható egy változóban teljes Tartományneve.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

A titkos kulcs toohello főcsomópont másolja. A kulcs mindenképpen szükséges toocreate egy ssh-kapcsolatot hello fürt összes csomópontján. Frissítse a hello felhasználói nevét, ha egy nem alapértelmezett érték lett megadva, amikor hello fürtöt hoz létre. 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

Hozzon létre egy SSH-kapcsolat hello főkiszolgáló (vagy hello első főkiszolgálójának) a DC/OS-alapú fürt. Frissítse a hello felhasználói nevét, ha egy nem alapértelmezett érték lett megadva, amikor hello fürtöt hoz létre.

```azurecli-interactive
ssh azureuser@$FQDN
```

Hozzon létre egy fájlt **cifsMount.sh**, és a következő tartalom másolása hello bele. 

Ez a parancsfájl használt toomount hello Azure fájlmegosztás. Frissítés hello `STORAGE_ACCT_NAME` és `ACCESS_KEY` korábban összegyűjtött változók hello adatokkal.

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
Hozzon létre egy második fájlt **getNodesRunScript.sh** és a következő tartalom másolása hello hello fájlba. 

Ez a parancsfájl deríti fel a fürt összes csomópontján, és majd futtatja a hello **cifsMount.sh** parancsfájl toomount hello fájlmegosztást mindegyik.

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

Futtassa a hello parancsfájl toomount hello Azure fájlmegosztás hello fürt összes csomópontján.

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

hello fájlmegosztás érhető el, `/mnt/share/dcosshare` hello fürt mindegyik csomópontján.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az Azure fájlmegosztás lett készült elérhető tooa DC/OS fürtben hello lépéseket követve:

> [!div class="checklist"]
> * Azure-tárfiók létrehozása
> * Fájlmegosztás létrehozása
> * Hello megosztás hello DC/OS-fürt csatlakoztatása

Előzetes toohello oktatóanyag következő toolearn kapcsolatos egy Azure-tároló beállításjegyzék integrálása a DC/OS az Azure-ban.  

> [!div class="nextstepaction"]
> [Terheléselosztási alkalmazások](container-service-dcos-acr.md)