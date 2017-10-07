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
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a><span data-ttu-id="6e72d-104">Az Azure Tárolószolgáltatás és Kubernetes Jenkins integráció</span><span class="sxs-lookup"><span data-stu-id="6e72d-104">Jenkins integration with Azure Container Service and Kubernetes</span></span> 
<span data-ttu-id="6e72d-105">Ebben az oktatóanyagban azt bízná hello folyamat tooset a több tároló alkalmazások folyamatos integrálása az Azure-tároló szolgáltatás Kubernetes hello Jenkins platformot használnak.</span><span class="sxs-lookup"><span data-stu-id="6e72d-105">In this tutorial, we walk through hello process tooset up continuous integration of a multi-container application into Azure Container Service Kubernetes using hello Jenkins platform.</span></span> <span data-ttu-id="6e72d-106">hello munkafolyamat hello tároló képet Docker központban frissíti, és a frissítés hello Kubernetes három munkaállomás-csoporttal egy központi telepítési bevezetés használatával.</span><span class="sxs-lookup"><span data-stu-id="6e72d-106">hello workflow updates hello container image in Docker Hub and upgrades hello Kubernetes pods using a deployment rollout.</span></span> 

## <a name="high-level-process"></a><span data-ttu-id="6e72d-107">Magas szintű folyamata</span><span class="sxs-lookup"><span data-stu-id="6e72d-107">High level process</span></span>
<span data-ttu-id="6e72d-108">Ebben a cikkben ismertetett hello lépéseken a következők:</span><span class="sxs-lookup"><span data-stu-id="6e72d-108">hello basic steps detailed in this article are:</span></span> 
- <span data-ttu-id="6e72d-109">A Tárolószolgáltatás Kubernetes fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="6e72d-109">Install a Kubernetes cluster in Container Service</span></span>
- <span data-ttu-id="6e72d-110">Jenkins beállítása és konfigurálása a hozzáférés tooContainer szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6e72d-110">Set up Jenkins and configure access tooContainer Service</span></span>
- <span data-ttu-id="6e72d-111">Jenkins munkafolyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e72d-111">Create a Jenkins workflow</span></span>
- <span data-ttu-id="6e72d-112">Hello CI/CD folyamat befejezési tooend tesztelése</span><span class="sxs-lookup"><span data-stu-id="6e72d-112">Test hello CI/CD process end tooend</span></span>

## <a name="install-a-kubernetes-cluster"></a><span data-ttu-id="6e72d-113">Kubernetes fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="6e72d-113">Install a Kubernetes cluster</span></span>
    
<span data-ttu-id="6e72d-114">Az Azure Tárolószolgáltatásban hello lépések használatával hello Kubernetes fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6e72d-114">Deploy hello Kubernetes cluster in Azure Container Service using hello following steps.</span></span> <span data-ttu-id="6e72d-115">Teljes dokumentációját helyezkedik [Itt](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="6e72d-115">Full documentation is located [here](container-service-kubernetes-walkthrough.md).</span></span>

### <a name="step-1-create-a-resource-group"></a><span data-ttu-id="6e72d-116">1. lépés:, Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="6e72d-116">Step 1: Create a resource group</span></span>
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-hello-cluster"></a><span data-ttu-id="6e72d-117">2. lépés: Hello fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6e72d-117">Step 2: Deploy hello cluster</span></span>
> [!NOTE]
> <span data-ttu-id="6e72d-118">hello alábbi lépés megköveteli egy helyi nyilvános SSH-kulcs hello ~/.ssh mappájában található.</span><span class="sxs-lookup"><span data-stu-id="6e72d-118">hello following steps require a local SSH public key stored in hello ~/.ssh folder.</span></span>
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

## <a name="set-up-jenkins-and-configure-access-toocontainer-service"></a><span data-ttu-id="6e72d-119">Jenkins beállítása és konfigurálása a hozzáférés tooContainer szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6e72d-119">Set up Jenkins and configure access tooContainer Service</span></span>

