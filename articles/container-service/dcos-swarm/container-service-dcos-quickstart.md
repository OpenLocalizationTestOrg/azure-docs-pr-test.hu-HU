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
# <a name="deploy-a-dcos-cluster"></a>A DC/OS-fürt központi telepítése

A DC/OS elosztott platformot kínál a futó modern és indexelése alkalmazások. Teljes Azure Tárolószolgáltatás egy éles készen áll a DC/OS-fürtben üzembe, akkor egyszerűen és gyorsan. A gyors üzembe helyezési részletek hello lépéseken toodeploy szükséges, a DC/OS-fürtről és a Futtatás alapvető munkaterhelés.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure 

Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>A DC/OS-fürt létrehozása

Hozzon létre egy DC/OS-fürtről hello [az acs létre](/cli/azure/acs#create) parancsot.

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

SSH-alagút hello tesztelhető tallózással túl`http://localhost`. Ha egy portot más, a 80-as használták, állítsa be a hello hely toomatch. 

Ha hello SSH-alagút létrehozása sikeresen megtörtént, hello DC/OS portal ad vissza.

![VEZÉNYLŐTÍPUSÚ FELHASZNÁLÓI FELÜLET](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>DC/OS parancssori felület telepítése

a DC/OS parancssori felület hello használt toomanage a DC/OS parancssori hello fürtöt. Hello DC/OS parancssori felület használatával hello telepítése [az acs vezénylőtípusú install-cli](/azure/acs/dcos#install-cli) parancsot. Ha Azure CloudShell használ, a DC/OS parancssori felület hello már telepítve van. 

Ha macOS vagy Linux hello Azure CLI-t futtat, szükség lehet a sudo toorun hello parancsot.

```azurecli
az acs dcos install-cli
```

Hello CLI hello fürttel használható, mielőtt konfigurált toouse hello SSH-alagút kell lennie. toodo úgy, futtassa a következő parancsot, hello port be, szükség esetén hello.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Alkalmazás futtatása

az ACS a DC/OS-fürt mechanizmus ütemezés hello alapértelmezett érték a Marathon. A Marathon használt toostart alkalmazás, és kezelhető hello állapot hello alkalmazás hello DC/OS-fürtön. tooschedule marathon, alkalmazás, hozzon létre egy fájlt *marathon-app.json*, és a következő tartalom másolása hello bele. 

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
dcos marathon app add marathon-app.json
```

toosee hello központi telepítési állapotának hello alkalmazást, és futtassa a következő parancs hello.

```azurecli
dcos marathon app list
```

Ha hello **Várakozás** oszlopérték vált *igaz* túl*hamis*, alkalmazás központi telepítése befejeződött.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

Nyilvános IP-címe hello hello DC/OS-fürt ügynökök beolvasni.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Hello alapértelmezett NGINX webhely böngészési toothis címet adja vissza.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>A DC/OS-fürt törlése

Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a DC/OS-fürtről és az összes kapcsolódó erőforrások parancsot.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezési a DC/OS-fürt telepítése után, és egy egyszerű Docker-tároló hello fürtön futtattak. További információ az Azure Tárolószolgáltatás toolearn toohello ACS oktatóanyagok továbbra is.

> [!div class="nextstepaction"]
> [Az ACS a DC/OS-fürt kezeléséhez](container-service-dcos-manage-tutorial.md)