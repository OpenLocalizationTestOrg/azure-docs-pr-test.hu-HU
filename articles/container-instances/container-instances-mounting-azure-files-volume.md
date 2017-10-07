---
title: "az Azure-fájlok kötet Azure tároló példányát aaaMounting"
description: "Ismerje meg, hogyan toomount egy Azure-fájlokat Azure tároló osztályt kötet toopersist állapota"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: d87215e06d5e5af40bfebcad17768ee45ccabbb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a><span data-ttu-id="07f7c-103">Azure-tároló osztályt az Azure fájlmegosztások csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="07f7c-103">Mounting an Azure file share with Azure Container Instances</span></span>

<span data-ttu-id="07f7c-104">Alapértelmezés szerint Azure tároló példányok állapot nélküli alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="07f7c-104">By default, Azure Container Instances are stateless.</span></span> <span data-ttu-id="07f7c-105">Ha hello tároló összeomlik, vagy leállítja, annak teljes állapota elvész.</span><span class="sxs-lookup"><span data-stu-id="07f7c-105">If hello container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="07f7c-106">toopersist állapot hello tároló hello élettartama meghaladja egy kötetet csatlakoztatnia kell egy külső áruházból.</span><span class="sxs-lookup"><span data-stu-id="07f7c-106">toopersist state beyond hello lifetime of hello container, you must mount a volume from an external store.</span></span> <span data-ttu-id="07f7c-107">Ez a cikk bemutatja, hogyan toomount egy Azure fájlmegosztás Azure tároló példányok való használatra.</span><span class="sxs-lookup"><span data-stu-id="07f7c-107">This article shows how toomount an Azure file share for use with Azure Container Instances.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="07f7c-108">Az Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="07f7c-108">Create an Azure file share</span></span>

<span data-ttu-id="07f7c-109">Az Azure fájlmegosztások használatához Azure tároló osztályt, akkor létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="07f7c-109">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="07f7c-110">Futtassa a következő parancsfájl toocreate hello a tárolási fiók toohost hello fájlmegosztás és hello megosztás saját magát.</span><span class="sxs-lookup"><span data-stu-id="07f7c-110">Run hello following script toocreate a storage account toohost hello file share and hello share itself.</span></span> <span data-ttu-id="07f7c-111">Vegye figyelembe, hogy hello tárfiók neve csak globálisan egyedi, így hello parancsfájl hozzáad egy véletlenszerű értéket toohello kezdőpontját meghatározó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="07f7c-111">Note that hello storage account name must be globally unique, so hello script adds a random value toohello base string.</span></span>

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create hello storage account with hello parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export hello connection string as an environment variable, this is used when creating hello Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create hello share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a><span data-ttu-id="07f7c-112">Szerezzen be tárfiókadatok hozzáférés</span><span class="sxs-lookup"><span data-stu-id="07f7c-112">Acquire storage account access details</span></span>

<span data-ttu-id="07f7c-113">egy Azure fájlmegosztás kötetként Azure tároló példányát toomount, szüksége három értékekre: hello tárfióknév hello megosztásnevet és hello tárelérési kulcs.</span><span class="sxs-lookup"><span data-stu-id="07f7c-113">toomount an Azure file share as a volume in Azure Container Instances, you need three values: hello storage account name, hello share name, and hello storage access key.</span></span> 

<span data-ttu-id="07f7c-114">Ha követte a fenti hello parancsfájl, hello tárfióknév hello végén véletlenszerű értéket hozták létre.</span><span class="sxs-lookup"><span data-stu-id="07f7c-114">If you used hello script above, hello storage account name was created with a random value at hello end.</span></span> <span data-ttu-id="07f7c-115">tooquery hello végső karakterláncban (többek között a következőket hello véletlenszerű része), a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="07f7c-115">tooquery hello final string (including hello random portion), use hello following commands:</span></span>

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="07f7c-116">hello megosztásnév már ismert (Ez *acishare* hello parancsfájlban a fenti), így most már hello tárfiók hívóbetűjét, amely segítségével található összes hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="07f7c-116">hello share name is already known (it is *acishare* in hello script above), so all that remains is hello storage account key, which can be found using hello following command:</span></span>

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a><span data-ttu-id="07f7c-117">Hozzáférés tárfiókadatok együtt az Azure key vault tárolásához</span><span class="sxs-lookup"><span data-stu-id="07f7c-117">Store storage account access details with Azure key vault</span></span>

