---
title: "az Azure-ban üzemeltető aaaUse Docker gép toocreate Linux |} Microsoft Docs"
description: "Ismerteti, hogyan toouse Docker gép toocreate Docker futtatja az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: iainfou
ms.openlocfilehash: 905c645add51c78305aac85a7013441b3a73972f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-docker-machine-toocreate-hosts-in-azure"></a>Hogyan toouse Docker gép toocreate futtatja az Azure-ban
Ez a következő cikket: részletek hogyan toouse [Docker gép](https://docs.docker.com/machine/) toocreate gazdagépek Azure-ban. Hello `docker-machine` parancs létrehoz egy Linux virtuális gép (VM) az Azure-ban, majd telepíti a Docker. A Docker-gazdagépekből Azure használatával kezelhetők hello helyi eszközök és munkafolyamatok.

## <a name="create-vms-with-docker-machine"></a>Hozzon létre virtuális gépek Docker gép
Először szerezze be az Azure-előfizetése Azonosítóját [az fiók megjelenítése](/cli/azure/account#show) az alábbiak szerint:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Létrehozott egy Docker állomás virtuális gépet az Azure-ban `docker-machine create` megadásával *azure* hello illesztőprogramként. További információkért lásd: hello [Docker Azure illesztőprogram dokumentáció](https://docs.docker.com/machine/drivers/azure/)

hello alábbi példa létrehoz egy nevű virtuális gép *myVM*, létrehoz egy felhasználói fiókot nevű *azureuser*, és a portot nyit meg *80* hello gazdagép-virtuális gép. Bármely kér toolog az Azure-fiók tooyour kövesse és Docker gép engedélyek toocreate megadni, és kezelheti az erőforrásokat.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    myvm
```

hello kimenete a következő példa hasonló toohello néz ki:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvmdocker) Completed machine pre-create checks.
Creating machine...
(myvmdocker) Querying existing resource group.  name="docker-machine"
(myvmdocker) Creating resource group.  name="docker-machine" location="westus"
(myvmdocker) Configuring availability set.  name="docker-machine"
(myvmdocker) Configuring network security group.  name="myvmdocker-firewall" location="westus"
(myvmdocker) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvmdocker) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvmdocker) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvmdocker) Creating public IP address.  name="myvmdocker-ip" static=false
(myvmdocker) Creating network interface.  name="myvmdocker-nic"
(myvmdocker) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvmdocker) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvmdocker"
Waiting for machine toobe running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH toobe available...
Detecting hello provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs toohello local machine directory...
Copying certs toohello remote machine...
Setting Docker configuration on hello remote daemon...
Checking connection tooDocker...
Docker is up and running!
toosee how tooconnect your Docker Client toohello Docker Engine running on this virtual machine, run: docker-machine env myvmdocker
```

## <a name="configure-your-docker-shell"></a>A Docker rendszerhéj konfigurálása
tooconnect tooyour Docker állomás Azure hello megfelelő kapcsolat beállításainak megadása. Amint hello kimeneti hello végén, tekintse meg hello kapcsolat a Docker-állomás az alábbiak szerint: 

```bash
docker-machine env myvmdocker
```

hello hasonló toohello a következő példa a kimenetre:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command tooconfigure your shell:
# eval $(docker-machine env myvmdocker)
```

toodefine hello kapcsolatbeállítások vagy futtatási hello akkor javasolt konfiguráció parancs (`eval $(docker-machine env myvmdocker)`), vagy manuálisan állíthatja be a hello környezeti változókat. 

## <a name="run-a-container"></a>Egy tároló futtatása
toosee egy tárolót a művelet lehetővé teszi, hogy a Futtatás alapvető NGINX-webkiszolgálót. Hozzon létre egy tárolót a `docker run` és tegye elérhetővé a webes forgalom 80-as porton az alábbiak szerint:

```bash
docker run -d -p 80:80 --restart=always nginx
```

hello hasonló toohello a következő példa a kimenetre:

```bash
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

Tárolók futtató nézet `docker ps`. hello következő egy példa a kimenetre látható hello NGINX tároló kitett 80-as port fut:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-hello-container"></a>Hello tároló
Szerezzen be Docker állomás hello nyilvános IP-címét az alábbiak szerint:


```bash
docker-machine ip myvmdocker
```

toosee hello tároló műveletben, nyisson meg egy webböngészőt, és írja be a hello hello megelőző parancs kimenetét hello feljegyzett nyilvános IP-cím:

![Futó ngnix tároló](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Következő lépések
Gazdagépek is létrehozhat a hello [Docker Virtuálisgép-bővítmény](dockerextension.md). A Docker Compose használ, tekintse meg a [Ismerkedés a Docker és az Azure-ban Compose](docker-compose-quickstart.md).
