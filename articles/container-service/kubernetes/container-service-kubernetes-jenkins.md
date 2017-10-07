---
title: "az Azure Tárolószolgáltatásban Kubernetes CI/CD aaaJenkins |} Microsoft Docs"
description: "Hogyan tooautomate CI/CD-ről a Jenkins toodeploy feldolgozni, és az Azure Tárolószolgáltatásban Kubernetes egy indexelése alkalmazás frissítése"
services: container-service
documentationcenter: 
author: chzbrgr71
manager: johny
editor: 
tags: acs, azure-container-service, jenkins
keywords: "Docker, tárolók, Kubernetes, Azure, Jenkins"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: e00e13bf06619bed73e82878777e55458ea3dd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Az Azure Tárolószolgáltatás és Kubernetes Jenkins integráció 
Ebben az oktatóanyagban azt bízná hello folyamat tooset a több tároló alkalmazások folyamatos integrálása az Azure-tároló szolgáltatás Kubernetes hello Jenkins platformot használnak. hello munkafolyamat hello tároló képet Docker központban frissíti, és a frissítés hello Kubernetes három munkaállomás-csoporttal egy központi telepítési bevezetés használatával. 

## <a name="high-level-process"></a>Magas szintű folyamata
Ebben a cikkben ismertetett hello lépéseken a következők: 
- A Tárolószolgáltatás Kubernetes fürt telepítése
- Jenkins beállítása és konfigurálása a hozzáférés tooContainer szolgáltatás
- Jenkins munkafolyamat létrehozása
- Hello CI/CD folyamat befejezési tooend tesztelése

## <a name="install-a-kubernetes-cluster"></a>Kubernetes fürt telepítése
    
