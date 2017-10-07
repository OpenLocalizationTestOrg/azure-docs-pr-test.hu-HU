---
title: "az Azure-ban Jenkins fejlesztési folyamat aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Jenkins virtuális gépet, hogy minden egyes kódja a Githubon ponttá véglegesíteni és összeállít egy új Docker tároló toorun Azure-ban az alkalmazás"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c079e3c9186c9da0a3e51e1823215779c565e0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>Hogyan toocreate egy Linux virtuális Gépet az Azure-ban Jenkins, a Githubon és a Docker-fejlesztési infrastruktúra
tooautomate hello build, és tesztelési fázis alkalmazásának fejlesztését, használhatja a folyamatos integrációt és a központi telepítés (CI/CD) folyamat. Ebben az oktatóanyagban létrehoz egy CI/CD folyamat egy Azure virtuális gépen történő is beleértve:

> [!div class="checklist"]
> * Jenkins virtuális gép létrehozása
> * Telepítse és konfigurálja a Jenkins
> * GitHub és Jenkins integrációjával webhook létrehozása
> * Hozzon létre és Jenkins feladatok létrehozása a Githubról eseményindító véglegesítése
> * Az alkalmazás Docker-lemezkép létrehozása
> * Ellenőrizze a Githubon véglegesíti és hozhat létre. új Docker-lemezkép alkalmazást futtató frissítések


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="create-jenkins-instance"></a>Jenkins példány létrehozása
Az oktatóanyag előző [hogyan toocustomize egy Linux virtuális gép első indításakor](tutorial-automate-vm-deployment.md), akkor megtanulta, hogyan tooautomate virtuális gép testreszabása a felhő inicializálás. Ez az oktatóanyag a virtuális gép egy felhőben inicializálás fájl tooinstall Jenkins és a Docker használja. 

Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* és a Beillesztés hello a következő konfigurációs. A felhő rendszerhéj hello nem a helyi számítógépen hozzon létre például hello fájlt. Adja meg `sensible-editor cloud-init-jenkins.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez. Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupJenkins* a hello *eastus* helye:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create). Használjon hello `--custom-data` paraméter toopass a felhő inicializálás konfigurációs fájlban. Adja meg a hello teljes elérési útja túl*felhő-init-jenkins.txt* ha kívül a jelen munkakönyvtár hello fájlt mentette.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Hello VM toobe létrehozása és konfigurálása néhány percet vesz igénybe.

tooallow webes forgalom tooreach a virtuális gép használata [az vm-port megnyitása](/cli/azure/vm#open-port) tooopen port *8080* Jenkins forgalom és a port *1337* hello Node.js-alkalmazás, amely használt toorun egy mintaalkalmazást:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Jenkins konfigurálása
tooaccess a Jenkins példány, szerezze be a virtuális gép hello nyilvános IP-címe:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Biztonsági okokból tooenter hello kezdeti rendszergazdai jelszó tárolt szövegfájlba a virtuális gép toostart hello Jenkins telepíteni kell. Hello hello előző lépés tooSSH tooyour VM beszerzett nyilvános IP-cím használata:

```bash
ssh azureuser@<publicIps>
```

Nézet hello `initialAdminPassword` a Jenkins telepítése, és másolja azt:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Hello fájl még nem érhető el, ha Várjon néhány percet a cloud inicializálás toocomplete hello Jenkins és a Docker telepítésről.

Most nyisson meg egy webböngészőt, és nyissa meg túl`http://<publicIps>:8080`. Hajtsa végre az alábbiak szerint hello kezdeti Jenkins beállítás:

- Adja meg a hello *initialAdminPassword* hello VM hello előző lépésben beszerzett.
- Kattintson a **beépülő modulok tooinstall kiválasztása**
- Keresse meg *GitHub* hello szövegmezőben hello tetején, válassza ki a hello *GitHub beépülő modul*, majd kattintson a **telepítése**
- toocreate Jenkins felhasználói fiók, töltse ki a kívánt hello űrlap. Biztonsági szempontból Folytatás hello alapértelmezett rendszergazdai fiók helyett az első Jenkins felhasználó kell létrehoznia.
- Ha befejezte, kattintson a **Jenkins használatának megkezdése**


