---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – DC/OS kezelése |} Microsoft Docs"
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
ms.openlocfilehash: b91c433bfd7e48ec405cc62be1486d9d4662839d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a>Azure Tárolószolgáltatás útmutató – DC/OS kezelése

A DC/OS elosztott platformot kínál a futó modern és indexelése alkalmazások. Teljes Azure Tárolószolgáltatás egy éles készen áll a DC/OS-fürtben üzembe, akkor egyszerűen és gyorsan. A gyors üzembe helyezési részletek lépéseken toodeploy szükséges, a DC/OS-fürtről és a Futtatás alapvető munkaterhelés.

> [!div class="checklist"]
> * Az ACS a DC/OS-fürt létrehozása
> * Csatlakoztassa toohello fürtöt
> * Hello DC/OS parancssori felület telepítése
> * Az alkalmazás toohello fürt központi telepítése
> * Alkalmazás skálázása hello fürtön
> * A DC/OS-fürt csomópontjai hello méretezése
> * Alapszintű DC/OS-kezelés
> * Hello DC/OS-fürt törlése

Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>A DC/OS-fürt létrehozása

Először hozzon létre egy erőforráscsoportot a hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westeurope* helyét.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Következő lépésként hozzon létre egy DC/OS-fürtről hello [az acs létre](/cli/azure/acs#create) parancsot.

hello alábbi példakód létrehozza a DC/OS-fürt nevű *myDCOSCluster* és SSH-kulcsok létrehozása, ha még nem léteznek. toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Pár perc múlva hello parancs befejeződik, és hello telepítési információt ad vissza.

## <a name="connect-toodcos-cluster"></a>Csatlakozás tooDC/OS-fürtről

A DC/OS-fürt létrehozása után célszerű egy SSH-alagúton keresztül fér hozzá. Futtassa a következő parancs tooreturn hello nyilvános IP-cím hello DC/OS főkiszolgáló hello. Az IP-cím egy változó tárolja, és hello következő lépésben szükség.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

toocreate hello SSH-alagút, futtassa a következő parancs hello és hello képernyőn megjelenő útmutatást. Ha a 80-as port már használatban van, hello parancs sikertelen lesz. Frissítés hello bújtatott port tooone nincsenek használatban, például a `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>DC/OS parancssori felület telepítése

Hello DC/OS parancssori felület használatával hello telepítése [az acs vezénylőtípusú install-cli](/azure/acs/dcos#install-cli) parancsot. Ha Azure CloudShell használ, a DC/OS parancssori felület hello már telepítve van. Ha macOS vagy Linux hello Azure CLI-t futtat, szükség lehet a sudo toorun hello parancsot.

```azurecli
az acs dcos install-cli
```

Hello CLI hello fürttel használható, mielőtt konfigurált toouse hello SSH-alagút kell lennie. toodo úgy, futtassa a következő parancsot, hello port be, szükség esetén hello.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Alkalmazás futtatása

az ACS a DC/OS-fürt mechanizmus ütemezés hello alapértelmezett érték a Marathon. A Marathon használt toostart alkalmazás, és kezelhető hello állapot hello alkalmazás hello DC/OS-fürtön. tooschedule marathon, alkalmazás, hozzon létre egy fájlt **marathon-app.json**, és a következő tartalom másolása hello bele. 

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

Futtassa a parancsot tooschedule hello alkalmazás toorun hello DC/OS-fürtön a következő hello.

```azurecli
dcos marathon app add marathon-app.json
```

toosee hello központi telepítési állapotának hello alkalmazást, és futtassa a következő parancs hello.

```azurecli
dcos marathon app list
```

Ha hello **feladatok** oszlopérték vált *0 vagy 1* túl*1/1*, alkalmazás központi telepítése befejeződött.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>A Marathon alkalmazás skálázása

Hello előző példában egy egypéldányos alkalmazás lett létrehozva. a központi telepítés, hogy hello alkalmazás három példányát érhetők el, megnyithatja hello tooupdate **marathon-app.json** fájlt, és hello példány tulajdonság too3 frissítése.

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

Hello segítségével hello alkalmazás frissítése `dcos marathon app update` parancsot.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

toosee hello központi telepítési állapotának hello alkalmazást, és futtassa a következő parancs hello.

```azurecli
dcos marathon app list
```

Ha hello **feladatok** oszlopérték vált *1/3* túl*3/1*, alkalmazás központi telepítése befejeződött.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Elérhető az internet-alkalmazás futtatása

hello ACS DC/OS-fürtről áll két csomópont-készlet, amely elérhető egy nyilvános hello internet, és egy saját, amely nem érhető el az internet hello. Alapértelmezés szerint hello hello titkos csomópontok, hello utolsó példában használt.

toomake egy alkalmazás elérhető az hello internet, azok toohello nyilvános csomópont-készletet. toodo úgy, hogy hello `acceptedResourceRoles` érték objektum `slave_public`.

Hozzon létre egy fájlt **nginx-public.json** és a következő tartalom másolása hello bele.

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

Futtassa a parancsot tooschedule hello alkalmazás toorun hello DC/OS-fürtön a következő hello.

```azurecli 
dcos marathon app add nginx-public.json
```

Nyilvános IP-cím hello hello DC/OS-fürt nyilvános ügynökök beolvasni.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Hello alapértelmezett NGINX webhely böngészési toothis címet adja vissza.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Skála DC/OS-fürtről

Az előző példákban hello alkalmazás méretezett toomultiple példány volt. hello DC/OS infrastruktúra méretezett tooprovide hosszabb vagy rövidebb számítási kapacitás is lehet. Ez a lépés hello [az acs méretezése]() parancsot. 

toosee hello aktuális száma a DC/OS-ügynökök, használja a hello [az acs megjelenítése](/cli/azure/acs#show) parancsot.

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

tooincrease hello száma too5, használja a hello [az acs méretezése](/cli/azure/acs#scale) parancsot. 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>A DC/OS-fürt törlése

Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a DC/OS-fürtről és az összes kapcsolódó erőforrások parancsot.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban megtanulhatta, kapcsolatos alapvető DC/OS fájlkezelési feladat hello következőket beleértve. 

> [!div class="checklist"]
> * Az ACS a DC/OS-fürt létrehozása
> * Csatlakoztassa toohello fürtöt
> * Hello DC/OS parancssori felület telepítése
> * Az alkalmazás toohello fürt központi telepítése
> * Alkalmazás skálázása hello fürtön
> * A DC/OS-fürt csomópontjai hello méretezése
> * Hello DC/OS-fürt törlése

Előzetes toohello következő útmutató toolearn kapcsolatos betölteni a DC/OS Azure terheléselosztási alkalmazást. 

> [!div class="nextstepaction"]
> [Terheléselosztási alkalmazások](container-service-load-balancing.md)