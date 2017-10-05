---
title: "Azure-tároló példányát az Azure fájlok kötet csatlakoztatása"
description: "Útmutató: Azure tároló osztályt állapot megőrizni az Azure fájlok kötet csatlakoztatása"
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
ms.openlocfilehash: 4248a3769ba8a0fb067b3904d55d487fe67e5778
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a><span data-ttu-id="46571-103">Azure-tároló osztályt az Azure fájlmegosztások csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="46571-103">Mounting an Azure file share with Azure Container Instances</span></span>

<span data-ttu-id="46571-104">Alapértelmezés szerint Azure tároló példányok állapot nélküli alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="46571-104">By default, Azure Container Instances are stateless.</span></span> <span data-ttu-id="46571-105">Ha a tároló összeomlik, vagy leállítja, annak teljes állapota elvész.</span><span class="sxs-lookup"><span data-stu-id="46571-105">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="46571-106">Állapot élettartama meghaladja a tároló megőrizni, a kötet csatlakoztatnia kell külső áruházban.</span><span class="sxs-lookup"><span data-stu-id="46571-106">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span> <span data-ttu-id="46571-107">Ez a cikk bemutatja, hogyan használható Azure tároló osztályt egy Azure fájlmegosztás csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="46571-107">This article shows how to mount an Azure file share for use with Azure Container Instances.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="46571-108">Az Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="46571-108">Create an Azure file share</span></span>

<span data-ttu-id="46571-109">Az Azure fájlmegosztások használatához Azure tároló osztályt, akkor létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="46571-109">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="46571-110">A következő parancsprogrammal hozzon létre egy tárfiókot, a fájlmegosztás és a megosztás saját magát.</span><span class="sxs-lookup"><span data-stu-id="46571-110">Run the following script to create a storage account to host the file share and the share itself.</span></span> <span data-ttu-id="46571-111">Vegye figyelembe, hogy a tárfiók neve csak globálisan egyedi, a parancsfájl egy véletlenszerű értékét hozzáadja a kezdőpontját meghatározó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="46571-111">Note that the storage account name must be globally unique, so the script adds a random value to the base string.</span></span>

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a><span data-ttu-id="46571-112">Szerezzen be tárfiókadatok hozzáférés</span><span class="sxs-lookup"><span data-stu-id="46571-112">Acquire storage account access details</span></span>

<span data-ttu-id="46571-113">Az Azure fájlmegosztások csatlakoztatása kötetként az Azure-tároló példányok, három értékek kell: a tárfiók nevét, a megosztás neve és a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="46571-113">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage access key.</span></span> 

<span data-ttu-id="46571-114">Ha követte a fenti parancsfájl, a tárfiók nevének végén véletlenszerű értéket hozták létre.</span><span class="sxs-lookup"><span data-stu-id="46571-114">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="46571-115">A végső karakterláncban (beleértve a véletlenszerű része) lekérdezéséhez használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="46571-115">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="46571-116">A megosztásnév már ismert (Ez *acishare* a parancsfájlban), hogy most már a tárfiók hívóbetűjét, amely a következő paranccsal található összes:</span><span class="sxs-lookup"><span data-stu-id="46571-116">The share name is already known (it is *acishare* in the script above), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a><span data-ttu-id="46571-117">Hozzáférés tárfiókadatok együtt az Azure key vault tárolásához</span><span class="sxs-lookup"><span data-stu-id="46571-117">Store storage account access details with Azure key vault</span></span>

<span data-ttu-id="46571-118">Tárfiókkulcsok védi a adatokhoz való hozzáférés, ezért javasoljuk, hogy az az Azure key vault tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="46571-118">Storage account keys protect access to your data, so we recommend storing them in an Azure key vault.</span></span> 

<span data-ttu-id="46571-119">Kulcstároló létrehozása az Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="46571-119">Create a key vault with the Azure CLI:</span></span>

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

<span data-ttu-id="46571-120">A `enabled-for-template-deployment` kapcsoló nyújt a Azure Resource Manager lekéréses titkokat a kulcstartót a központi telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="46571-120">The `enabled-for-template-deployment` switch allows Azure Resource Manager to pull secrets from your key vault at deployment time.</span></span>

<span data-ttu-id="46571-121">A tárfiók hívóbetűjét, az új titkos kulcsot a key vaultban. tároló:</span><span class="sxs-lookup"><span data-stu-id="46571-121">Store the storage account key as a new secret in the key vault:</span></span>

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-the-volume"></a><span data-ttu-id="46571-122">A kötet csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="46571-122">Mount the volume</span></span>

<span data-ttu-id="46571-123">Az Azure fájlmegosztások csatlakoztatása a tárolóban lévő kötetként két lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="46571-123">Mounting an Azure file share as a volume in a container is a two-step process.</span></span> <span data-ttu-id="46571-124">Először akkor adja meg a megosztást a tárolócsoport definiálása adatait, majd egy vagy több csoport tárolók csatlakoztatott kötet módjának megadása.</span><span class="sxs-lookup"><span data-stu-id="46571-124">First, you provide the details of the share as part of defining the container group, then you specify how you want the volume mounted within one or more of the containers in the group.</span></span>