## <a name="create-github-webhook"></a>GitHub webhook létrehozása
tooconfigure hello integráció a github webhelyen, nyissa meg hello [Node.js Hello World sample app](https://github.com/Azure-Samples/nodejs-docs-hello-world) hello Azure-minták tárházból. toofork hello tárház tooyour saját GitHub-fiók, kattintson a hello **elágazás** hello jobb felső sarkában található gombra.

Hozzon létre egy webhook létrehozott hello elágazás belül:

- Kattintson a **beállítások**, majd jelölje be **integrációja és a szolgáltatások** hello bal oldalán.
- Kattintson a **-szolgáltatás hozzáadása a**, majd adja meg *Jenkins* a Szűrő mezőbe.
- Válassza ki *Jenkins (GitHub beépülő modul)*
- A hello **Jenkins hook URL-cím**, adja meg `http://<publicIps>:8080/github-webhook/`. Meg kell hello záró /
- Kattintson a **szolgáltatás hozzáadása**

![GitHub webhook ágazik el tooyour-tárház hozzáadása](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Jenkins feladat létrehozása
toohave Jenkins válaszoljon tooan esemény a Githubon véglegesítése kód, például hozzon létre egy Jenkins feladatot. 

Kattintson a Jenkins webhely **hozzon létre új feladatokat** hello kezdőlapról:

- Adja meg *HelloWorld* feladat neve. Válasszon **Freestyle projekt**, majd jelölje be **OK**.
- A hello **általános** szakaszban jelölje be **GitHub** projektre, és adja meg a villás tárház URL-CÍMÉT, például *https://github.com/iainfoulds/nodejs-docs-hello-world*
- A hello **forrás kód felügyeleti** szakaszban jelölje be **Git**, adja meg a villás tárház *.git* URL-címet, például *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- A hello **Build eseményindítók** szakaszban jelölje be **GitHub hook eseményindítója a következőnek: GITscm lekérdezési**.
- A hello **Build** területen válasszon **Hozzáadás összeállítása lépés**. Válassza ki **hajtható végre a rendszerhéj**, majd adja meg `echo "Testing"` toocommand ablakban.
- Kattintson a **mentése** hello feladatok ablak hello alján.


## <a name="test-github-integration"></a>GitHub-integráció tesztelése
tootest hello Jenkins, GitHub integrációja az elágazáshoz változása véglegesítése. 

Vissza a Githubon webes felhasználói felülete, válassza ki a villás tárház, és kattintson a hello **index.js** fájlt. Kattintson hello ceruza ikonra tooedit ezt a fájlt, sor: 6 olvassa be:

```nodejs
response.end("Hello World!");
```

toocommit módosításait, kattintson a hello **változtatások véglegesítése a határidő** hello alsó gombra.

Jenkins, az új buildverziót elindul, a hello **előzmények Build** hello bal alsó sarkában a feladat lap szakasza. Kattintson a hello build számú hivatkozásra, és válassza ki **a konzol kimeneti** a hello bal mérete. Megtekintheti a Jenkins tesz a kódban van lekért GitHub és hello build művelet kimenetének üdvözlőüzenetére hello lépéseket `Testing` toohello konzol. Minden alkalommal, amikor egy véglegesítési legyen a Githubon, hello webhook egészítse ki tooJenkins és indul el, így új buildverziót.


## <a name="define-docker-build-image"></a>Adja meg a Docker build kép
a Githubon véglegesítések alapján futó toosee hello Node.js alkalmazás lehetővé teszi, hogy egy Docker-lemezkép toorun hello alkalmazás elkészítésére. hello kép össze egy Dockerfile, amely meghatározza, hogyan tooconfigure hello tároló, amely hello alkalmazást futtat. 

Hello SSH-kapcsolat tooyour VM módosítsa az előző lépésben létrehozott hello feladat után nevű toohello Jenkins munkaterület könyvtár. A fenti példában, amely nevű *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Fájl létrehozása a könyvtár munkaterület `sudo sensible-editor Dockerfile` és a Beillesztés hello követő tartalmát. Győződjön meg arról, hogy teljes Dockerfile van hello lemásolva megfelelően, különösen akkor hello első sor:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

A Dockerfile hello alapszintű Node.js lemezkép Alpine Linux használ, tesz elérhetővé port 1337 app Hello World hello futtatja, akkor hello app fájlokat másolja és inicializálja azt.


## <a name="create-jenkins-build-rules"></a>Jenkins összeállítási szabályok létrehozása
Az előző lépésben létrehozott, amelyek kimenete egy üzenet toohello konzol alapvető Jenkins build szabály. Lehetővé teszi, hogy hozzon létre hello összeállítása lépés toouse a Dockerfile és hello alkalmazás futtatása.

A Jenkins példánya válassza az előző lépésben létrehozott hello feladat. Kattintson a **konfigurálása** hello bal oldalán és toohello görgetve **Build** szakasz:

- Távolítsa el a meglévő `echo "Test"` összeállítása lépés. Kattintson a hello közötti piros hello jobb felső sarkában hello meglévő összeállítása lépés párbeszédpanel.
- Kattintson a **Hozzáadás összeállítása lépés**, majd jelölje be **rendszerhéj végrehajtása**
- A hello **parancs** mezőbe, írja be a következő Docker parancsok hello, majd válassza ki **mentése**:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

hello Docker build lépéseket kép és a hello Jenkins buildszám, akkor is fenntartható a képek előzményeit címke létrehozása. Hello alkalmazást futtató meglévő tárolókkal leáll, és eltávolítja majd. Új tároló van, akkor hello lemezkép használatával elindult és hello legújabb véglegesíti a Githubon alapján a Node.js-alkalmazást futtat.


## <a name="test-your-pipeline"></a>A folyamat tesztelése
toosee hello egész folyamat a művelet szerkesztése hello *index.js* újra a villás GitHub-tárház fájlt, és kattintson a **módosítás véglegesítése**. GitHub hello webhook meghatározásával Jenkins új feladat indítja el. Toocreate hello Docker kép néhány másodpercet vesz igénybe, és indítsa el az alkalmazást egy új tárolóba.

Szükség esetén újra be hello nyilvános IP-címet a virtuális gép:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Nyisson meg egy webböngészőt, és írja be `http://<publicIps>:1337`. A Node.js-alkalmazás jelenik meg, és által adott jelentéseket tükrözik hello legújabb véglegesíti a Githubon elágazás a következőképpen:

![Futó Node.js-alkalmazás](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Ellenőrizze egy másik Szerkesztés toohello *index.js* fájlt a Githubon és a véglegesítési hello módosítása. Várjon néhány másodpercet, amíg a Jenkins hello feladat toocomplete, majd frissítse a webes böngésző toosee hello frissített verziója az alkalmazás fut egy új tároló az alábbiak szerint:

![Node.js-alkalmazás futtatása után egy másik GitHub véglegesítési](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban GitHub toorun Jenkins összeállítási feladat minden kód véglegesítés konfigurált, és majd egy Docker-tároló tootest az alkalmazás üzembe helyezése. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Jenkins virtuális gép létrehozása
> * Telepítse és konfigurálja a Jenkins
> * GitHub és Jenkins integrációjával webhook létrehozása
> * Hozzon létre és Jenkins feladatok létrehozása a Githubról eseményindító véglegesítése
> * Az alkalmazás Docker-lemezkép létrehozása
> * Ellenőrizze a Githubon véglegesíti és hozhat létre. új Docker-lemezkép alkalmazást futtató frissítések

További információt következő útmutató toolearn toohello előzetes toointegrate a Visual Studio Team Services Jenkins.

> [!div class="nextstepaction"]
> [Alkalmazások telepítése a Jenkins és Team Services](tutorial-build-deploy-jenkins.md)