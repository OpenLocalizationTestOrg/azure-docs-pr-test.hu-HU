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
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Azure-tároló osztályt az Azure fájlmegosztások csatlakoztatása

Alapértelmezés szerint Azure tároló példányok állapot nélküli alkalmazások. Ha hello tároló összeomlik, vagy leállítja, annak teljes állapota elvész. toopersist állapot hello tároló hello élettartama meghaladja egy kötetet csatlakoztatnia kell egy külső áruházból. Ez a cikk bemutatja, hogyan toomount egy Azure fájlmegosztás Azure tároló példányok való használatra.

## <a name="create-an-azure-file-share"></a>Az Azure-fájlmegosztás létrehozása

Az Azure fájlmegosztások használatához Azure tároló osztályt, akkor létre kell hoznia. Futtassa a következő parancsfájl toocreate hello a tárolási fiók toohost hello fájlmegosztás és hello megosztás saját magát. Vegye figyelembe, hogy hello tárfiók neve csak globálisan egyedi, így hello parancsfájl hozzáad egy véletlenszerű értéket toohello kezdőpontját meghatározó karakterlánc.

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

## <a name="acquire-storage-account-access-details"></a>Szerezzen be tárfiókadatok hozzáférés

egy Azure fájlmegosztás kötetként Azure tároló példányát toomount, szüksége három értékekre: hello tárfióknév hello megosztásnevet és hello tárelérési kulcs. 

Ha követte a fenti hello parancsfájl, hello tárfióknév hello végén véletlenszerű értéket hozták létre. tooquery hello végső karakterláncban (többek között a következőket hello véletlenszerű része), a következő parancsok hello használata:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

hello megosztásnév már ismert (Ez *acishare* hello parancsfájlban a fenti), így most már hello tárfiók hívóbetűjét, amely segítségével található összes hello a következő parancsot:

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Hozzáférés tárfiókadatok együtt az Azure key vault tárolásához

Tárfiókkulcsok adatvédelemben hozzáférés tooyour, ezért azt javasoljuk, egy az Azure key vault tárolja őket. 

Hozzon létre egy kulcstartót hello Azure parancssori felület:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

Hello `enabled-for-template-deployment` kapcsoló lehetővé teszi, hogy az Azure Resource Manager toopull titkokat a kulcstartót a központi telepítéskor.

Az új titkos kulcsot a key vaultban hello hello tárfiók kulcsa tárolni:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-hello-volume"></a>Hello kötet csatlakoztatása

Az Azure fájlmegosztások csatlakoztatása a tárolóban lévő kötetként két lépésből áll. Először adjon hello részletek hello megosztás definiálása hello a tárolócsoport, majd egy vagy több hello tárolók hello csoport csatlakoztatott hello kötet módjának megadása.

toodefine hello kötetek toomake érhető el elhelyezésre, használni szeretne hozzáadni egy `volumes` tömb toohello tartalmazó csoport definícióban hello Azure Resource Manager sablon, majd hivatkozunk ezekre hello egyes tárolók hello meghatározását.

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

hello sablon paraméterként, amely külön paraméterfájl megadható hello tárolási fióknevet és kulcsot tartalmaz. toopopulate hello paraméterfájl, szüksége lesz értékek: hello a tárfiók nevét, az az Azure key vault erőforrás-azonosító hello és hello titkos kulcs toostore hello használt kulcstároló neve. Ha már elvégezte az előző lépéseket, kaphat ezeket az értékeket a következő parancsfájl hello:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Hello értékek beszúrása hello paraméterek fájlba:

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

## <a name="deploy-hello-container-and-manage-files"></a>Hello tároló üzembe és kezelhet fájlokat

Meghatározott hello sablonnal hello tárolókat hozhat létre, és a csatlakoztatási a kötet hello Azure parancssori felület használatával. Feltételezve, hogy hello sablon fájl neve *azuredeploy.json* adott hello paraméterek fájl neve és *azuredeploy.parameters.json*, majd hello a parancssorban:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

Miután hello tároló elindul, hello egyszerű webalkalmazást telepített hello keresztül használhatja **seanmckenna/aci-hellofiles** kép, toohello hello Azure fájlmegosztás megadott hello csatlakoztatási elérési úton lévő fájlok kezeléséhez. Hello webalkalmazás keresztül hello következő hello IP-cím beszerzése:

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

Egy eszköz, például hello használhatja [Microsoft Azure Tártallózó](http://storageexplorer.com) tooretrieve és hello fájl writen toohello fájlmegosztás vizsgálhatja meg.

>[!NOTE]
> További információ az Azure Resource Manager-sablonokkal, alkalmazásparaméter-fájlokat, és a hello Azure parancssori felület telepítése toolearn lásd [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Következő lépések

- Az első tároló hello Azure tároló példányok használatával telepítheti [– első lépések](container-instances-quickstart.md)
- További tudnivalók: hello [Azure tároló példányok és tároló orchestrators közötti kapcsolat](container-instances-orchestrator-relationship.md)