<span data-ttu-id="46571-125">A kötetek csatlakoztatása az elérhetővé tenni kívánt megadásához adja hozzá a `volumes` a tömb a tároló meghatározása az Azure Resource Manager sablon, majd hivatkozhat őket az egyes tárolókban definíciójában.</span><span class="sxs-lookup"><span data-stu-id="46571-125">To define the volumes you want to make available for mounting, add a `volumes` array to the container group definition in the Azure Resource Manager template, then reference them in the definition of the individual containers.</span></span>

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

<span data-ttu-id="46571-126">A sablon tartalmazza a tárfiók nevét és a kulcs biztosítható, hogy egy külön paraméterek fájlban paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="46571-126">The template includes the storage account name and key as parameters, which can be provided in a separate parameters file.</span></span> <span data-ttu-id="46571-127">A paraméterek fájl feltöltéséhez, szüksége lesz a következő három érték: a tárfiók nevét, az erőforrás-azonosítója a az Azure key vault, és a titkos kulcstároló neve, a kulcs tárolására használt.</span><span class="sxs-lookup"><span data-stu-id="46571-127">To populate the parameters file, you will need three values: the storage account name, the resource ID of your Azure key vault, and the key vault secret name that you used to store the storage key.</span></span> <span data-ttu-id="46571-128">Ha már elvégezte az előző lépéseket, ezeket az értékeket a következő parancsfájl kaphat:</span><span class="sxs-lookup"><span data-stu-id="46571-128">If you have followed previous steps, you can get these values with the following script:</span></span>

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

<span data-ttu-id="46571-129">Szúrja be az értékeket a paraméterek fájlba:</span><span class="sxs-lookup"><span data-stu-id="46571-129">Insert the values into the parameters file:</span></span>

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

## <a name="deploy-the-container-and-manage-files"></a><span data-ttu-id="46571-130">A tároló üzembe és kezelhet fájlokat</span><span class="sxs-lookup"><span data-stu-id="46571-130">Deploy the container and manage files</span></span>

<span data-ttu-id="46571-131">A sablon definiált a tároló létrehozása, és csatlakoztassa a kötetet, az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="46571-131">With the template defined, you can create the container and mount its volume using the Azure CLI.</span></span> <span data-ttu-id="46571-132">Feltételezve, hogy a sablon fájl neve *azuredeploy.json* és a paraméterfájlban nevű *azuredeploy.parameters.json*, akkor a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="46571-132">Assuming that the template file is named *azuredeploy.json* and that the parameters file is named *azuredeploy.parameters.json*, then the command line is:</span></span>

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

<span data-ttu-id="46571-133">A tároló elindul, ha a egyszerű webalkalmazást telepített keresztül is használhatja a **seanmckenna/aci-hellofiles** lemezképet, a megadott csatlakoztatási elérési úton Azure fájlmegosztás kezelése a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="46571-133">Once the container starts up, you can use the simple web app deployed via the **seanmckenna/aci-hellofiles** image, to the manage files in the Azure file share at the mount path that you specified.</span></span> <span data-ttu-id="46571-134">Az IP-cím a következő webalkalmazás az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="46571-134">Obtain the ip address for the web app via the following:</span></span>

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

<span data-ttu-id="46571-135">Egy eszköz, például használhatja a [Microsoft Azure Tártallózó](http://storageexplorer.com) kérhető le, és vizsgálja meg a fájl writen a fájlmegosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="46571-135">You can use a tool like the [Microsoft Azure Storage Explorer](http://storageexplorer.com) to retrieve and inspect the file writen to the file share.</span></span>

>[!NOTE]
> <span data-ttu-id="46571-136">Azure Resource Manager-sablonok használatával kapcsolatos további tudnivalókért alkalmazásparaméter-fájlokat, és az Azure parancssori Felülettel történő telepítése, lásd: [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](../azure-resource-manager/resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="46571-136">To learn more about using Azure Resource Manager templates, parameter files, and deploying with the Azure CLI, see [Deploy resources with Resource Manager templates and Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="46571-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46571-137">Next steps</span></span>

- <span data-ttu-id="46571-138">Az első tároló használata az Azure-tároló példányok telepítése [– első lépések](container-instances-quickstart.md)</span><span class="sxs-lookup"><span data-stu-id="46571-138">Deploy for your first container using the Azure Container Instances [quick start](container-instances-quickstart.md)</span></span>
- <span data-ttu-id="46571-139">További tudnivalók a [Azure tároló példányok és tároló orchestrators közötti kapcsolat](container-instances-orchestrator-relationship.md)</span><span class="sxs-lookup"><span data-stu-id="46571-139">Learn about the [relationship between Azure Container Instances and container orchestrators](container-instances-orchestrator-relationship.md)</span></span>