<span data-ttu-id="07f7c-118">Tárfiókkulcsok adatvédelemben hozzáférés tooyour, ezért azt javasoljuk, egy az Azure key vault tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="07f7c-118">Storage account keys protect access tooyour data, so we recommend storing them in an Azure key vault.</span></span> 

<span data-ttu-id="07f7c-119">Hozzon létre egy kulcstartót hello Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="07f7c-119">Create a key vault with hello Azure CLI:</span></span>

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

<span data-ttu-id="07f7c-120">Hello `enabled-for-template-deployment` kapcsoló lehetővé teszi, hogy az Azure Resource Manager toopull titkokat a kulcstartót a központi telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="07f7c-120">hello `enabled-for-template-deployment` switch allows Azure Resource Manager toopull secrets from your key vault at deployment time.</span></span>

<span data-ttu-id="07f7c-121">Az új titkos kulcsot a key vaultban hello hello tárfiók kulcsa tárolni:</span><span class="sxs-lookup"><span data-stu-id="07f7c-121">Store hello storage account key as a new secret in hello key vault:</span></span>

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-hello-volume"></a><span data-ttu-id="07f7c-122">Hello kötet csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="07f7c-122">Mount hello volume</span></span>

<span data-ttu-id="07f7c-123">Az Azure fájlmegosztások csatlakoztatása a tárolóban lévő kötetként két lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="07f7c-123">Mounting an Azure file share as a volume in a container is a two-step process.</span></span> <span data-ttu-id="07f7c-124">Először adjon hello részletek hello megosztás definiálása hello a tárolócsoport, majd egy vagy több hello tárolók hello csoport csatlakoztatott hello kötet módjának megadása.</span><span class="sxs-lookup"><span data-stu-id="07f7c-124">First, you provide hello details of hello share as part of defining hello container group, then you specify how you want hello volume mounted within one or more of hello containers in hello group.</span></span>

<span data-ttu-id="07f7c-125">toodefine hello kötetek toomake érhető el elhelyezésre, használni szeretne hozzáadni egy `volumes` tömb toohello tartalmazó csoport definícióban hello Azure Resource Manager sablon, majd hivatkozunk ezekre hello egyes tárolók hello meghatározását.</span><span class="sxs-lookup"><span data-stu-id="07f7c-125">toodefine hello volumes you want toomake available for mounting, add a `volumes` array toohello container group definition in hello Azure Resource Manager template, then reference them in hello definition of hello individual containers.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "requests": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "ports": [{
            "port": 80
          }],
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }  
      }],
      "osType": "Linux",
      "ipAddress": {
        "type": "Public",
        "ports": [{
          "protocol": "tcp",
          "port": "80"
        }]
      },
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
          "shareName": "acishare",
          "storageAccountName": "[parameters('storageaccountname')]",
          "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

<span data-ttu-id="07f7c-126">hello sablon paraméterként, amely külön paraméterfájl megadható hello tárolási fióknevet és kulcsot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="07f7c-126">hello template includes hello storage account name and key as parameters, which can be provided in a separate parameters file.</span></span> <span data-ttu-id="07f7c-127">toopopulate hello paraméterfájl, szüksége lesz értékek: hello a tárfiók nevét, az az Azure key vault erőforrás-azonosító hello és hello titkos kulcs toostore hello használt kulcstároló neve.</span><span class="sxs-lookup"><span data-stu-id="07f7c-127">toopopulate hello parameters file, you will need three values: hello storage account name, hello resource ID of your Azure key vault, and hello key vault secret name that you used toostore hello storage key.</span></span> <span data-ttu-id="07f7c-128">Ha már elvégezte az előző lépéseket, kaphat ezeket az értékeket a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="07f7c-128">If you have followed previous steps, you can get these values with hello following script:</span></span>

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

