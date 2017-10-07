---
title: "egy Azure DC/OS-fürt ACR aaaUsing |} Microsoft Docs"
description: "Egy Azure-tároló beállításjegyzék használata a DC/OS fürtben, az Azure Tárolószolgáltatásban"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: "Docker, tárolók, Micro-szolgáltatások, a Mesos, a Azure, a fájlmegosztás, a cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 9a2802da788b50147d8b4259436bdcdb0c929db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-acr-with-a-dcos-cluster-toodeploy-your-application"></a>A DC/OS-fürt toodeploy ACR használni az alkalmazás

Ez a cikk azt felfedezés hogyan toouse Azure tároló beállításjegyzék DC/OS-fürthöz. ACR használatával tooprivately tároló lehetővé teszi, és tároló-lemezképek kezelése. Ez az oktatóanyag ismerteti a következő feladatok hello:

> [!div class="checklist"]
> * Telepítse az Azure-tároló beállításjegyzék, (ha szükséges)
> * A DC/OS-fürtről ACR hitelesítés beállítása
> * Egy kép toohello Azure tároló beállításjegyzék feltöltése
> * A tároló lemezkép hello Azure tároló beállításjegyzék-ről futtatva

Az ACS a DC/OS fürtben toocomplete hello lépések ebben az oktatóanyagban van szüksége. Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.

Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Telepítse az Azure tároló beállításjegyzék

Szükség esetén hozzon létre egy Azure-tárolóba beállításjegyzék hello [az acr létrehozása](/cli/azure/acr#create) parancsot. 

hello alábbi példa létrehoz egy beállításjegyzéket egy véletlenszerű előállításához a neve. hello beállításjegyzék is konfigurálva van egy rendszergazdai fiókkal hello segítségével `--admin-enabled` argumentum.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

Hello beállításkulcs létrehozása után hello Azure CLI kimeneti adatok hasonló toohello következő. Jegyezze fel a hello `name` és `loginServer`, ezek a későbbi lépésekben használhatók.

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Hello tároló beállításjegyzék hitelesítő adatainak használatával hello lekérése [az acr hitelesítő adatok megjelenítése](/cli/azure/acr/credential) parancsot. Helyettesítő hello `--name` az egyik hello utolsó lépésben feljegyzett hello. Jegyezze fel a egy jelszó, szükség esetén egy későbbi lépésben.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Azure tároló beállításjegyzékkel kapcsolatos további információkért lásd: [bemutatása tooprivate Docker-tároló nyilvántartó](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>ACR hitelesítés kezelésére szolgál

hello hagyományos módon személyes beállításjegyzékből toopush és lekéréses kép toofirst hello beállításjegyzék hitelesíteni. toodo hello használja, ezért `docker login` minden ügyfélen, amelyet a tooaccess hello titkos beállításjegyzék parancsot. A DC/OS-fürtről sok csomópontok, ezek mindegyike kell, hogy a felhasználók hitelesítése a hello ACR toobe, szerepelhetnek benne, ezért hasznos tooautomate között a folyamat minden egyes csomópontjára. 

### <a name="create-shared-storage"></a>Megosztott tároló létrehozása

Ez a folyamat használja az Azure fájlmegosztások csatlakoztatva hello fürt mindegyik csomópontján. Ha már nem beállított megosztott tárolót, lásd: [a DC/OS fürtben található fájlmegosztás beállítása](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>ACR hitelesítés konfigurálása

Szereznie hello hello DC/OS fő és tárolható egy változóban teljes Tartományneve.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Hozzon létre egy SSH-kapcsolat hello főkiszolgáló (vagy hello első főkiszolgálójának) a DC/OS-alapú fürt. Frissítse a hello felhasználói nevét, ha egy nem alapértelmezett érték lett megadva, amikor hello fürtöt hoz létre.

```azurecli-interactive
ssh azureuser@$FQDN
```

Futtassa a következő parancs toologin toohello Azure tároló beállításjegyzék hello. Cserélje le a hello `--username` hello tároló beállításjegyzék és hello hello nevű `--password` hello foglalt jelszavak egyikével. Cserélje le az utolsó argumentumnak hello *mycontainerregistry.azurecr.io* hello példájában hello loginServer nevű hello tároló beállításjegyzék. 

Ezzel a paranccsal tárolhatók hello-értékek hitelesítési helyileg hello `~/.docker` elérési útja.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Hozzon létre egy hello tároló beállításazonosítókat hitelesítési tartalmazó tömörített fájl.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

A fájl toohello megosztott fürttároló másolja. Ebben a lépésben elérhetővé teszi hello fájl hello DC/OS-fürt összes csomópontján.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-tooacr"></a>Kép tooACR feltöltése

Most a fejlesztési számítógépén, vagy bármely más rendszer Docker telepítve, a lemezkép létrehozása, és töltse fel az Azure-tároló beállításjegyzék toohello.

Hozzon létre egy tároló hello Ubuntu lemezképéről.

```azurecli-interactive
docker run ubunut --name base-image
```

Most rögzítési hello tárolót az új lemezképet. hello lemezkép nevének kell tooinclude hello `loginServer` hello tároló registrywith formátuma a neve `loginServer/imageName`.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

Bejelentkezés a hello Azure tároló beállításjegyzék. Hello neve hello loginServer nevű lecseréléséhez hello felhasználónév – hello nevű hello tároló beállításjegyzék és hello – hello foglalt jelszavak egyikével jelszót.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Végezetül feltöltése hello kép toohello ACR beállításjegyzék. Ez a példa feltölt egy képet vezénylőtípusú-bemutató nevű.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>Futtassa a képfájl ACR

egy kép hello ACR beállításjegyzékből toouse hozzon létre egy fájlt nevek *acrDemo.json* és a következő szöveg másolása hello bele. Cserélje le hello lemezképnév hello tároló beállításjegyzék loginServer nevének és a lemezkép neve, például `loginServer/imageName`. Jegyezze fel a hello `uris` tulajdonság. Ez a tulajdonság tárolja hello hello tároló beállításjegyzék hitelesítési-fájl helyét, amely ebben az esetben hello DC/OS-fürt minden csomópontja csatlakoztattak hello Azure fájlmegosztás.

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

DC/c CLI hello hello alkalmazás központi telepítését.

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban a DC/OS toouse többek között a következő hello Azure tároló beállításjegyzék feladatok rendelkezik konfigurálása:

> [!div class="checklist"]
> * Telepítse az Azure-tároló beállításjegyzék, (ha szükséges)
> * A DC/OS-fürtről ACR hitelesítés beállítása
> * Egy kép toohello Azure tároló beállításjegyzék feltöltése
> * A tároló lemezkép hello Azure tároló beállításjegyzék-ről futtatva