### <a name="step-1-install-jenkins"></a><span data-ttu-id="6e72d-120">1. lépés: Jenkins telepítése</span><span class="sxs-lookup"><span data-stu-id="6e72d-120">Step 1: Install Jenkins</span></span>
1. <span data-ttu-id="6e72d-121">Hozzon létre egy Azure virtuális gép Ubuntu 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="6e72d-121">Create an Azure VM with Ubuntu 16.04 LTS.</span></span>  <span data-ttu-id="6e72d-122">Mivel hello későbbi lépések fog kell tooconnect toothis VM segítségével bash a helyi gépén, set hello "Hitelesítési típus" too'SSH nyilvános kulcs "és a Beillesztés hello nyilvános SSH-kulcsot, amely a ~/.ssh mappában helyileg van tárolva.</span><span class="sxs-lookup"><span data-stu-id="6e72d-122">Since later in hello steps you will need tooconnect toothis VM using bash on your local machine, set hello 'Authentication type' too'SSH public key' and paste hello SSH public key that is stored locally in your ~/.ssh folder.</span></span>  <span data-ttu-id="6e72d-123">Továbbá jegyezze fel a hello "Felhasználónév" meg, mivel ez a felhasználónév szükséges tooview hello Jenkins irányítópult és a későbbi lépésekben Jenkins VM toohello csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="6e72d-123">Also, take note of hello 'User name' that you specify since this user name will be needed tooview hello Jenkins dashboard and for connecting toohello Jenkins VM in later steps.</span></span>
2. <span data-ttu-id="6e72d-124">Ezek keresztül Jenkins telepítése [utasításokat](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="6e72d-124">Install Jenkins via these [instructions](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span></span> <span data-ttu-id="6e72d-125">Egy részletes oktatóanyag jelenleg [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span><span class="sxs-lookup"><span data-stu-id="6e72d-125">A more detailed tutorial is at [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span></span>
3. <span data-ttu-id="6e72d-126">tooview hello Jenkins irányítópult a helyi gépén, hello Azure hálózati biztonsági csoport tooallow 8080-as porton frissítése egy bejövő szabályt, amely lehetővé teszi a 8080-as hozzáférés tooport hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="6e72d-126">tooview hello Jenkins dashboard on your local machine, update hello Azure network security group tooallow port 8080 by adding an inbound rule that allows access tooport 8080.</span></span>  <span data-ttu-id="6e72d-127">Port továbbítás azt is megteheti, előfordulhat, hogy beállítása a parancs futtatásával:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span><span class="sxs-lookup"><span data-stu-id="6e72d-127">Alternatively, you may setup port forwarding by running this command: `ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span></span>
4. <span data-ttu-id="6e72d-128">Csatlakozás tooyour Jenkins server hello böngészővel toohello nyilvános IP-cím megnyitásával (http:// < your_jenkins_public_ip >: 8080) feloldása hello Jenkins irányítópult hello az első alkalommal hello kezdeti rendszergazdai jelszóval.</span><span class="sxs-lookup"><span data-stu-id="6e72d-128">Connect tooyour Jenkins server using hello browser by navigating toohello public IP (http://<your_jenkins_public_ip>:8080) and unlock hello Jenkins dashboard for hello first time with hello initial admin password.</span></span>  <span data-ttu-id="6e72d-129">hello rendszergazdai jelszó a hello Jenkins VM /var/lib/jenkins/secrets/initialAdminPassword tárolódik.</span><span class="sxs-lookup"><span data-stu-id="6e72d-129">hello admin password is stored at /var/lib/jenkins/secrets/initialAdminPassword on hello Jenkins VM.</span></span>  <span data-ttu-id="6e72d-130">Egy egyszerű módot tooget, ez a jelszó nem tooSSH be hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="6e72d-130">An easy way tooget this password is tooSSH into hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="6e72d-131">Ezt követően: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span><span class="sxs-lookup"><span data-stu-id="6e72d-131">Next, run: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span></span>
5. <span data-ttu-id="6e72d-132">Telepítse a Docker hello Jenkins gép ezen keresztül a [utasításokat](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span><span class="sxs-lookup"><span data-stu-id="6e72d-132">Install Docker on hello Jenkins machine via these [instructions](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span></span> <span data-ttu-id="6e72d-133">Ez lehetővé teszi a Docker parancsok toobe Jenkins feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="6e72d-133">This allows for Docker commands toobe run in Jenkins jobs.</span></span>
6. <span data-ttu-id="6e72d-134">Konfigurálja a Docker engedélyek tooallow Jenkins tooaccess hello Docker végpontot.</span><span class="sxs-lookup"><span data-stu-id="6e72d-134">Configure Docker permissions tooallow Jenkins tooaccess hello Docker endpoint.</span></span>

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. <span data-ttu-id="6e72d-135">Telepítés `kubectl` Jenkins a parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="6e72d-135">Install `kubectl` CLI on Jenkins.</span></span> <span data-ttu-id="6e72d-136">További részletekért erővel [telepítése és beállítása a kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="6e72d-136">More details are at [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>  <span data-ttu-id="6e72d-137">Jenkins feladatok "kubectl" toomanage használnak, illetve toohello Kubernetes fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6e72d-137">Jenkins jobs will use 'kubectl' toomanage and deploy toohello Kubernetes cluster.</span></span>

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-toohello-kubernetes-cluster"></a><span data-ttu-id="6e72d-138">2. lépés: A hozzáférés toohello Kubernetes fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="6e72d-138">Step 2: Set up access toohello Kubernetes cluster</span></span>

> [!NOTE]
> <span data-ttu-id="6e72d-139">Több megközelítés tooaccomplishing hello lépések van.</span><span class="sxs-lookup"><span data-stu-id="6e72d-139">There are multiple approaches tooaccomplishing hello following steps.</span></span> <span data-ttu-id="6e72d-140">Amely az Ön a legegyszerűbb hello módszert használja.</span><span class="sxs-lookup"><span data-stu-id="6e72d-140">Use hello approach that is easiest for you.</span></span>
>

1. <span data-ttu-id="6e72d-141">Másolás hello `kubectl` konfigurációs fájl toohello Jenkins számítógépre, hogy Jenkins hozzáférés toohello Kubernetes fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6e72d-141">Copy hello `kubectl` config file toohello Jenkins machine so that Jenkins jobs have access toohello Kubernetes cluster.</span></span> <span data-ttu-id="6e72d-142">Ezek az utasítások azt feltételezik, hogy egy másik gépről bash használunk, mint Jenkins VM hello és, hogy egy helyi nyilvános SSH-kulcs hello gép ~/.ssh mappában tárolja.</span><span class="sxs-lookup"><span data-stu-id="6e72d-142">These instructions assume that you are using bash from a different machine than hello Jenkins VM and that a local SSH public key is stored in hello machine's ~/.ssh folder.</span></span>

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. <span data-ttu-id="6e72d-143">A Jenkins ellenőrizze, hogy hello Kubernetes fürt érhető el.</span><span class="sxs-lookup"><span data-stu-id="6e72d-143">Validate from Jenkins that hello Kubernetes cluster is accessible.</span></span>  <span data-ttu-id="6e72d-144">toodo, az SSH-ból hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="6e72d-144">toodo this, SSH into hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="6e72d-145">Ezután ellenőrizze a Jenkins sikeresen csatlakozni tud a fürt tooyour: `kubectl cluster-info`.</span><span class="sxs-lookup"><span data-stu-id="6e72d-145">Next, verify Jenkins can successfully connect tooyour cluster: `kubectl cluster-info`.</span></span>
    

## <a name="create-a-jenkins-workflow"></a><span data-ttu-id="6e72d-146">Jenkins munkafolyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e72d-146">Create a Jenkins workflow</span></span>

### <a name="prerequisites"></a><span data-ttu-id="6e72d-147">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6e72d-147">Prerequisites</span></span>

- <span data-ttu-id="6e72d-148">A kód tárház GitHub-fiók.</span><span class="sxs-lookup"><span data-stu-id="6e72d-148">GitHub account for code repo.</span></span>
- <span data-ttu-id="6e72d-149">Docker Hub fiók toostore és frissítés képek.</span><span class="sxs-lookup"><span data-stu-id="6e72d-149">Docker Hub account toostore and update images.</span></span>
- <span data-ttu-id="6e72d-150">Indexelése alkalmazás újbóli létrehozása és frissítése.</span><span class="sxs-lookup"><span data-stu-id="6e72d-150">Containerized application that can be rebuilt and updated.</span></span> <span data-ttu-id="6e72d-151">Ez a minta tároló alkalmazással Golang írt: https://github.com/chzbrgr71/go-web</span><span class="sxs-lookup"><span data-stu-id="6e72d-151">You can use this sample container app written in Golang: https://github.com/chzbrgr71/go-web</span></span> 

> [!NOTE]
> <span data-ttu-id="6e72d-152">hello következő lépéseket kell végrehajtani a saját GitHub-fiók.</span><span class="sxs-lookup"><span data-stu-id="6e72d-152">hello following steps must be performed in your own GitHub account.</span></span> <span data-ttu-id="6e72d-153">Érzi, hogy a fenti tárház, szabad tooclone hello, de a saját fiók tooconfigure hello webhookok kell használnia, és Jenkins eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="6e72d-153">Feel free tooclone hello above repo, but you must use your own account tooconfigure hello webhooks and Jenkins access.</span></span>
>

### <a name="step-1-deploy-initial-v1-of-application"></a><span data-ttu-id="6e72d-154">1. lépés: Kezdeti v1 alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6e72d-154">Step 1: Deploy initial v1 of application</span></span>
1. <span data-ttu-id="6e72d-155">Hozza létre a hello fejlesztői gépen hello alkalmazását a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="6e72d-155">Build hello app from hello developer machine with hello following commands.</span></span> <span data-ttu-id="6e72d-156">Cserélje le `myrepo` az Ön által.</span><span class="sxs-lookup"><span data-stu-id="6e72d-156">Replace `myrepo` with your own.</span></span>
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. <span data-ttu-id="6e72d-157">Leküldéses kép tooDocker központ.</span><span class="sxs-lookup"><span data-stu-id="6e72d-157">Push image tooDocker Hub.</span></span>

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. <span data-ttu-id="6e72d-158">Toohello Kubernetes fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6e72d-158">Deploy toohello Kubernetes cluster.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="6e72d-159">Hello szerkesztése `go-web.yaml` tooupdate fájl, a tároló kép és a tárházban.</span><span class="sxs-lookup"><span data-stu-id="6e72d-159">Edit hello `go-web.yaml` file tooupdate your container image and repo.</span></span>
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a><span data-ttu-id="6e72d-160">2. lépés: Jenkins rendszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e72d-160">Step 2: Configure Jenkins system</span></span>
1. <span data-ttu-id="6e72d-161">Kattintson a **Jenkins kezelése** > **rendszer konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-161">Click **Manage Jenkins** > **Configure System**.</span></span>
2. <span data-ttu-id="6e72d-162">A **GitHub**, jelölje be **GitHub-kiszolgáló hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-162">Under **GitHub**, select **Add GitHub Server**.</span></span>
3. <span data-ttu-id="6e72d-163">Hagyja **API URL-címe** alapértelmezettként.</span><span class="sxs-lookup"><span data-stu-id="6e72d-163">Leave **API URL** as default.</span></span>
4. <span data-ttu-id="6e72d-164">A **hitelesítő adatok**, vegye fel a Jenkins hitelesítő adatok használatával **titkos szöveg**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-164">Under **Credentials**, add a Jenkins credential using **Secret text**.</span></span> <span data-ttu-id="6e72d-165">Azt javasoljuk, GitHub személyes hozzáférési jogkivonatok, amelyek a GitHub felhasználói fiók beállítások vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6e72d-165">We recommend using GitHub personal access tokens, which are configured in your GitHub user account settings.</span></span> <span data-ttu-id="6e72d-166">Ezzel kapcsolatban további részletek [itt.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span><span class="sxs-lookup"><span data-stu-id="6e72d-166">More details on this [here.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span></span>
5. <span data-ttu-id="6e72d-167">Kattintson a **tesztkapcsolat** tooensure ennek megfelelően van-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6e72d-167">Click **Test connection** tooensure this is configured correctly.</span></span>
6. <span data-ttu-id="6e72d-168">A **globális tulajdonságok**, adjon hozzá egy környezeti változó `DOCKER_HUB` , és adja meg a Docker Hub jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6e72d-168">Under **Global Properties**, add an environment variable `DOCKER_HUB` and provide your Docker Hub password.</span></span> <span data-ttu-id="6e72d-169">(Ez akkor hasznos, ebben a bemutatóban szereplő, de egy éles telepítési forgatókönyvhöz egy biztonságosabb módszert igényel.)</span><span class="sxs-lookup"><span data-stu-id="6e72d-169">(This is useful in this demo, but a production scenario would require a more secure approach.)</span></span>
7. <span data-ttu-id="6e72d-170">Mentse.</span><span class="sxs-lookup"><span data-stu-id="6e72d-170">Save.</span></span>

![Jenkins GitHub-hozzáférés](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-hello-jenkins-workflow"></a><span data-ttu-id="6e72d-172">3. lépés: Hello Jenkins munkafolyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e72d-172">Step 3: Create hello Jenkins workflow</span></span>
1. <span data-ttu-id="6e72d-173">Hozzon létre egy Jenkins elemet.</span><span class="sxs-lookup"><span data-stu-id="6e72d-173">Create a Jenkins item.</span></span>
2. <span data-ttu-id="6e72d-174">Adjon meg egy nevet (például "Ugrás-web"), majd **Freestyle projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-174">Provide a name (for example, "go-web") and select **Freestyle Project**.</span></span> 
3. <span data-ttu-id="6e72d-175">Ellenőrizze **GitHub-projekt** , és adja meg a hello URL-cím tooyour GitHub-tárház.</span><span class="sxs-lookup"><span data-stu-id="6e72d-175">Check **GitHub project** and provide hello URL tooyour GitHub repo.</span></span>
4. <span data-ttu-id="6e72d-176">A **forrás kód felügyeleti**, adja meg a hello GitHub-tárház URL-CÍMÉT és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="6e72d-176">In **Source Code Management**, provide hello GitHub repo URL and credentials.</span></span> 
5. <span data-ttu-id="6e72d-177">Adja hozzá a **összeállítása lépés** típusú **hajtható végre a rendszerhéj** és hello használja a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="6e72d-177">Add a **Build Step** of type **Execute shell** and use hello following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. <span data-ttu-id="6e72d-178">Adja hozzá egy másik **összeállítása lépés** típusú **hajtható végre a rendszerhéj** és hello használja a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="6e72d-178">Add another **Build Step** of type **Execute shell** and use hello following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins build lépései](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. <span data-ttu-id="6e72d-180">Hello Jenkins elem mentése és tesztelés **Build most**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-180">Save hello Jenkins item and test with **Build Now**.</span></span>

### <a name="step-4-connect-github-webhook"></a><span data-ttu-id="6e72d-181">4. lépés: Csatlakozás GitHub webhook</span><span class="sxs-lookup"><span data-stu-id="6e72d-181">Step 4: Connect GitHub webhook</span></span>
1. <span data-ttu-id="6e72d-182">A létrehozott hello Jenkins elem kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-182">In hello Jenkins item you created, click **Configure**.</span></span>
2. <span data-ttu-id="6e72d-183">A **Build eseményindítók**, jelölje be **GitHub hook eseményindítója a következőnek: GITScm lekérdezési** és **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-183">Under **Build Triggers**, select **GitHub hook trigger for GITScm polling** and **Save**.</span></span> <span data-ttu-id="6e72d-184">Ez automatikusan konfigurálja az hello GitHub webhook.</span><span class="sxs-lookup"><span data-stu-id="6e72d-184">This automatically configures hello GitHub webhook.</span></span>
3. <span data-ttu-id="6e72d-185">Kattintson a webes nyissa meg a GitHub-tárház, **beállítások > Webhookok**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-185">In your GitHub repo for go-web, click **Settings > Webhooks**.</span></span>
4. <span data-ttu-id="6e72d-186">Győződjön meg arról, hogy hello Jenkins webhook URL-cím felvétele sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="6e72d-186">Verify that hello Jenkins webhook URL was added successfully.</span></span> <span data-ttu-id="6e72d-187">hello URL-címet a "github webhook" kell végződnie.</span><span class="sxs-lookup"><span data-stu-id="6e72d-187">hello URL should end in "github-webhook".</span></span>

![Jenkins webhook konfiguráció](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-hello-cicd-process-end-tooend"></a><span data-ttu-id="6e72d-189">Hello CI/CD folyamat befejezési tooend tesztelése</span><span class="sxs-lookup"><span data-stu-id="6e72d-189">Test hello CI/CD process end tooend</span></span>

1. <span data-ttu-id="6e72d-190">Frissítse a kódot hello tárház és leküldéses/szinkronizálási hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="6e72d-190">Update code for hello repo and push/synch with hello GitHub repository.</span></span>
2. <span data-ttu-id="6e72d-191">Hello Jenkins konzolról, ellenőrizze a hello **Build előzmények** , és ellenőrizze, hogy hello feladat futott.</span><span class="sxs-lookup"><span data-stu-id="6e72d-191">From hello Jenkins console, check hello **Build History** and validate that hello job has run.</span></span> <span data-ttu-id="6e72d-192">Konzol kimeneti toosee részleteinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="6e72d-192">View console output toosee details.</span></span>
3. <span data-ttu-id="6e72d-193">Kubernetes, a hello részleteinek megtekintése a központi telepítés frissítése:</span><span class="sxs-lookup"><span data-stu-id="6e72d-193">From Kubernetes, view details of hello upgraded deployment:</span></span>

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a><span data-ttu-id="6e72d-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e72d-194">Next steps</span></span>

- <span data-ttu-id="6e72d-195">Azure tároló beállításjegyzék telepítése, és egy biztonságos tárházban lemezképeket menteni.</span><span class="sxs-lookup"><span data-stu-id="6e72d-195">Deploy Azure Container Registry and store images in a secure repository.</span></span> <span data-ttu-id="6e72d-196">Lásd: [Azure tároló beállításjegyzék docs](https://docs.microsoft.com/azure/container-registry).</span><span class="sxs-lookup"><span data-stu-id="6e72d-196">See [Azure Container Registry docs](https://docs.microsoft.com/azure/container-registry).</span></span>
- <span data-ttu-id="6e72d-197">Egy összetettebb munkafolyamat tartalmaz egymás melletti központi telepítés és automatikus tesztek a Jenkins felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6e72d-197">Build a more complex workflow that includes side-by-side deployment and automated tests in Jenkins.</span></span>
- <span data-ttu-id="6e72d-198">CI/CD Jenkins és Kubernetes kapcsolatos további információkért lásd: hello [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span><span class="sxs-lookup"><span data-stu-id="6e72d-198">For more information about CI/CD with Jenkins and Kubernetes, see hello [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span></span>