<span data-ttu-id="07f7c-129">Hello értékek beszúrása hello paraméterek fájlba:</span><span class="sxs-lookup"><span data-stu-id="07f7c-129">Insert hello values into hello parameters file:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },    
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-hello-container-and-manage-files"></a><span data-ttu-id="07f7c-130">Hello tároló üzembe és kezelhet fájlokat</span><span class="sxs-lookup"><span data-stu-id="07f7c-130">Deploy hello container and manage files</span></span>

<span data-ttu-id="07f7c-131">Meghatározott hello sablonnal hello tárolókat hozhat létre, és a csatlakoztatási a kötet hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="07f7c-131">With hello template defined, you can create hello container and mount its volume using hello Azure CLI.</span></span> <span data-ttu-id="07f7c-132">Feltételezve, hogy hello sablon fájl neve *azuredeploy.json* adott hello paraméterek fájl neve és *azuredeploy.parameters.json*, majd hello a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="07f7c-132">Assuming that hello template file is named *azuredeploy.json* and that hello parameters file is named *azuredeploy.parameters.json*, then hello command line is:</span></span>

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

<span data-ttu-id="07f7c-133">Miután hello tároló elindul, hello egyszerű webalkalmazást telepített hello keresztül használhatja **seanmckenna/aci-hellofiles** kép, toohello hello Azure fájlmegosztás megadott hello csatlakoztatási elérési úton lévő fájlok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="07f7c-133">Once hello container starts up, you can use hello simple web app deployed via hello **seanmckenna/aci-hellofiles** image, toohello manage files in hello Azure file share at hello mount path that you specified.</span></span> <span data-ttu-id="07f7c-134">Hello webalkalmazás keresztül hello következő hello IP-cím beszerzése:</span><span class="sxs-lookup"><span data-stu-id="07f7c-134">Obtain hello ip address for hello web app via hello following:</span></span>

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

<span data-ttu-id="07f7c-135">Egy eszköz, például hello használhatja [Microsoft Azure Tártallózó](http://storageexplorer.com) tooretrieve és hello fájl writen toohello fájlmegosztás vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="07f7c-135">You can use a tool like hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) tooretrieve and inspect hello file writen toohello file share.</span></span>

>[!NOTE]
> <span data-ttu-id="07f7c-136">További információ az Azure Resource Manager-sablonokkal, alkalmazásparaméter-fájlokat, és a hello Azure parancssori felület telepítése toolearn lásd [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](../azure-resource-manager/resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="07f7c-136">toolearn more about using Azure Resource Manager templates, parameter files, and deploying with hello Azure CLI, see [Deploy resources with Resource Manager templates and Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="07f7c-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07f7c-137">Next steps</span></span>

- <span data-ttu-id="07f7c-138">Az első tároló hello Azure tároló példányok használatával telepítheti [– első lépések](container-instances-quickstart.md)</span><span class="sxs-lookup"><span data-stu-id="07f7c-138">Deploy for your first container using hello Azure Container Instances [quick start](container-instances-quickstart.md)</span></span>
- <span data-ttu-id="07f7c-139">További tudnivalók: hello [Azure tároló példányok és tároló orchestrators közötti kapcsolat](container-instances-orchestrator-relationship.md)</span><span class="sxs-lookup"><span data-stu-id="07f7c-139">Learn about hello [relationship between Azure Container Instances and container orchestrators](container-instances-orchestrator-relationship.md)</span></span>
