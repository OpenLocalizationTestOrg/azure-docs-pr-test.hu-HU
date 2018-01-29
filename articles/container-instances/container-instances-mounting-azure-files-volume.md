---
title: "Azure-tároló példányát az Azure fájlok kötet csatlakoztatása"
description: "Útmutató: Azure tároló osztályt állapot megőrizni az Azure fájlok kötet csatlakoztatása"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: be502e6aef39ee4ed8cfc1f8926cb556dc1defb1
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/10/2018
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Azure-tároló osztályt az Azure fájlmegosztások csatlakoztatása

Alapértelmezés szerint Azure tároló példányok állapot nélküli alkalmazások. Ha a tároló összeomlik, vagy leállítja, annak teljes állapota elvész. Állapot élettartama meghaladja a tároló megőrizni, a kötet csatlakoztatnia kell külső áruházban. Ez a cikk bemutatja, hogyan használható Azure tároló osztályt egy Azure fájlmegosztás csatlakoztatásához.

> [!NOTE]
> Az Azure fájlmegosztások csatlakoztatása korlátozódik jelenleg Linux tárolók. Dolgozunk ennek érdekében minden funkció Windows tárolók, amíg a jelenlegi platform különbségek található [kvótái és az Azure-tároló példányok régiónkénti elérhetőség](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>Az Azure-fájlmegosztás létrehozása

Az Azure fájlmegosztások használatához Azure tároló osztályt, akkor létre kell hoznia. A következő parancsprogrammal hozzon létre egy tárfiókot, a fájlmegosztás és a megosztás saját magát. A tárfiók nevének globálisan egyedi, kell lennie, a parancsfájl egy véletlenszerű értékét hozzáadja a kezdőpontját meghatározó karakterlánc.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $ACI_PERS_RESOURCE_GROUP --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="get-storage-credentials"></a>Tároló hitelesítő adatainak lekérése

Az Azure fájlmegosztások csatlakoztatása kötetként az Azure-tároló példányok, három értékek kell: a tárfiók nevét, a megosztás neve és a hozzáférési kulcsot.

Ha követte a fenti parancsfájl, a tárfiók nevének végén véletlenszerű értéket hozták létre. A végső karakterláncban (beleértve a véletlenszerű része) lekérdezéséhez használja a következő parancsokat:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

A megosztásnév már ismert (definiálva *acishare* a parancsfájlban), hogy most már a tárfiók hívóbetűjét, amely a következő paranccsal található összes:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Tároló és a csatlakoztatási kötet telepítése

Az Azure fájlmegosztások csatlakoztatása a tárolóban lévő kötetként, adja meg a megosztás és a kötet csatlakoztatási pontjának a tárolóhoz létrehozásakor [az tároló létrehozása][az-container-create]. Ha követte a fenti lépéseket, akkor is csatlakoztathatja a megosztáshoz a következő paranccsal hozhat létre tárolót, korábban létrehozott:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image seanmckenna/aci-hellofiles \
    --ip-address Public \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

## <a name="manage-files-in-mounted-volume"></a>Csatlakoztatott kötet lévő fájlok kezeléséhez

A tároló elindul, ha a egyszerű webalkalmazást telepített keresztül is használhatja a [seanmckenna/aci-hellofiles] [ aci-hellofiles] kép kezelése az Azure-fájlmegosztáshoz megadott csatlakoztatási elérési úton található fájlokat. A web app és az IP-cím beszerzése a [az tároló megjelenítése] [ az-container-show] parancs:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --output table
```

Használhatja a [Azure-portálon] [ portal] vagy egy eszköz, például a [Microsoft Azure Tártallózó] [ storage-explorer] kérhető le, és vizsgálja meg a fájl írása a fájlmegosztás.

## <a name="mount-multiple-volumes"></a>Több kötet csatlakoztatása

Egy tároló-példányban több kötet csatlakoztatása, ha már telepítette használatával egy [Azure Resource Manager sablon](/azure/templates/microsoft.containerinstance/containergroups).

Először adja meg a fájlmegosztás adatait, és adja meg a kötetek program kitölti a `volumes` tömb a a `properties` a sablon szakasza. Például, ha a két Azure fájlmegosztásokat nevű létrehozott *megosztas1* és *share2* tárfiókban *myStorageAccount*, a `volumes` tömb jelent. a következőhöz hasonló:

```json
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Ezt követően a tároló csoport, amelyben szeretné csatlakoztatni a kötetek minden egyes tároló esetén feltöltése a `volumeMounts` tömb a a `properties` szakasz tároló-definícióban. Például ez csatlakoztatja a két kötet *myvolume1* és *myvolume2*, korábban definiált:

```json
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Az Azure Resource Manager sablonnal tároló példány szemléltet, olvassa el [központi telepítése az Azure-tároló esetekben több tárolócsoportok](container-instances-multi-container-group.md).

## <a name="next-steps"></a>További lépések

További tudnivalók a következő kapcsolatot: [Azure tároló példányok és tároló orchestrators](container-instances-orchestrator-relationship.md).

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/seanmckenna/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show