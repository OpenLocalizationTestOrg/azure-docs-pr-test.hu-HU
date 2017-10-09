---
title: "az Azure-ban Docker számítógép üzemelteti aaaCreate Docker |} Microsoft Docs"
description: "Docker gép toocreate docker-gazdagépekből az Azure használatát ismerteti."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: fbf67e8189bbf33f874c4a9b619a931f28ccee12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Docker gazdagépek létrehozása az Azure-ban a docker-machine segítségével
Futó [Docker](https://www.docker.com/) tárolók igényel a gazdagép virtuális gép futó hello docker démon.
Ez a témakör ismerteti, hogyan toouse hello [docker-gép](https://docs.docker.com/machine/) toocreate új Linux virtuális gépek, konfigurálva hello Docker démon, Azure-ban futó parancsot. 

**Megjegyzés:** 

* *Ez a cikk docker-gép verziójának 0.9.0-rc2, vagy nagyobb függ.*
* *Windows-tárolók docker-gép közelében jövőbeli hello keresztül támogatják*

## <a name="create-vms-with-docker-machine"></a>Hozzon létre virtuális gépek Docker gép
Létrehozott egy docker állomás virtuális gépet az Azure-ban hello `docker-machine create` parancs használatával hello `azure` illesztőprogram. 

hello Azure illesztőprogram igényel az előfizetés-azonosító. Használhatja a hello [Azure CLI](cli-install-nodejs.md) vagy hello [Azure Portal](https://portal.azure.com) tooretrieve az Azure-előfizetéséhez. 

**Hello Azure portál használatával**

* Válassza ki **előfizetések** a hello bal oldali navigációs lapjáról, és másolja hello előfizetés-azonosító.

**Hello Azure parancssori felület használatával**

* Típus ```azure account list``` és másolási hello előfizetés-azonosító.

Típus `docker-machine create --driver azure` toosee hello beállítások és az alapértelmezett értékekre.
Azt is láthatja, hello [Docker Azure illesztőprogram dokumentáció](https://docs.docker.com/machine/drivers/azure/) vonatkozó információ. 

hello alábbi példa alapul hello [alapértelmezett értékek](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), de igény szerint állítsa be ezeket az értékeket: 

* Azure-dns hello nyilvános IP-cím társított hello név és generált tanúsítványokat. Ez az a virtuális gép hello DNS-nevét. hello virtuális gép ezután biztonságosan állítsa le, kiadási hello dinamikus IP, és adja meg a hello képességét tooreconnect hello vm újra egy új IP-elindulása után. hello előtagja UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com régióban egyedinek kell lennie.
* Nyissa meg a 80-as portot a virtuális gép hello a kimenő internet-hozzáféréséhez
* prémium szintű tároló gyorsabb VM tooutilize hello mérete
* prémium szintű storage használt hello méretű lemez

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>A docker-gép docker állomás
Miután egy bejegyzést a docker-gépen ahhoz, hogy a gazdagép, hello alapértelmezett gazdagép docker parancsok futtatásakor állíthatja be.

## <a name="using-powershell"></a>A PowerShell használata
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Bash használatával
```bash
eval $(docker-machine env MyDockerHost)
```

Most futtathatja docker parancsok hello megadott állomás ellen

```
docker ps
docker info
```

## <a name="run-a-container"></a>Egy tároló futtatása
Egy konfigurált gazdagéppel most futtathatja egy egyszerű web server tootest hogy helyesen lett-e konfigurálva a gazdagépen.
Itt azt a szabványos nginx-lemezkép használatához adja meg, hogy kell-e figyelni 80-as porton, és, hogy ha hello állomás virtuális gép újraindul, hello tárolót fog újraindul, valamint (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

hello kimeneti hasonlóan kell kinéznie hello következő:

```
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-hello-container"></a>Hello tároló
Vizsgálja meg a futó tárolók használatával `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

És tárolót, típusú futtató toosee hello `docker-machine ip <VM name>` toofind hello IP cím tooenter hello böngészőben:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Futó ngnix tároló](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Összefoglalás
Docker-számítógéppel könnyen megadhat docker-gazdagépekből az Azure-ban a az egyes docker állomás érvényesítést.
Termelési környezetben futtató tároló, lásd: hello [Azure Tárolószolgáltatásban](http://aka.ms/AzureContainerService)

a Visual Studio .NET Core alkalmazások toodevelop lásd [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)

