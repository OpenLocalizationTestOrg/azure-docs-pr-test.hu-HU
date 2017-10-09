---
title: "a Linux virtuális gépek Azure-ban a Docker Compose aaaUse |} Microsoft Docs"
description: "Hogyan toouse Docker és a Linux virtuális gépek a Compose hello Azure parancssori felület"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: cf7254ad4813ccdc641fcacbb06ed1514a93cee5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-docker-and-compose-toodefine-and-run-a-multi-container-application-in-azure"></a>Beolvasása használatába a Docker és Compose toodefine és futtatni egy több tároló alkalmazást az Azure-ban
A [Compose](http://github.com/docker/compose), egy egyszerű szöveges fájl toodefine álló több Docker-tároló egy alkalmazás használatát. Majd lépett fel az alkalmazást, amelyet minden egy parancs toodeploy meghatározott környezetét. Tegyük fel ez a cikk bemutatja, hogyan tooquickly beállítani egy WordPress-bloghoz egy háttérrendszerrel MariaDB SQL-adatbázis egy Ubuntu virtuális gép. Használhatja a Compose tooset mentése összetettebb alkalmazásokat is.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Linux virtuális gépet egy Docker-gazdagépként
Különböző Azure eljárások és a rendelkezésre álló képeket vagy a Resource Manager-sablonok hello Azure piactér toocreate Linux virtuális gép használja, és állítsa be a Docker-gazdagépként. Lásd például: [hello Docker Virtuálisgép-bővítmény toodeploy a környezet használatával](dockerextension.md) tooquickly hozzon létre egy Ubuntu virtuális gép hello Azure Docker Virtuálisgép-bővítmény használatával egy [gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Hello Docker Virtuálisgép-bővítmény használata esetén a virtuális gép automatikusan be van állítva egy Docker-gazdagépként, és új már telepítve van.


### <a name="create-docker-host-with-azure-cli-20"></a>Az Azure CLI 2.0 Docker állomás létrehozása
Legutóbbi telepítés hello [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).

Először hozzon létre egy erőforráscsoportot a Docker környezetében [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westus* helye:

```azurecli
az group create --name myResourceGroup --location westus
```

Ezután telepítse a virtuális gép és [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) , amely tartalmazza az Azure Docker Virtuálisgép-bővítmény hello [a Githubon az Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Adja meg a saját értékeit *newStorageAccountName*, *adminUsername*, *adminPassword*, és *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Hello telepítési toofinish néhány percet vesz igénybe. Hello telepítés befejeződése után [áthelyezése toonext lépéssel](#verify-that-compose-is-installed) tooSSH tooyour virtuális gép. 

Opcionálisan tooinstead visszatérési felügyeletének toohello kérése és a telepítés lehetővé hello továbbra hello háttérben, adja hozzá a hello `--no-wait` parancs megelőző toohello jelzőt. Ez a folyamat lehetővé teszi a tooperform más tevékenységet hello CLI közben hello telepítési továbbra is fennáll, néhány percig. Majd megtekintheti az hello Docker-gazdagép állapotával kapcsolatos adatokat [az vm megjelenítése](/cli/azure/vm#show). hello alábbi példa ellenőrzi hello nevű virtuális gép állapotát hello *myDockerVM* (alapértelmezett neve hello sablonból hello – ne módosítsa a nevet) nevű hello erőforráscsoportban *myResourceGroup*:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

Ha ez a parancs visszaadja *sikeres*, hello telepítés véget ért, és a következő lépés hello a virtuális gép SSH toohello is.


## <a name="verify-that-compose-is-installed"></a>Győződjön meg arról, hogy telepítve van-e a Compose
Hello telepítés befejeződése után SSH tooyour új Docker állomás hello DNS-név használatával megadott központi telepítése során. Használhat `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview részleteit a virtuális Gépet, beleértve a hello DNS-nevét.

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

alkotó toocheck hello VM, futtassa a következő parancs hello telepítve van:

```bash
docker-compose --version
```

Kimenet jelenik meg hasonló túl*docker compose 1.6.2, a 4d 72027 build*.

> [!TIP]
> Ha egy másik módszer toocreate egy Docker-állomás és kell tooinstall állítható össze saját magának, lásd: hello [dokumentáció ír](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Egy docker-compose.yml konfigurációs fájl létrehozása
Ezután létrehozhat egy `docker-compose.yml` fájl, amely csak a szöveg konfigurációs fájl, a toodefine hello Docker tárolók toorun hello virtuális gép. hello fájl minden egyes tárolóban hello kép toorun határozza meg (vagy annak oka lehet egy Dockerfile a build), szükséges környezeti változókat és a függőségeket, a portok és a tárolók közötti hello hivatkozásokat. További részletek a yml fájl Szintaxis: [hivatkozást a Compose](https://docs.docker.com/compose/compose-file/).

Hozzon létre hello *docker-compose.yml* fájlt az alábbiak szerint:

```bash
touch docker-compose.yml
```

A kedvenc text editor tooadd bizonyos adatok toohello fájlt használja. hello alábbi példában hello *vi* szerkesztő:

```bash
vi docker-compose.yml
```

Illessze be a következő példa a szövegfájlba hello. Ezt a konfigurációt használja a képek a hello [DockerHub beállításjegyzék](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello nyílt forráskódú bloggolás és a tartalom rendszerrel) és a csatolt háttér MariaDB SQL-adatbázis. Adja meg a saját *MYSQL_ROOT_PASSWORD* az alábbiak szerint:

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-hello-containers-with-compose"></a>Kezdő hello tárolók Compose
Az azonos hello könyvtárhoz, mint a *docker-compose.yml* fájl, futtassa a következő parancs hello (a használt környezettől függően szükség lehet a toorun `docker-compose` használatával `sudo`):

```bash
docker-compose up -d
```

A paranccsal elindítja a megadott hello Docker-tároló *docker-compose.yml*. Egy percet, amíg ez a lépés toocomplete vesz igénybe. A következő példa kimenet hasonló toohello jelenik meg:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Lehet, hogy toouse hello **-d** , hogy hello tárolók folyamatos Futtatás hello háttérben indítási lehetőséget.


hello tárolók, amelyek tooverify típus `docker-compose ps`. Hasonlót kell megjelennie:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Most már közvetlenül a 80-as porton VM hello tooWordPress is elérheti. Nyisson meg egy webböngészőt, és írja be a virtuális gép hello DNS-nevét (például `http://mypublicdns.westus.cloudapp.azure.com`). Meg kell jelennie hello WordPress kezdőképernyőjén, ahol fejezheti be hello telepítését, valamint Ismerkedés a hello alkalmazás.

![WordPress kezdőképernyője][wordpress_start]

## <a name="next-steps"></a>Következő lépések
* Nyissa meg toohello [Docker VM bővítményének használati útmutatója](https://github.com/Azure/azure-docker-extension/blob/master/README.md) további beállítások tooconfigure Docker és a Docker virtuális gép új. Például egy elem tooput hello Compose yml fájl (átalakított tooJSON) közvetlenül a Docker Virtuálisgép-bővítmény hello hello konfigurálása.
* Tekintse meg a hello [állítható össze a parancssori útmutatójának](http://docs.docker.com/compose/reference/) és [felhasználói útmutató](http://docs.docker.com/compose/) további példák a kialakításához, és több tároló alkalmazások központi telepítéséhez.
* Az Azure Resource Manager-sablon, vagy használja a saját vagy annak egy része volt a hello [közösségi](https://azure.microsoft.com/documentation/templates/), egy Azure virtuális gép Docker és egy alkalmazás toodeploy állítsa be a Compose. Például hello [központi telepítése egy WordPress-bloghoz az Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) sablon Docker használ, és új tooquickly WordPress telepítése egy Ubuntu virtuális gép a MySQL-fájlok.
* Próbálja a Docker Compose integrálása a Docker Swarm-fürt. Lásd: [használatával állítsa össze a Swarm](https://docs.docker.com/compose/swarm/) forgatókönyvek esetén.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