Az Azure Tárolószolgáltatásban hello lépések használatával hello Kubernetes fürt központi telepítése. Teljes dokumentációját helyezkedik [Itt](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>1. lépés:, Hozzon létre egy erőforráscsoportot
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-hello-cluster"></a>2. lépés: Hello fürt központi telepítése
> [!NOTE]
> hello alábbi lépés megköveteli egy helyi nyilvános SSH-kulcs hello ~/.ssh mappájában található.
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-toocontainer-service"></a>Jenkins beállítása és konfigurálása a hozzáférés tooContainer szolgáltatás

### <a name="step-1-install-jenkins"></a>1. lépés: Jenkins telepítése
1. Hozzon létre egy Azure virtuális gép Ubuntu 16.04 LTS.  Mivel hello későbbi lépések fog kell tooconnect toothis VM segítségével bash a helyi gépén, set hello "Hitelesítési típus" too'SSH nyilvános kulcs "és a Beillesztés hello nyilvános SSH-kulcsot, amely a ~/.ssh mappában helyileg van tárolva.  Továbbá jegyezze fel a hello "Felhasználónév" meg, mivel ez a felhasználónév szükséges tooview hello Jenkins irányítópult és a későbbi lépésekben Jenkins VM toohello csatlakozni.
2. Ezek keresztül Jenkins telepítése [utasításokat](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). Egy részletes oktatóanyag jelenleg [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. tooview hello Jenkins irányítópult a helyi gépén, hello Azure hálózati biztonsági csoport tooallow 8080-as porton frissítése egy bejövő szabályt, amely lehetővé teszi a 8080-as hozzáférés tooport hozzáadásával.  Port továbbítás azt is megteheti, előfordulhat, hogy beállítása a parancs futtatásával:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Csatlakozás tooyour Jenkins server hello böngészővel toohello nyilvános IP-cím megnyitásával (http:// < your_jenkins_public_ip >: 8080) feloldása hello Jenkins irányítópult hello az első alkalommal hello kezdeti rendszergazdai jelszóval.  hello rendszergazdai jelszó a hello Jenkins VM /var/lib/jenkins/secrets/initialAdminPassword tárolódik.  Egy egyszerű módot tooget, ez a jelszó nem tooSSH be hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Ezt követően: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Telepítse a Docker hello Jenkins gép ezen keresztül a [utasításokat](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts). Ez lehetővé teszi a Docker parancsok toobe Jenkins feladatok futtatásához.
6. Konfigurálja a Docker engedélyek tooallow Jenkins tooaccess hello Docker végpontot.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Telepítés `kubectl` Jenkins a parancssori felület. További részletekért erővel [telepítése és beállítása a kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).  Jenkins feladatok "kubectl" toomanage használnak, illetve toohello Kubernetes fürt központi telepítése.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-toohello-kubernetes-cluster"></a>2. lépés: A hozzáférés toohello Kubernetes fürt beállítása

> [!NOTE]
> Több megközelítés tooaccomplishing hello lépések van. Amely az Ön a legegyszerűbb hello módszert használja.
>

1. Másolás hello `kubectl` konfigurációs fájl toohello Jenkins számítógépre, hogy Jenkins hozzáférés toohello Kubernetes fürt rendelkezik. Ezek az utasítások azt feltételezik, hogy egy másik gépről bash használunk, mint Jenkins VM hello és, hogy egy helyi nyilvános SSH-kulcs hello gép ~/.ssh mappában tárolja.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. A Jenkins ellenőrizze, hogy hello Kubernetes fürt érhető el.  toodo, az SSH-ból hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Ezután ellenőrizze a Jenkins sikeresen csatlakozni tud a fürt tooyour: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Jenkins munkafolyamat létrehozása

### <a name="prerequisites"></a>Előfeltételek

- A kód tárház GitHub-fiók.
- Docker Hub fiók toostore és frissítés képek.
- Indexelése alkalmazás újbóli létrehozása és frissítése. Ez a minta tároló alkalmazással Golang írt: https://github.com/chzbrgr71/go-web 

> [!NOTE]
> hello következő lépéseket kell végrehajtani a saját GitHub-fiók. Érzi, hogy a fenti tárház, szabad tooclone hello, de a saját fiók tooconfigure hello webhookok kell használnia, és Jenkins eléréséhez.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>1. lépés: Kezdeti v1 alkalmazás központi telepítése
1. Hozza létre a hello fejlesztői gépen hello alkalmazását a következő parancsok hello. Cserélje le `myrepo` az Ön által.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Leküldéses kép tooDocker központ.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Toohello Kubernetes fürt központi telepítése.
    
    > [!NOTE] 
    > Hello szerkesztése `go-web.yaml` tooupdate fájl, a tároló kép és a tárházban.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>2. lépés: Jenkins rendszer konfigurálása
1. Kattintson a **Jenkins kezelése** > **rendszer konfigurálása**.
2. A **GitHub**, jelölje be **GitHub-kiszolgáló hozzáadása**.
3. Hagyja **API URL-címe** alapértelmezettként.
4. A **hitelesítő adatok**, vegye fel a Jenkins hitelesítő adatok használatával **titkos szöveg**. Azt javasoljuk, GitHub személyes hozzáférési jogkivonatok, amelyek a GitHub felhasználói fiók beállítások vannak konfigurálva. Ezzel kapcsolatban további részletek [itt.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
5. Kattintson a **tesztkapcsolat** tooensure ennek megfelelően van-e konfigurálva.
6. A **globális tulajdonságok**, adjon hozzá egy környezeti változó `DOCKER_HUB` , és adja meg a Docker Hub jelszavát. (Ez akkor hasznos, ebben a bemutatóban szereplő, de egy éles telepítési forgatókönyvhöz egy biztonságosabb módszert igényel.)
7. Mentse.

![Jenkins GitHub-hozzáférés](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-hello-jenkins-workflow"></a>3. lépés: Hello Jenkins munkafolyamat létrehozása
1. Hozzon létre egy Jenkins elemet.
2. Adjon meg egy nevet (például "Ugrás-web"), majd **Freestyle projekt**. 
3. Ellenőrizze **GitHub-projekt** , és adja meg a hello URL-cím tooyour GitHub-tárház.
4. A **forrás kód felügyeleti**, adja meg a hello GitHub-tárház URL-CÍMÉT és hitelesítő adatait. 
5. Adja hozzá a **összeállítása lépés** típusú **hajtható végre a rendszerhéj** és hello használja a következő szöveget:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Adja hozzá egy másik **összeállítása lépés** típusú **hajtható végre a rendszerhéj** és hello használja a következő szöveget:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins build lépései](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Hello Jenkins elem mentése és tesztelés **Build most**.

### <a name="step-4-connect-github-webhook"></a>4. lépés: Csatlakozás GitHub webhook
1. A létrehozott hello Jenkins elem kattintson **konfigurálása**.
2. A **Build eseményindítók**, jelölje be **GitHub hook eseményindítója a következőnek: GITScm lekérdezési** és **mentése**. Ez automatikusan konfigurálja az hello GitHub webhook.
3. Kattintson a webes nyissa meg a GitHub-tárház, **beállítások > Webhookok**.
4. Győződjön meg arról, hogy hello Jenkins webhook URL-cím felvétele sikeresen megtörtént. hello URL-címet a "github webhook" kell végződnie.

![Jenkins webhook konfiguráció](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-hello-cicd-process-end-tooend"></a>Hello CI/CD folyamat befejezési tooend tesztelése

1. Frissítse a kódot hello tárház és leküldéses/szinkronizálási hello GitHub-tárházban.
2. Hello Jenkins konzolról, ellenőrizze a hello **Build előzmények** , és ellenőrizze, hogy hello feladat futott. Konzol kimeneti toosee részleteinek megtekintése.
3. Kubernetes, a hello részleteinek megtekintése a központi telepítés frissítése:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Következő lépések

- Azure tároló beállításjegyzék telepítése, és egy biztonságos tárházban lemezképeket menteni. Lásd: [Azure tároló beállításjegyzék docs](https://docs.microsoft.com/azure/container-registry).
- Egy összetettebb munkafolyamat tartalmaz egymás melletti központi telepítés és automatikus tesztek a Jenkins felépítéséhez.
- CI/CD Jenkins és Kubernetes kapcsolatos további információkért lásd: hello [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).
